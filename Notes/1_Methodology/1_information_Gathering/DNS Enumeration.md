## Local Name Resolution (`hosts` File)

In HTB/CPTS machines and real-world assessments, web applications often use Virtual Hosting (vhosts). Accessing the server via IP directly will fail or return default pages. You **must** map the IP to the hostname locally.

#### File Locations
- **Linux / macOS:** `/etc/hosts`
    
- **Windows:** `C:\Windows\System32\drivers\etc\hosts`

#### Syntax & Quick Editing
```
# Syntax: <IP_Address> <Hostname> [<Alias> ...]

# Quick append in Linux terminal:
echo "10.129.x.x target.htb sub.target.htb" | sudo tee -a /etc/hosts
```
## Practical Commands (`dig`)
```
# Query A Record
dig A target.htb @10.129.x.x

# Query CNAME Record (Check for Subdomain Takeover)
dig CNAME sub.target.htb @10.129.x.x

# Query TXT Records (Tech Stack / Verification)
dig TXT target.htb @10.129.x.x

# Query MX Records
dig MX target.htb @10.129.x.x

# Query ALL available records
dig ANY target.htb @10.129.x.x
```
#### Reverse DNS Lookup (PTR Query)
```
# Map an IP back to a hostname
dig -x 10.129.x.x @10.129.x.x
```

## Manual Querying with `dig`

```
# Query IPv4 Address (A Record)
dig target.htb A

# Query IPv6 Address (AAAA Record)
dig target.htb AAAA

# Query Mail Servers (MX Record)
dig target.htb MX

# Query Name Servers (NS Record)
dig target.htb NS

# Query Text Records (TXT Record)
dig target.htb TXT

# Query Canonical Name / Alias (CNAME Record)
dig target.htb CNAME

# Query Start of Authority (SOA Record)
dig target.htb SOA

# Query ALL available records
dig target.htb ANY
```

#### Targeted & Advanced `dig` Commands
```
# Query a Specific DNS Server (Crucial for HTB / Internal Targets)
dig @10.129.x.x target.htb

# Reverse DNS Lookup (Map IP to Hostname)
dig -x 10.129.x.x @10.129.x.x

# Trace Full Resolution Path (Root -> TLD -> Authoritative)
dig +trace target.htb
```
## Output Formatting for Scripting & Automation

When building Bash one-liners or piping output to other tools (`httpx`, `nmap`, `ffuf`), raw `dig` output contains too much clutter. Use these flags to clean it up:
```
# Output ONLY the IP / Answer (Best for scripting)
dig +short target.htb

# Output ONLY the Answer Section (Hides headers, footers, and questions)
dig +noall +answer target.htb A
```
## Subdomain Brute Forcing 
`dnsenum` is a comprehensive Perl-based script that handles standard DNS enumeration, zone transfer attempts, Google scraping, reverse lookups, WHOIS, and active brute-forcing.

```
dnsenum --enum <targ
et.com> -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```
    
    - Performs standard enumeration alongside a dictionary-based brute-force attack to discover hidden subdomains.
        
    - Flag Breakdown:
        
        --enum: A shortcut flag that bundles several default tuning options for standard enumeration.
            
         -f <path_to_wordlist>`: Specifies the wordlist to use for the brute-force attack.
            
        -r: Enables **recursive** subdomain brute-forcing. (If it finds `dev.target.com`, it will subsequently attempt to brute-force subdomains of that subdomain, e.g., `api.dev.target.com`).

## Zone Transfer AXFR 

#### Exploitation Commands (DIG)
To attempt a zone transfer, you must direct your request specifically to one of the target's authoritative name servers (which you can find beforehand using `dig target.com NS`).

- `dig axfr @<Name_Server> <Target_Domain>`
    
    - **Usage:** Instructs `dig` to request a Full Zone Transfer (`axfr`) from the specified DNS server.
        
- **Practical Example:**
    
    - `dig axfr @nsztm1.digi.ninja zonetransfer.me`
        
    - _(Note: `zonetransfer.me` is intentionally misconfigured for testing purposes)._

## Virtual Hosts 
#### Primary Tool: `gobuster` (VHost Mode)
`gobuster` has a dedicated `vhost` mode that specifically fuzzes the HTTP `Host` header while sending the traffic to a fixed IP or base URL.

- `gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain`
    
    - **Usage:** The standard command to fuzz virtual hosts.
        
    - **Flag Breakdown:**
        
        - `vhost`: Tells Gobuster to fuzz the `Host` header instead of directories or DNS.
            
        - `-u http://<IP_or_Domain>`: The base target URL.
            
        - `-w <path_to_wordlist>`: The wordlist (e.g., SecLists `subdomains-top1million-110000.txt`).
            
        - `--append-domain`: **Crucial for newer Gobuster versions.** It appends the base domain to each word in the wordlist (e.g., if the word is `admin`, it tests `admin.target.htb`).
		    
    - **Additional Tuning Flags:**

		- `-t 50`: Increases the number of concurrent threads to speed up the scan (default is 10).
    
		- `-k`: Ignores SSL/TLS certificate errors (essential when scanning HTTPS IPs with invalid or self-signed certs).
    
		-  `-o output.txt`: Saves the results to a file for reporting or further testing.

