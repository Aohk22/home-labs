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

IP: 192.168.0.2  
OS: Arch Linux  
HIDS: OSSEC  
WAF: ModSecurity  
This machine will run web servers for WAF testing.  

## PC2

IP: 192.168.0.3  
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
- ~~Figure out where to place the IDS, firewall...~~  
- ~~Figure out how to connect the machines on VB~~   
- Document VM setup (on going) 

Setup all devices on VirtualBox  

- Setup PC1 ModSecurity, OSSEC. (~~basic~~, server)
- Setup PC2 Security Onion.
- Setup PC3 Snort (IPS), OPNSense, Copying traffic to Security Onion.
- Setup Kali Linux.

# VirtualBox setup documentation

## PC1

**Installed using**: archlinux-2025.02.01-x86_64.iso

**Boot**: EFI mode  
**Partition**:

| Type | Size |
| --- | --- |
| EFI system partition | 500 MB |
| Linux swap | 1 GB |
| Linux root | 7.5 GB |

**Root partition type**: ext4  
**Installed packages**:  
The base packages, intel-ucode, vim, man-db, man-pages, firefox.   
**Bootloader**: systemd-boot. (default configuration)  
**Network manager**: 
systemd-network,  
IP: 192.168.0.2,  
Full `/etc/systemd/network/20-wired.network`:  

```
[Match]
Name=enp0s3

[Network]
Address=192.168.0.2
Gateway=192.168.0.1
```

Adapter connected to *inner* VBox's internal network
