# Active Directory and Routing Setup 

#### Goal: Set-up Routing and Active Directory

![Project Screenshot](../images/Phase1networkimage.png)

In this one, the following VMs will be used
-	Server 2025 Standard – Active Directory  Server and Domain Controller(192.168.1.10)
    - Servers LAN Segment(192.168.1.0/28)

-	PfSense CE Firewall – Firewall and Router 
  EM0 -WAN (DYNAMIC - Internet)
  Em1 - LANMANAGEMENT(192.168.1.17)
  Em2(192.168.1.1)
  Opt1(192.168.1.33)
    - Internal Management Lan Segment (192.168.1.16/28)
    - Servers LAN Segment(192.168.1.0/28)
    - Office LAN Segment (192.168.1.32/28)



The goal of this lab is to setup and install Active directory Domain Services, set up the forrest, join the management pc to the domain and setup firewall and routing between the two networks.

## STEP1: STATIC IP CONFIGURATIONS

After installation, i assigned the interfaces to the correct networks and set up Static IP addresses for now..
-	Server Static IP
![](../images/phase1/serverstaticip.png)

-	Management PC Static IP

-	PfSense Firewall IPs

![](../images/phase1/pfsenseips.png)

-	Firewall Rules to permit traffic for now
Once that is completed, test connection fromManagement PC to Server
![](../images/phase1/pfsenserules1.png)
![](../images/phase1/connectiontest.png)
 
 

## Step2: INSTALLING ACTIVE DIRECTORY DOMAIN SERVICES ON THE SERVER AND SETTING UP THE FOREST
Now we would be installing the Active directory Domain Services using PoweShell cmdlet
#### Install-WindowsFeature -Name “AD-Domain-Services”

![](../images/phase1/ADinstall.png)


### Creating a New Local Admin user for management
![](../images/phase1/adminuser.png)
 

### Starting the OpenSSH Server for Remote Management
- To get the status of the OpenSSH Server I used te powershell command
     #### Get-Service -Name “sshd”
- To start the service, I used
     #### Start-Service -Name “sshd”

![](../images/phase1/sshenable.png)
 
### Turned Off Firewall Profiles to allow SSH temporarily; 
Firewall rules will be added when hardening
- #### Set-NetFirewallProfile -Profile Domain, Private, Public -Enabled False
- #### Get-NetFirewallProfile | Select-Object Name, Enabled

![](../images/phase1/firewallruleslocal.png)
 

### Creating the forest and promoting the SERVER to domain controller
![](../images/phase1/forrestcreate.png)



## STEP3: CONFIRMATIONS, USER CREATION AND ENABLING REMOTE DESKTOP

### Confirming the Server is part of the domain, DNS Resolution, Forest and Domain FSMO Roles
- Active Directory confirmation
     #### Get-ADDomainController -Identity “SERVER-AD”
- DNS Resolution
     #### Resolve-DNSName -Name “lighthouse.local”
- Roles Confirmation
  - Forest-wide
     #### Get-ADForest | Select-Object DomainNamingMaster, SchemaMaster
  - Domain-Wide
     #### Get-ADDomain | Select-Object PDCEmulator, RIDMaster, InfrastructureMaster

![](../images/phase1/adconfirmation.png)
 

### Add the Domain Admin Account to Domain Admins Group, and remove it from local Admins Group
![](../images/phase1/admingroupedit.png)

Created this simple script to handle User Creation

### Creating a new user account
#### $FullName = Read-Host "Enter User Full Name: "
#### $FirstName = Read-Host "Enter User First Name: "
#### $LastName = Read-Host "Enter User Last Name: "
#### $UserName = Read-Host "Enter User User Name (firstInitial_Lastname): "
#### $PrincipalName = Read-Host "Enter User Principal Name: (username@lighthouse.local): "
#### $Password = Read-Host -AsSecureString "Enter User Temp Password: "

#### New-ADUser -Name "$FullName" `
#### -GivenName "$FirstName" `
#### -Surname "$LastName" `
#### -SAMAccountName "$UserName" `
#### -UserPrincipalName "$PrincipalName" `
#### -Path "CN=Users,DC=lighthouse,DC=local" `
#### -Server "SERVER-AD" `
#### -AccountPassword $Password `
#### -Enabled $true `
#### -ChangePasswordAtLogon $true

![](../images/phase1/newusercreate.png)

### Enabling Remote Desktop
![](../images/phase1/remotedesktopenable.png)
 




