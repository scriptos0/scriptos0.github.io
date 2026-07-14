---
title: Nmap Cheatsheet
published: 2026-07-24
description: Nmap Cheatsheet to help make your network scanning a whole lot easier. If you always forget the exact flags for OS detection or fast scans, this guide is for you. It’s straight to the point, easy to read, and perfect for your cybersecurity toolkit. 
image: ./nmaplogo.png
tags: [Linux, Nmap, Commands, Cheatsheet, Tools]
category: Cheatsheet
draft: false
---

# Nmap

## Multiple Targets Scan

Initial Host Discovery

```bash
nmap -sn <IP>/<CIDR> -oG hosts.gnmap
```

Export hosts into a hostlist

```bash
grep "Up" hosts.gnmap | awk '{print $2}' > hosts.txt
```

Port Scan

```bash
nmap -sS -p- --open -Pn -n --min-rate 5000 -iL hosts.txt -oN ports.txt
```

Extract Ports

```bash
grep '^[0-9]' ports.txt | cut -d '/' -f1 | sort -u | xargs | tr '' ','
```

Service, Version & NSE Detection

```bash
nmap -sCV -p<ports> -iL hosts.txt -oN scan.txt
```

## Single Target Scan

TCP Port Scan

```bash
nmap -sS -p- --open --min-rate 5000 <IP> -vvv -n -Pn -oG TCPports.txt
```

UDP Port Scan   **(Optional)**

```bash
nmap -sU -p- --open --min-rate 500 <IP> -vvv -n -Pn -oG UDPports.txt
```

Extract TCP Ports

```bash
grep '^[0-9]' TCPports.txt | cut -d '/' -f 1 | xargs | sort -u | tr ' ' ','
```

Extract UDP Ports   **(Optional)**

```bash
grep '^[0-9]' UDPports.txt | cut -d '/' -f 1 | xargs | sort -u | tr ' ' ','
```

Service Version Detection on Open Ports

```bash
nmap -sCV -p<ports> <IP> -oN portscan.txt
```

## Firewall Evasion

```bash
sudo nmap -sCV -sS -Pn -n -p- <IP> --disable-arp-ping --source-port 53 -D RND:2
```

It's important to note that there are thousands of ways to bypass firewalls. Below, I'll list the methods most commonly used by cybersecurity professionals when they want to bypass firewalls

**MTU (--mtu):** The MTU (Maximum Transmission Unit) evasion technique involves adjusting the size of the packets being sent to avoid detection by the firewall. Nmap allows you to manually configure the maximum packet size to ensure that packets are small enough to pass through the firewall undetected.

**Data Length (--data-length):** This technique involves adjusting the length of the data sent so that it is short enough to pass through the firewall undetected. Nmap allows users to manually configure the length of the data sent so that it is small enough to evade detection by the firewall.

**Source Port (--source-port):** This technique involves manually configuring the source port number of the packets being sent to avoid detection by the firewall. Nmap allows users to manually specify a random source port or a specific port to evade detection by the firewall.

**Decoy (-D):** This evasion technique in Nmap allows the user to send fake packets to the network to confuse intrusion detection systems and avoid detection by the firewall. The -D command allows the user to send fake packets along with the actual scan packets to hide their activity.

**Fragmented (-f):** This technique involves fragmenting the packets sent so that the firewall cannot recognize the traffic as a scan. The -f option in Nmap allows you to fragment the packets and send them separately to avoid detection by the firewall.

**Spoof-Mac (--spoof-mac):** This evasion technique involves changing the packet's MAC address to avoid detection by the firewall. Nmap allows the user to manually configure the MAC address to avoid detection by the firewall.

**Stealth Scan (-sS):** This technique is one of the most commonly used methods for performing stealth scans and avoiding detection by the firewall. The -sS command allows users to perform a SYN scan without establishing a full connection, thereby avoiding detection by the firewall.

**min-rate (--min-rate):** This technique allows the user to control the speed to the packets that are sent to avoid detection by the firewall.





