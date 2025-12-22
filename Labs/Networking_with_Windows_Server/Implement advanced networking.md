## LAB: CONFIGURE ADVANCED HYPER-V NETWORKING FEATURES

### LAB SCENARIO
Contoso Corporation has implemented Hyper-V to manage server and client virtualization initiatives. You must implement and test network connectivity for virtual machines (VMs).

------------------------------------------------------------

## EXERCISE 1: CREATE HYPER-V VIRTUAL SWITCHES AND CONFIGURE NIC TEAMING

### TASK 1: VERIFY CURRENT HYPER-V NETWORK CONFIGURATION
[ ] Sign in to LON-HV1 as Administrator / Pa55w.rd
[ ] Open Hyper-V Manager
[ ] Select Virtual Switch Manager
[ ] Select the switch named Private Network
[ ] Verify it connects to the Private network

------------------------------------------------------------

### TASK 2: CREATE VIRTUAL SWITCHES
[ ] Open Virtual Switch Manager
[ ] Select New virtual network switch → External → Create
[ ] Name: External Switch
[ ] Verify External network + Microsoft Hyper-V Network Adapter
[ ] Ensure Allow management OS to share adapter is checked
[ ] Select OK → Yes
[ ] Reopen Virtual Switch Manager and confirm External Switch exists
[ ] Create another switch → Internal → Create
[ ] Name: Internal Switch
[ ] Select OK
[ ] Close Virtual Switch Manager

------------------------------------------------------------

### TASK 3: CREATE AND CONNECT VIRTUAL NETWORK ADAPTERS
[ ] In Hyper-V Manager, right-click Server1 → Settings
[ ] Verify three existing network adapters
[ ] Connect all three adapters to External Switch
[ ] Select OK
[ ] Open Windows PowerShell ISE on LON-HV1
[ ] Run:

Add-VMNetworkAdapter -VMName Server1 -Name "External Network Adapter"
Connect-VMNetworkAdapter -VMName Server1 -Name "External Network Adapter" -SwitchName "External Switch"

------------------------------------------------------------

### TASK 4: USE THE HYPER-V VIRTUAL SWITCHES
[ ] In Hyper-V Manager → Server1 → Settings
[ ] Verify External Network Adapter uses External Switch
[ ] Start Server1
[ ] Sign in as Administrator / Pa55w.rd
[ ] Open Server Manager → Local Server
[ ] Open Ethernet 4 → Status → Details
[ ] Verify DHCP IP assigned from LON-DC1

------------------------------------------------------------

### TASK 5: ENABLE NIC TEAMING
[ ] In Server Manager → Local Server
[ ] Select NIC Teaming (Disabled)
[ ] Select Tasks → New Team
[ ] Team name: Server1 NIC Team
[ ] Select Ethernet, Ethernet2, Ethernet3, Ethernet4
[ ] Expand Additional properties
[ ] Set Ethernet4 as Standby adapter
[ ] Select OK
[ ] Wait for Status = OK
[ ] Refresh Local Server
[ ] Verify single Server1 NIC Team adapter exists

------------------------------------------------------------

## EXERCISE 2: CONFIGURE ADVANCED FEATURES OF A VIRTUAL SWITCH

### TASK 1: CONFIGURE AND TEST DHCP GUARD
[ ] On LON-HV1, attach Server2 to External Switch
[ ] Start Server2
[ ] Sign in as Administrator / Pa55w.rd
[ ] Install DHCP Server role
[ ] Create scope:
    - Name: Unauthorized Scope
    - Range: 172.16.0.200–172.16.0.210
    - Gateway: 172.16.0.1
    - Activate scope
[ ] Shut down Server2
[ ] On LON-HV1, open Windows PowerShell ISE
[ ] Run:

Set-VMNetworkAdapter -VMName Server2 -DhcpGuard On

[ ] Verify DHCP Guard enabled in Network Adapter → Advanced Features

------------------------------------------------------------

### TASK 2: VERIFY DHCP GUARD FUNCTIONALITY
[ ] Start Server1
[ ] Open Windows PowerShell ISE (Admin)
[ ] Run:

ipconfig /release
ipconfig /renew

[ ] Verify DHCP server remains LON-DC1

------------------------------------------------------------

### TASK 3: CONFIGURE HYPER-V SWITCH EMBEDDED TEAMING (SET)
[ ] On LON-HV1, open Windows PowerShell ISE
[ ] Run:

Get-NetAdapter
New-VMSwitch -Name SET1 -NetAdapterName "Ethernet 2","Ethernet 3"

[ ] Open Virtual Switch Manager
[ ] Verify SET1 exists
[ ] Attach Server2 to SET1

------------------------------------------------------------

### TASK 4: CONFIGURE BANDWIDTH MANAGEMENT
[ ] In Hyper-V Manager → Server1 → Settings
[ ] Select External Network Adapter
[ ] Enable Bandwidth management
[ ] Set Maximum bandwidth = 100
[ ] Select OK

------------------------------------------------------------

### RESULTS
You have successfully:
- Created Hyper-V virtual switches
- Configured virtual network adapters
- Enabled NIC Teaming
- Implemented DHCP Guard
- Configured SET
- Applied bandwidth management
