# PHASE 2 - PART 1: Workstation, User, Group and OU Creation

In this Phase, I will be setting up the two workstations L-PC1, and L-PC2 in their respective subnets, then joining them to the domain. After, Organizational Units will be created, then Groups, and then workstations and User locations will be modified.


 

### Renaming the PAW

 <img width="975" height="566" alt="image" src="https://github.com/user-attachments/assets/b184ed23-9d31-4f19-a2ef-9ca89bdd9196" />

---

### Joined the PAW to the Domain

 CMDLET used
 #### Add-Computer -domain "domain"
<img width="975" height="654" alt="image" src="https://github.com/user-attachments/assets/6e1bea1d-a3a1-4580-9b1b-cc888873a11e" />

---




## Renaming and joining the standard workstations to the domain
### Renaming Both workstations to L-PC1 and L-PC2 respectively
<img width="833" height="487" alt="image" src="https://github.com/user-attachments/assets/1da9437e-f3ef-4f9f-8668-0f40e5f1ab42" />

<img width="839" height="491" alt="image" src="https://github.com/user-attachments/assets/f57eca40-8cf3-4caa-8343-4d0fef84ba27" />

---

### Network Properties and Pings for PC1 and PC2
<img width="975" height="566" alt="image" src="https://github.com/user-attachments/assets/dbe907f4-dd43-44ba-9235-338eb5a124ab" />

<img width="850" height="498" alt="image" src="https://github.com/user-attachments/assets/6929e5d8-3555-4745-977a-10dd649e0a7c" />

---

### Joining Both PCs to the Domain
<img width="975" height="571" alt="image" src="https://github.com/user-attachments/assets/f65da0d5-879a-439d-844f-27adc0369f0a" />

<img width="975" height="574" alt="image" src="https://github.com/user-attachments/assets/8c6e502f-73ae-443d-84ae-caff59ac0950" />

### Test User login after Restart
<img width="975" height="576" alt="image" src="https://github.com/user-attachments/assets/e28195df-dfaa-4546-a19d-73afa7b8cead" />

---





## Installing RSAT tools on PAW for remote management of the server
To get a list of all available RSAT tools online, i used
   #### get-windowscapability -online -name "Rsat*" | Select-Object name


To Install
   #### get-windowscapability -online -name "Rsat-Name..."
<img width="975" height="586" alt="image" src="https://github.com/user-attachments/assets/0e68c394-b529-49c7-a11b-13dd00018dfb" />
<img width="975" height="551" alt="image" src="https://github.com/user-attachments/assets/799bd887-ec3d-47e2-abb6-36947ad9ce40" />

---

 




## SETTING UP OUs

CMDLET - 
   #### New-ADOrganizationalUnit -name "...." -Path "...."
### London Branch
<img width="975" height="537" alt="image" src="https://github.com/user-attachments/assets/22545bed-63ba-4e79-b4c4-7e2366ae2bf2" />

<img width="975" height="546" alt="image" src="https://github.com/user-attachments/assets/00d4bcf6-c3d1-42aa-bd26-36f79d4defa0" />


---


#### Moving PCs to the right OUs to enable GPO Management of Devices
CMDLET - 
   #### Move-ADObject
<img width="975" height="546" alt="image" src="https://github.com/user-attachments/assets/1ab1b172-daab-44e0-ab56-3272da00ad69" />
<img width="975" height="532" alt="image" src="https://github.com/user-attachments/assets/925e7528-b2b5-415d-8275-2285a02d2a21" />
<img width="975" height="551" alt="image" src="https://github.com/user-attachments/assets/a54cb6da-0517-4bda-90ce-12448bac2159" />

---




 
## Group Creations
CMDLET - 
   #### New-ADGroup
---

### Department / User Groups (4 groups)
These are your core identity + access groups:
-	HR_Users
-	Finance_Users
-	IT_Users
-	Sales_Users

Covers: file access, folder permissions, GPO targeting
<img width="975" height="537" alt="image" src="https://github.com/user-attachments/assets/22912739-22de-4a70-84f2-d245bc96f2f1" />

---
### Admin / IT Control Groups (5 groups)
These enable delegation + restricted admin roles:
- Server_Admins
- Helpdesk_Admins
- Database_Admins
- Security_Admins
- System_Admins
<img width="975" height="519" alt="image" src="https://github.com/user-attachments/assets/47ed4f2a-07f2-4ffc-803e-0078e3a303b7" />

---

### Security Policy control groups (4 groups)
These will allow Policies to apply or exclude a defined scope of users or devices
- USB_Allowed_Users
- Local_Admin_Access
- Password_Expiry
- Remote Desktop Access

Covers: security enforcement, device control, policy filtering
<img width="975" height="526" alt="image" src="https://github.com/user-attachments/assets/14a70655-e471-41f6-b7ad-fccbddfb6e20" />

---

### Resource Access Groups ( 5 groups)
 
For file shares + printers:
-	HR_File_Access
-	IT_File_Access
-	FINANCE_File_Access
-	SALES_File_Access
-	SHARED_File_Access

Covers: NTFS permissions + shared drives

---





## Moving created users to their respective OUs
<img width="975" height="543" alt="image" src="https://github.com/user-attachments/assets/30265e8c-c8d3-46dd-b0bb-c47d3059806f" />

 




