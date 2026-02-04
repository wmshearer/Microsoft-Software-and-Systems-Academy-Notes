# ANSWER
Yes — I was condensing it by stopping at Exercise 1 (because you previously told me “one set of instructions at a time” and “don’t jump ahead”).

# FIX
You want a FULL checkbox checklist that includes EVERY command and step you pasted (Exercise 1–4 + the next lab sections you included). Below is the complete reproduction list with NO trimming.

# LAB 1 — Configuring Windows PowerShell, and Finding and Running Commands

## EXERCISE 1 — CONFIGURING THE WINDOWS POWERSHELL CONSOLE APPLICATION

### TASK 1 — START POWERSHELL AS ADMIN + PIN TO TASKBAR
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Confirm the result is **Windows PowerShell** (NOT **Windows PowerShell (x86)**)
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Verify title bar starts with **Administrator:** and does NOT include **(x86)**
- [ ] On the taskbar, right-click the **Windows PowerShell** icon
- [ ] Click **Pin to taskbar**

### TASK 2 — CONFIGURE THE POWERSHELL CONSOLE APPLICATION
#### Set Consolas font (Size 16)
- [ ] Click the **control box** (top-left corner of the PowerShell window)
- [ ] Click **Properties**
- [ ] Click the **Font** tab
- [ ] In **Font**, select **Consolas**
- [ ] In **Size**, select **16**

#### Choose display colors (optional)
- [ ] Click the **Colors** tab
- [ ] Review **Screen Text** color options
- [ ] Review **Screen Background** color options
- [ ] (Optional) Use the color picker to adjust for readability

#### Resize window + remove horizontal scroll bar
- [ ] Click the **Layout** tab
- [ ] Under **Window Size**, adjust **Width** and **Height** until the console preview fits inside **Window Preview**
- [ ] Under **Screen Buffer Size**, set **Width** = the same value as **Window Size Width**
- [ ] Click **OK**

### TASK 3 — START A SHELL TRANSCRIPT
- [ ] In the PowerShell console, type:
      Start-Transcript C:\DayOne.txt
- [ ] Press **Enter**
- [ ] Confirm transcript started message appears
- [ ] Know transcript continues until:
      - [ ] You run `Stop-Transcript` OR
      - [ ] You close the PowerShell window
- [ ] (Optional) Open `C:\DayOne.txt` later to review transcript contents

## EXERCISE 2 — CONFIGURING WINDOWS POWERSHELL ISE

### TASK 1 — OPEN POWERSHELL ISE AS ADMINISTRATOR
- [ ] In the (Admin) PowerShell console, type:
      ise
- [ ] Press **Enter**
- [ ] Confirm **Windows PowerShell ISE** opens
- [ ] Close the ISE window
- [ ] On the taskbar, right-click the **Windows PowerShell** icon
- [ ] Click **Run ISE as Administrator**
- [ ] Confirm ISE is running as Administrator

### TASK 2 — CUSTOMIZE ISE (SINGLE-PANE, HIDE COMMANDS, FONT SIZE)
- [ ] In ISE toolbar, click **Show Script Pane Maximized**
- [ ] Click **Hide Script Pane** (up-arrow icon) to display the console
- [ ] (Alternative) Press **Ctrl+R** to toggle script pane visibility
- [ ] Click **Show Command Add-on** (if Commands pane not showing) to show it
- [ ] Click **Show Command Add-on** again to hide the Commands pane
- [ ] Use the **slider (lower-right)** to adjust font size until comfortable
- [ ] Close **Windows PowerShell ISE**
- [ ] Close **Windows PowerShell** console

## EXERCISE 3 — FINDING AND RUNNING WINDOWS POWERSHELL COMMANDS

### TASK 1 — FIND COMMANDS THAT ACCOMPLISH SPECIFIED TASKS
- [ ] On **AZ-040T00A-LON-CL1**, taskbar: right-click **Windows PowerShell**
- [ ] Click **Run as Administrator**

#### Find the “Resolve” DNS command
- [ ] Run one of:
      Get-Help *resolve*
- [ ] OR run:
      Get-Command *resolve*
- [ ] OR run:
      Get-Command -Verb resolve
- [ ] Identify **Resolve-DnsName** in results

#### Find “Adapter” commands leading to Set-NetAdapter
- [ ] Run one of:
      Get-Help *adapter*
- [ ] OR run:
      Get-Command *adapter*
- [ ] OR run:
      Get-Command -Noun *adapter*
- [ ] OR run:
      Get-Command -Verb Set -Noun *adapter*
- [ ] Identify **Set-NetAdapter** in results
- [ ] Run:
      Get-Help Set-NetAdapter
- [ ] Locate the **-MACAddress** parameter in the help output

#### Find “Sched” -> ScheduledTasks module -> Enable-ScheduledTask
- [ ] Run one of:
      Get-Help *sched*
- [ ] OR run:
      Get-Command *sched*
- [ ] OR run:
      Get-Module *sched* -ListAvailable
- [ ] Identify module **ScheduledTasks**
- [ ] Run:
      Get-Command -Module *ScheduledTasks*
- [ ] Identify **Enable-ScheduledTask**

#### Find “Block” -> Block-SmbShareAccess and learn what it does
- [ ] Run one of:
      Get-Command -Verb Block
- [ ] OR run:
      Get-Help *block*
- [ ] Identify **Block-SmbShareAccess**
- [ ] Run:
      Get-Help Block-SmbShareAccess
- [ ] Note: it applies a **Deny** entry to the file share **DACL**

