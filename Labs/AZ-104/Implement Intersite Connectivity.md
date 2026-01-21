LAB: Configure Connectivity Between Segmented Virtual Networks

LAB SCENARIO
Your organization separates core IT services (DNS, security, etc.) from manufacturing systems.
In some scenarios, communication is required between these segmented areas.
In this lab, you configure connectivity using virtual network peering, Network Watcher,
Azure PowerShell testing, and custom routes.

================================================================================

TASK 1: Create a core services virtual machine and virtual network

- [ ] Sign in to the Azure portal
  URL: `https://portal.azure.com`

- [ ] Open Virtual Machines
  Action: Search for "Virtual Machines"

- [ ] Create a new virtual machine
  Action: Select `Create` → `Virtual machine`

- [ ] Configure Basics tab
  Subscription: your subscription
  Resource group: `az104-rg5` (create new if needed)
  Virtual machine name: `CoreServicesVM`
  Region: `(US) East US`
  Availability options: `No infrastructure redundancy required`
  Security type: `Standard`
  Image: `Windows Server 2025 Datacenter - x64 Gen2`
  Size: `Standard_D2s_v3`
  Username: `localadmin`
  Password: provide a complex password
  Public inbound ports: `None`

- [ ] Continue to Disks tab
  Action: Accept defaults → `Next : Networking`

- [ ] Create a new virtual network
  Virtual network: `Create new`

- [ ] Configure virtual network
  Name: `CoreServicesVnet`
  Address range: `10.0.0.0/16`
  Subnet name: `Core`
  Subnet address range: `10.0.0.0/24`

- [ ] Select OK to save virtual network settings

- [ ] Monitoring tab
  Boot diagnostics: `Disable`

- [ ] Create the VM
  Action: `Review + create` → `Create`

NOTE:
You do not need to wait for deployment to complete before continuing.

================================================================================

TASK 2: Create a virtual machine in a different virtual network

- [ ] Open Virtual Machines
  Action: Azure portal → Virtual Machines

- [ ] Create a new virtual machine
  Action: `Create` → `Virtual machine`

- [ ] Configure Basics tab
  Subscription: your subscription
  Resource group: `az104-rg5`
  Virtual machine name: `ManufacturingVM`
  Region: `(US) East US`
  Availability options: `No infrastructure redundancy required`
  Security type: `Standard`
  Image: `Windows Server 2025 Datacenter - x64 Gen2`
  Size: `Standard_D2s_v3`
  Username: `localadmin`
  Password: provide a complex password
  Public inbound ports: `None`

- [ ] Disks tab
  Action: Accept defaults → `Next : Networking`

- [ ] Create a new virtual network
  Virtual network: `Create new`

- [ ] Configure Manufacturing virtual network
  Name: `ManufacturingVnet`
  Address range: `172.16.0.0/16`
  Subnet name: `Manufacturing`
  Subnet address range: `172.16.0.0/24`

- [ ] Monitoring tab
  Boot diagnostics: `Disable`

- [ ] Create the VM
  Action: `Review + create` → `Create`

================================================================================

TASK 3: Use Network Watcher to test VM connectivity

NOTE:
Ensure both virtual machines are fully deployed and running.

- [ ] Open Network Watcher
  Action: Search for "Network Watcher"

- [ ] Open Connection troubleshoot
  Path: Network diagnostic tools → Connection troubleshoot

- [ ] Configure connection troubleshoot
  Source type: `Virtual machine`
  Source VM: `CoreServicesVM`
  Destination type: `Virtual machine`
  Destination VM: `ManufacturingVM`
  Preferred IP version: `Both`
  Protocol: `TCP`
  Destination port: `3389`
  Source port: leave blank
  Diagnostic tests: defaults

- [ ] Run diagnostic test
  Action: Select `Run diagnostic tests`

EXPECTED RESULT:
Connectivity test shows `Unreachable`
Reason: VMs are in different virtual networks with no peering.

================================================================================

TASK 4: Configure virtual network peering

- [ ] Open CoreServicesVnet
  Path: Virtual networks → CoreServicesVnet

- [ ] Open Peerings
  Path: Settings → Peerings

- [ ] Add peering
  Action: `+ Add`

- [ ] Configure peering (Core → Manufacturing)
  Peering link name: `CoreServicesVnet-to-ManufacturingVnet`
  Virtual network: `ManufacturingVnet (az104-rg5)`
  Allow CoreServicesVnet to access ManufacturingVnet: selected
  Allow forwarded traffic: selected

- [ ] Configure reverse peering (Manufacturing → Core)
  Peering link name: `ManufacturingVnet-to-CoreServicesVnet`
  Allow ManufacturingVnet to access CoreServicesVnet: selected
  Allow forwarded traffic: selected

- [ ] Create peering
  Action: Select `Add`

- [ ] Verify peering status
  Ensure both peerings show `Connected`
  Refresh page if needed

================================================================================

TASK 5: Use Azure PowerShell to test VM connectivity

- [ ] Record CoreServicesVM private IP
  Path: CoreServicesVM → Overview → Networking → Private IP address

- [ ] Open ManufacturingVM
  Path: Virtual Machines → ManufacturingVM

- [ ] Run PowerShell command
  Path: Operations → Run command → RunPowerShellScript

- [ ] Execute test command
  Command:
  `Test-NetConnection <CoreServicesVM_Private_IP> -Port 3389`

EXPECTED RESULT:
Test succeeds
Reason: Virtual network peering enables connectivity

================================================================================

TASK 6: Create a custom route

- [ ] Add perimeter subnet
  Path: CoreServicesVnet → Subnets → `+ Subnet`

  Name: `perimeter`
  Address range: `10.0.1.0/24`
  Action: `Add`

- [ ] Create route table
  Path: Route tables → `+ Create`

  Resource group: `az104-rg5`
  Region: `East US`
  Name: `rt-CoreServices`
  Propagate gateway routes: `No`

- [ ] Open route table
  Action: Select `rt-CoreServices`

- [ ] Add route
  Path: Settings → Routes → `+ Add`

  Route name: `PerimetertoCore`
  Destination type: `IP Addresses`
  Destination IP addresses: `10.0.0.0/16`
  Next hop type: `Virtual appliance`
  Next hop address: `10.0.1.7`

- [ ] Associate route table to subnet
  Path: Subnets → `+ Associate`

  Virtual network: `CoreServicesVnet`
  Subnet: `Core`

NOTE:
This user-defined route forces traffic through a future Network Virtual Appliance (NVA).

================================================================================

CLEANUP (OPTIONAL)

- [ ] Delete resource group via portal
  Resource group: `az104-rg5`

- [ ] Azure PowerShell:
  `Remove-AzResourceGroup -Name az104-rg5`

- [ ] Azure CLI:
  `az group delete --name az104-rg5`

KEY TAKEAWAYS
- Virtual networks are isolated by default
- Virtual network peering enables private connectivity
- Traffic flows over Microsoft backbone
- User-defined routes override system routes
- Network Watcher helps diagnose connectivity issues
