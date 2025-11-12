# PowerShell Command Reference — Networking and Active Directory

## 1. Network Configuration and Connectivity

**Purpose:** Verify connectivity, inspect configuration, and configure IP, DNS, and routing.

**Common Scenarios**
- Testing connectivity to servers or domain controllers  
- Assigning or updating static IP addresses  
- Configuring DNS servers  
- Managing default routes and validating changes  

## AD Common Terms
## Key Active Directory Concepts

| Concept | Description |
|----------|-------------|
| **Active Directory (AD)** | Centralized directory service that manages users, computers, and policies in a domain. |
| **Objects** | The individual items stored in AD, such as users, groups, computers, printers, and organizational units. Each object has a unique set of attributes. |
| **Domain** | Logical grouping of objects within AD that share the same database and security policies. |
| **Organizational Unit (OU)** | Container for organizing users, groups, and computers within a domain. Helps delegate administrative control. |
| **Forest** | The top-level structure in AD that contains one or more domains sharing a common schema and global catalog. |
| **Tree** | A collection of one or more domains within a forest that share a contiguous namespace. |
| **Trusts** | Relationships that allow users from one domain to access resources in another domain. Can be one-way or two-way. |
| **Group Policy (GPO)** | A set of rules that control environment settings for users and computers. Used to enforce security and configuration policies. |
| **Domain Controller (DC)** | Server that authenticates users, stores the AD database, and enforces domain policies. |
| **DNS Integration** | AD relies on DNS for locating domain controllers and resolving domain names. Proper DNS configuration is critical for AD functionality. |
| **PowerShell AD Module** | Provides cmdlets for managing AD objects (users, groups, computers, OUs, etc.) from the command line. |
| **Schema** | Defines all object types and attributes that can exist in the directory. Acts as the blueprint for AD structure. |

---

**Tip:**  
Use `Get-Module ActiveDirectory` in PowerShell to verify that the **PowerShell AD module** is installed and loaded before running AD-related cmdlets.
## 1. Network Configuration and Connectivity
### Commands

```powershell
# 1. Test connectivity to a specific host (lab example)
Test-Connection LON-DC1

# Target-agnostic: test any host
Test-Connection <ComputerNameOrIP>
# Example:
# Test-Connection google.com


# 2. View current IP configuration
Get-NetIPConfiguration


# 3. List all IP addresses assigned to network interfaces
Get-NetIPAddress

# View specific IP details by interface
Get-NetIPAddress -InterfaceAlias "Ethernet"


# 4. Add a new IP address to an interface (lab example)
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.30 -PrefixLength 16 -DefaultGateway 172.16.0.1

# Target-agnostic: create a new IP configuration
New-NetIPAddress -IPAddress <IPAddress> -InterfaceAlias "<AdapterName>" -PrefixLength <PrefixLength> -DefaultGateway <GatewayIP>
# Required info: IP address, subnet prefix length, default gateway, and adapter name.


# 5. Modify an existing IP configuration (lab example)
Set-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.30 -PrefixLength 16 -DefaultGateway 172.16.0.1

# Target-agnostic: update IP settings
Set-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress> -PrefixLength <PrefixLength> -DefaultGateway <GatewayIP>
# Use this to change the subnet mask or gateway without removing the address.


# 6. Remove an IP address (lab example)
Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.40 -Confirm:$false

# Target-agnostic
Remove-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress> -Confirm:$false


# 7. Add a new static route (lab example)
New-NetRoute -DestinationPrefix 0.0.0.0/0 -InterfaceAlias Ethernet -NextHop 172.16.0.2 -RouteMetric 10

# Target-agnostic
New-NetRoute -DestinationPrefix <Network/Prefix> -InterfaceAlias "<AdapterName>" -NextHop <NextHopIP> -RouteMetric <Metric>
# Required info: destination prefix, interface alias, next hop, and optional metric.


# 8. Remove default route (lab example)
Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false

# Target-agnostic
Remove-NetRoute -InterfaceAlias "<AdapterName>" -DestinationPrefix 0.0.0.0/0 -Confirm:$false


# 9. View routing table entries
Get-NetRoute

# Find the best local route to a remote address
Find-NetRoute -RemoteIPAddress <IPAddress>


# 10. Configure DNS settings
# View DNS server addresses for an adapter
Get-DnsClientServerAddress -InterfaceAlias "Ethernet"

# Set DNS servers (lab example)
Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses 172.16.0.11,172.16.0.12

# Target-agnostic
Set-DnsClientServerAddress -InterfaceAlias "<AdapterName>" -ServerAddresses <DNS_IP_1>,<DNS_IP_2>


# 11. Set a connection-specific DNS suffix (lab example)
Set-DnsClient -InterfaceAlias Ethernet -ConnectionSpecificSuffix "adatum.com"

# Target-agnostic
Set-DnsClient -InterfaceAlias "<AdapterName>" -ConnectionSpecificSuffix "<DomainName>"
# Use this to define a DNS suffix for a specific interface.


# 12. Recheck network configuration
Get-NetIPConfiguration


# 13. Retest connectivity
Test-Connection LON-DC1

# Target-agnostic: confirm access to key systems
Test-Connection <ComputerNameOrIP>

# Active Directory Object Management

```powershell
# 1. Create a new contact object (lab example)
New-ADObject -Name JohnSmithcontact -Type contact -DisplayName "John Smith (Contoso.com)"

