# Create a VM with a Template

## Task 1 — Explore the QuickStart Gallery and Locate a Template

- [ ] Sign in to the Azure portal:
      https://portal.azure.com

- [ ] Username:
      Az900User-57139651@cloudslice.onmicrosoft.com
- [ ] TAP:
      @lab.CloudPortalCredential(LabUser).AccessToken

- [ ] Open a new browser window and go to:
      https://azure.microsoft.com/en-us/resources/templates/?azure-portal=true
- [ ] Browse available Azure QuickStart templates

- [ ] Open another browser tab/window and go to:
      https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-simple-windows

- [ ] Scroll down and select:
      Deploy to Azure
- [ ] When redirected, sign in using provided credentials

- [ ] Select:
      Edit template
- [ ] Review JSON structure (parameters, variables)
- [ ] Locate VM name parameter
- [ ] Change the VM name to:
      myVMTemplate
- [ ] Save changes

- [ ] Configure deployment parameters:
      Subscription: keep default  
      Resource group: use default from dropdown  
      Region: keep default  
      VM Size: Standard_D2s_v3  
      Admin username: azureuser  
      Admin password: Pa$$w0rd1234  
      DNS label prefix: myvmtemplatexxxx  
      OS version: 2019-datacenter-gensecond  

- [ ] Click **Review + Create**
- [ ] Monitor the deployment


## Task 2 — Verify and Monitor Your Virtual Machine Deployment

- [ ] In Azure portal, open **All services**
- [ ] Search for and select:
      Virtual machines
- [ ] Confirm your new VM is present and running

- [ ] Select the VM
- [ ] In Overview pane, open:
      Monitoring
      (scroll down to view charts)

- [ ] Review available charts:
      CPU (average)  
      Network (total)  
      Disk bytes (total)

- [ ] Select any chart:
      Add metric
      Change chart type if desired

- [ ] Return to the Overview blade

- [ ] Select:
      Activity log
- [ ] Click:
      Add filter
- [ ] Experiment filtering by different event types & operations

## Optional Cleanup

- [ ] Search for:
      Resource groups
- [ ] Select the resource group used for the VM
- [ ] Click:
      Delete resource group
- [ ] Confirm resource group name
- [ ] Click Delete
- [ ] Monitor Notifications for deletion progress

