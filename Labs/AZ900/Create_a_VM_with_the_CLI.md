# Create a VM with the CLI

## Task 1 — Configure the Cloud Shell

- [ ] Sign in to the Azure portal:
      https://portal.azure.com

- [ ] Username:
      Az900User-57140328@cloudslice.onmicrosoft.com
- [ ] TAP:
      @#SVQ997

- [ ] Open **Azure Cloud Shell** (top-right icon)

- [ ] In the Welcome dialog:
      Select **Bash**

- [ ] When prompted:
      “You have no storage mounted” → Select **Advanced settings**

- [ ] Fill in the following:
      Resource Group: use default from dropdown  
      Storage Account: create new (globally unique name, e.g. cloudshellxyzstorage)  
      File Share: cloudshellfileshare  

- [ ] Select **Create Storage**

---

## Task 2 — Use CLI to Create a Virtual Machine

- [ ] Ensure **Bash** is selected in Cloud Shell

- [ ] Verify resource groups:

      az group list --output table

- [ ] Create the virtual machine:

      az vm create \
      --name myVMCLI \
      --resource-group myRGCLI-lod57140328 \
      --image Ubuntu2204 \
      --location EastUS2 \
      --admin-username azureuser \
      --admin-password Pa$$w0rd1234

      (Windows users would replace "\" with "^")

- [ ] Wait 2–3 minutes for deployment to complete

- [ ] Close Cloud Shell

- [ ] In Azure portal:
      Search → Virtual machines → Verify **myVMCLI** is running

---

## Task 3 — Execute Commands in the Cloud Shell

- [ ] Open **Azure Cloud Shell**
- [ ] Ensure **Bash** is selected

- [ ] Retrieve VM details:

      az vm show --resource-group myRGCLI-lod57140328 --name myVMCLI --show-details --output table

- [ ] Stop the VM:

      az vm stop --resource-group myRGCLI-lod57140328 --name myVMCLI

- [ ] Verify status:

      az vm show --resource-group myRGCLI-lod57140328 --name myVMCLI --show-details --output table

---

## Task 4 — Review Azure Advisor Recommendations

- [ ] In Azure portal:
      All services → Advisor

- [ ] Select:
      Overview  
      (Review categories: Reliability, Security, Performance, Cost)

- [ ] Select:
      All recommendations  
      Review suggested actions

- [ ] Note:
      Recommendations can be downloaded as CSV/PDF  
      Alerts can be created for recommendations  

---

## Optional Cleanup

- [ ] Search:
      Resource groups
- [ ] Select:
      myRGCLI-lod57140328 (or the group used in your lab)
- [ ] Click:
      Delete resource group
- [ ] Confirm name → Delete
- [ ] Monitor Notifications for progress

