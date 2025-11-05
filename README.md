# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used 

- Windows 10 (21H2)
- Ubuntu Server 20.04

## Actions and Observations

This starts with me setting up 2 VMs in Azure (`VM1` and `VM2`) in the same resource group and connecting to `VM1`
- [Download Wireshark](https://www.wireshark.org/download.html)
- Open Wireshark
- Select `Ethernet` to observe network traffic

![](https://safe.reku.me/azure-network-protocols/1.png?raw)
- Type `ICMP` at the top
- Currently there's no `ICMP` traffic, let's create our own
- Get the private IP address of `VM2`
- Open PowerShell
- Run `ping [VM2_IP]`, where `[VM2_IP]` is the private IP address of `VM2`

![](https://safe.reku.me/azure-network-protocols/2.png?raw)
- Run `ping [VM2_IP] -t`

![](https://safe.reku.me/azure-network-protocols/3.png?raw)
- Open up Azure's dashboard
- Go into VM2's network security group
- Create a new inbound security rule
- Select `ICMPv4` as the protocol
- Type in`200` for the priority
- Here I name it `DENY_ANY_ICMP` to be self-explanatory

![](https://safe.reku.me/azure-network-protocols/4.png?raw)
- Go back to `VM1`
- Now we're getting no responses from the ping request

![](https://safe.reku.me/azure-network-protocols/5.png?raw)
- Go back to Azure's dashboard
- To undo, I can either select `Allow` instead of `Deny` for the `Action`, or delete the rule
- I will select `Allow`

![](https://safe.reku.me/azure-network-protocols/6.png?raw)
- Go back to `VM1`
- Now we're back to getting responses from the ping request

![](https://safe.reku.me/azure-network-protocols/7.png?raw)
- At the top, replace `icmp` with `ssh`
- `SSH` into `VM2`

![](https://safe.reku.me/azure-network-protocols/8.png?raw)
- Run basic commands (`pwd`, `ls`, etc.) and observe the traffic

![](https://safe.reku.me/azure-network-protocols/9.png?raw)
- At the top, replace `ssh` with `dhcp`
- Run `ipconfig /renew`

![](https://safe.reku.me/azure-network-protocols/10.png?raw)
- At the top, replace `dhcp` with `dns`
- Run `nslookup` on any domain name (`www.google.com` and `www.amazon.com` here)

![](https://safe.reku.me/azure-network-protocols/11.png?raw)
- At the top, replace `dns` with `tcp.port == 3389`
- Since I used `RDP` to connect, and `RDP` is a constant live stream, there's traffic without us doing anything

![](https://safe.reku.me/azure-network-protocols/12.png?raw)
