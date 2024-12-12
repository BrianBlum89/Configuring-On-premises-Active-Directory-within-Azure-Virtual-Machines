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
- PowerShell
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

Now we are logging into the DC-1 virtual machine while in there we will disable the windows firewall so we can test connectivity normally we wouldnt do this but for the purpose of this tutorial we will be so go ahead and log into your Dc-1 Machine right click the windows button click run and type "wf.msc" this will bring up the windows firewall settings from there go ahead and turn them all off and then click apply this will allow us to ping off of DC-1 later from the client VM to ensure everything is set up correctly. 


![disable firewall](https://github.com/user-attachments/assets/79165a3c-3885-487f-9b80-a0d74710d6ee)


## Step 7 Set Client-1's DNS setting to Dc-1's private IP Address

So what we are going to do now is set Client-1s DNS setting to Dc-1s private IP address doing this will allow the client virtual machine to join the domain normally in real life there would probably be thousands of clients joined to the domain but for this tutorial we will create just creat one.
first thing we will need to go to DC-1s virtual machine settings in azure and find its private IP address copy it then we will go to client-1 and click 
Networking-> Networksettings-> network interface/ip Configuration ->DNS servers->custom
and from there you will paste in the IP address of DC-1 and click save 
doing this allows us to join the domain. we will now from the Azure portal restart Client-1 

![image](https://github.com/user-attachments/assets/22dbc9e0-31bd-4846-a959-457ba739dade)


## Step 8 Log into Client-1 and attempt to Ping DC-1

From here we are going to log into client-1 using remote desktop and going to attempt to ping DC-1s private IP address using Powershell. once logged on just search powershell and it should come up. type ping and then input DC-1s private IP address if all the steps above were done correctly you will recive a ping back. If you do not recieve a ping back and you want to trouble shoot, you are either on a different virtual networks than DC-1 or windows firewall on Dc-1 wasnt completely shut off 

![windows powershel](https://github.com/user-attachments/assets/a555d395-a09b-41df-8ecd-119ec4479ff8)

![ipconfig all](https://github.com/user-attachments/assets/fb2ac959-2b0a-4872-943b-3a1d157457ea)

## Step 9 Install Active Directory 

You are now going to log into DC-1's virtual machine and in the server manager menu click Roles and features. Keep clicking next until you come to the server roles tab and from there 
click ->Active Directory Domain Services-> features and install 

![deploy active direcotry services](https://github.com/user-attachments/assets/8cae3e0c-6abe-426d-9915-24d048d1ca2f)

![installing ad](https://github.com/user-attachments/assets/2a025dee-d1be-4cc0-94c2-27950d6e4b20)

## Step 10 Promote Dc-1 VM to a Domain Controller 

You are now going to go back to the service manager Dashboard and click the flag in the upper right hand corner and then click promote this server to domain controller 

![promote DC1](https://github.com/user-attachments/assets/73c02603-a5df-4ced-840a-18887bf1b7a0)

once that has been done you will click "Add a new forest" and name your domain 
create a DSRM password click next

![add a new forest](https://github.com/user-attachments/assets/b909a711-9bbe-43b7-81f4-999dd63eb92f)

Once done with the Prereq check you can just click install 

![install](https://github.com/user-attachments/assets/bf857620-3137-4863-ba48-a8b5de6180bd)

Once its done installing the system will automatically restart and log you out 

![reset](https://github.com/user-attachments/assets/fd22a981-acd2-401e-b50a-95460e3a391e)

## Step 11 Login into Dc-1 as a domain controller 

Now we will log back into DC-1 now that we are a domain controller will need to login differently. 
you will need to specify that you want to login as a Domain user and you will have to specify which domain as shown below 

![my domain login](https://github.com/user-attachments/assets/2eb03f0f-6e05-4437-923d-7a7220ccbf26)

## 12 Create domain admin user within the domain. 

Now we are going to create organizational units and then and admin user and place them within the domain. 
To Start this we will click the windows button->windows admininistrative tools-> active directory users and computers. Then we will create an organizational folder title "_EMPLOYEES" and then another one called "_ADMINS"
To do this you will right click mydomain.com->new->Organizational unit

![creating org units](https://github.com/user-attachments/assets/36156d2a-52ab-42a4-8a37-bb24d67f7850)

Now we will create a new employee, and we will add them to the Domain Admins security group. 
To do this you will go to the admins folder right click -> New -> User
from there you will create you employee and set their password 

![jane doe](https://github.com/user-attachments/assets/3edc9f44-819f-4504-8d7e-6bafc8125172)

Once finished you will see them in you admins directory 

![in directory](https://github.com/user-attachments/assets/84dcc054-f304-43e0-9e69-841cec9e5906)

Just because they are in your _ADMINS folder doesnt make them an admin so from here we will right click their name -> properties -> member of -> add-> then type domain admins -> ok -> apply 
<p>Now this account is an actual domain admin, 
and this Completes Tutorial for Active Directory Within Azure VMs.