#### Search “branch” (full-text search)
- [ ] Run:
      Get-Help *branch*
- [ ] Confirm it does a full-text search and does NOT directly find BranchCache cache clearing

#### Find “cache” -> Clear-BCCache
- [ ] Run one of:
      Get-Help *cache*
- [ ] OR run:
      Get-Command *cache*
- [ ] OR run:
      Get-Command -Verb clear
- [ ] Identify **Clear-BCCache**

#### Find firewall rule command -> Get-NetFirewallRule
- [ ] Run one of:
      Get-Help *firewall*
- [ ] OR run:
      Get-Command *firewall*
- [ ] OR run:
      Get-Help *rule*
- [ ] OR run:
      Get-Command *rule*
- [ ] Identify **Get-NetFirewallRule**
- [ ] Run:
      Get-Help Get-NetFirewallRule -Full
- [ ] Find the **-Enabled** parameter in help

#### Find local IP address command -> Get-NetIPAddress
- [ ] Run:
      Get-Help *address*
- [ ] Identify **Get-NetIPAddress**

#### Find print job suspend command -> Suspend-PrintJob
- [ ] Run:
      Get-Command -Verb suspend
- [ ] Identify **Suspend-PrintJob**

#### Find cmd.exe “Type” equivalent (alias) and content command
- [ ] Run:
      Get-Alias Type
- [ ] Confirm it maps to **Get-Content**
- [ ] OR run:
      Get-Command -Noun *content*
- [ ] Identify **Get-Content**

### TASK 2 — RUN COMMANDS TO ACCOMPLISH SPECIFIED TASKS
- [ ] Confirm you are signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**

#### Enabled firewall rules
- [ ] Run:
      Get-NetFirewallRule -Enabled True

#### All local IPv4 addresses
- [ ] Run:
      Get-NetIPAddress -AddressFamily IPv4

#### Set BITS startup type to Automatic
- [ ] Run:
      Set-Service -Name BITS -StartupType Automatic

#### Test connection to LON-DC1 (returns only True/False)
- [ ] (If needed) Log in as **adatum\administrator** with password **Pa55w.rd**
- [ ] Run:
      Test-Connection -ComputerName LON-DC1 -Quiet

#### Newest 10 Security event log entries
- [ ] Run:
      Get-EventLog -LogName Security -Newest 10

## EXERCISE 4 — USING ABOUT FILES

### TASK 1 — LOCATE AND REVIEW ABOUT HELP FILES
- [ ] Confirm you are still signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**

#### Find wildcard string comparison operators
- [ ] Run:
      Get-Help *comparison*
- [ ] Run:
      Get-Help about_comparison_operators -ShowWindow
- [ ] In the help window, find **-Like** operator:
      - [ ] In Find box type: wildcard
      - [ ] Click **Next**
- [ ] Note: typical operators are **not case-sensitive**
- [ ] Note: case-sensitive operators are listed in the same About file

#### Display COMPUTERNAME environment variable
- [ ] Run:
      $env:computername
- [ ] (Concept) This technique is documented in **about_environment_variables**

#### Review code signing help
- [ ] Run:
      Get-Help *signing*
- [ ] Run:
      Get-Help about_signing
- [ ] Note: **New-SelfSignedCertificate** creates a self-signed digital certificate

- [ ] Click **Next** to advance to the next lab (when instructed)

# LAB 2 — Performing Local System Administration with PowerShell

## EXERCISE 1 — CREATING AND MANAGING ACTIVE DIRECTORY OBJECTS

### TASK 1 — CREATE A NEW OU
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Confirm result is **Windows PowerShell** (NOT x86)
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      New-ADOrganizationalUnit -Name London

### TASK 2 — CREATE A GROUP
- [ ] Run:
      New-ADGroup "London Admins" -GroupScope Global

### TASK 3 — CREATE USER + COMPUTER, ADD USER TO GROUP
- [ ] Run:
      New-ADUser -Name Ty -DisplayName "Ty Carlson"
- [ ] Run:
      Add-ADGroupMember "London Admins" -Members Ty
- [ ] Run:
      New-ADComputer LON-CL2

### TASK 4 — MOVE OBJECTS INTO THE LONDON OU
- [ ] Run:
      Move-ADObject -Identity "CN=London Admins,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
- [ ] Run:
      Move-ADObject -Identity "CN=Ty,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
- [ ] Run:
      Move-ADObject -Identity "CN=LON-CL2,CN=Computers,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"

## EXERCISE 2 — CONFIGURING NETWORK SETTINGS ON WINDOWS SERVER

### TASK 1 — TEST CONNECTION + REVIEW CONFIG
- [ ] Switch to **AZ-040T00A-LON-SVR1**
- [ ] Log in as **adatum\administrator** with password **Pa55w.rd**
- [ ] Right-click **Start**
- [ ] Click **Windows PowerShell (Admin)**
- [ ] Run:
      Test-NetConnection LON-DC1
- [ ] Run:
      Get-NetIPConfiguration
- [ ] Note the **IP address**, **default gateway**, and **DNS server**

### TASK 2 — CHANGE THE SERVER IP ADDRESS
- [ ] Run:
      New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.15 -PrefixLength 16
- [ ] Run:
      Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11
- [ ] Type **Y** and press **Enter** for both confirmation prompts

### TASK 3 — CHANGE DNS SETTINGS + DEFAULT GATEWAY
- [ ] Run:
      Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 172.16.0.12
- [ ] Run:
      Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false
- [ ] Run:
      New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2

### TASK 4 — VERIFY + TEST CHANGES
- [ ] Run:
      Get-NetIPConfiguration