# Target-agnostic: create a contact object
New-ADObject -Name <ContactName> -Type contact -DisplayName "<DisplayName>"
# Optional: include -Path "<OU_DN>" to place the contact in a specific OU.


# 2. Retrieve all AD objects of type 'contact'
Get-ADObject -Filter "ObjectClass -eq 'contact'"
# Use to verify contact creation or list all contacts.


# 3. Update an object's description (lab example)
Set-ADObject -Identity "CN=Lara Raisic,OU=IT,DC=Adatum,DC=com" -Description "Member of support team"

# Target-agnostic: update any object's description
Set-ADObject -Identity "<DistinguishedName>" -Description "<DescriptionText>"

# Helper: find DistinguishedName (DN) for a user or group
Get-ADUser <UserName>   | Select DistinguishedName
Get-ADGroup <GroupName> | Select DistinguishedName


# 4. View a user's description property (lab example)
Get-ADUser Lara -Properties Description

# Target-agnostic
Get-ADUser <UserName> -Properties Description


# 5. Rename an Active Directory object (lab example)
Rename-ADObject -Identity "CN=Helpdesk,OU=IT,DC=Adatum,DC=com" -NewName SupportTeam

# Target-agnostic
Rename-ADObject -Identity "<DistinguishedName>" -NewName "<NewObjectName>"


# 6. Retrieve group details (lab example)
Get-ADGroup helpdesk

# Target-agnostic
Get-ADGroup <GroupName>
```

## 2. Managing Windows Firewall with PowerShell

**Purpose:** Manage Windows Firewall rules and profiles using PowerShell’s **NetSecurity** module.

**Common Scenarios**
- Enabling or disabling specific firewall rules  
- Creating new inbound or outbound rules for applications or ports  
- Viewing existing rules and firewall profiles  
- Exporting or modifying rule settings for troubleshooting  

---

### Commands

```powershell
# 1. View all existing firewall rules
Get-NetFirewallRule

# Filter by rule name or display group
Get-NetFirewallRule -DisplayName "*Remote*"
Get-NetFirewallRule -DisplayGroup "Remote Access"


# 2. Enable or disable a firewall rule (lab example)
Enable-NetFirewallRule -DisplayGroup "Remote Access"
Disable-NetFirewallRule -DisplayName "File and Printer Sharing (SMB-In)"

# Target-agnostic
Enable-NetFirewallRule -DisplayName "<RuleName>"
Disable-NetFirewallRule -DisplayName "<RuleName>"


# 3. Modify firewall rule properties
Set-NetFirewallRule -DisplayName "<RuleName>" -Enabled True -Action Allow
Set-NetFirewallRule -DisplayGroup "Remote Access" -Enabled False


# 4. Create a new firewall rule (lab example)
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow

