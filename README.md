# Active Directory Home Lab with Security Policies

## Description
The project demonstrates Active Directory administration, Group Policy management, and cybersecurity best practices, providing hands-on experience in securing an enterprise environment.

## Prerequisites
- Virtualization software: **VMware Workstation, VirtualBox, or Hyper-V**
- **Windows Server 2019/2022 ISO**
- **Windows 10/11 ISO** (for client machine)

### Network Configuration:
- Set up an **Internal Network** in the virtualization software to isolate the lab.

## Step 1: Set Up Windows Server (Domain Controller)
### 1. Install Windows Server
- Create a new virtual machine and install Windows Server
<p align="justified">
<br><img src= "https://i.imgur.com/m6eW03o.png"><br>
<br><img src= "https://i.imgur.com/us6cikB.png"><br>
<br><img src= "https://i.imgur.com/KBwz9Rv.png"><br>
<br><img src= "https://i.imgur.com/v4c28l0.png"><br>
<br><img src= "https://i.imgur.com/uMR7g1E.png"><br>
<br><img src= "https://i.imgur.com/fdZ8x1l.png"><br>

### 2. Install Active Directory Domain Services (AD DS)
- Open **Server Manager** → **Manage** → **Add Roles and Features**.
- Select **Active Directory Domain Services (AD DS)**.
- Click **Next** and **Install**.
<p align="justified">
<br><img src= "https://i.imgur.com/1rhKeQp.png"><br>
<br><img src= "https://i.imgur.com/1rhKeQp.png"><br>

- Promote the server to a **Domain Controller**:
  - Select **Add a new forest** (e.g., `lab.local`).
  - Set a **Directory Services Restore Mode (DSRM) password**.
  - Reboot after installation.
 <p align="justified">
<br><img src="https://i.imgur.com/A2RnMo7.png"></br>
<br><img src= "https://i.imgur.com/BxIvDe7.png"><br>
<br><img src= "https://i.imgur.com/IwhxnRv.png"><br> 

## Step 2: Set Up a Windows 10/11 Client
- Install Windows 10/11 on another VM.
- Join the client to the domain:
  - Go to **System Properties** → **Change settings**.
  - Enter domain **"example.com"** and authenticate with **Domain Admin credentials**.
  - Restart the client.

## Step 3: Create Organizational Units (OU) and User Accounts
### 1. Create OUs
- Open **Active Directory Users and Computers (ADUC)**.
- Right-click **example.com** → **New** → **Organizational Unit (OU)**.
- Create:
  - `IT`
  - `HR`
  - `Finance`
<p align="justified">
<br><img src=  "https://i.imgur.com/TCmm24b.png"></br>
<br><img src="https://i.imgur.com/jk6DbTx.png"></br>

### 2. Create Users
- Inside each OU, create users (e.g., `SueYou` in **IT** OU).
- Right-click **Users** → **New** → **User**.
- Assign a password and enable **User must change password at next logon**.
<p align="justified">
<br><img src=  "https://i.imgur.com/cB10FEe.png"></br>
<br><img src= "https://i.imgur.com/GbL8hHd.png"></br>
<br><img src= "https://i.imgur.com/ceJ2Vym.png"></br>

## Step 4: Configure GPO for Security Policies
### 1. Open Group Policy Management
- Run `gpedit.msc` or open **Group Policy Management Console (GPMC)**.

### 2. Create a New GPO for Security Policies
- Right-click **Group Policy Objects** → **New**.
- Name it **Security Policy**.
- Right-click the **Security Policy GPO** → **Edit**.

## Step 5: Configure Password Policy
- Navigate to:  
  `Computer Configuration` → `Policies` → `Windows Settings` → `Security Settings` → `Account Policies` → `Password Policy`.
- Configure:
  - **Enforce password history:** `10 passwords remembered`
  - **Maximum password age:** `45 days`
  - **Minimum password age:** `1 day`
  - **Minimum password length:** `12 characters`
  - **Password must meet complexity requirements:** `Enabled`
  - **Store passwords using reversible encryption:** `Disabled`

## Step 6: Configure Account Lockout Policy
- Navigate to:  
  `Computer Configuration` → `Policies` → `Windows Settings` → `Security Settings` → `Account Policies` → `Account Lockout Policy`.
- Configure:
  - **Account lockout threshold:** `5 invalid attempts`
  - **Account lockout duration:** `15 minutes`
  - **Reset account lockout counter after:** `15 minutes`

## Step 7: Apply GPO to the Domain
- In **Group Policy Management Console (GPMC)**:
  - Right-click **lab.local** → **Link an Existing GPO**.
  - Select **Security Policy**.
  - Click **OK**.

## Step 8: Verify the Policies
### 1. Run `gpupdate` to force policy updates
```powershell
gpupdate /force
```
- Restart the client machines.

### 2. Check Effective Policy
```powershell
gpresult /r
```
- Ensure the **Security Policy GPO** is applied.

### 3. Test Password Policy
- Try setting a password that doesn’t meet complexity requirements.

### 4. Test Account Lockout
- Enter the wrong password **5 times** and check if the account gets locked.