- [ ] Note the **IP address**, **default gateway**, and **DNS server**
- [ ] Run:
      Test-Connection LON-DC1
- [ ] Note: response should be slower (may vary)

## EXERCISE 3 — CREATING A WEBSITE

### TASK 1 — INSTALL IIS ROLE
- [ ] On **AZ-040T00A-LON-SVR1**, run:
      Install-WindowsFeature Web-Server
- [ ] Wait for IIS installation to complete

### TASK 2 — CREATE WEBSITE FOLDER
- [ ] Run:
      New-Item C:\inetpub\wwwroot\London -Type directory

### TASK 3 — CREATE IIS WEBSITE + TEST IN BROWSER
- [ ] Run:
      New-IISSite London -PhysicalPath C:\inetpub\wwwroot\London -BindingInformation "172.16.0.15:8080:"
- [ ] Click **Internet Explorer** on the taskbar
- [ ] Browse to:
      http://172.16.0.15:8080
- [ ] Expect an error about directory listing (this is expected)
- [ ] Confirm error details show physical path:
      C:\inetpub\wwwroot\London
- [ ] Click **Next** to advance when instructed

# LAB 3 — Using PowerShell Pipeline

## EXERCISE 1 — SELECTING, SORTING, DISPLAYING DATA USING PIPELINE BINDING

### TASK 1 — DISPLAY CURRENT DAY OF YEAR
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      help *date*
- [ ] Run:
      Get-Help Get-Date -Parameter * | Where-Object { $_.PipelineInput -match "true" }
- [ ] Run:
      Get-Date
- [ ] Run:
      Get-Date | Get-Member
- [ ] Run:
      Get-Date | Select-Object -Property DayOfYear
- [ ] Run:
      Get-Date | Select-Object -Property DayOfYear | fl

### TASK 2 — DISPLAY INFORMATION ABOUT INSTALLED HOTFIXES
- [ ] Run:
      Get-Command *hotfix*
- [ ] Run:
      Get-Help Get-Hotfix -Parameter * | Where-Object { $_.PipelineInput -match "true" }
- [ ] Run:
      Get-Hotfix | Get-Member
- [ ] Run:
      Get-Hotfix | Select-Object -Property HotFixID,InstalledOn,InstalledBy
- [ ] Run:
      Get-Hotfix | Select-Object -Property HotFixID,@{n='HotFixAge';e={(New-TimeSpan -Start $PSItem.InstalledOn).Days}},InstalledBy

### TASK 3 — DISPLAY DHCP SCOPES FROM DHCP SERVER
- [ ] Run:
      help *scope*
- [ ] Run:
      Help Get-DHCPServerv4Scope -ShowWindow
- [ ] Close the help window
- [ ] Run:
      Get-DHCPServerv4Scope -ComputerName LON-DC1
- [ ] Run:
      Get-DHCPServerv4Scope -ComputerName LON-DC1 | Select-Object -Property ScopeId,SubnetMask,Name | fl

### TASK 4 — DISPLAY SORTED LIST OF ENABLED FIREWALL RULES
- [ ] Run:
      help *rule*
- [ ] Run:
      Get-NetFirewallRule
- [ ] Run:
      Help Get-NetFirewallRule -ShowWindow
- [ ] Close the help window
- [ ] Run:
      Get-NetFirewallRule -Enabled True
- [ ] Run:
      Get-NetFirewallRule -Enabled True | Format-Table -Wrap
- [ ] Run:
      Get-NetFirewallRule -Enabled True | Select-Object -Property DisplayName,Profile,Direction,Action | Sort-Object -Property Profile,DisplayName | ft -GroupBy Profile

### TASK 5 — DISPLAY SORTED LIST OF NETWORK NEIGHBORS
- [ ] Run:
      help *neighbor*
- [ ] Run:
      help Get-NetNeighbor -ShowWindow
- [ ] Close the help window
- [ ] Run:
      Get-NetNeighbor
- [ ] Run:
      Get-NetNeighbor | Sort-Object -Property State
- [ ] Run:
      Get-NetNeighbor | Sort-Object -Property State | Select-Object -Property IPAddress,State | Format-Wide -GroupBy State -AutoSize

### TASK 6 — DISPLAY DNS NAME RESOLUTION CACHE
- [ ] Run:
      Test-NetConnection LON-DC1
- [ ] Run:
      help *cache*
- [ ] Run:
      Get-DnsClientCache
- [ ] Run:
      Get-DnsClientCache | Select Name,Type,TimeToLive | Sort Name | Format-List
- [ ] Note: record type numbers map to DNS types (example: 1=A, 5=CNAME)

### CLOSE WINDOWS
- [ ] Close all open windows

# MODE
Checkbox mode

# CONTINUATION — Exercise 2: Filtering objects

## TASK 1 — DISPLAY ALL USERS IN THE USERS CONTAINER (AD)

- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **PowerShell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] In the Administrator: Windows PowerShell window, run:
      help *user*
- [ ] Note: identify **Get-ADUser**
- [ ] Run:
      Get-Help Get-ADUser -ShowWindow
- [ ] Note: **-Filter** is mandatory; review examples
- [ ] Close the **Get-ADUser** help window
- [ ] Run:
      Get-ADUser -Filter * | ft
- [ ] Run:
      Get-ADUser -Filter * -SearchBase "cn=Users,dc=Adatum,dc=com" | ft

## TASK 2 — REPORT: SECURITY EVENT LOG ENTRIES WITH EVENT ID 4624

