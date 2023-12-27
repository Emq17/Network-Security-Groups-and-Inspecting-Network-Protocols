<p align="center">
<img src="https://i.imgur.com/JCvB6u1.png" alt="Traffic Examination"/>
</p>

<h1 align="center">Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this walkthrough, I'll help you observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies</h2>

- Azure Virtual Machines
- Azure Network Security Groups (Firewall Resource)
- Microsoft Remote Desktop (Macbook)
- Various Command-Line Tools (Windows 10)
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems</h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04 (Linux)

<h2>Overview</h2>

<p align="center">
<img src="https://i.imgur.com/mmWW6oj.png" alt="Traffic Examination"/>
</p>

<h2>Step-By-Step Walkthrough</h2>

<h3>Create 2 Virtual Machines</h3>

- Create One Virtual Machine on Microsoft Azure
    - Set `Resource Group` to "RG-Lab"
    - Set `Virtual Machine Name` to "VM-1"
    - Set `Region` to "(US) West US 3"
    - Set `Image` to "Windows 10 Pro, version 22H2 - x64 Gen2"
    - Set `Size` to "Standard_E2s_v3 - 2 vcpus, 16 GiB memory"
    - Set `Username` to "labuser"
    - Set `Password` to "Password1234"
    - Check `Licensing` box
    - Select `Review + create` then `create` after validation

![Screen Shot 2023-12-26 at 7 05 37 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/deefd5e2-e72d-47c6-aa60-de76e32e5b02)

![Screen Shot 2023-12-26 at 7 10 05 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/49575622-1ebe-48c9-b048-cdc20f4aaf56)

- Create Another Virtual Machine On Microsoft
     - Set `Resource Group` as the same one in VM-1: "RG-Lab"
     - Set `Virtual Machine Name` to "VM-2"
     - Set `Region` to "(US) West US 3"
     - Set `Image` to "Ubuntu Server 22.04 LTS - x64 Gen2"
     - Set `Size` to "Standard_E2s_v3 - 2 vcpus, 16 GiB memory"
     - Set `Authentication type` to **Password**
     - Set `Username` to "Linux"
     - Select `Next`

![Screen Shot 2023-12-26 at 7 18 58 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/74e6eeff-ec5c-4aae-a249-c270c770af77)

![Screen Shot 2023-12-26 at 7 19 51 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/565a2428-75ff-47ad-a337-c054f56a0e03)

- Select `Next : Disks >` & `Next : Networking >`
- Set `Virtual Network` to "VM-1-vnet"
- Select `Review + check` then `create` after validation

![Screen Shot 2023-12-26 at 7 23 00 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/7dc8882e-65b8-489a-9c69-1140369738ab)

<h3>Installing Wireshark on VM-1</h3>

- Remote Desktop Connect to VM-1
    - Refer here if needed: https://github.com/Emq17/Establishing-Virtual-Machines-With-Remote-Desktop

![Screen Shot 2023-12-26 at 7 31 12 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/9340e472-6088-4748-ab38-df1328e962e3)

![Screen Shot 2023-12-26 at 7 32 02 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/90babd41-5e1d-40f1-92df-dd260bc5d344)

- Open `Microsoft Edge`
- Type in https://wireshark.org/download.html into the Search Bar
- Select `Windows x64 Installer`
- Select `Open File` in your downloads folder to run the .exe file
- Install everything with its default settings

![Screen Shot 2023-12-26 at 7 42 17 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/dd7e70d3-e540-4b8a-a70d-382b276e9ad7)

<h3>Observing ICMP Traffic using Wireshark</h3>

>_The Internet Control Message Protocol (ICMP) is a protocol that devices within a network use to communicate problems with data transmission._

- Run `Wireshark`
- Click `Ethernet`
- Click the Blue Shark fin icon at thet top left corner to start capturing packets 
- Observe the activity constantly going in the background in VM-1

![Screen Shot 2023-12-26 at 7 47 49 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/30c828c5-172e-4502-9b1c-116a8c59b4c8)

