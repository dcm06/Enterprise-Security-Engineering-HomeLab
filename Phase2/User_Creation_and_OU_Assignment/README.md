Phase 2 Part 2: USER CREATION AND OU ASSIGNMENT

 
 
 
 



 
 

-	User Group Mangement
 
Linked the GPO to the Computers OU







 

-	Enforce GPOs for baseline hardening

Added a Separate Admin Account for all Admins
Web Admin Permissions for Sarah Scott
 


Helpdesk Permissions for Jennifer Wu
 














GPO Management

-	GPO creation and security group setup
o	Local Admin Access
Created a GPO named Local Admin Access – Then added the domain Workstation admins group and the System Admins group to the Local Administrators Group.
 

-	Admin Only Access on PAW
 

 
 

Testing
	Using Sarah Scott Regular Account to log in
	 
Using admin account
 

Deny Domain Admin Logon on Standard Workstations

 
 


Testing with Domain admin account 
 

With Regular account – 
 

With System Admin Account - 
 
 



Deny Regular Users and SYS Admins logon to DCs
 
 
 
 

Trying with Regular User Account
 


Trying with SYS Admin Account
 

Trying with Domain Admin Account
 

Trying with Enterprise/BreakGlass Account
 




Restricting GPO manaegemt to Security admins only
 
Assigning Security adin rights to the IT Operations manager for now
 


Creating a Domain Admins OU for designated Administrators of the domain
 

DNS Admins
 

Custon Server Admins group
 
 
 




Delegating Access for Admins
Doing this we would be using the tiered model
Tier 0 – Domain Admins OU, PAWs, Break Glass, AdminAccounts, Domain Controllers, GPO links to Tier 0

Tier 1 -  Servers, server admins

Tier 2 – Workstations, Helpdesk Admins, Standard Users

Only admins in a tier will be able to access their tier’s resources
Group	OU(s) delegated	Rights granted	Explicitly denied / not delegated
Helpdesk Admins	Users and ComputersOU (standard users and computers)	Reset password, force password change at next logon, unlock account, edit basic user info (phone, description, office)	No rights on Groups, , SERVERS, PAWs, AdminAccounts, Domain Admins OU
Workstation Admins			
Server Admins	SERVERS OU	Full control of computer objects in Servers OU (join, manage, reset account), manage server-specific security groups if you scope a Groups\Server-Groups sub-OU	No rights on PAWs, AdminAccounts, Domain Admins OU, Users
Security Admins	Domain-wide (or GPO container specifically) + Groups OU	Create/edit/link GPOs (Group Policy Creator Owners equivalent, scoped), manage security groups, edit audit policy GPOs	Not delegated object-modify rights over Domain Admins OU or AdminAccounts unless dual-control is intended
Web Admins (e.g., Sarah Scott)	Specific sub-OU or specific computer objects (web servers only)	Manage only the specific web server computer objects / a scoped security group for web app access	No rights elsewhere in SERVERS OU, no user-object rights at all
System Admins	Computers + local admin via the GPO you already built	Local admin rights (via Restricted Groups GPO), not necessarily AD object delegation	—
Domain Admins	Everything (built-in)	Full control	Should be near-empty membership; used only via PAW, ideally with time-bound/PIM-style access if you want to go further
Break Glass accounts	N/A — not delegated to anyone for routine use	Emergency access only	Should not be in any of the above groups' scope; password sealed/rotated, usage alerted on



Helpdesk Admins
 
 
 

WORKSTATION ADMINS
Converted workstation admins to be the group thet handles regular workstation management and not local admin access, that will be done using  System admins
 

SECURITY ADMINS
Removed security admins group from built in group policy creator owner group, then delegated duties to domain
  


 

	Checking group policy access.
 





1.	Advanced Audit Policy Configuration
Domain Controllers Audit policy
 
 
 
 
 
 
 
 
 
 




 Workstation Audit Policy
-	Same as DC but without DS Access and Account logon 

Priviledged Audit Policy
-	Same as the rest except no DS Access and Account logon audinting
-	Audit special logon is set to success and failure
-	PNP
 
 


Account Logon (on DCs — this is where Kerberos/NTLM auth actually gets logged)
Subcategory	Setting
Credential Validation	Success + Failure
Kerberos Authentication Service	Success + Failure
Kerberos Service Ticket Operations	Success + Failure

h	
Account Management
Subcategory	Setting
User Account Management	Success + Failure
Security Group Management	Success + Failure
Computer Account Management	Success
Other Account Management Events	Success + Failure
Logon/Logoff
Subcategory	Setting
Logon	Success + Failure
Logoff	Success
Account Lockout	Success + Failure
Special Logon (privileged/admin logons)	Success
Object Access — noisy if enabled everywhere, so scope via SACLs on specific objects (Domain Admins OU, AdminAccounts, PAWs, sensitive file shares) rather than blanket-enabling
Subcategory	Setting
File System	Success + Failure (only on SACL'd objects)
Removable Storage	Success + Failure
Policy Change — critical for catching GPO/permission tampering
Subcategory	Setting
Audit Policy Change	Success + Failure
Authentication Policy Change	Success
Authorization Policy Change	Success
Privilege Use
Subcategory	Setting
Sensitive Privilege Use	Success + Failure
Detailed Tracking — this is the one that gives you command-line visibility
Subcategory	Setting
Process Creation	Success
Process Termination	Success (optional, high volume)
PNP Activity	Success (catches unauthorized USB device installs)
DS Access (Domain Controllers only)
Subcategory	Setting
Directory Service Changes	Success + Failure
Directory Service Access	Success (scope with SACLs — very noisy otherwise)
System
Subcategory	Setting
Security State Change	Success
Security System Extension	Success
System Integrity	Success + Failure


 BASELINE HARDENING
-	SERVER
 
	Settings
o	Configure the Behaviour of the sudo command – Set to Disable
Computer configuration -> Administrative templates -> system
 
o	Disable Internet Explorer 11 Launch Via COM Automation - Configured as Enabled to prevent legacy scripts and applications from programmatically launching Internet Explorer 11 using COM automation interfaces
Computer configuration -> Administrative templates -> Windows Components -> Internet Explorer


 

o	Network security: Restrict NTLM: Audit Incoming NTLM Traffic
Set Enabled for auditing all accounts
Computer Configuration → Security Settings -> Local Policies -> Security Options
 

o	Network security: Restrict NTLM: Audit NTLM authentication in this domain Configured as Enable all on DC
Computer Configuration → Security Settings -> Local Policies -> Security Options
 

o	Network security: Restrict NTLM: Outgoing NTLM traffic to remote servers Configured as Audit all on both MS and DC
Computer Configuration → Security Settings -> Local Policies -> Security Options
 

o	Printer: Configure RPC connection settings Enforce the default, RPC over TCP with Authentication Enabled, on both MS and DC
Computer Configuration → Administrative Templates -> Printers
 

o	Printer: Configure RPC listener settings Configure as RPC over TCP | Kerberos on MS
Computer Configuration → Administrative Templates -> Printers
 