# Target-agnostic: create inbound or outbound rules
New-NetFirewallRule -DisplayName "<RuleName>" -Direction <Inbound|Outbound> -Protocol <TCP|UDP> -LocalPort <PortNumber> -Action <Allow|Block>
# Required info: rule name, direction, protocol, port, and desired action.


# 5. Remove an existing rule
Remove-NetFirewallRule -DisplayName "Allow HTTP"

# Target-agnostic
Remove-NetFirewallRule -DisplayName "<RuleName>"


# 6. Duplicate (copy) an existing rule
Copy-NetFirewallRule -DisplayName "<ExistingRule>" -NewDisplayName "<CopiedRuleName>"


# 7. Rename an existing rule
Rename-NetFirewallRule -DisplayName "<OldRuleName>" -NewDisplayName "<NewRuleName>"


# 8. View firewall profiles (Domain, Private, Public)
Get-NetFirewallProfile

# Example: show all profiles and whether they’re enabled
Get-NetFirewallProfile | Select Name, Enabled


# 9. Configure firewall profile settings
# Disable the public firewall profile
Set-NetFirewallProfile -Profile Public -Enabled False

# Enable all profiles and ensure notifications are on
Set-NetFirewallProfile -All -Enabled True -NotifyOnListen True
```

# Active Directory User and Group Management

**Purpose:**  
Create, modify, and manage Active Directory users and groups.  
This includes creating new accounts, setting attributes, and managing group memberships from either the **user perspective** or the **group perspective**.

**Common Scenarios**
- Creating new security or distribution groups  
- Adding and removing members from groups  
- Viewing or modifying group properties  
- Viewing all groups a user belongs to  
- Updating user attributes like address or department  

---

### Commands

```powershell
# 1. Create a new Active Directory group (lab example)
New-ADGroup -Name HelpDesk -Path "ou=IT,dc=Adatum,dc=com" -GroupScope Global

# Target-agnostic: create any AD group
New-ADGroup -Name <GroupName> -Path "<OU_DN>" -GroupScope <Global|DomainLocal|Universal> -GroupCategory <Security|Distribution> -ManagedBy <ManagerAccount>
# Required info: OU Distinguished Name, group scope, and (optionally) category or manager.


# 2. Create a new AD user (lab example)
New-ADUser -Name "Jane Doe" -Department "IT" -Enabled $true

# Target-agnostic
New-ADUser -Name "<UserDisplayName>" -Department "<Department>" -Enabled $true
# Optional parameters: -SamAccountName, -UserPrincipalName, -AccountPassword, -Title, -EmailAddress, -Path.


# 3. Add users to a group (lab example)
Add-ADGroupMember "HelpDesk" -Members "Lara","Jane Doe"

# Target-agnostic
Add-ADGroupMember "<GroupName>" -Members <User1>,<User2>,<User3>
# Required info: exact group name and user identifiers.


# 4. Remove a user from a group
Remove-ADGroupMember -Identity "HelpDesk" -Members "Jane Doe" -Confirm:$false

# Target-agnostic
Remove-ADGroupMember -Identity "<GroupName>" -Members <UserName> -Confirm:$false


# 5. View members of a group (lab example)
Get-ADGroupMember HelpDesk

# Target-agnostic
Get-ADGroupMember <GroupName>


# 6. View all groups a user belongs to (lab example)
Get-ADPrincipalGroupMembership "Jane Doe"

# Target-agnostic
Get-ADPrincipalGroupMembership <UserName>
# Displays all groups the specified user or computer is a member of.


# 7. Add a user to multiple groups at once (user-focused)
Add-ADPrincipalGroupMembership -Identity "Lara" -MemberOf "HelpDesk","Sales"

# Target-agnostic
Add-ADPrincipalGroupMembership -Identity <UserName> -MemberOf <Group1>,<Group2>,<Group3>


# 8. Update a user’s address information (lab example)
Set-ADUser Lara -StreetAddress "1530 Nowhere Ave." -City "Winnipeg" -State "Manitoba" -Country "CA"

# Target-agnostic
Set-ADUser <UserName> -StreetAddress "<Address>" -City "<City>" -State "<StateOrProvince>" -Country "<CountryCode>"
# Required info: user name and correct address details.