- You should see it start to spam traffic just because a lot of things are happening in the background

![Screen Shot 2023-12-26 at 7 49 26 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/0b4902c8-c6e3-42aa-aa68-8b78e12e85bf)

- Let's start filtering this traffic
- Type in "ICMP" and Enter

![Screen Shot 2023-12-26 at 7 51 59 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/404360f4-4df1-4736-9091-9aaf8260eb21)

- Observe no activity under the ICMP protocol

![Screen Shot 2023-12-26 at 7 52 50 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/e3f58f81-4aa4-4c6d-a1a4-97ee6ba7c90c)

- Return to the Azure Portal
- Go to VM-2's Overview Page
- Copy the `Private IP Address` (I'll be using 10.0.0.5)

![Screen Shot 2023-12-26 at 7 54 29 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/a6655fba-a722-4871-bcf3-8efc95eb665f)

- Return to VM-1
- Open `PowerShell` 
- Type in "ping -t" + "VM-2's PRIVATE IP Address"
- Observe on Wireshark the results of packets being perpetually sent and received

![Screen Shot 2023-12-26 at 8 27 52 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/c157ae4a-de35-480e-aebd-211b332ea89e)

- Now type in `ping www.google.com -4` using the ipv4 address
- You can see how you are sending traffic to Google and we're receiving traffic back

![Screen Shot 2023-12-26 at 8 32 25 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/05b86056-bab9-4df1-8267-7d1ef0ab6dff)

- Lets go ahead and clear our data by clicking this icon below

![Screen Shot 2023-12-26 at 8 39 10 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/722eed68-8e7d-4ee9-9ca5-cf963d335d40)

- Now lets send out a perpetual ping where it should ping forever
- While this is going on lets go ahead and change the firewall settings on VM-2 to block ICMP traffic from coming through
- This should help us get a better understanding for how firewalls and networking work 

![Screen Shot 2023-12-26 at 8 41 35 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/629d2321-c475-479c-a46c-b736155accb6)

- Return to the Azure Portal
- Type "Network Security Groups" into the search bar

![Screen Shot 2023-12-26 at 8 46 11 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/3a0fc661-78d9-455f-bfeb-232920a5431b)

- Select `VM-2-nsg`
- Select `Inbound Security Rules` underneath "settings" on your left
- Select `Add`

![Screen Shot 2023-12-26 at 8 47 15 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/5e917087-5184-4e6e-9117-6a468cb6f91e)

- Change the Protocol to "icmp"
- Change the Action to "Deny"
- Change the Priority to "200"
- Change the Name to "DENY_ICMP_PING_FROM_ANYWHERE"
- Select `Add`

![Screen Shot 2023-12-26 at 8 53 19 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/559ad656-8947-4317-be5c-2ea0d3fe9390)

- Observe the request timing out and the denial of data packets from Wireshark 

![Screen Shot 2023-12-26 at 8 55 31 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/a9de0a2d-767b-4e74-81bd-8a537817ce6c)

- Now for fun, return back to VM-2's Inbound security rules
- Click on the rule we just created

![Screen Shot 2023-12-26 at 8 57 43 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/c9e7a310-a758-4034-90f4-67914ee726e1)

- Change it back to how it was by selecting `Allow` instead of `Deny`
- Hit `Save`

![Screen Shot 2023-12-26 at 8 58 50 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/da20b774-6b78-43d9-ad5f-cf4053aae6cf)

- Now you can see the traffic coming in again and observe the approval of data packets being sent/received
- Press `ctrl + c` to stop PowerShell

![Screen Shot 2023-12-26 at 9 00 54 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/0373a23a-7532-46ce-8f65-015663988079)

<h3>Observing SSH Traffic using Wireshark</h3>

>_SSH or Secure Shell is a network communication protocol that enables two computers to communicate._

- In Wireshark, type "ssh" or "tcp.port == 22"
- Type in "ssh" + "Linux" + "@Private IP"
- Now that we're filtering for ssh traffic, observe the empty activity

![Screen Shot 2023-12-26 at 9 04 05 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/40e0a2d1-f34e-48cc-a1e6-dd57bcaaa693)

- PowerShell should be asking you if you want to continue, type in "yes" then type in the password: Password1234 (There is no visual indicator of you typing, but the inputs are still being read)
- You should now have a secure connection into VM-2 & are able to run Linux commands
- As you type in the PowerShell you can see that it goes through the network and traffic automatically comes through to Wireshark

![Screen Shot 2023-12-26 at 9 20 05 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/919ea4ae-638f-4df2-87fc-7406de3fa06a)

- Type in `id` into Powershell
- This will give you the identity group information for VM-2's user
- Observe the new traffic on Wireshark
    - Use `uname -a` and it will tell us about the actual OS it's running on 
    - Use `pwd` for "print working directory"
    - Use `ls -lasth` to list out the folders and files in the current directory
    - Use `touch hi.txt` to make a file named "hi.txt"
- While you mess around with these commands, because we're using SSH to communicate with VM-2, observe how traffic is being spammed onto Wireshark
- Type in `exit` to close the linked connection and go back into VM-1's command line

![Screen Shot 2023-12-26 at 9 24 15 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/719133c4-d9ba-42d6-9ca6-22aff5f039d2)

- Rememebr like I said earlier, you can also type in `tcp.port == 22` to filter out SSH traffic

![Screen Shot 2023-12-26 at 9 46 24 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/4aa1e7e0-9021-4ae3-8391-2e99e98473d0)

- Let's go ahead and observe more traffic

<h3>Observing DHCP Traffic using Wireshark</h3>

>_DHCP is a client-server protocol in which servers manage a pool of unique IP addresses, as well as information about client configuration parameters._

- In Wireshark, type in "DHCP" at the top so we can start filtering that type of traffic
- In PowerShell, type `ipconfig /renew`
- Observe the new DHCP activity

![Screen Shot 2023-12-26 at 9 55 07 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/c9759ff9-6cc3-4eab-a244-a19302abbad3)

<h3>Observing DNS Traffic using Wireshark</h3>

- In Wireshark, type `DNS` or `udp.port == 53`
- Type in `nslookup www.google.com`
- Observe the large volume of traffic

![Screen Shot 2023-12-26 at 10 02 00 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/f79a4660-114b-4106-9961-1767f50efd80)

- We can do one more and type in `udp.port == 53` at the top of Wireshark
- Then `nslookup www.disney.com` in PowerShell
- Besides the traffic coming in, you can see some of the different IP addresses that Disney also uses
- Wireshark is able to provide us a breakdown of the packet regarding IP addresses, that it implemented the user datagram protocol, which source port & destination port it used, & etc. 

![Screen Shot 2023-12-26 at 10 07 22 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/8099f01a-f2ed-44b7-af4e-dc5e3e260d20)

<h3>Observing RDP Traffic using Wireshark</h3>

>_RDP is a secure, interoperable protocol that creates secure connections between clients, servers and virtual machines._

- In Wireshark, type in `RDP` or `tcp.port == 3389`
- Observe the RDP Activity
- Remember that we're currently using a live active RDP session to connect with and interact with our physical host computer right now (My Macbook to VM-1)
- So it only makes sense to why the large amount of traffic is coming through to Wireshark
- If you observe the actual Source IP coming in, that is most likely our actual IP address while the Destination IP is VM-1's Private IP address

![Screen Shot 2023-12-26 at 10 19 37 PM](https://github.com/Emq17/Network-Security-Groups-and-Inspecting-Network-Protocols/assets/147126755/68649bf8-4e3c-4214-a69d-d2826094c1ac)

# Conclusion 

- I recommend going through this lab a couple of times to be able to get a good understanding of how everything works 
- Try and get to the point where you can implement this naturally
- The goal is to build more of an intuition of firewalls, NSG's, & maybe even memorize the port numbers listed earlier
- The more you do it the better you'll understand Azure, Networking, & cloud computing in general

