# Deploy Active Directory Services

## Exercise 1 — Deploy AD DS

## Task 1 — Install AD DS Binaries

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] Confirm **Server Manager** opens automatically

- [ ] Open **Windows PowerShell ISE**:
      Start → Windows PowerShell ISE

- [ ] Install AD DS on **LON-SVR1**:
      Install-WindowsFeature -Name AD-Domain-Services -ComputerName LON-SVR1

- [ ] Verify AD DS role installation on **LON-SVR1**:
      Get-WindowsFeature -ComputerName LON-SVR1

      - [ ] In the output, confirm:
            - Active Directory Domain Services is installed (checked)
            - Under:
                  Remote Server Administration Tools  
                  → Role Administration Tools  
                  → AD DS and AD LDS Tools  
              only **Active Directory module for Windows PowerShell** is installed
              (no GUI tools like ADAC)

- [ ] If AD DS does not appear installed:
      Wait a few minutes, then run:
      Get-WindowsFeature -ComputerName LON-SVR1
      again


## Task 2 — Prepare AD DS Installation and Promote a Remote Server

### Add LON-SVR1 to Server Manager on LON-DC1

- [ ] On **LON-DC1** in **Server Manager**:
      Select **All Servers** view

- [ ] On **Manage** menu:
      Select **Add Servers**

- [ ] In **Add Servers** dialog:
      - Keep default search settings
      - Click **Find Now**
      - In **Active Directory** list:
            Select **LON-SVR1**
            Click arrow to move it to **Selected**
            Click **OK**

### Remotely Configure AD DS Using Server Manager

- [ ] On **LON-DC1** in **Server Manager**:
      Confirm:
      - AD DS role installation on **LON-SVR1** is complete
      - **LON-SVR1** is listed in **All Servers**

- [ ] Click the **Notifications** flag

- [ ] In post-deployment configuration:
      Select **Promote this server to a domain controller**

- [ ] In **Active Directory Domain Services Configuration Wizard**:

      #### Deployment Configuration

      - [ ] Operation:
            Add a domain controller to an existing domain
      - [ ] Domain:
            Contoso.com

      - [ ] Under **Supply the credentials to perform this operation**:
            Click **Change**
      - [ ] In **Credentials for deployment operation**:
            User name: Contoso\Administrator  
            Password: Pa55w.rd  
            Click **OK**, then **Next**

      #### Domain Controller Options

      - [ ] Clear:
            Domain Name System (DNS) server  
            Global Catalog (GC)
      - [ ] Ensure:
            Read-only domain controller (RODC) is **cleared**

      - [ ] Directory Services Restore Mode (DSRM) password:
            Enter and confirm: Pa55w.rd
      - [ ] Click **Next**

      #### Additional Options

      - [ ] Click **Next**

      #### Paths

      - [ ] Keep defaults:
            Database folder: C:\Windows\NTDS  
            Log files folder: C:\Windows\NTDS  
            SYSVOL folder: C:\Windows\SYSVOL  
      - [ ] Click **Next**

      #### Review Options

      - [ ] Click **View script** to open the generated PowerShell script in Notepad

### Edit the Generated Script