- [ ] In the Administrator: Windows PowerShell window, run:
      Get-EventLog -LogName Security |
      Where EventID -eq 4624 | Measure-Object | fw
- [ ] Run:
      Get-EventLog -LogName Security |
      Where EventID -eq 4624 |
      Select TimeWritten,EventID,Message
- [ ] Run:
      Get-EventLog -LogName Security |
      Where EventID -eq 4624 |
      Select TimeWritten,EventID,Message -Last 10 | fl

## TASK 3 — LIST ENCRYPTION CERTIFICATES INSTALLED ON THE COMPUTER

- [ ] In the Administrator: Windows PowerShell window, run:
      Get-ChildItem -Path CERT: -Recurse
- [ ] Run:
      Get-ChildItem -Path CERT: -Recurse |
      Get-Member
- [ ] Run ONE of the following (either is acceptable):

      Option A:
      Get-ChildItem -Path CERT: -Recurse |
      Where HasPrivateKey -eq $False | Select-Object -Property FriendlyName,Issuer | fl

      Option B:
      Get-ChildItem -Path CERT: -Recurse |
      Where { $PSItem.HasPrivateKey -eq $False } | Select-Object -Property FriendlyName,Issuer | fl

- [ ] Run:
      Get-ChildItem -Path CERT: -Recurse |
      Where { $PSItem.HasPrivateKey -eq $False -and $PSItem.NotAfter -gt (Get-Date) -and $PSItem.NotBefore -lt (Get-Date) } |
      Select-Object -Property NotBefore,NotAfter,FriendlyName,Issuer | ft -wrap

## TASK 4 — REPORT: DISK VOLUMES RUNNING LOW ON SPACE

- [ ] In the Administrator: Windows PowerShell window, run:
      Get-Volume
- [ ] Note: if you didn’t know the command, you could run `Help *volume*`
- [ ] Run:
      Get-Volume | Get-Member
- [ ] Note: identify **SizeRemaining**
- [ ] Run:
      Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 } | fl
- [ ] Run:
      Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .99 } |
      Select-Object DriveLetter, @{n='Size';e={'{0:N2}' -f ($PSItem.Size/1MB)}}
- [ ] Run:
      Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .1 }
- [ ] Note: this might return no output if every volume has >10% free space

## TASK 5 — REPORT: SPECIFIED CONTROL PANEL ITEMS

- [ ] In the Administrator: Windows PowerShell window, run:
      help *control*
- [ ] Note: identify **Get-ControlPanelItem**
- [ ] Run:
      Get-ControlPanelItem
- [ ] Run:
      Get-ControlPanelItem -Category 'System and Security' | Sort Name
- [ ] Note: you don’t have to use Where-Object for this
- [ ] Run:
      Get-ControlPanelItem -Category 'System and Security' |
      Where-Object -FilterScript {-not ($PSItem.Category -notlike '*System and Security*')} |
      Sort Name

## EXERCISE 2 — COMPLETION CHECK
- [ ] You produced filtered lists/reports that include only the specified data
- [ ] Continue when ready

# STOP POINT
Reply **NEXT** and I will continue with:
“Using PowerShell pipeline” → Exercise 1: Enumerating objects (Task 1–3)


# MODE
Checkbox mode

# EXERCISE 2 — CONVERTING OBJECTS

## TASK 1 — UPDATE ACTIVE DIRECTORY USER INFORMATION

- [ ] Sign in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator** (password: **Pa55w.rd**)
- [ ] Click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**

### NOTE (IMPORTANT)
- [ ] Long commands may WRAP on screen — type each command as ONE LINE and press **Enter only after the full command** is typed.

### Query IT users in London (show Name/Department/City)
- [ ] Run (single line):
      Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Select-Object -Property Name,Department,City | Sort Name

### Set Office for those users
- [ ] Run (single line):
      Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Set-ADUser -Office 'LON-A/1000'

### Verify Office was set (show Name/Department/City/Office)
- [ ] Run (single line):
      Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Select-Object -Property Name,Department,City,Office | Sort Name


## TASK 2 — GENERATE FILES LISTING AD USERS IN THE IT DEPARTMENT

### Review ConvertTo-Html help
- [ ] Run:
      help ConvertTo-Html -ShowWindow
- [ ] Close the **ConvertTo-Html** help window

### Create HTML report (UserReport.html)
- [ ] Run (enter as ONE LINE; wrapping is OK):
      Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Sort Name | Select-Object -Property Name,Department,City,Office | ConvertTo-Html -Property Name,Department,City -PreContent Users | Out-File E:\UserReport.html

### Open / review HTML report
- [ ] Run:
      Invoke-Expression E:\UserReport.html
- [ ] Review the report in the browser window (if it opens)

### Export CLIXML report (UserReport.xml)
- [ ] Run (enter as ONE LINE; wrapping is OK):
      Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Sort Name | Select-Object -Property Name,Department,City,Office | Export-Clixml E:\UserReport.xml
- [ ] Review the report displayed on the web browser page (if it opens) and close the browser window

### Export CSV report (UserReport.csv)
- [ ] Run (enter as ONE LINE; wrapping is OK):
      Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | Sort Name | Select-Object -Property Name,Department,City,Office | Export-Csv E:\UserReport.csv

### Open CSV in Notepad
- [ ] Open **File Explorer**
- [ ] Browse to **E:\**
- [ ] Right-click **UserReport.csv**
- [ ] Click **Open with**
- [ ] Select **Notepad**
- [ ] Review file contents in Notepad

