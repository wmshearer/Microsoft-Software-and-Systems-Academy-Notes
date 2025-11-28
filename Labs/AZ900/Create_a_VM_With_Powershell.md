# Configure Cloud Shell & Deploy a VM with PowerShell

## Task 1 — Configure the Cloud Shell

- [ ] Sign in to the Azure portal:
      https://portal.azure.com

- [ ] Open **Azure Cloud Shell** (top-right of the Azure portal)
- [ ] When prompted to choose Bash or PowerShell:
      Select **PowerShell**

- [ ] On the Getting Started screen:
      Select **Mount storage account**
      Select your subscription → **Apply**

- [ ] Select:
      I want to create a storage account → Next

- [ ] Enter the following settings:
      Resource Group: myRGPS  
      Storage account: cloudshell57140232  
      File share: shellstorage  

- [ ] Select **Create Storage**

---

## Task 2 — Create a Resource Group and Virtual Machine

- [ ] Ensure **PowerShell** is selected in the Cloud Shell environment

- [ ] Verify resource groups:
      (run the command below)

      Get-AzResourceGroup | Format-Table

- [ ] Create a virtual machine using:

      New-AzVm `
      -ResourceGroupName "myRGPS" `
      -Name "myVMPS" `
      -Location "East US" `
      -VirtualNetworkName "myVnetPS" `
      -SubnetName "mySubnetPS" `
      -SecurityGroupName "myNSGPS" `
      -PublicIpAddressName "myPublicIpPS"

- [ ] When prompted, provide:
      Username: Azureuser  
      Password: Pa$$w0rd1234  

- [ ] After deployment completes, close the Cloud Shell pane

- [ ] In the Azure portal:
      Search → Virtual machines → Confirm **myVMPS** is running

- [ ] Open the VM and verify:
      Overview  
      Networking  

---

## Task 3 — Execute Commands in the Cloud Shell

- [ ] Open **Azure Cloud Shell** again
- [ ] Ensure **PowerShell** is selected

- [ ] Retrieve VM details:

      Get-AzVM -name myVMPS -status | Format-Table -autosize

- [ ] Stop the VM:

      Stop-AzVM -ResourceGroupName myRGPS -Name myVMPS

- [ ] Confirm **Yes** when prompted  
- [ ] Wait for **Succeeded**

- [ ] Verify VM state:

      Get-AzVM -name myVMPS -status | Format-Table -autosize

- [ ] Close Cloud Shell

---

## Task 4 — Review Azure Advisor Recommendations

- [ ] In the portal, open:
      All services → Advisor

- [ ] Select **Overview**
      (View categories: Reliability, Security, Performance, Cost)

- [ ] Select:
      All recommendations
- [ ] Review each recommendation and suggested action

- [ ] Note:
      You can download recommendations as CSV or PDF  
      You can also create alerts  

---

## Optional Cleanup

- [ ] Search:
      Resource groups
- [ ] Select:
      myRGPS
- [ ] Click **Delete resource group**
- [ ] Confirm name  
- [ ] Click **Delete**
- [ ] Monitor Notifications

