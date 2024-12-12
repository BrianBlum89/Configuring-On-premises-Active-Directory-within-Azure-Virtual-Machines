# Configuring-On-premises-Active-Directory-within-Azure-Virtual-Machines

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>


<h1>Project Scope - Using Active Directory w/Azure</h1>
This tutorial outlines the way a computer user can utilize Active Directory inside of the 
Microsoft Azure cloud enviornment.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Users and Computers
- Domain Controller - Windows Server 2022  </b> 
- Client-1 - Windows 10 

## Step 1 Create a Resource Group 

<p>
To start this tutorial you will want to start by creating a resource group 

![first create a resource group in azure](https://github.com/user-attachments/assets/cec15846-cb70-48f1-ad68-e5c2e9cbd850)
<P>
  
</P>

## Step 2 Create a Virtual Network and Subnet

We will Create a Virtual Network in Azure, you can do this by searching virtual network. We will then list our resource group as teh active-directory-resource group we just created and lets name our Virtual network

![creat VN](https://github.com/user-attachments/assets/a3185319-95ff-44d3-b013-c53b9500786f)

## Step 3 Create the Domain Controller Virtual machine

We are now going to create two virtual machines the first being our Domain Controller VM we will name it DC-1(Domain Controller) It is important to make sure it is in the Resource group we created earlier as well as the same region we made our virtual network in. Make sure for the server you are choosing the Windows Server 2022 data center azure edition click through all your tabs at the bottom until you hit networking once there make sure your on the virtual network we created earlier and then click review + create

</p>
<br />


![create domain controller](https://github.com/user-attachments/assets/1c6da2a8-064e-4ac2-be57-ab4f89cd7495)


## Step 4 Create Client Virtual Machine

Now we are going to go ahead and creat our second Virtual machine we will name it client-1
just like before make sure you are in the some resource group and region however this time for the server
we will choose windows 10 pro run through all your tabs and when you get to network make sure you are on the same network as the Domain controller VM and then click Review + Create


![client vm](https://github.com/user-attachments/assets/566f14a4-4484-4d9b-9a28-dec7d677ee09)

## Step 5 Change DC-1 VM private IP address to become static   

Now that both your virtual machines are created we are going to go back into our DC-1 virtual machine and change its private IP address to become static. Doing this will ensure that our Private IP address doesnt change from here on out regardless of how many times the Vm is restarted.
To do this you will click 
->DC-1 -> network settings -> dc-170_z1 (primary) / Ipconfig (primary) -> Ipconfig -> static -> save


![static](https://github.com/user-attachments/assets/fc6440a9-4e66-4496-9e25-25ab8d09418e)



## Step 6 Log into DC-1 and disable firwall to check connectivity  

Now we are logging into the DC-1 virtual machine while in there we will disable the windows firewall so we can test connectivity normally we wouldnt do this but for the purpose of this tutorial we will be so go ahead and log into your Dc-1 Machine right click the windows button click run and type "wf.msc" this will bring up the windows firewall settings from there go ahead and turn them all off and then click apply 


![disable firewall](https://github.com/user-attachments/assets/79165a3c-3885-487f-9b80-a0d74710d6ee)


## Step 7 Set Client-1's DNS setting to Dc-1's private IP Address

So what we are going to do now is set Client-1s DNS setting to Dc-1s private IP address doing this will allow the client virtual machine to join the domain normally in real life there would probably be thousands of clients joined to the domain but for this tutorial we will create just creat one.
first thing we will need to go to DC-1s virtual machine settings in azure and find its private IP address copy it then we will go to client-1 and click 
Networking-> Networksettings-> network interface/ip Configuration ->DNS servers->custom
and from there you will paste in the IP address of DC-1 and click save 
doing this allows us to join the domain. we will now from the Azure portal restart Client-1 

## Step 8 log into Client-1 and attempt to Ping DC-1

From here we are going to log into client-1 using remote desktop and going to attempt to ping DC-1s private IP address using Powershell. once logged on just search powershell and it should come up. type ping and then input DC-1s private IP address if all the steps above were done correctly you will recive a ping back. If you do not recieve a ping back and you want to trouble shoot, you are either on a different virtual networks than DC-1 or windows firewall on Dc-1 wasnt completely shut off 

![windows powershel](https://github.com/user-attachments/assets/a555d395-a09b-41df-8ecd-119ec4479ff8)

![ipconfig all](https://github.com/user-attachments/assets/fb2ac959-2b0a-4872-943b-3a1d157457ea)






