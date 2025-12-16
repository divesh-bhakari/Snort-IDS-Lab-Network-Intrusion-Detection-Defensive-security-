# 🛡️ Snort IDS Home Lab — Network Intrusion Detection & Traffic Monitoring

## Table of Contents
1. Introduction
2. Prerequisites
3. Network Topology
4. Step 1: Installing Snort on Ubuntu
5. Step 2: Configuring Snort (HOME_NET)
6. Step 3: Understanding Snort Rule Syntax
7. Step 4: Detecting ICMP (Ping) Traffic
8. Step 5: Detecting FTP Traffic
9. Step 6: Detecting SSH Traffic
10. Troubleshooting
11. Next Steps & Future Improvements
12. Conclusion

---

## Introduction
This project demonstrates the deployment of **Snort as a Network Intrusion Detection System (IDS)** in a virtualized home lab environment. The lab focuses on detecting common network activities such as **ICMP (Ping), FTP, and SSH** using **custom Snort rules**.

The objectives of this lab are:
- Understand how IDS works at the network level
- Learn Snort configuration and rule writing
- Monitor live network traffic and alerts
- Validate detections using multiple machines

---

## Prerequisites

| Requirement | Description |
|------------|------------|
| RAM | Minimum 8 GB recommended |
| Hypervisor | VirtualBox |
| Operating Systems | Ubuntu (Snort IDS), Kali Linux, Windows 11 |
| Network Mode | Promiscuous Mode enabled |
| Tools | Snort IDS |

---

## Network Topology

Windows 11  →  ICMP  
Kali Linux  →  FTP / SSH  
          ↓  
Ubuntu VM (Snort IDS)

Snort monitors all incoming traffic and generates alerts based on custom rules.

---

## Step 1: Installing Snort on Ubuntu

Update system packages:
sudo apt update && sudo apt upgrade -y

Install Snort:
sudo apt install snort -y

During installation, provide the correct network interface and IP range when prompted.

---

## Step 2: Configuring Snort (HOME_NET)

Open the Snort configuration file:
sudo nano /etc/snort/snort.conf

Locate the HOME_NET variable and set it to your Ubuntu VM IP address.

To find your IP address:
ip a

Save and exit the file.

---

## Step 3: Understanding Snort Rule Syntax

Basic Snort rule format:

ACTION PROTOCOL SOURCE_IP SOURCE_PORT -> DESTINATION_IP DESTINATION_PORT (OPTIONS)

Key fields:
- alert : Generates an alert
- protocol : icmp / tcp
- msg : Alert message
- sid : Unique rule ID
- rev : Rule revision number

---

## Step 4: Detecting ICMP (Ping) Traffic

Edit the local rules file:
sudo nano /etc/snort/rules/local.rules

Add the rule:
alert icmp any any -> $HOME_NET any (msg:"Ping detected"; sid:100001; rev:1;)

Start Snort:
sudo snort -q -l /var/log/snort -i ens33 -A console -c /etc/snort/snort.conf

From Windows 11, ping the Ubuntu VM:
ping <Ubuntu_VM_IP>

ICMP alerts will appear in the Snort console.

---

## Step 5: Detecting FTP Traffic

Edit the local rules file:
sudo nano /etc/snort/rules/local.rules

Add the FTP rule:
alert tcp any any -> 192.168.56.104 21 (msg:"FTP detected"; sid:100002; rev:1;)

Restart Snort:
sudo snort -q -l /var/log/snort -i ens33 -A console -c /etc/snort/snort.conf

From Kali Linux:
ftp <ubuntu_vm_ip> 21

FTP alerts will be generated.

---

## Step 6: Detecting SSH Traffic

Edit the local rules file:
sudo nano /etc/snort/rules/local.rules

Add the SSH rule:
alert tcp any any -> $HOME_NET 22 (msg:"SSH detected"; sid:100003; rev:1;)

Restart Snort:
sudo snort -q -l /var/log/snort -i ens33 -A console -c /etc/snort/snort.conf

From Kali Linux:
ssh <username>@<Ubuntu_VM_IP>

SSH alerts will appear in real time.

---

## Troubleshooting
- Ensure the correct interface (ens33) is used
- Verify HOME_NET is correctly set
- Enable promiscuous mode in VirtualBox
- Restart Snort after rule changes

---

## Next Steps & Future Improvements
- Add port scan and brute-force detection rules
- Integrate Snort logs with Splunk or ELK Stack
- Configure Snort in IPS (inline) mode
- Build alert dashboards

---

## Conclusion
This lab demonstrates how **Snort functions as a Network IDS** by monitoring live traffic, detecting ICMP, FTP, and SSH activity, and generating real-time alerts.

Disclaimer: This project is for educational purposes only.