# 9. Retrieve specific user attributes (lab example)
Get-ADUser Lara -Properties StreetAddress,City,State,Country

# Target-agnostic
Get-ADUser <UserName> -Properties StreetAddress,City,State,Country
# Useful for verification after updating user details.
```

## Notes
```powershell
*-ADGroupMember cmdlets modify group membership directly (focus on the group).

*-ADPrincipalGroupMembership cmdlets modify which groups a user/computer belongs to (focus on the member).
```
Use -Confirm:$false to skip confirmation prompts for batch operations.

Always verify your changes using:
```powershell
Get-ADGroupMember <GroupName>
Get-ADPrincipalGroupMembership <UserName>
```

When creating groups, remember:

- GroupScope: Defines where the group can be used (Global, DomainLocal, or Universal).

- GroupCategory: Defines its purpose (Security vs. Distribution).

- ManagedBy: Assigns a manager account to administer that group.

# Managing Groups and Memberships

**Purpose:**  
Manage existing Active Directory groups, configure group scope and category, delegate group ownership, and maintain membership consistency.

**Common Scenarios**
- Creating new global or domain local groups  
- Changing group scope or category (security vs. distribution)  
- Assigning group managers  
- Viewing or modifying group members  
- Managing multiple memberships efficiently  

---

### Commands

```powershell
# 1. Create a new Global security group (lab example)
New-ADGroup -Name FileServerAdmins -GroupScope Global -GroupCategory Security -Path "OU=IT,DC=Adatum,DC=com" -ManagedBy "Lara"

# Target-agnostic: create any AD group
New-ADGroup -Name <GroupName> -GroupScope <Global|DomainLocal|Universal> -GroupCategory <Security|Distribution> -Path "<OU_DN>" -ManagedBy "<UserOrGroup>"
# Required info: OU path, scope, category, and manager (optional).


# 2. Modify group properties (rename, description, manager)
Set-ADGroup -Identity "FileServerAdmins" -Description "Admins responsible for file server operations" -ManagedBy "Lara"

# Target-agnostic
Set-ADGroup -Identity "<GroupName>" -Description "<DescriptionText>" -ManagedBy "<ManagerName>"
# Used to delegate group management or update metadata.


# 3. Display group details
Get-ADGroup -Identity FileServerAdmins -Properties *

# Target-agnostic
Get-ADGroup -Identity "<GroupName>" -Properties *


# 4. Add new members to a group
Add-ADGroupMember -Identity FileServerAdmins -Members "Lara","Jane Doe"

# Target-agnostic
Add-ADGroupMember -Identity "<GroupName>" -Members <User1>,<User2>,<User3>


# 5. Remove members from a group
Remove-ADGroupMember -Identity FileServerAdmins -Members "Jane Doe" -Confirm:$false

# Target-agnostic
Remove-ADGroupMember -Identity "<GroupName>" -Members <UserName> -Confirm:$false


# 6. List all members of a group
Get-ADGroupMember -Identity FileServerAdmins

# Target-agnostic
Get-ADGroupMember -Identity "<GroupName>"


# 7. View which groups a specific user belongs to
Get-ADPrincipalGroupMembership -Identity "Lara"

# Target-agnostic
Get-ADPrincipalGroupMembership -Identity "<UserName>"


# 8. Add a user to multiple groups (user-focused)
Add-ADPrincipalGroupMembership -Identity "Lara" -MemberOf "HelpDesk","FileServerAdmins"

# Target-agnostic
Add-ADPrincipalGroupMembership -Identity "<UserName>" -MemberOf <Group1>,<Group2>,<Group3>


# 9. Delete a group
Remove-ADGroup -Identity FileServerAdmins -Confirm:$false

# Target-agnostic
Remove-ADGroup -Identity "<GroupName>" -Confirm:$false
```

## Notes
```powershell
GroupScope:
Determines where the group can be used and which objects can be members.

Global — Can include users/groups from the same domain and be assigned permissions anywhere in the forest.

DomainLocal — Can include users/groups from any domain but used only in its own domain.

Universal — Can include members from any domain and be used anywhere.

GroupCategory:
Defines the purpose of the group.

Security — Used to assign permissions.

Distribution — Used for email distribution lists only.

