_All config is subject to change_

# Tech stack

IPS: Snort  
HIDS: OSSEC  
Firewall: OPNSense  
Logging: Security Onion  
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
~~Use this for threat hunting/alerts.~~  
Alerts, IDS requires standalone architecture which has too much requirement for me.   
For simplicity, I added another NIC for connecting to my PC to get to the SOC (security onion console).  

## PC3

IP: 192.168.1.1, 192.168.0.1  
OS: Arch Linux  
IPS: Snort  
Firewall: OPNSense (just for experience)  
This will acts as router with 2 NICs, the router will copy traffic into PC2 (Sec. Onion) for logging.  
If this turns out too hard to do, I'll just use vbox promiscuous mode.

## PC4

IP: 192.168.1.2  
OS: Kali Linux  
Used for network testing.
  
# To dos

_List can be changed as I go through_

## ~~List going-to-use stuff~~  
## ~~Complete the diagram~~  

- ~~Figure out where to place the IDS, firewall...~~  
- ~~Figure out how to connect the machines on VB~~   

## Setup all devices on VirtualBox  

### Setup PC1 ModSecurity, OSSEC. (~~basic~~, HIDS, HTTP server)

### Setup PC2 Security Onion. (~~installation~~, ~~internal NIC~~)

### Setup PC3 Snort (IPS), OPNSense, Copying traffic to Security Onion.

- ~~OS setup~~
- ~~Basic interface configuration~~ (tested pinging between machines)
- ~~Packet forwarding~~
- Packet cloning into PC2 (Security Onion) for logging
- Firewall
- Snort IPS mode

### Setup Kali Linux. (~~installation~~)

# VirtualBox setup documentation

## PC1 (192.168.0.2)

**Installed using**: archlinux-2025.02.01-x86_64.iso
**Installation guide**: https://wiki.archlinux.org/title/Installation_guide

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
Address=192.168.0.2/24
Gateway=192.168.0.1
```

Adapter connected to *inner* VBox's internal network

## PC2 (192.168.0.3)

**Installed using**: `securityonion-2.4.111-20241217.iso`  
**Installation guide**: https://docs.securityonion.net/en/2.4/installation.html   
Do check the [hardware specs.](https://docs.securityonion.net/en/2.4/hardware.html)

Architecture: [Import](https://docs.securityonion.net/en/2.4/architecture.html#import)  
*Also if you are also installing in import mode, salt-master is not needed so ignore the error.*  

## PC3 (192.168.0.1, 192.168.1.1)

Partition and packages are the same as PC2.  
**Network manager**:  
systemd-networkd,  
IP: 192.168.1.1, 192.168.0.1;  
Network files are configured just like PC1, don't need to add gateway.

**Packet forwarding:** I used iptables using [this](https://wiki.archlinux.org/title/Router#Connection_sharing) guide.  
**Firewall (OPNSense):**  
Follow this [guide](https://ipv6.rs/tutorial/Arch_Linux/OPNsense/).

## PC4 (192.168.1.2)

Installed using prebuilt image (look on their official website).
File name: `kali-linux-2024.4-virtualbox-amd64.gz`

# Screenshots

Demonstrate pinging between devices routed by PC3 (Arch router)
![Pasted image 20250219202905.png](img/Pasted%20image%2020250219202905.png)
