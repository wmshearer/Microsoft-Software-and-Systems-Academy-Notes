LAB: Azure Virtual Networks, NSG/ASG, and Azure DNS

DESCRIPTION
This lab guides you through creating Azure virtual networks using the portal and ARM templates,
securing traffic with Network Security Groups (NSGs) and Application Security Groups (ASGs),
and configuring both public and private Azure DNS zones.

================================================================================

EXERCISE 1: Create a virtual network with subnets using the portal (CoreServicesVnet)

- [ ] Sign in to the Azure portal
  URL: `https://portal.azure.com`

- [ ] Open Virtual Networks
  Action: Search for "Virtual Networks" and select it

- [ ] Create a new virtual network
  Action: Select `Create`

- [ ] Configure Basics tab
  Resource group: `az104-rg4` (create if needed)
  Name: `CoreServicesVnet`
  Region: `(US) East US`

- [ ] Configure IP address space
  IPv4 address space: `10.20.0.0/16`
  Action: Remove default entry and add this value

- [ ] Add subnet: SharedServicesSubnet
  Subnet name: `SharedServicesSubnet`
  Address range: `10.20.10.0/24`

- [ ] Add subnet: DatabaseSubnet
  Subnet name: `DatabaseSubnet`
  Address range: `10.20.20.0/24`

- [ ] Delete default subnet if present

- [ ] Create the virtual network
  Action: `Review + create` → `Create`

- [ ] Verify CoreServicesVnet
  Confirm address space and subnets are correct

- [ ] Export ARM template
  Path: CoreServicesVnet → Automation → Export template
  Action: Download and extract files locally
  Verify `template.json` exists

================================================================================

EXERCISE 2: Create a virtual network and subnets using a template (ManufacturingVnet)

- [ ] Open `template.json` in an editor

- [ ] Replace CoreServicesVnet with ManufacturingVnet
- [ ] Replace `10.20.0.0` with `10.30.0.0`

- [ ] Modify subnet values
  Replace `SharedServicesSubnet` → `SensorSubnet1`
  Replace `10.20.10.0/24` → `10.30.20.0/24`
  Replace `DatabaseSubnet` → `SensorSubnet2`
  Replace `10.20.20.0/24` → `10.30.21.0/24`

- [ ] Save `template.json`

- [ ] Edit `parameters.json`
  Replace `CoreServicesVnet` with `ManufacturingVnet`
  Save file

- [ ] Deploy custom template
  Portal path: Deploy a custom template → Build your own template
  Load modified `template.json`
  Load modified `parameters.json`
  Resource group: `az104-rg4`
  Action: `Review + create` → `Create`

- [ ] Verify ManufacturingVnet and subnets were created

================================================================================

EXERCISE 3: Configure communication between ASG and NSG

CREATE APPLICATION SECURITY GROUP

- [ ] Open Application Security Groups
- [ ] Create ASG
  Name: `asg-web`
  Resource group: `az104-rg4`
  Region: `East US`

CREATE NETWORK SECURITY GROUP

- [ ] Open Network Security Groups
- [ ] Create NSG
  Name: `myNSGSecure`
  Resource group: `az104-rg4`
  Region: `East US`

- [ ] Associate NSG with subnet
  VNet: `CoreServicesVnet`
  Subnet: `SharedServicesSubnet`

CONFIGURE INBOUND RULE (ALLOW ASG)

- [ ] Add inbound rule
  Source: Application Security Group
  ASG: `asg-web`
  Destination ports: `80,443`
  Protocol: `TCP`
  Action: Allow
  Priority: `100`
  Name: `AllowASG`

CONFIGURE OUTBOUND RULE (DENY INTERNET)

- [ ] Add outbound rule
  Destination: Service tag
  Service tag: `Internet`
  Protocol: Any
  Action: Deny
  Priority: `4096`
  Name: `DenyInternetOutbound`

================================================================================

EXERCISE 4: Configure public and private Azure DNS zones

PUBLIC DNS ZONE

- [ ] Create DNS zone
  Name: `contoso.com`
  Resource group: `az104-rg4`
  Region: `East US`

- [ ] Add A record
  Name: `www`
  Type: `A`
  TTL: `1`
  IP address: `10.1.1.4`

- [ ] Validate resolution
  Command:
  `nslookup www.contoso.com <name-server>`

PRIVATE DNS ZONE

- [ ] Create private DNS zone
  Name: `private.contoso.com`
  Resource group: `az104-rg4`
  Region: `East US`

- [ ] Link private DNS zone to ManufacturingVnet
  Link name: `manufacturing-link`
  Virtual network: `ManufacturingVnet`

- [ ] Add private A record
  Name: `sensorvm`
  Type: `A`
  TTL: `1`
  IP address: `10.1.1.4`

================================================================================

CLEANUP (OPTIONAL)

- [ ] Delete resource group
  Portal: Delete resource group → `az104-rg4`

- [ ] Azure PowerShell:
  `Remove-AzResourceGroup -Name az104-rg4`

- [ ] Azure CLI:
  `az group delete --name az104-rg4`
