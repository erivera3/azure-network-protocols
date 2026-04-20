<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1 align="center">Network Security Groups (NSGs) & Traffic Inspection Between Azure Virtual Machines</h1>

This project demonstrates how to deploy Azure virtual machines, analyze real network traffic using Wireshark, and control communication using Network Security Groups (NSGs). The lab focuses on understanding how systems communicate and how security rules impact network behavior.

---

## 🎯 Goals & Objectives

The goal of this project was to build a working network environment in Azure and understand how traffic flows between systems at the packet level.

By the end of this lab, I aimed to:

- Deploy multiple virtual machines in a shared network
- Capture and analyze traffic using Wireshark
- Observe key protocols (ICMP, SSH, DHCP, DNS, RDP)
- Apply and test Network Security Group rules
- Understand how security configurations affect connectivity
- Improve troubleshooting through direct observation

---

## 📌 Overview

In this project, I created a Windows and Ubuntu virtual machine within the same Azure Virtual Network to simulate a basic network environment. Using Wireshark, I captured and analyzed traffic between the machines while generating different types of network activity.

I then modified Network Security Group rules to block and allow traffic, which demonstrated how cloud-level security controls influence communication between systems.

---

## 🧰 Technologies Used

- Microsoft Azure (Virtual Machines)
- Network Security Groups (NSGs)
- Wireshark
- Remote Desktop Protocol (RDP)
- SSH
- PowerShell
- DNS
- DHCP
- ICMP

---

## 💻 Environment

- Windows 10 Virtual Machine
- Ubuntu Virtual Machine
- Azure Virtual Network (shared)
- Azure Subnet
- Network Security Group
- Wireshark installed on the Windows VM

---

## ⚙️ Implementation

### 1. Infrastructure Setup

- Created a Resource Group
- Deployed Windows and Ubuntu virtual machines
- Ensured both VMs were placed in the same Virtual Network and Subnet

This setup allowed communication over private IP addresses within a controlled environment.

<p align="center">
  <img src="images/vm-setup.png" width="700">
</p>

---

### 2. ICMP Traffic Observation

- Installed Wireshark on the Windows VM
- Captured live network traffic
- Generated ICMP traffic by:
  - Pinging the Ubuntu VM (private IP)
  - Pinging a public website (google.com)

Observed successful request and reply packets, confirming connectivity.

<p align="center">
  <img src="images/icmp-traffic.png" width="700">
</p>

---

### 3. Network Security Group Testing

- Initiated a continuous ping from the Windows VM to the Ubuntu VM
- Applied an NSG rule to block inbound ICMP traffic

Observed behavior:
- Ping requests continued
- Replies stopped
- Wireshark showed one-way traffic

After re-enabling ICMP:
- Replies resumed immediately
- Connectivity was restored

This demonstrated how NSGs control traffic flow without affecting outbound requests.

<p align="center">
  <img src="images/nsg-block.png" width="700">
</p>

---

### 4. SSH Traffic Observation

- Filtered Wireshark for SSH traffic
- Initiated an SSH session from Windows to Ubuntu
- Executed commands within the session

Observed encrypted traffic on port 22, confirming secure remote communication.

<p align="center">
  <img src="images/ssh-traffic.png" width="700">
</p>

---

### 5. DHCP Traffic Observation

- Filtered Wireshark for DHCP traffic
- Issued a new IP request using:

```powershell
ipconfig /renew
