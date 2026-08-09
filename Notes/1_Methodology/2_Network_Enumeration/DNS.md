## Basic DNS Queries (dig)
###### Queries the Start of Authority (SOA) record for a specific domain
```
dig soa [www.inlanefreight.com](https://www.inlanefreight.com)
```
###### Queries the Name Server (NS) records for a domain
```
dig ns inlanefreight.htb @10.129.14.128
```
###### Sends an `ANY` query to the DNS server, requesting all available DNS records
```
dig ANY inlanefreight.htb @10.129.14.128
```
## Version Enumeration
###### query the DNS server's software version
```
dig CH TXT version.bind 10.129.120.85
```
## Zone Transfers (AXFR)
###### Asynchronous Full Transfer Zone (AXFR) request against the target DNS server
```
dig axfr inlanefreight.htb @10.129.14.128
```
###### Attempts a zone transfer specifically targeting an internal subdomain/zone
```
dig axfr internal.inlanefreight.htb @10.129.14.128
```
## Subdomain Brute-Forcing 
```
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```
- A Bash one-liner that iterates through a wordlist (from SecLists), appending each word as a subdomain to the target domain, and queries the specific DNS server (`10.129.14.128`) for A records. The output is filtered and saved to `subdomains.txt`.

```
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```
- Uses the `dnsenum` tool to automate DNS enumeration. It performs standard queries, attempts zone transfers, and executes a dictionary-based brute-force attack to discover subdomains, saving the results to an output file.
## Config Analysis
###### Reads the local BIND9 configuration file on a Linux server
```
cat /etc/bind/named.conf.local
```
###### Reads a specific Forward Zone file (BIND format)
```
cat /etc/bind/db.domain.com
```
###### Reads a Reverse Name Resolution Zone file
```
cat /etc/bind/db.10.129.14
```