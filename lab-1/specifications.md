# Tech stack

IPS: Snort  
HIDS: OSSEC  
Firewall: OPNSense (just for experience)  
Logging (pcaps): Security Onion  
Web Application Firewall: ModSecurity  

# Virtual machine specifications

Virtualization software: VirtualBox

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
This will act as router.  

## PC4

IP: 192.168.1.2  
OS: Kali Linux  
Used for network testing.
  
# My Setup Process

## Complete the topology  

## Setup all devices on VirtualBox  

### Setup PC1 ModSecurity, OSSEC.

### Setup PC2 Security Onion.

### Setup PC3 Snort (IPS mode), OPNSense.

- OS setup
- Basic interface configuration (tested pinging between machines)
- Packet forwarding

After basic installation you are ready to capture pcaps on PC1 - using whatever tool you like and import it to Security Onion for analysis.

- Firewall
- Snort IPS mode

### Setup Kali Linux.

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

**Network manager:** This is done using NetworkManager, change or add these lines:  
*inner* VM internal network interface config `/etc/NetworkManager/system-connections/enp0s8.nmconnection`:  
```
[ipv4]
address=192.168.0.3/24
gateway=192.168.0.1
method=manual
```  
Bridged adapter interface should work out of the box.

## PC3 (192.168.0.1, 192.168.1.1)

Partition and packages are the same as PC2.  

**Network manager**:  
systemd-networkd,  
IP: 192.168.1.1, 192.168.0.1.  
*For installing packages, remember to [configure DNS](https://man.archlinux.org/man/resolv.conf.5.en) for extra bridge interface, interface listed above is only for internal lab environment.*

**Packet forwarding:**  
I used iptables using this [guide](https://wiki.archlinux.org/title/Router#Connection_sharing).  

**Firewall (OPNSense):**  
Follow this [guide](https://ipv6.rs/tutorial/Arch_Linux/OPNsense/).

## PC4 (192.168.1.2)

Installed using prebuilt image (look on their official website).
File name: `kali-linux-2024.4-virtualbox-amd64.gz`

# Screenshots

Demonstrate pinging between devices routed by PC3 (Arch router)
![Pasted image 20250219202905.png](img/Pasted%20image%2020250219202905.png)

Security Onion is installed in import mode, we will be using the Grid page to import pcap or evtx files.  
![Pasted image 20250221191215.png](img/Pasted%20image%2020250221191215.png)  

_All config is subject to change_
