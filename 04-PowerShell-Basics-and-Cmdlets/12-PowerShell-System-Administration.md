# PowerShell Command Reference — System Administration

This section focuses on managing **server roles**, **Hyper-V virtual machines**, and **Internet Information Services (IIS)** using PowerShell.  
Each area introduces key modules and cmdlets for automating typical server administration tasks.

---

## 1. Managing Server Roles and Features

**Purpose:**  
Install, remove, and list Windows Server roles, features, and services using the **ServerManager** PowerShell module.

**Common Scenarios**
- Checking installed or available server roles  
- Installing specific features (like Hyper-V or Web Server)  
- Removing unnecessary features  
- Automating configuration during server deployment  

---

### Commands

```powershell
# 1. List all installed and available roles and features (lab example)
Get-WindowsFeature

# Target-agnostic
Get-WindowsFeature | Select-Object DisplayName, Name, InstallState
# Displays whether each feature is Installed, Available, or Removed.


# 2. Install a specific Windows feature (lab example)
Install-WindowsFeature "nlb"

# Target-agnostic
Install-WindowsFeature -Name "<FeatureName>" -IncludeManagementTools
# Example:
# Install-WindowsFeature -Name "Web-Server" -IncludeManagementTools
# Required info: feature or role name as shown by Get-WindowsFeature.


# 3. Uninstall a Windows feature (lab example)
Uninstall-WindowsFeature -Name "Web-Server"

# Target-agnostic
Uninstall-WindowsFeature -Name "<FeatureName>"
# Removes the feature from the server; use -Remove to delete installation files as well.

```
## Notes

- The ServerManager module is available only on Windows Server systems.
- On client systems (like Windows 10/11), these cmdlets are not supported.
- To find feature names before installation, use:
```powershell
Get-WindowsFeature | Where-Object {$_.InstallState -eq "Available"}
```

- Adding -IncludeManagementTools ensures related admin tools (like MMC snap-ins) are installed.
- Combine this with remote management cmdlets (e.g., -ComputerName) to configure multiple servers at once.
```powershell
**Knowledge Check**

1️⃣ You need to identify which server roles and features are installed on LON-SVR1.  
✅ **Get-WindowsFeature**

2️⃣ Which PowerShell module supersedes the WebAdministration module for managing IIS websites?  
✅ **IISAdministration**

```
## Managing Windows 10 and File Permissions with PowerShell

**Purpose:**  
Use PowerShell to manage Windows 10 system settings, services, processes, logs, and file or folder permissions through the built-in management and security modules.

---

### Managing Windows 10 Settings (Microsoft.PowerShell.Management)

**Common Scenarios**
- Retrieve detailed computer information  
- Manage services and processes  
- Clear event logs and recycle bins  
- Restart or shut down computers  
- Automate workstation maintenance tasks  

---

### Commands

```powershell
# 1. List all cmdlets in the Microsoft.PowerShell.Management module
Get-Command -Module Microsoft.PowerShell.Management


# 2. Retrieve detailed system information
Get-ComputerInfo
# Displays hardware, OS, BIOS, and Windows build data.


# 3. Get the latest five Application log errors
Get-EventLog -LogName Application -Newest 5 -EntryType Error


# 4. Clear the Application event log
Clear-EventLog -LogName Application


# 5. Retrieve all running processes
Get-Process


# 6. Stop a specific process
Stop-Process -Name "notepad"


# 7. View all system services
Get-Service


# 8. Restart a specific service
Restart-Service -Name "wuauserv"
# Example: Restart Windows Update service.


# 9. Shut down or restart the computer
Stop-Computer -Force
Restart-Computer -Force


# 10. Empty the recycle bin
Clear-RecycleBin -Force

Notes

The Microsoft.PowerShell.Management module is built into PowerShell by default.

Get-EventLog is only available in Windows PowerShell 5.1 (not in PowerShell 7+).

For cross-platform event viewing, use Get-WinEvent.

Use -ComputerName with many cmdlets for remote administration.
```
# Managing File and Folder Permissions (Microsoft.PowerShell.Security)
## Common Scenarios

- Retrieve or modify file system permissions
- Copy ACLs between files or directories
- Add or remove user access rules
- Audit and verify security settings

```powershell
# 1. List all cmdlets in the Microsoft.PowerShell.Security module
Get-Command -Module Microsoft.PowerShell.Security


# 2. View security descriptor (ACL) for a folder
Get-Acl -Path "C:\Folder1" | Format-List


# 3. View detailed access permissions for a folder
(Get-Acl -Path "C:\Folder1").Access


# 4. Display specific access properties in a table
(Get-Acl -Path "C:\Folder1").Access |
Format-Table IdentityReference, FileSystemRights, AccessControlType, IsInherited


# 5. Create a new permission rule and apply it
$ACL = Get-Acl -Path "C:\Folder1"
$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule("User1","Modify","Allow")
$ACL.SetAccessRule($AccessRule)
$ACL | Set-Acl -Path "C:\Folder1"


# 6. Remove access permissions for a user (alternative example)
$ACL = Get-Acl -Path "C:\Folder1"
$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule("User1","Modify","Allow")
$ACL.RemoveAccessRule($AccessRule)
$ACL | Set-Acl -Path "C:\Folder1"


# 7. Copy the security descriptor from one folder to another
Get-Acl -Path "C:\Folder1" | Set-Acl -Path "C:\Folder2"
# After execution, Folder2 inherits identical permissions from Folder1.
```