- [ ] In **Notepad**:
      - [ ] Delete all comment lines (starting with `#`)
      - [ ] Remove the **Import-Module** line
      - [ ] Remove all grave accents (`) at the end of lines
      - [ ] Remove line breaks so:
            Install-ADDSDomainController and all parameters are on **one line**

- [ ] Select the full command line:
      Place cursor at start → press **Shift+End** → menu **Edit → Copy**

- [ ] Switch back to the **AD DS Configuration Wizard**
      - [ ] Click **Cancel**
      - [ ] When prompted, select **Yes** to confirm cancellation

### Run Install-ADDSDomainController Remotely with Invoke-Command

- [ ] Switch to **Windows PowerShell ISE** on **LON-DC1**

- [ ] At the PowerShell prompt, type:
      Invoke-Command -ComputerName LON-SVR1 { }

- [ ] Place cursor **between** the braces `{ }`
      Paste the copied Install-ADDSDomainController one-line command

- [ ] Final line should be:

      Invoke-Command -ComputerName LON-SVR1 {Install-ADDSDomainController -NoGlobalCatalog:$true -Credential (Get-Credential) -CriticalReplicationOnly:$false -DatabasePath "C:\Windows\NTDS" -DomainName "Contoso.com" -InstallDns:$false -LogPath "C:\Windows\NTDS" -NoRebootonCompletion:$false -SiteName "Default-First-Site-Name" -SysvolPath "C:\Windows\SYSVOL" -Force:$true }

- [ ] Press **Enter** to start the installation

- [ ] In **Windows PowerShell Credential Request**:
      User name: Contoso\Administrator  
      Password: Pa55w.rd  
      Click **OK**

- [ ] When prompted for **SafeModeAdministratorPassword**:
      Enter: Pa55w.rd  
      Press **Enter**

- [ ] When prompted for confirmation:
      Enter: Pa55w.rd  
      Press **Enter**

- [ ] Wait until:
      Status: Success
      (LON-SVR1 restarts automatically)

- [ ] Close Notepad **without** saving

- [ ] After **LON-SVR1** restarts:
      - [ ] On **LON-DC1**, return to **Server Manager**
      - [ ] Select **AD DS** node on the left
      - [ ] Confirm:
            LON-SVR1 appears as a domain controller  
            Warning notification cleared (Refresh if needed)


## Task 3 — Run the AD DS Best Practices Analyzer

- [ ] On **LON-DC1** in **Server Manager**:
      Select **All Servers** view

- [ ] Scroll to **Best Practices Analyzer** section

- [ ] Open **Tasks** menu:
      Select **Start BPA Scan**

- [ ] In **Select Servers** dialog:
      - [ ] Select:
            LON-DC1.Contoso.com  
            LON-SVR1.Contoso.com
      - [ ] Click **Start Scan**

- [ ] Wait for BPA scan to complete

- [ ] Review BPA results:
      - [ ] Note entries with:
            Information  
            Warning  
      - [ ] Select some entries to read details and recommendations

- [ ] Result:
      New domain controller deployed (LON-SVR1)
      AD DS Best Practices Analyzer reviewed for both DCs

# Exercise 2 — Administer AD DS

## Task 1 — Use the Active Directory Administrative Center

### Navigate within Active Directory Administrative Center

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] In **Server Manager**:
      Tools → **Active Directory Administrative Center**

- [ ] Expand the ADAC window

- [ ] In the navigation pane:
      Select **Tree View** tab  
      Expand **Contoso (local)**


### Perform an Administrative Task (Reset Password)

- [ ] In ADAC, select **Overview**

- [ ] In **Reset Password** section:
      User name: `Contoso\Anita`  
      Password: `Pa55w.rd`  
      Confirm password: `Pa55w.rd`  

- [ ] Clear:
      User must change password at next log on

- [ ] Select **Apply**
      (Password reset completed)


### Search for Objects

- [ ] In **Global Search**:
      Enter: `Lon`
      Press **Enter**

- [ ] Confirm:
      All objects beginning with **Lon** are displayed


### Create Objects

- [ ] In the navigation pane:
      Expand **Contoso (local)** → select **Computers**

- [ ] In **Tasks** pane under **Computers**:
      New → **Computer**

- [ ] In **Create Computer** dialog:
      Computer name: `LON-CL4`  
      Computer (NetBIOS) name: `LON-CL4`  

- [ ] Select **OK**


### View All Object Attributes

- [ ] In ADAC:
      Select **Contoso (local)** → open **Computers**

- [ ] Select **LON-CL4**

- [ ] In Tasks pane under **LON-CL4**:
      Select **Properties**

- [ ] In properties window:
      Scroll to **Extensions**
      Select **Attribute Editor**

- [ ] Review all AD DS attributes for the computer object

- [ ] Click **Cancel** to close window


### Use Windows PowerShell History Viewer

- [ ] In ADAC:
      Select **Windows PowerShell History** toolbar (bottom of window)

- [ ] Review the cmdlet used for password reset:
      **Set-ADAccountPassword**

- [ ] Review the cmdlet used for new computer creation:
      **New-ADComputer**

- [ ] Close all windows


## Result

- You used ADAC to administer AD DS  
- You reviewed PowerShell cmdlets executed automatically in the background
