_All config is subject to change_

# Tech stack

IPS: Snort
HIDS: OSSEC
Firewall: OPNSense
Logging, SIEM: Security Onion

Web Application Firewall: ModSecurity

# Virtual machine specifications

Virtualization software: VirtualBox (I don't have another device for Proxmox)

## PC1

IP: 192.168.0.3
OS: Arch Linux
HIDS: OSSEC
WAF: ModSecurity
This machine will run web servers for WAF testing.

## PC2

IP: 192.168.0.4
OS: Security Onion
Use this for threat hunting/alerts.

## PC3

IP: 192.168.1.1, 192.168.0.1
OS: Arch Linux
IPS: Snort
Firewall: OPNSense
This will acts as router with 2 NICs, the router will copy traffic into PC2 (Sec. Onion) for logging.

## PC4

IP: 192.168.1.2
OS: Kali Linux
Used for pentesting.

# To dos

_List can be changed as I go through_

~~List going-to-use stuff~~  
Complete the diagram  
 -> ~~Figure out where to place the IDS, firewall...~~
 -> Figure out how to connect the machines on VB  
 -> Document VM setup  
Setup all devices on VirtualBox  
Configure IPS  
Configure IDS  
Configure traffic monitoring  
Configure WAF
Configure SecOnion
