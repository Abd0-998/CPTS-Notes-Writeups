## Scanning & Footprinting

```
nmap -sV -sC 10.129.201.248 -p3389 --script rdp*
```
    
    - Scans the default RDP port (3389) using version detection (`-sV`) and default scripts (`-sC`). The `--script rdp*` flag executes all RDP-specific NSE scripts to extract information such as NLA (Network Level Authentication) status, product version, and hostname.
        
```
nmap -sV -sC 10.129.201.248 -p3389 --packet-trace --disable-arp-ping -n
```
    
    - Scans the RDP port while utilizing `--packet-trace` to manually track and inspect the contents of individual packets. This is useful for identifying the RDP cookies (e.g., `mstshash=nmap`) that Nmap sends, which might be flagged by EDR solutions on hardened networks.

## RDP Security Check Tool (Installation & Usage)

```
sudo cpan
```
    
    - Opens the CPAN shell to install Perl modules.
        
```
install Encoding::BER
```
    
    - A command executed inside the CPAN shell to install the dependency required for the `rdp-sec-check` script.
        
```
git clone [https://github.com/CiscoCXSecurity/rdp-sec-check.git](https://github.com/CiscoCXSecurity/rdp-sec-check.git) && cd rdp-sec-check
```
    
    - Clones the `rdp-sec-check.pl` repository and navigates into its directory.
        
```
./rdp-sec-check.pl 10.129.201.248
```
    
    - Runs the script to unauthentically identify the security settings and supported encryption methods of an RDP server based on its handshakes.

## RDP Service Interaction

```
xfreerdp /u:cry0l1t3 /p:"P455w0rd!" /v:10.129.201.248
```
    
    - Uses the `xfreerdp` client on Linux to authenticate and connect to the target RDP server, establishing a GUI session.