## EXERCISE 2 — COMPLETION CHECK
- [ ] You queried AD users and updated **Office** for IT users in London
- [ ] You generated:
      - [ ] E:\UserReport.html
      - [ ] E:\UserReport.xml
      - [ ] E:\UserReport.csv

- [ ] Click **Next** to advance to the next lab when instructed


# NEXT LAB — Using PSProviders and PSDrives with PowerShell (≈30 minutes)

## EXERCISE 1 — CREATING FILES AND FOLDERS ON A REMOTE COMPUTER

### TASK 1 — CREATE A NEW FOLDER ON A REMOTE COMPUTER
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      Get-Help New-Item -ShowWindow
- [ ] In the help output, note **-Name** and **-ItemType** + review examples
- [ ] Close the **New-Item** help window
- [ ] Run:
      New-Item -Path \\Lon-Svr1\C$\ -Name ScriptShare -ItemType Directory

### TASK 2 — CREATE A NEW PSDrive MAPPING TO THE REMOTE FILE FOLDER
- [ ] Run:
      Get-Help New-PSDrive -ShowWindow
- [ ] Review: **-Name**, **-Root**, **-PSProvider** + examples
- [ ] Close the **New-PSDrive** help window
- [ ] Run:
      New-PSDrive -Name ScriptShare -Root \\Lon-Svr1\c$\ScriptShare -PSProvider FileSystem

### TASK 3 — CREATE A FILE ON THE MAPPED DRIVE
- [ ] Run:
      Get-Help Set-Location -ShowWindow
- [ ] Review help and close the **Set-Location** help window
- [ ] Run:
      Set-Location ScriptShare:
- [ ] Run:
      New-Item script.txt
- [ ] Run:
      Get-ChildItem
- [ ] Verify **script.txt** is listed


## EXERCISE 2 — CREATING A REGISTRY KEY FOR FUTURE SCRIPTS

### TASK 1 — CREATE THE REGISTRY KEY
- [ ] Run:
      Get-ChildItem -Path HKCU:\Software
- [ ] Run:
      New-Item -Path HKCU:\Software -Name Scripts

### TASK 2 — CREATE A REGISTRY VALUE (PSDriveName)
- [ ] Run:
      Set-Location HKCU:\Software\Scripts
- [ ] Run:
      New-ItemProperty -Path HKCU:\Software\Scripts -Name "PSDriveName" -Value "ScriptShare"
- [ ] Run:
      Get-ItemProperty . -Name PSDriveName


## EXERCISE 3 — CREATING A NEW ACTIVE DIRECTORY GROUP

### TASK 1 — CREATE A PSDrive MAPPING TO THE USERS CONTAINER IN AD DS
- [ ] Run:
      Import-Module ActiveDirectory
- [ ] Run:
      New-PSDrive -Name AdatumUsers -Root "CN=Users,DC=Adatum,DC=com" -PSProvider ActiveDirectory
- [ ] Run:
      Set-Location AdatumUsers:

### TASK 2 — CREATE THE LONDON DEVELOPERS GROUP
- [ ] Run:
      New-Item -ItemType group -Path . -Name "CN=London Developers"
- [ ] Run:
      Get-ChildItem
- [ ] Verify **London Developers** is listed

- [ ] Click **Next** to advance to the next lab when instructed


# NEXT LAB — Querying Information by Using WMI and CIM (≈45 minutes)

## EXERCISE 1 — QUERYING INFORMATION BY USING WMI

### TASK 1 — QUERY IP ADDRESSES
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*configuration*' | Sort Name
- [ ] Notice **Win32_NetworkAdapterConfiguration**
- [ ] Run:
      Get-WmiObject -Class Win32_NetworkAdapterConfiguration | Where DHCPEnabled -eq $False | Select IPAddress
- [ ] (Optional) Pipe the first command to `Get-Member` to review available properties

### TASK 2 — QUERY OS VERSION INFORMATION
- [ ] Run:
      Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*operating*' | Sort Name
- [ ] Notice **Win32_OperatingSystem**
- [ ] Run:
      Get-WmiObject -Class Win32_OperatingSystem | Get-Member
- [ ] Notice **Version**, **ServicePackMajorVersion**, **BuildNumber**
- [ ] Run:
      Get-WmiObject -Class Win32_OperatingSystem | Select Version,ServicePackMajorVersion,BuildNumber

### TASK 3 — QUERY COMPUTER SYSTEM HARDWARE INFORMATION
- [ ] Run:
      Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*system*' | Sort Name
- [ ] Notice **Win32_ComputerSystem**
- [ ] Run:
      Get-WmiObject -Class Win32_ComputerSystem | Format-List -Property *
- [ ] Run:
      Get-WmiObject -Class Win32_ComputerSystem | Select Manufacturer,Model,@{n='RAM';e={$PSItem.TotalPhysicalMemory}}

### TASK 4 — QUERY SERVICE INFORMATION
- [ ] Run:
      Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*service*' | Sort Name
- [ ] Notice **Win32_Service**
- [ ] Run:
      Get-WmiObject -Class Win32_Service | FL *
- [ ] Run:
      Get-WmiObject -Class Win32_Service -Filter "Name LIKE 'S%'" | Select Name,State,StartName
- [ ] Leave the PowerShell console open for the next exercise


## EXERCISE 2 — QUERYING INFORMATION BY USING CIM

### TASK 1 — QUERY USER ACCOUNTS
- [ ] Run:
      Get-CimClass -ClassName *user*
- [ ] Notice **Win32_UserAccount**
- [ ] Run:
      Get-CimInstance -Class Win32_UserAccount | Get-Member