# Notes
```powershell
- The Access Control List (ACL) defines which users and groups can access a resource and what level of access they have.
- FileSystemAccessRule objects are used to define permission entries for users or groups.
- Always verify permissions after modification using Get-Acl.
- Administrative privileges are required to modify ACLs on protected folders.
```

## 5. Connecting with Data Stores using PowerShell Providers

**Purpose:**  
PowerShell providers act as adapters that make different data stores (like the registry, environment variables, and file system) accessible as if they were file system drives.  
This allows you to use familiar commands such as `Get-ChildItem`, `Set-Item`, and `Remove-Item` to manage data across different technologies.

---

### Understanding PowerShell Providers

**Common Scenarios**
- Browsing registry keys as folders  
- Managing environment variables as items  
- Viewing PowerShell functions and variables in memory  
- Accessing files and folders using consistent syntax  

---

### Key Concepts

| Concept | Description |
|----------|-------------|
| **Provider** | An adapter that exposes a data store (like Registry or FileSystem) as a hierarchical structure you can navigate. |
| **Item** | Represents an individual element in the provider (e.g., a file, folder, or registry key). |
| **ItemProperty** | Represents a property of an item (e.g., a registry value). |
| **Dynamic Providers** | Providers that adapt to new technologies, such as IIS with third-party extensions. |
| **Drive (PSDrive)** | A logical representation of a provider location, such as `C:` (FileSystem) or `HKLM:` (Registry). |

---

### Common Built-in Providers

| Provider | Description |
|-----------|-------------|
| **Registry** | Access to registry keys and values (`HKLM:` and `HKCU:`). |
| **Alias** | Access to PowerShell aliases (like `dir`, `ls`, `cp`). |
| **Environment** | Access to environment variables. |
| **FileSystem** | Access to local drives, folders, and files. |
| **Function** | Access to PowerShell functions currently in memory. |
| **Variable** | Access to PowerShell variables in memory. |

---

### Commands

```powershell
# 1. List all loaded PowerShell providers
Get-PSProvider

# Displays providers such as FileSystem, Registry, Alias, Environment, Function, and Variable.


# 2. List available capabilities for each provider
Get-PSProvider | Select-Object Name,Capabilities

# Common capabilities include:
# - ShouldProcess: supports -WhatIf / -Confirm
# - Filter: supports filtering
# - Include / Exclude: supports wildcards
# - Credentials: supports alternate credentials
# - Transactions: supports transactional changes


# 3. View help information for a specific provider
Get-Help about_FileSystem_Provider

# Target-agnostic example:
Get-Help about_<ProviderName>_Provider
# Example: Get-Help about_Registry_Provider


# 4. List cmdlets that work with providers
Get-Command *-Item, *-ItemProperty

# Shows commands like:
# Get-Item, Set-Item, Remove-Item
# Get-ItemProperty, Set-ItemProperty, Clear-ItemProperty


# 5. Example: Explore registry keys as if browsing folders
Get-ChildItem HKLM:\Software

# Example: Access environment variables like files
Get-ChildItem Env:

```
# Notes

- Providers let you manage multiple data types with the same syntax used for file systems.
- New providers load automatically when related modules are imported (e.g., the ActiveDirectory provider loads with Import-Module ActiveDirectory).
- You can navigate within a provider using standard commands like Set-Location (cd) and Get-ChildItem (ls).
- Only the Registry provider supports transactions (-UseTransaction).

## 6. Using PowerShell Drives (PSDrives)

**Purpose:**  
PowerShell Drives (PSDrives) provide a consistent way to access different data stores—like file systems, registries, certificates, or aliases—as if they were file system drives.  
Each PSDrive is based on a PowerShell provider that defines how the data store is accessed and manipulated.

---

### Understanding PowerShell Drives

**Common Scenarios**
- Navigating between file systems and the registry with the same commands  
- Creating custom PSDrives for registry keys, folders, or network shares  
- Managing certificates or environment variables through the drive structure  
- Simplifying automation by treating all data stores as hierarchical paths  

---

### Default Drives Created at Startup

| Drive | Description |
|--------|--------------|
| **C:** | FileSystem drive for the local hard disk |
| **HKLM:** | Registry drive for HKEY_LOCAL_MACHINE |
| **HKCU:** | Registry drive for HKEY_CURRENT_USER |
| **Env:** | Environment variable store |
| **Variable:** | PowerShell variable store |
| **Function:** | PowerShell function store |
| **Alias:** | PowerShell alias store |
| **Cert:** | Certificate store for CurrentUser and LocalMachine |
| **WSMan:** | Web Services for Management configuration drive |

