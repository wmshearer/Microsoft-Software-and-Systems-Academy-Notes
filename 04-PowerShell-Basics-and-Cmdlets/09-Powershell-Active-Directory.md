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


# 3. Add a new IP address to an interface (lab example)
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.30 -PrefixLength 16

# Remove an IP address (lab example)
Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.40


# Target-agnostic: discover adapter, then assign/remove IP
# Step 1: List adapters to find the correct InterfaceAlias
Get-NetAdapter

# Step 2: Assign an IP address
New-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress> -PrefixLength <PrefixBits>

# Step 3: Remove an IP address
Remove-NetIPAddress -InterfaceAlias "<AdapterName>" -IPAddress <IPAddress>
# Required info: adapter name, IP address, prefix length.


# 4. Set DNS server address (lab example)
Set-DnsClientServerAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11

# Target-agnostic: set one or more DNS servers
Set-DnsClientServerAddress -InterfaceAlias "<AdapterName>" -ServerAddresses <DNS_IP_1>,<DNS_IP_2>
# Required info: adapter name, DNS server IP(s).


# 5. Remove default route (lab example)
Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false

# Target-agnostic: remove default route
Remove-NetRoute -InterfaceAlias "<AdapterName>" -DestinationPrefix 0.0.0.0/0 -Confirm:$false
# Required info: adapter name; verify it's the correct default route.


# 6. Add a default route with next hop (lab example)
New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2

# Target-agnostic: add default route
New-NetRoute -InterfaceAlias "<AdapterName>" -DestinationPrefix 0.0.0.0/0 -NextHop <GatewayIP>
# Required info: adapter name, correct gateway IP.


# 7. Recheck current network configuration
Get-NetIPConfiguration


# 8. Retest connectivity
Test-Connection LON-DC1

# Target-agnostic: confirm access to key systems
Test-Connection <ComputerNameOrIP>

## 3. Active Directory User and Group Management
```
**Purpose:** Create groups and users, manage membership, and view or update user attributes.

**Common Scenarios**
- Creating new security or distribution groups  
- Adding users to existing groups  
- Viewing group memberships  
- Updating user address and contact information  
- Retrieving user attributes for documentation or audits  

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
# Active Directory User and Group Management
### Commands

```powershell
# 1. Create a new Active Directory group (lab example)
New-ADGroup -Name HelpDesk -Path "ou=IT,dc=Adatum,dc=com" -GroupScope Global

# Target-agnostic: create any AD group
New-ADGroup -Name <GroupName> -Path "<OU_DN>" -GroupScope <Global|DomainLocal|Universal>
# Required info: OU Distinguished Name, desired group scope.


# 2. Create a new AD user (lab example)
New-ADUser -Name "Jane Doe" -Department "IT"

# Target-agnostic
New-ADUser -Name "<UserDisplayName>" -Department "<Department>"
# Optional parameters: -SamAccountName, -UserPrincipalName, -AccountPassword, -Enabled $true.


# 3. Add users to a group (lab example)
Add-ADGroupMember "HelpDesk" -Members "Lara","Jane Doe"

# Target-agnostic
Add-ADGroupMember "<GroupName>" -Members <User1>,<User2>,<User3>
# Required info: exact group name and user identifiers.


# 4. View members of a group (lab example)
Get-ADGroupMember HelpDesk

# Target-agnostic
Get-ADGroupMember <GroupName>


# 5. Update a user’s address information (lab example)
Set-ADUser Lara -StreetAddress "1530 Nowhere Ave." -City "Winnipeg" -State "Manitoba" -Country "CA"

# Target-agnostic
Set-ADUser <UserName> -StreetAddress "<Address>" -City "<City>" -State "<StateOrProvince>" -Country "<CountryCode>"
# Required info: user name and correct address details.


# 6. View all groups a user belongs to (lab example)
Get-ADPrincipalGroupMembership "Jane Doe"

# Target-agnostic
Get-ADPrincipalGroupMembership <UserName>
# Displays all groups the specified user is a member of.


# 7. Retrieve specific user attributes (lab example)
Get-ADUser Lara -Properties StreetAddress,City,State,Country

# Target-agnostic
Get-ADUser <UserName> -Properties StreetAddress,City,State,Country
# Useful for verification after updating user details.
```