- [ ] Run:
      Get-CimInstance -Class Win32_UserAccount | Format-Table -Property Caption,Domain,SID,FullName,Name
- [ ] Notice the list includes domain + local accounts

### TASK 2 — QUERY BIOS INFORMATION
- [ ] Run:
      Get-CimClass -ClassName *bios*
- [ ] Notice **Win32_BIOS**
- [ ] Run:
      Get-CimInstance -Class Win32_BIOS

### TASK 3 — QUERY NETWORK ADAPTER CONFIGURATION INFORMATION
- [ ] Run:
      Get-CimInstance -Classname Win32_NetworkAdapterConfiguration
- [ ] Run:
      Get-CimInstance -Classname Win32_NetworkAdapterConfiguration -ComputerName LON-DC1

### TASK 4 — QUERY USER GROUP INFORMATION
- [ ] Run:
      Get-CimClass -ClassName *group*
- [ ] Notice **Win32_Group**
- [ ] Run:
      Get-CimInstance -ClassName Win32_Group -ComputerName LON-DC1
- [ ] Leave the PowerShell console open for the next exercise


## EXERCISE 3 — INVOKING METHODS

### TASK 1 — INVOKE A CIM METHOD (REBOOT REMOTE OS)
- [ ] Run:
      Invoke-CimMethod -ClassName Win32_OperatingSystem -ComputerName LON-DC1 -MethodName Reboot
- [ ] Notice response includes **ReturnValue=0** and **PSComputerName=LON-DC1**
- [ ] Switch to **AZ-040T00A-LON-DC1** and observe restart
- [ ] After restart completes, switch back to **AZ-040T00A-LON-CL1**

### TASK 2 — INVOKE A WMI METHOD (CHANGE WINRM START MODE)
- [ ] Run:
      Get-Service WinRM | FL *
- [ ] Note: **StartType** is **Manual**
- [ ] Run:
      Get-WmiObject -Class Win32_Service -Filter "Name='WinRM'" | Invoke-WmiMethod -Name ChangeStartMode -Argument 'Automatic'
- [ ] Run:
      Get-Service WinRM | FL *
- [ ] Note: **StartType** is now **Automatic**

- [ ] Click **Next** to advance to the next lab when instructed


# NEXT LAB — Using Variables, Arrays, and Hash Tables in PowerShell (≈45 minutes)

## EXERCISE 1 — WORKING WITH VARIABLE TYPES

### TASK 1 — USE STRING VARIABLES
- [ ] On **AZ-040T00A-LON-CL1**, click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      $logPath = "C:\Logs\"
- [ ] Run:
      $logPath.GetType()
- [ ] Run:
      $logPath | Get-Member
- [ ] Run:
      $logFile = "log.txt"
- [ ] Run:
      $logPath += $logFile
- [ ] Run:
      $logPath
- [ ] Run:
      $logPath.Replace("C:","D:")
- [ ] Run:
      $logPath = $logPath.Replace("C:","D:")
- [ ] Run:
      $logPath
- [ ] Leave PowerShell open for next task

### TASK 2 — USE DATETIME VARIABLES
- [ ] Run:
      $today = Get-Date
- [ ] Run:
      $today.GetType()
- [ ] Run:
      $today | Get-Member
- [ ] Run:
      $logFile = [string]$today.Year + "-" + $today.Month + "-" + $today.Day + "-" + $today.Hour + "-" + $today.Minute + ".txt"
- [ ] Run:
      $cutOffDate = $today.AddDays(-30)
- [ ] Run:
      Get-ADUser -Properties LastLogonDate -Filter {LastLogonDate -gt $cutOffDate}
- [ ] Leave PowerShell open for next exercise


## EXERCISE 2 — USING ARRAYS

### TASK 1 — USE AN ARRAY TO UPDATE DEPARTMENT FOR USERS
- [ ] Run:
      $mktgUsers = Get-ADUser -Filter {Department -eq "Marketing"} -Properties Department
- [ ] Run:
      $mktgUsers.count
- [ ] Run:
      $mktgUsers[0]
- [ ] Run:
      $mktgUsers | Set-ADUser -Department "Business Development"
- [ ] Run:
      $mktgUsers | Format-Table Name,Department
- [ ] Verify: Department values in $mktgUsers variable haven’t changed
- [ ] Run:
      Get-ADUser -Filter {Department -eq "Marketing"}
- [ ] Run:
      Get-ADUser -Filter {Department -eq "Business Development"}
- [ ] Leave PowerShell open for next task

### TASK 2 — USE AN ARRAY LIST
- [ ] Run:
      [System.Collections.ArrayList]$computers="LON-SRV1","LON-SRV2","LON-DC1"
- [ ] Run:
      $computers.IsFixedSize
- [ ] Run:
      $computers.Add("LON-DC2")
- [ ] Run:
      $computers.Remove("LON-SRV2")
- [ ] Run:
      $computers


## EXERCISE 3 — USING HASH TABLES

### TASK 1 — USE A HASH TABLE
- [ ] Run:
      $mailList=@{"Frank"="Frank@fabriakm.com";"Libby"="LHayward@contso.com";"Matej"="MSTaojanov@tailspintoys.com"}
- [ ] Run:
      $mailList
- [ ] Run:
      $mailList.Libby
- [ ] Run:
      $mailList.Libby="Libby.Hayward@contoso.com"
- [ ] Run:
      $mailList.Add("Stela","Stela.Sahiti")
- [ ] Run:
      $mailList.Remove("Frank")
- [ ] Run:
      $mailList
- [ ] Close the Windows PowerShell prompt

- [ ] Click **Next** to advance to the next lab when instructed


