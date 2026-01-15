#project #placement-preparation 
ref - https://www.youtube.com/watch?v=4t4kBkMsDbQ (Network Chuck)

**Scanning**
> Try to remember that -s means scan, so -sT = TCP scan, -sU = UDP scan

`nmap -sT target_ip` = perform full `T`CP scan
`nmap -sU target_ip` = perform `U`DP scan 
`nmap -sn target_ip` = perform pi`N`g scan
`nmap -sV target_ip` = scan the ser`V`ices 
`nmap -sA target_ip` = detect firew`A`lls

`nmap -A target_ip` = aggressive scan, scan OS and services as well

> Now -p is for scanning ports. Followed by this flag, you can provide either 1 port, a range of ports, or all ports.

`nmap -p 80 target_ip` = scan port 80
`nmap -p 1-20 target_ip` = scan ports 1 to 20
`nmap -p- target_ip` = scan all ports

**Hotshots !!**
`nmap --script vuln target_ip`:
This runs a NSE script (Nmap Scanning Engine)
it is a custom-written code used within the Nmap network scanning tool to perform vulnerability detection on a target system

`nmap --traceroute target_ip`:
gives the path taken from source to target