ManagedBy:
Assigns an owner for the group. That user can manage membership through tools like ADUC or scripts (depending on delegation).

Membership Management:

Use Add-ADGroupMember when working from the group perspective.

Use Add-ADPrincipalGroupMembership when working from the user perspective.

Verification Tips

# Verify group properties
Get-ADGroup -Identity "<GroupName>" -Properties GroupScope,GroupCategory,ManagedBy

# Verify membership
Get-ADGroupMember -Identity "<GroupName>"
Get-ADPrincipalGroupMembership -Identity "<UserName>"
```

# Managing Computer Accounts

**Purpose:**  
Create, modify, and manage computer accounts in Active Directory.  
These cmdlets help ensure computers are properly joined to the domain, located in the correct OU, and have valid trust relationships with domain controllers.

**Common Scenarios**
- Pre-staging computer accounts before deployment  
- Moving computers between organizational units  
- Resetting machine passwords or repairing domain trust  
- Viewing and updating computer properties  

---

### Commands

```powershell
# 1. Create a new computer account (lab example)
New-ADComputer -Name LON-CL10 -Path "OU=Marketing,DC=Adatum,DC=com" -Enabled $true

# Target-agnostic: create a computer object in a specific OU
New-ADComputer -Name <ComputerName> -Path "<OU_DN>" -Enabled $true
# Required info: computer name and OU Distinguished Name.


# 2. Retrieve a computer account’s properties
Get-ADComputer -Identity LON-CL10 -Properties *

# Target-agnostic
Get-ADComputer -Identity <ComputerName> -Properties *
# Use -Properties * to display all attributes of a computer object.


# 3. Modify computer properties (description, location, etc.)
Set-ADComputer -Identity LON-CL10 -Description "Marketing desktop" -Location "Floor 2, Winnipeg Office"

# Target-agnostic
Set-ADComputer -Identity <ComputerName> -Description "<Text>" -Location "<LocationInfo>"


# 4. Disable or enable a computer account
Disable-ADAccount -Identity LON-CL10
Enable-ADAccount -Identity LON-CL10

# Target-agnostic
Disable-ADAccount -Identity <ComputerName>
Enable-ADAccount -Identity <ComputerName>


# 5. Move a computer to a new OU
Move-ADObject -Identity "CN=LON-CL10,OU=Marketing,DC=Adatum,DC=com" -TargetPath "OU=Sales,DC=Adatum,DC=com"

# Target-agnostic
Move-ADObject -Identity "<DistinguishedName>" -TargetPath "<TargetOU_DN>"
# Required info: current and target Distinguished Names.


# 6. Verify or repair domain trust relationship (run locally)
Test-ComputerSecureChannel -Repair

# Target-agnostic
Test-ComputerSecureChannel -Repair
# Must be executed on the computer experiencing the trust issue.


# 7. Reset a computer account’s domain password
Reset-ComputerMachinePassword

# Target-agnostic
Reset-ComputerMachinePassword
# Run locally to re-establish secure channel with the domain controller.


# 8. Remove a computer account
Remove-ADComputer -Identity LON-CL10 -Confirm:$false

# Target-agnostic
Remove-ADComputer -Identity <ComputerName> -Confirm:$false
```

## Notes

```powershell
-Enabled $true ensures the account is active upon creation.

Test-ComputerSecureChannel is used to verify trust between a workstation and the domain — run it on the local machine, not remotely.

Reset-ComputerMachinePassword resets the secure channel password (similar to rejoining the domain).

Move-ADObject works on any AD object, including computers, when reorganizing OUs.
```

# Managing Organizational Units (OUs)

**Purpose:**  
Create, modify, and manage **Organizational Units (OUs)** in Active Directory.  
OUs are used to logically organize users, computers, and groups, making it easier to apply Group Policies and delegate administrative control.

**Common Scenarios**
- Creating new OUs for departments or locations  
- Protecting OUs from accidental deletion  
- Moving objects between OUs  
- Delegating administrative permissions  

---

### Commands

```powershell
# 1. Create a new OU (lab example)
New-ADOrganizationalUnit -Name Sales -Path "OU=Marketing,DC=Adatum,DC=com" -ProtectedFromAccidentalDeletion $true