# NEXT LAB — Using Scripts with PowerShell (≈150 minutes)

## EXERCISE 1 — SIGNING A SCRIPT

### TASK 1 — INSTALL A CODE SIGNING CERTIFICATE (MMC)
- [ ] Click **Start**
- [ ] Type **mmc.exe**
- [ ] Click **mmc.exe**
- [ ] In MMC: click **File**
- [ ] Click **Add/Remove Snap-in**
- [ ] Select **Certificates**
- [ ] Click **Add**
- [ ] Select **My user account**
- [ ] Click **Finish**
- [ ] Click **OK**
- [ ] Expand **Certificates - Current User**
- [ ] Select **Personal**
- [ ] Right-click **Personal**
- [ ] Hover **All Tasks**
- [ ] Click **Request New Certificate**
- [ ] In wizard: **Before You Begin** → click **Next**
- [ ] **Select Certificate Enrollment Policy** → select **Active Directory Enrollment Policy** → click **Next**
- [ ] **Request Certificates** → check **Adatum Code Signing** → click **Enroll**
- [ ] **Certificate Installation Results** → click **Finish**
- [ ] Expand **Personal**
- [ ] Click **Certificates**
- [ ] Verify code signing certificate is present
- [ ] Close MMC
- [ ] Select **No** when prompted to save console settings

### TASK 2 — DIGITALLY SIGN A SCRIPT (POWERSHELL)
- [ ] Click **Start**
- [ ] Type **powershell**
- [ ] Click **Windows PowerShell**
- [ ] Run:
      Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
- [ ] Run:
      $cert = Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
- [ ] Run:
      Set-Location E:\Mod07\Labfiles
- [ ] Run:
      Rename-Item HelloWorld.txt HelloWorld.ps1
- [ ] Run:
      Set-AuthenticodeSignature -FilePath HelloWorld.ps1 -Certificate $cert

### TASK 3 — SET EXECUTION POLICY + RUN SCRIPT
- [ ] Run:
      Set-ExecutionPolicy AllSigned
- [ ] When prompted, enter **Y** then press **Enter**
- [ ] Run:
      .\HelloWorld.ps1
- [ ] If prompted about untrusted publisher, enter **A** then press **Enter**
- [ ] Run:
      Set-ExecutionPolicy Unrestricted
- [ ] When prompted, enter **Y** then press **Enter**
- [ ] Close Windows PowerShell prompt


## EXERCISE 2 — PROCESSING AN ARRAY WITH A FOREACH LOOP

### TASK 1 — CREATE A TEST GROUP
- [ ] Click **Start**
- [ ] Type **powershell**
- [ ] Click **Windows PowerShell**
- [ ] Run:
      New-ADGroup -Name IPPhoneTest -GroupScope Universal -GroupCategory Security
- [ ] Run:
      Move-ADObject "CN=IPPhoneTest,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=IT,DC=Adatum,DC=com"
- [ ] Run:
      Add-ADGroupMember IPPhoneTest -Members Abbi,Ida,Parsa,Tonia

### TASK 2 — CREATE A SCRIPT TO CONFIGURE ipPhone ATTRIBUTE
- [ ] Locate script at:
      E:\Mod07\Labfiles\AZ-040_Mod07_Ex2_LAK.txt


## EXERCISE 3 — PROCESSING ITEMS USING IF STATEMENTS

### TASK 1 — CREATE services.txt WITH SERVICE NAMES
- [ ] Click **Start**
- [ ] Type **powershell**
- [ ] Click **Windows PowerShell**
- [ ] Run:
      Set-Location E:\Mod07\Labfiles
- [ ] Run:
      New-Item services.txt -ItemType File
- [ ] Run:
      Get-Service "Print Spooler" | Select -ExpandProperty Name | Out-File services.txt -Append
- [ ] Run:
      Get-Service "Windows Time" | Select -ExpandProperty Name | Out-File services.txt -Append

### TASK 2 — CREATE SCRIPT THAT STARTS STOPPED SERVICES
- [ ] Locate script at:
      E:\Mod07\Labfiles\AZ-040_Mod07_Ex3_LAK.txt


## EXERCISE 4 — CREATING USERS BASED ON A CSV FILE
- [ ] Script located at:
      E:\Mod07\Labfiles\AZ-040_Mod07_Ex4_LAK.txt

## EXERCISE 5 — QUERYING DISK INFO FROM REMOTE COMPUTERS
- [ ] Perform all steps on **AZ-040T00A-LON-CL1**
- [ ] Script located at:
      E:\Mod07\Labfiles\AZ-040_Mod07_Ex5_LAK.txt

## EXERCISE 6 — UPDATE SCRIPT TO USE ALTERNATE CREDENTIALS
- [ ] Script located at:
      E:\Mod07\Labfiles\AZ-040_Mod07_Ex6_LAK.txt

- [ ] Click **Next** to advance when instructed


# NEXT LAB — Performing Remote Administration with PowerShell (≈60 minutes)

## EXERCISE 1 — ENABLING REMOTING ON LOCAL COMPUTER

### TASK 1 — ENABLE REMOTING FOR INCOMING CONNECTIONS
- [ ] Confirm signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator** (password: **Pa55w.rd**)
- [ ] Click **Start**
- [ ] Type **powershell**
- [ ] Right-click **Windows PowerShell**
- [ ] Click **Run as administrator**
- [ ] Run:
      Set-ExecutionPolicy RemoteSigned
- [ ] Select **Yes** (or type **Y**) to confirm
- [ ] Run:
      Enable-PSremoting -SkipNetworkProfileCheck
