# File Shares and Permissions

## Overview
This repository documents the process of creating file shares and setting permissions in an Active Directory environment. This exercise ensures that users have appropriate access to shared folders based on their security group memberships.

## Prerequisites
- **Windows Server (Domain Controller - DC-1)**
- **Windows Client Machine (Client-1)**
- **Active Directory Users and Computers (ADUC)**
- **File Explorer & Network Access**

## Step 1: Start the Virtual Machines
1. **Turn on** the **DC-1** and **Client-1** VMs in the **Azure Portal** if they are powered off.

![Screenshot 2025-03-20 225758](https://github.com/user-attachments/assets/301c19cf-30f7-4a12-8a28-2e7a4691adf7)

## Step 2: Create Sample File Shares with Permissions
1. **Log into DC-1** as the domain admin (`mydomain.com\jane_admin`).

![Screenshot 2025-03-20 233441](https://github.com/user-attachments/assets/8fa19a56-7f62-4f42-b32c-9b64c9d7b7a2)

2. **Log into Client-1** as a normal user (`mydomain\<someuser>`).

![Screenshot 2025-03-20 233644](https://github.com/user-attachments/assets/7ba86ce7-22e2-45fe-ad52-0aebad480169)

![Screenshot 2025-03-20 233759](https://github.com/user-attachments/assets/d623d749-c085-4ec7-864f-a557f2cb58f4)

3. On **DC-1**, navigate to the `C:\` drive and create the following folders:
   - `read-access`
   - `write-access`
   - `no-access`
   - `accounting`

![Screenshot 2025-03-20 234211](https://github.com/user-attachments/assets/4c54ea53-362c-4200-8468-69726db3b97d)

4. **Set Folder Permissions**:
   - `read-access`: Grant **Domain Users** `Read` permissions.
 
![Screenshot 2025-03-20 234652](https://github.com/user-attachments/assets/911c3250-96c2-43ca-8dd0-6d9226389cff)

![Screenshot 2025-03-20 234801](https://github.com/user-attachments/assets/22f20ece-394f-4fba-9ec8-b409ae5d02ee)

![Screenshot 2025-03-20 234822](https://github.com/user-attachments/assets/49133f98-3c54-442e-8cf7-886c9e260af1)


   - `write-access`: Grant **Domain Users** `Read/Write` permissions.

![Screenshot 2025-03-20 235204](https://github.com/user-attachments/assets/b0b5ab9a-b790-477e-aa15-817ba3276c7f)


   - `no-access`: Grant **Domain Admins** `Read/Write` permissions.

![Screenshot 2025-03-20 235320](https://github.com/user-attachments/assets/02fb5205-655a-40de-9705-f60b177e2ca5)


   - `accounting`: (Permissions will be set later.)


6. **Share Each Folder**:
   - Right-click the folder → `Properties` → `Sharing` → `Advanced Sharing` → Check `Share this folder`.

![Screenshot 2025-03-20 235629](https://github.com/user-attachments/assets/8d2b6b49-4a33-4e52-89ad-804abe01b5cc)


    - Click `Permissions` and assign the appropriate group and access level.

![Screenshot 2025-03-20 235746](https://github.com/user-attachments/assets/868ee201-40b4-4f49-9c08-a517405cdb71)


## Step 3: Test File Share Access as a Normal User

1. On **Client-1**, open File Explorer, type `\\dc-1` in the search bar, and press `Enter`.

![Screenshot 2025-03-21 000208](https://github.com/user-attachments/assets/5dc9d15d-e238-4638-8357-2161baf5a52b)


2. Attempt to access each shared folder:
   - **Which folders can you access?**
   - **Which folders allow you to create or modify files?**
   - **Does the behavior match the assigned permissions?**

![Screenshot 2025-03-21 000540](https://github.com/user-attachments/assets/b6e3f90b-5f43-44e0-8e2d-0a9a2e38562f)

![Screenshot 2025-03-21 000652](https://github.com/user-attachments/assets/ed3a575d-6840-4b1a-ab58-dca07d217bbe)

![Screenshot 2025-03-21 000815](https://github.com/user-attachments/assets/599e3421-24b9-41f4-b250-051e68708429)


The user logged in to client-1 is a member of Domain Users. They can open files in read-access, but cannot create or modify them. In the write-access folder, the user can both view and create files as expected. The no-access folder is inaccessible to the user, as they are not part of the Domain Admins group.
## Step 4: Create an "ACCOUNTANTS" Security Group and Test Access

1. **On DC-1**, open **Active Directory Users and Computers** (`ADUC`).

2. Create a new **Security Group** called `ACCOUNTANTS`.

![Screenshot 2025-03-21 001419](https://github.com/user-attachments/assets/1eac2c98-790b-4fda-932b-b125631bfb33)


3. Set the following **folder permissions** for `accounting`:
   - Assign `ACCOUNTANTS` **Read/Write** access.

![Screenshot 2025-03-21 001851](https://github.com/user-attachments/assets/93fb5e6e-dffc-4a54-bd69-371d9a90ca5e)

4. **Test User Access**:
   - On **Client-1**, as `<someuser>`, attempt to access `\\DC-1\accounting`.
   - The access should be **denied**.

![Screenshot 2025-03-21 001951](https://github.com/user-attachments/assets/795aa33c-723f-4877-91dd-b2943a6e8080)


5. **Add User to ACCOUNTANTS Group**:
   - On **DC-1**, add `<someuser>` to the `ACCOUNTANTS` security group.

![Screenshot 2025-03-21 002316](https://github.com/user-attachments/assets/6939bf5b-a730-4e1d-84a5-057d85e2f863)

![Screenshot 2025-03-21 002400](https://github.com/user-attachments/assets/633b874d-2678-43f0-be7f-ac8c34b0633f)


6. **Retry Access**:
   - Log out of **Client-1** and log back in as `<someuser>`.
   - Attempt to access `\\DC-1\accounting` again.
   - The user should now have access.

![Screenshot 2025-03-21 002613](https://github.com/user-attachments/assets/4527188d-0940-413a-b521-61ed3aaac5a1)


## Step 5: Cleanup (Optional)
- If testing is complete, **delete the VMs** or continue using them for practice.



## Conclusion
This guide demonstrated how to:
- Create shared folders on a domain controller
- Assign permissions to users and groups
- Test access using a domain-joined client

By following these steps, you’ve ensured that users only access what they’re authorized to, a key principle of security and efficient system management.