# Target-agnostic: create an OU in a specific container
New-ADOrganizationalUnit -Name <OUName> -Path "<ParentOU_DN>" -ProtectedFromAccidentalDeletion <True|False>
# Required info: OU name and parent OU or domain path.


# 2. Retrieve OU details
Get-ADOrganizationalUnit -Identity "OU=Sales,OU=Marketing,DC=Adatum,DC=com" -Properties *

# Target-agnostic
Get-ADOrganizationalUnit -Identity "<OU_DN>" -Properties *
# Displays properties such as DistinguishedName, ProtectedFromAccidentalDeletion, and Linked GPOs.


# 3. List all OUs in the domain
Get-ADOrganizationalUnit -Filter *

# Target-agnostic
Get-ADOrganizationalUnit -Filter *
# Use filtering or the -SearchBase parameter to target specific containers.


# 4. Modify OU properties (rename, protection status)
Set-ADOrganizationalUnit -Identity "OU=Sales,OU=Marketing,DC=Adatum,DC=com" -Description "Sales Department OU" -ProtectedFromAccidentalDeletion $false

# Target-agnostic
Set-ADOrganizationalUnit -Identity "<OU_DN>" -Description "<Text>" -ProtectedFromAccidentalDeletion <True|False>


# 5. Rename an OU
Rename-ADObject -Identity "OU=Sales,OU=Marketing,DC=Adatum,DC=com" -NewName "SalesDept"

# Target-agnostic
Rename-ADObject -Identity "<OU_DN>" -NewName "<NewOUName>"
# The Rename-ADObject cmdlet works with OUs and other AD objects.


# 6. Move an OU to a different parent OU
Move-ADObject -Identity "OU=SalesDept,OU=Marketing,DC=Adatum,DC=com" -TargetPath "OU=Corporate,DC=Adatum,DC=com"

# Target-agnostic
Move-ADObject -Identity "<SourceOU_DN>" -TargetPath "<TargetParentOU_DN>"


# 7. Delete an OU (lab example)
Remove-ADOrganizationalUnit -Identity "OU=SalesDept,OU=Corporate,DC=Adatum,DC=com" -Confirm:$false

# Target-agnostic
Remove-ADOrganizationalUnit -Identity "<OU_DN>" -Confirm:$false
# To delete a protected OU, first set -ProtectedFromAccidentalDeletion $false.
```
## Notes

OUs are containers, not security principals — they cannot be assigned permissions directly like users or groups.

Use -ProtectedFromAccidentalDeletion $true to prevent unwanted deletions (enabled by default).

The Distinguished Name (DN) uniquely identifies the OU’s location within the directory.

Delegation of control (for managing users, computers, etc.) is typically performed via GUI tools like Active Directory Users and Computers, but OUs can still be manipulated through PowerShell.

### Verification Tips
```powershell
# Verify OU protection status and description
Get-ADOrganizationalUnit -Identity "<OU_DN>" -Properties Description,ProtectedFromAccidentalDeletion

# Verify OU hierarchy
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName
```

# Managing Generic Active Directory Objects

**Purpose:**  
Work with any Active Directory object type (users, groups, computers, contacts, printers, OUs, etc.).  
These cmdlets are flexible and allow you to perform bulk operations, move objects, or restore deleted ones from the AD Recycle Bin.

**Common Scenarios**
- Creating or modifying non-user AD objects (like contacts or shared folders)  
- Moving objects between OUs or containers  
- Restoring deleted items from the AD Recycle Bin  
- Synchronizing objects between domain controllers  

---

### Commands

```powershell
# 1. Create a new AD object (lab example)
New-ADObject -Name "AnaBowmanContact" -Type contact -Path "OU=Marketing,DC=Adatum,DC=com"

# Target-agnostic
New-ADObject -Name <ObjectName> -Type <ObjectType> -Path "<OU_DN>" -OtherAttributes @{AttributeName="Value"}
# Required info: object name, type (e.g., contact, container), and OU path.


# 2. Retrieve AD objects by class type
Get-ADObject -Filter "ObjectClass -eq 'contact'"

