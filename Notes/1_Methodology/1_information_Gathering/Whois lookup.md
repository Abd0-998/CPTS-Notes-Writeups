## Tool Installation
```
sudo apt update && sudo apt install whois -y
```
    
    - Installs the `whois` command-line utility on Debian/Ubuntu-based systems (like Parrot OS or Kali Linux) if it is not already present.

## Basic Enumeration
```
whois <domain.com> 
```
    
    - Queries the public WHOIS database to retrieve registration and ownership details about a specific domain.

## Key data points 

When you run a WHOIS query on a target, always look for the following fields

#### Creation / Registration Date
- Identifies how old the domain is
- Newly registered domains are often associated with phishing campaigns
#### Expiry Date 
- Shows when the domain registration runs out.
- Domains close to expiration can be potential Domain/Subdomain Takeover vulnerabilities
#### Registrant Organization & Contact Info
- Reveals the legal entity that owns the domain and their administrative emails
- targets scope and finding other domains owned by the same company
#### Name Servers (NS)
- Reveals the DNS infrastructure the target is using
- If they use custom name servers (e.g., `ns1.target.com`), they manage their own DNS
#### Domain Status
- Displays security locks
- This tells you how well-secured the domain registration account is