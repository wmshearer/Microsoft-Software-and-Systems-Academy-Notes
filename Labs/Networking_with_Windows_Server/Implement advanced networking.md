## LAB: CONFIGURE ADVANCED HYPER-V NETWORKING FEATURES

### EXERCISE 1: CREATE HYPER-V VIRTUAL SWITCHES AND CONFIGURE NIC TEAMING

#### TASK 1: VERIFY CURRENT HYPER-V NETWORK CONFIGURATION
- [ ] Sign in to LON-HV1 as Administrator / Pa55w.rd
- [ ] Open Hyper-V Manager
- [ ] Open Virtual Switch Manager
- [ ] Select **Private Network**
- [ ] Verify it is connected to the Private network

#### TASK 2: CREATE VIRTUAL SWITCHES
- [ ] In Virtual Switch Manager → New virtual network switch
- [ ] Select **External** → Create Virtual Switch
- [ ] Name: External Switch
- [ ] Connection type: External network
- [ ] Adapter: Microsoft Hyper-V Network Adapter
- [ ] Enable: Allow management OS to share adapter
- [ ] Apply changes → Yes
- [ ] Reopen Virtual Switch Manager and verify External Switch

- [ ] Create new virtual network switch
- [ ] Select **Internal** → Create Virtual Switch
- [ ] Name: Internal Switch
- [ ] Verify Internal network selected
- [ ] Close Virtual Switch Manager

#### TASK 3: CREATE VIRTUAL NETWORK ADAPTERS
- [ ] In Hyper-V Manager → Server1 → Settings
- [ ] Verify 3 network adapters exist
- [ ] Connect all existing adapters to **External Switch**
- [ ] Select OK

- [ ] Open Windows PowerShell ISE on LON-HV1
- [ ] Run:
  
  Add-VMNetworkAdapter -VMName Server1 -Name "External Network Adapter"

- [ ] Run:
  
  Connect-VMNetworkAdapter -VMName Server1 -Name "External Network Adapter" -SwitchName "External Switch"

#### TASK 4: USE THE HYPER-V VIRTUAL SWITCHES
- [ ] Start and connect to Server1
- [ ] Sign in as Administrator / Pa55w.rd
- [ ] Open Server Manager → Local Server
- [ ] Verify Ethernet 4 shows IPv4 DHCP / IPv6 enabled
- [ ] Open Network Connections → Ethernet 4 → Status → Details
- [ ] Confirm IP assigned from DHCP on LON-DC1
- [ ] Leave Server Manager open

#### TASK 5: ENABLE NIC TEAMING
- [ ] In Server Manager → Local Server
- [ ] Click **NIC Teaming: Disabled**
- [ ] Create New Team
- [ ] Team name: Server1 NIC Team
- [ ] Select adapters:
  - [ ] Ethernet
  - [ ] Ethernet2
  - [ ] Ethernet3
  - [ ] Ethernet4
- [ ] Teaming Mode: Switch Independent
- [ ] Load Balancing: Address Hash
- [ ] Set Standby adapter: Ethernet4
- [ ] Confirm Status = OK
- [ ] Verify Ethernet4 is Standby
- [ ] Refresh Local Server page
- [ ] Confirm single NIC Team using DHCP
- [ ] Close Server1 VM window

---

### EXERCISE 2: CONFIGURE ADVANCED FEATURES OF A VIRTUAL SWITCH

#### TASK 1: CONFIGURE AND TEST DHCP GUARD
- [ ] On LON-HV1, attach Server2 to External Switch
- [ ] Start Server2
- [ ] Sign in as Administrator / Pa55w.rd
- [ ] Install DHCP Server role
- [ ] Create DHCP scope:
  - [ ] Name: Unauthorized Scope
  - [ ] Range: 172.16.0.200–172.16.0.210
  - [ ] Subnet Mask: 255.255.0.0
  - [ ] Gateway: 172.16.0.1
  - [ ] Activate scope
- [ ] Shut down Server2

- [ ] On LON-HV1, open Windows PowerShell ISE
- [ ] Run:
  
  Set-VMNetworkAdapter -VMName Server2 -DhcpGuard On

- [ ] Verify in Hyper-V Manager → Server2 → Network Adapter → Advanced Features
- [ ] Confirm DHCP Guard is enabled

- [ ] Start Server1
- [ ] Open PowerShell ISE (Admin)
- [ ] Run:
  
  IPConfig /release

- [ ] Run:
  
  IPConfig /renew

- [ ] Verify DHCP lease still comes from LON-DC1

#### TASK 2: CONFIGURE HYPER-V SWITCH EMBEDDED TEAMING (SET)
- [ ] On LON-HV1, open PowerShell ISE
- [ ] Run:
  
  Get-NetAdapter

- [ ] Identify Ethernet 2 and Ethernet 3
- [ ] Run:
  
  New-VMSwitch -Name SET1 -NetAdapterName "Ethernet 2","Ethernet 3"

- [ ] Open Virtual Switch Manager
- [ ] Verify SET1 exists
- [ ] Attach Server2 to SET1

#### TASK 3: CONFIGURE BANDWIDTH MANAGEMENT
- [ ] In Hyper-V Manager → Server1 → Settings
- [ ] Select External Network Adapter
- [ ] Enable Bandwidth Management
- [ ] Set Maximum bandwidth: 100
- [ ] Select OK

---

### RESULTS
- [ ] Hyper-V virtual switches configured
- [ ] NIC Teaming enabled inside VM
- [ ] DHCP Guard successfully blocking rogue DHCP
- [ ] Switch Embedded Teaming validated
- [ ] Bandwidth management applied