# Target-agnostic
Get-ADObject -Filter "ObjectClass -eq '<ObjectClass>'"
# Examples: 'user', 'computer', 'group', 'organizationalunit'.


# 3. Modify object attributes (e.g., description)
Set-ADObject -Identity "CN=Lara Raisic,OU=IT,DC=Adatum,DC=com" -Description "Support team member"

# Target-agnostic
Set-ADObject -Identity "<DistinguishedName>" -Description "<DescriptionText>"


# 4. Rename an existing AD object
Rename-ADObject -Identity "CN=HelpDesk,OU=IT,DC=Adatum,DC=com" -NewName "SupportTeam"

# Target-agnostic
Rename-ADObject -Identity "<DistinguishedName>" -NewName "<NewObjectName>"


# 5. Move an AD object to another OU
Move-ADObject -Identity "CN=Jane Doe,OU=IT,DC=Adatum,DC=com" -TargetPath "OU=Sales,DC=Adatum,DC=com"

# Target-agnostic
Move-ADObject -Identity "<DistinguishedName>" -TargetPath "<TargetOU_DN>"
# Commonly used for reorganizing user or computer objects.


# 6. Restore a deleted AD object (from Recycle Bin)
Restore-ADObject -Identity "GUID-of-deleted-object"

# Target-agnostic
Restore-ADObject -Identity <ObjectGUID>
# Requires that the AD Recycle Bin feature is enabled in your domain.


# 7. Synchronize an AD object between domain controllers
Sync-ADObject -Identity "CN=Lara Raisic,OU=IT,DC=Adatum,DC=com" -Source DC1 -Destination DC2

# Target-agnostic
Sync-ADObject -Identity "<DistinguishedName>" -Source <SourceDC> -Destination <TargetDC>
# Ensures recent changes replicate immediately between specific domain controllers.


# 8. Delete an AD object
Remove-ADObject -Identity "CN=OldContact,OU=Marketing,DC=Adatum,DC=com" -Confirm:$false

# Target-agnostic
Remove-ADObject -Identity "<DistinguishedName>" -Confirm:$false
```

## Notes

- *-ADObject cmdlets are universal — they work with any object type in Active Directory.

- These cmdlets are faster than type-specific cmdlets (like Set-ADUser) when you don’t need type filtering.

- The -OtherAttributes parameter lets you set multiple custom attributes in one command.

- Restore-ADObject only works if the Active Directory Recycle Bin is enabled in your forest.

- Sync-ADObject can be used to replicate critical updates immediately (for example, after password resets).
```powershell
# View a specific object's properties
Get-ADObject -Identity "<DistinguishedName>" -Properties *

# Check recently restored or moved items
Get-ADObject -Filter "whenChanged -gt (Get-Date).AddMinutes(-10)" | Select-Object Name,whenChanged

# Confirm object replication
Sync-ADObject -Identity "<DistinguishedName>" -Source <SourceDC> -Destination <TargetDC>
```
# 🧩 Module Assessment — Active Directory PowerShell

---

### **Question 1**
> You need to create a Global security group named `Group1`.  
> Which type of parameter will you use to specify a Global group when using the `New-ADGroup` PowerShell cmdlet?

**Answer:** ✅ `-GroupScope`

**Explanation:**  
The `-GroupScope` parameter defines **how far the group’s permissions extend** within Active Directory.  
Possible values:
- `DomainLocal`
- `Global`
- `Universal`

**Example:**
```powershell
New-ADGroup -Name "Group1" -GroupScope Global -GroupCategory Security
```
Question 2
```powershell
You need to use PowerShell to review the Department and Email Address for an AD user named User1.
How will you format the PowerShell command?
```
**Answer:** ✅ Get-ADUser -Identity User1 -Properties Department,EmailAddress

Explanation:
The -Identity parameter targets a specific user, and -Properties tells PowerShell to retrieve specific attributes that aren’t shown by default.

**Example:**
```powershell
Get-ADUser -Identity User1 -Properties Department,EmailAddress |
Select-Object Name, Department, EmailAddress


Output Example:

Name	Department	EmailAddress
User1	IT Support	user1@adatum.com
```