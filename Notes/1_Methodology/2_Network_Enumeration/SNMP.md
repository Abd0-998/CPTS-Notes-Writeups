## Config Analysis & Service Management
###### Reads the default configuration of the SNMP daemon.
```
`cat /etc/snmp/snmpd.conf | grep -v "#" | sed -r '/^\s*$/d'`
```
## SNMP Enumeration (SNMPwalk)
###### Queries the OIDs with their information using SNMP version 2c and a known community string
```
snmpwalk -v2c -c public 10.129.14.128
```
## Brute-Forcing Community Strings (OneSixtyOne)
###### installs the onesixtyone tool.
```
sudo apt install onesixtyone
```
###### Brute-forces the names of the community strings using a wordlist
```
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
```
## Brute-Forcing OIDs (Braa)
###### Installs the braa tool.
```
sudo apt install braa
```
###### The base syntax for using braa.
```
braa <community string>@<IP>:.1.3.6.*
```
###### Uses the known community string ("public") to brute-force the OID tree on the target IP.
```
braa public@10.129.14.128:.1.3.6.*
```
## Dangerous Settings 
###### provides access to the full OID tree without authentication.
```
rwuser noauth
```
###### provides access to the full OID tree regardless of where the requests were sent from.
```
rwcommunity <community string> <IPv4 address>
```
###### Same access as `rwcommunity` with the difference of using IPv6.
```
rwcommunity6 <community string> <IPv6 address>
```


