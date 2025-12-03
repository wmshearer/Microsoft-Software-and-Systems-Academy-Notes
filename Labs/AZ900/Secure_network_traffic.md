# Secure Network Traffic

## Task 1 — Create a Virtual Machine

- [ ] Sign in to Azure portal:
      https://portal.azure.com

- [ ] Username:
      Az900User-57286179@cloudslice.onmicrosoft.com
- [ ] TAP:
      bExb2mGQ.

- [ ] From **All services**:
      Search → **Virtual machines**
      Select **+ Add / + Create / + Virtual machine**

- [ ] On **Basics** tab configure:
      Subscription: use default provided  
      Resource group: use default from dropdown  
      Virtual machine name: SimpleWinVM  
      Region: (US) East US  
      Image: Windows Server 2019 Datacenter Gen 2  
      Size: Standard D2s v3  
      Administrator username: azureuser  
      Administrator password: Pa$$w0rd1234  
      Inbound port rules: None  
      Security type: Standard  

- [ ] Go to **Networking** tab:
      NIC network security group: None  

- [ ] Go to **Management** tab:
      Boot diagnostics: Disable  

- [ ] Click **Review + create** → after validation → **Create**

- [ ] Monitor deployment until complete

- [ ] From deployment blade / Notifications:
      Select **Go to resource**

- [ ] On **SimpleWinVM** blade:
      Open **Networking**
      Confirm:
        - No NSG on NIC
        - No NSG on subnet
      Note:
        - Network interface name for use in next task


## Task 2 — Create a Network Security Group

- [ ] From **All services**:
      Search → **Network security groups**
      Select **+ Add / + Create**

- [ ] On **Basics** tab:
      Subscription: default  
      Resource group: default from dropdown  
      Name: myNSGSecure  
      Region: (US) East US  

- [ ] Click **Review + create** → **Create**

- [ ] After deployment:
      Select **Go to resource**

- [ ] Under **Settings**:
      Select **Network interfaces**
      Select **Associate**

- [ ] Choose the NIC identified in Task 1
      (NIC attached to SimpleWinVM)


## Task 3 — Configure Inbound Rule to Allow RDP

- [ ] In Azure portal:
      Navigate to **SimpleWinVM** blade

- [ ] On **Overview**:
      Select **Connect**

- [ ] Try to connect via **RDP**:
        - Download RDP file
        - Attempt connection
      Expect:
        - Connection fails due to NSG blocking RDP
      Close error window

- [ ] On **SimpleWinVM** blade:
      Under **Settings** → **Networking**
      Confirm:
        - myNSGSecure (attached to NIC) only allows:
          - Virtual network traffic
          - Load balancer probes

- [ ] On **Inbound port rules** tab for myNSGSecure:
      Select **Add inbound port rule**

      Source: Any  
      Source port ranges: *  
      Destination: Any  
      Destination port ranges: 3389  
      Protocol: TCP  
      Action: Allow  
      Priority: 300  
      Name: AllowRDP  

- [ ] Select **Add**
- [ ] Wait for rule to provision

- [ ] Return to **Connect → RDP**
      Download / open RDP file again
      Log on with:
        Username: azureuser  
        Password: Pa$$w0rd1234  

- [ ] Confirm RDP connection now succeeds


## Task 4 — Configure Outbound Rule to Deny Internet Access

- [ ] In the RDP session to **SimpleWinVM**:
      Open **Internet Explorer**

- [ ] Browse to:
      https://www.bing.com
      (Work through IE Enhanced Security prompts)
- [ ] Confirm site loads successfully
- [ ] Close Internet Explorer

- [ ] Back in Azure portal:
      Go to **SimpleWinVM** → **Networking**

- [ ] Select **Outbound port rules** tab
      Note existing default rule:
        - AllowInternetOutbound (cannot be removed)

- [ ] For NSG **myNSGSecure**:
      Select **Add outbound port rule**

      Source: Any  
      Source port ranges: *  
      Destination: Service Tag  
      Destination service tag: Internet  
      Destination port ranges: *  
      Protocol: TCP  
      Action: Deny  
      Priority: 4000  
      Name: DenyInternet  

- [ ] Click **Add**
- [ ] Wait for rule to provision

- [ ] In RDP session on SimpleWinVM:
      Open Internet Explorer
      Browse to:
        https://www.microsoft.com
      (Work through IE Enhanced Security prompts)
- [ ] Confirm page does **not** load (Internet blocked by NSG)


## Optional Cleanup

- [ ] In Azure portal:
      Search → **Resource groups**
- [ ] Select the resource group used for this lab
- [ ] Click **Delete resource group**
- [ ] Type the resource group name to confirm
- [ ] Click **Delete**
- [ ] Monitor **Notifications** until deletion completes