---

### Commands

```powershell
# 1. View all available PowerShell drives
Get-PSDrive

# Displays drives like C, HKLM, HKCU, Env, Cert, Alias, Function, and Variable.


# 2. Create a new PowerShell drive (lab example)
New-PSDrive -Name MyData -PSProvider FileSystem -Root "C:\Data"

# Target-agnostic: create a PSDrive mapped to any folder or registry path
New-PSDrive -Name "<DriveName>" -PSProvider "<ProviderName>" -Root "<Path>"
# Example:
# New-PSDrive -Name Logs -PSProvider FileSystem -Root "D:\ServerLogs"


# 3. Remove a custom PowerShell drive
Remove-PSDrive -Name MyData


# 4. Navigate to a PSDrive path
Set-Location HKLM:\SOFTWARE\Microsoft
# or use alias: cd HKLM:\SOFTWARE\Microsoft


# 5. List child items within a drive
Get-ChildItem Env:
Get-ChildItem HKCU:\Software


# 6. View properties of a drive
Get-PSDrive -Name Env | Format-List *


# 7. Explore provider capabilities for a drive
(Get-PSDrive -Name HKLM).Provider.Capabilities
```

## Working with PowerShell Drive Locations
### Command	Description
- Get-Location	Displays the current working location
- Set-Location	Changes the current working directory (alias: cd)
- Push-Location	Saves the current location to a stack (alias: pushd)
- Pop-Location	Returns to the previous saved location (alias: popd)
```powershell
# Example: Moving between file system and registry
Get-Location
Set-Location C:\Users
Push-Location HKLM:\SOFTWARE
Pop-Location
```

# Managing File System Data with PSDrives
```Powershell
# 1. View contents of a folder (alias for Get-ChildItem)
Dir

# 2. Create a new folder
New-Item -Path "C:\Lab" -ItemType Directory

# 3. Create a new text file
New-Item -Path "C:\Lab\Notes.txt" -ItemType File

# 4. Remove a file or folder
Remove-Item -Path "C:\Lab\Notes.txt" -Force

# 5. Remove a folder and its contents
Remove-Item -Path "C:\Lab" -Recurse -Force

# 6. Move or rename items
Move-Item -Path "C:\Lab\Notes.txt" -Destination "C:\Lab\Archive\Notes.txt"
Rename-Item -Path "C:\Lab\Notes.txt" -NewName "Notes-Old.txt"

# 7. Search recursively for items
Get-ChildItem -Path "C:\Lab" -Recurse -Filter "*.log"
```
# Managing the Registry with PSDrives
```powershell

# 1. View registry keys under a path
Get-ChildItem HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion

# 2. View registry values
Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

# 3. Retrieve a specific registry value
Get-ItemPropertyValue HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run -Name WindowsDefender

# 4. Create a new registry PSDrive
New-PSDrive -Name AppReg -PSProvider Registry -Root HKCU:\Software\AppData

# 5. Modify registry entries using -Type parameter
Set-ItemProperty -Path HKCU:\Software\AppData -Name "Version" -Value "1.0" -Type String

# 6. Transactions (Registry-only feature)
Start-Transaction
Set-ItemProperty -Path HKLM:\Software\Test -Name Setting -Value "Enabled" -UseTransaction
Complete-Transaction
```
# Managing Certificates with the Cert Drive

```powershell

# 1. List all certificates in the local machine store
Get-ChildItem Cert:\LocalMachine\My

# 2. View certificates expiring soon
Get-ChildItem Cert:\LocalMachine\My -ExpiringInDays 30

# 3. Retrieve SSL certificates by domain name
Get-ChildItem Cert:\LocalMachine\My -DnsName "*.contoso.com"

# 4. Create a new self-signed certificate
New-SelfSignedCertificate -DnsName "webapp.contoso.com" -CertStoreLocation "Cert:\LocalMachine\My"

# 5. Open the Certificates MMC from PowerShell
Invoke-Item Cert:

## 7. Passing Pipeline Objects and Parameters

**Purpose:**  
The PowerShell pipeline allows you to pass objects from one command to another — making automation clean and efficient.  
Understanding how PowerShell binds pipeline input (`ByValue` and `ByPropertyName`) helps you predict and control command behavior.

---

### Common Scenarios
- Automating multiple commands by piping output directly into input  
- Using `ByValue` and `ByPropertyName` to bind pipeline data correctly  
- Renaming or expanding properties to match parameter names  
- Using parenthetical commands when the pipeline can’t be used  

---

### 1. Pipeline Parameter Binding Overview

When data is piped from one command to another, PowerShell determines **which parameter** of the receiving command should accept the input.  
This is called **Pipeline Parameter Binding**.

Two binding methods:
- **ByValue** → Directly passes the object itself (type match).  
- **ByPropertyName** → Passes matching property values (name match).  

Example:

```powershell
Get-ADUser -Filter {Name -eq 'Perry Brill'} | Set-ADUser -City Seattle
```