- [ ] If prompted, answer **Y** to all prompts
- [ ] Run:
      help *sessionconfiguration*
- [ ] Note: identify **Get-PSSessionConfiguration**
- [ ] Run:
      Get-PSSessionConfiguration
- [ ] Verify **2 to 4** session configurations exist
- [ ] Leave PowerShell open


## EXERCISE 2 — PERFORMING ONE-TO-ONE REMOTING

### TASK 1 — CONNECT TO REMOTE COMPUTER + INSTALL FEATURE
- [ ] Confirm still signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**
- [ ] Run:
      Enter-PSSession -ComputerName LON-DC1
- [ ] After connected, run:
      Install-WindowsFeature NLB
- [ ] Wait for completion
- [ ] Disconnect by running:
      Exit-PSSession

### TASK 2 — TEST MULTI-HOP REMOTING
- [ ] Run:
      Enter-PSSession -ComputerName LON-DC1
- [ ] From inside that session, run:
      Enter-PSSession -ComputerName LON-CL1
- [ ] Expect a “second hop” style error (by design/default)
- [ ] Close the connection by running:
      Exit-PSSession

### TASK 3 — OBSERVE REMOTING LIMITATIONS (GUI APP)
- [ ] Confirm signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**
- [ ] Run:
      Enter-PSSession -ComputerName localhost
- [ ] Run:
      Notepad
- [ ] Observe: shell appears to “hang” (GUI cannot display over remoting)
- [ ] Press **Ctrl+C** to cancel
- [ ] Disconnect:
      Exit-PSSession


## EXERCISE 3 — PERFORMING ONE-TO-MANY REMOTING

### TASK 1 — GET PHYSICAL NETWORK ADAPTERS FROM TWO COMPUTERS
- [ ] Confirm signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**
- [ ] Run:
      help *adapter*
- [ ] Note: identify **Get-NetAdapter**
- [ ] Run:
      help Get-NetAdapter
- [ ] Note: identify **-Physical** parameter
- [ ] Run:
      Invoke-Command -ComputerName LON-CL1,LON-DC1 -ScriptBlock { Get-NetAdapter -Physical }

### TASK 2 — COMPARE LOCAL VS REMOTE OBJECT OUTPUT
- [ ] Run:
      Get-Process | Get-Member
- [ ] Run:
      Invoke-Command -ComputerName LON-DC1 -ScriptBlock { Get-Process } | Get-Member
- [ ] Note: remote objects are deserialized (fewer methods)


## EXERCISE 4 — USING IMPLICIT REMOTING

### TASK 1 — CREATE A PERSISTENT REMOTING CONNECTION
- [ ] Confirm signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**
- [ ] If PowerShell closed: open as admin again
- [ ] Run:
      $dc = New-PSSession -ComputerName LON-DC1
- [ ] Run:
      $dc
- [ ] Verify connection is available

### TASK 2 — IMPORT AND USE A MODULE FROM A SERVER
- [ ] Run:
      Get-Module -ListAvailable -PSSession $dc
- [ ] Run:
      Get-Module -ListAvailable -PSSession $dc | Where { $_.Name -Like '*share*' }
- [ ] Run:
      Import-Module -PSSession $dc -Name SMBShare -Prefix DC
- [ ] Run:
      Get-DCSMBShare
- [ ] Run:
      Get-SMBShare

### TASK 3 — CLOSE ALL OPEN REMOTING CONNECTIONS
- [ ] Run:
      Get-PSSession | Remove-PSSession
- [ ] Verify closed by running:
      Get-PSSession
- [ ] Confirm no sessions return


## EXERCISE 5 — MANAGING MULTIPLE COMPUTERS

### TASK 1 — CREATE PSSessions TO TWO COMPUTERS
- [ ] Confirm signed in to **AZ-040T00A-LON-CL1** as **Adatum\Administrator**
- [ ] Open PowerShell if not open
- [ ] Run:
      $computers = New-PSSession -ComputerName LON-CL1,LON-DC1
- [ ] Run:
      $computers
- [ ] Verify 2 connections show available

### TASK 2 — REPORT: FIREWALL RULES FROM TWO COMPUTERS
- [ ] Run:
      Get-Module *security* -ListAvailable
- [ ] Note **Net-Security** module
- [ ] Run:
      Invoke-Command -Session $computers -ScriptBlock { Import-Module NetSecurity }
- [ ] Run:
      Get-Command -Module NetSecurity
- [ ] (Optional) Run:
      Help Get-NetFirewallRule -ShowWindow
- [ ] Close help window
- [ ] Run:
      Invoke-Command -Session $computers -ScriptBlock { Get-NetFirewallRule -Enabled True } | Select Name,PSComputerName
- [ ] Unload module:
      Invoke-Command -Session $computers -ScriptBlock { Remove-Module NetSecurity }

### TASK 3 — HTML REPORT: LOCAL DISK INFO FROM TWO COMPUTERS
- [ ] Run:
      Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3"
- [ ] Run remotely:
      Invoke-Command -Session $computers -ScriptBlock { Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" }
- [ ] Note report must include: Computer name, Drive letter, Free space (bytes), Total size (bytes)
- [ ] Produce HTML output:
      Invoke-Command -Session $computers -ScriptBlock { Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" } | ConvertTo-Html -Property PSComputerName,DeviceID,FreeSpace,Size

### TASK 4 — CLOSE ALL OPEN PSSessions
- [ ] Run:
      Get-PSSession | Remove-PSSession

## END
- [ ] Click **End** to end the lab
