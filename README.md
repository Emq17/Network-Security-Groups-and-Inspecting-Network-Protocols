<p align="center">
<img src="https://i.imgur.com/Ig6J0xo.png" alt="Traffic Examination"/>
</p>

<h1 align="center">Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

>**Note***
>_This tutorial uses materials created in the previous demonstration: [Establishing Virtual Machines With Remote Desktop](https://github.com/Emq17/Establishing-Virtual-Machines-With-Remote-Desktop)_
<h2>Environments and Technologies</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems</h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04 (Linux)

<h2>High-Level Steps</h2>

- Setting up Two Virtual Machines
- Using Remote Desktop Connection (RDP) to VM-1 and installing Wireshark
- Using Wireshark and PowerShell to observe Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)

<h2>Actions and Observations</h2>

<h3>&#9312 Create 2 Virtual Machines</h3>

- Create One Virtual Machine on Microsoft Azure
    - Set `Resource Group` to **VM1_Group**
    - Set `Virtual Machine Name` to **VM-1**
    - Set `Region` to **(US) West US 3**
    - Set `Image` to **Windows 10 Pro, version 22H2**
    - Set `Size` to **Standard_E2s_v3 - 2 vcpus, 16 GiB memory**
    - Set `Username` to **winuser**
    - Set `Password` to **osticketPassword1
    - Select `Review + create` until you're Validated
 
     ![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/1e7f94f9-d602-408c-817d-6faed271bc85)


- Create Another Virtual Machine On Microsoft
     - Set `Resource Group` to **VM1_Group**
     - Set `Virtual Machine Name` to **VM-2**
     - Set `Image` to **Ubuntu Server 20.04 LTS**
     - Set `Authentication type` to **Password**
     - Set `Username` to **linuser**
     - Select `Next`
 
       ![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/13ebcb28-8a94-463a-9d22-edc859614c39)

- Set `Virtual Network` to **VM-1-vnet**
- Set `Public IP` to **(new) Vm-2-ip**
- Select `Review + check`
- Proceed with creating the Virtual Machine

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/a0db3cf6-8ff0-4d34-9091-fba65792f1a6)

<h3>&#9313 Installing Wireshark on VM-1</h3>

- Connect to VM-1
- Open `Microsoft Edge`
- Type **https://www.wireshark.org/download.html** into the Search Bar
- Select `Windows Intel Installer`
- Select `Open File` to run the .exe file

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/6fcb0a87-2879-442d-8d29-9780e9d5bdd8)

- The Installation Prompt will appear
- Select `Next` until you reach start installing
- Select `Finish`

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/dccde23c-29b7-4941-82b6-952293f2d1d8)

<h3>&#9314 Observing ICMP Traffic using Wireshark</h3>

>_The Internet Control Message Protocol (ICMP) is a protocol that devices within a network use to communicate problems with data transmission._

- Run `Wireshark`
- Click the Blue Shark fin icon
- Observe the activity constantly going in the background in VM-1

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/4c8c980a-f48a-4b54-b596-1f0a08c2a3f8)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/e2829eb8-963a-4eaa-af5f-011dd614dc5b)

- Type in **ICMP** and Enter
- Observe no activity under the ICMP protocol

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/25cc4df6-8c95-4e76-ba15-86d368756f60)

- Return to the Azure Portal
- Go to VM-2's OVerview Page
- Copy the `Private IP Address` (I'll be using 10.0.0.5)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/b9ca634f-1003-4577-84a1-b9f9d0ee9e98)

- Return to VM-1
- Open `PowerShell` and Select `Run as Administrator`
- Type in **ping -t "VM-2 PRIVATE IP Address"**
- Observe on Wireshark the results of packets being perpetually sent and received

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/b495a7dc-5685-458b-ad55-4de4b7703c0e)

- Return to the Azure Portal
- Type **Network Security Groups** into the search bar
- Select `VM-2-nsg`
- Select `Inbound Security Rules`
- Select `Add`
- Change the Protocol to **icmp**
- Change the Action to **Deny**
- Change the Priority to **200**
- Change the Name to **DENY_ICMP_PING_FROM_ANYWHERE**
- Select `Add`
- Observe the denial of data packets from Wireshark

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/4f734fc7-d4e8-479c-b43e-dda026c6be6e)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/a231860e-9f4a-43b2-824c-bc5393f50310)

- Return to the Azure Portal
- Go back into ICMP Security Rule
- Select `Allow` instead of `Deny`
- Select `Save`
- Observe the Approval of data packets being sent and received
- Press `ctrl + c` to stop PowerShell

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/67d4dcc8-a20d-4036-ab12-023024b7be33)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/e45cf4b1-b9d8-479e-a08b-dd553183e23c)

<h3>&#9315 Observing SSH Traffic using Wireshark</h3>

>_SSH or Secure Shell is a network communication protocol that enables two computers to communicate._

- In Wireshark, type **ssh** or **tcp.port == 22**
- Observe the empty activity

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/f7516ef6-85cc-45d1-aa40-ddba4d3a3360)

- In PowerShell, type **ssh linuser@(private ip address)**
- Type **yes** and enter
- Type the Password
  >**Note***
  >_there is no visual indicator of you typing, but the inputs are being read_
- Press Enter
- The commands are now in Linux formant

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/233e2d7a-06e0-441f-8999-1dcf5cb1d22a)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/e886b7c0-8d42-40a5-926c-18e90d5f088d)

- Type **id** into Powershell
- This will give you the identity group information for VM-2's user
- Observe the new traffic on Wireshark
- Type **exit** to close the linked connection

ex![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/683bf38f-7c16-48fb-b00d-c7925cc1d7b9)

<h3>&#9316 Observing DHCP Traffic using Wireshark</h3>

>_DHCP is a client-server protocol in which servers manage a pool of unique IP addresses, as well as information about client configuration parameters._

- In Wireshark, Type **dhcp**
- Observe Empty activity

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/45d144f7-39b8-4593-a3d7-da855b956674)

- In PowerShell, type **ipconfig /renew**
- Wait for Virtual Machine to regain Connection
- Observe the new DHCP Activity

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/9c503a66-99d6-425e-81c6-8070342ce4a6)

<h3>&#9317 Observing DNS Traffic using Wireshark</h3>

- In Wireshark, type **dns** or **udp.port == 53**
- Observe the large volume of traffic
- Clear the boxes by pressing the `Restart current capture` button (Green Shark fin Icon)

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/cb697b04-4ed2-4c3a-ad1e-4ea57462031f)

- In PowerShell, Type **nslookup ww.google.com**
- Observe the new DNS traffic in Wireshark

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/e632bd41-9ce0-47ba-8c6b-94875c5ce092)

<h3>&#9318 Observing RDP Traffic using Wireshark</h3>

>_RDP is a secure, interoperable protocol that creates secure connections between clients, servers and virtual machines._

- In Wireshark, Type **rdp** or **tcp.port == 3389**
- Observe the RDP Activity

![image](https://github.com/CarlosAlvarado0718/Network-Protocols/assets/140138198/c0f48e92-f8a9-4356-a7d4-67a725671318)

---
<h1>ALL DONE!! CONGRATS!!!</h1>