#### Post-Discovery (Local Resolution)
If you discover a hidden VHost (e.g., `forum.inlanefreight.htb`) that does not exist in public DNS, your browser will not be able to resolve it. You **must** map it locally before you can interact with it.

- `echo "<Target_IP> forum.inlanefreight.htb" | sudo tee -a /etc/hosts`
    
    - **Usage:** Appends the newly discovered VHost to your local `hosts` file, allowing your browser and proxy (like Burp Suite) to successfully route traffic to the hidden application using the correct `Host` header.

## Certificate logs
#### Practical Commands (CLI Enumeration)
Instead of relying solely on the web interface, you can use the terminal to rapidly query `crt.sh`, parse the JSON output, and filter for specific keywords (like `dev`, `api`, `staging`, etc.).
###### Querying `crt.sh` with `curl` and `jq`

```
curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[] | select(.name_value | contains("dev")) | .name_value' | sort -u
```

**Command Breakdown:**

- `curl -s "[https://crt.sh/?q=facebook.com&output=json](https://crt.sh/?q=facebook.com&output=json)"`: Silently fetches the certificate transparency logs for the target domain (`facebook.com`) and requests the output in JSON format.
    
- `jq -r`: A command-line JSON processor. The `-r` flag outputs raw strings (removing quotation marks).
    
- `.[] | select(.name_value | contains("dev"))`: Iterates through the JSON array and filters the output to only show entries where the `name_value` (the domain/subdomain name) contains the specific string `"dev"`. _(Note: You can change "dev" to anything relevant, or remove the `select` block entirely to dump all subdomains)._
    
- `.name_value`: Extracts only the domain/subdomain name from the filtered JSON objects.
    
- `sort -u`: Sorts the final list alphabetically and removes any duplicates (`-u` for unique).

## Key Takeaways 

- **Virtual Hosts (vhosts):** Always check if adding discovered subdomains or hostnames to `/etc/hosts` yields different web content.
    
- **Subdomain Takeovers:** Always investigate `CNAME` records returning a `NXDOMAIN` status or pointing to third-party services that are no longer active.
    
- **Active Directory Recon:** Look out for `SRV` records when targeting internal Active Directory networks to locate Domain Controllers and LDAP services.

- **Target Specific Name Servers (`@IP`):** In HTB machines and internal assessments, public DNS servers (like `8.8.8.8`) won't resolve internal domain names. Always direct your `dig` queries to the target machine's IP address (`dig @10.129.x.x target.htb`).
    
- **Use `+short` for Speed:** When you need a quick IP or list of subdomains to feed into another tool, `dig +short` gives you a clean list without needing `grep` or `awk`.
    
- **ANY Queries Caution:** Modern DNS servers often drop or truncate `ANY` queries (per RFC 8482) to prevent reflection attacks. If `ANY` returns nothing, query specific types (`A`, `AAAA`, `MX`, `TXT`, `NS`) individually.