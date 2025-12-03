# Implement Azure Key Vault

## Task 1 — Create an Azure Key Vault

- [ ] Sign in to the Azure portal:
      https://portal.azure.com

- [ ] Username:
      Az900User-57285766@cloudslice.onmicrosoft.com
- [ ] TAP:
      aTdsT2e5.

- [ ] From **All services**:
      Search → **Key vaults**
      Select **+ Add / + Create**

- [ ] Configure **Basics**:
      Subscription: use default supplied  
      Resource group: use default from dropdown  
      Key vault name: keyvaulttestxxxx (replace xxxx with unique letters/digits)  
      Location: East US  
      Pricing tier: Standard  

- [ ] Configure **Access configuration**:
      Permission model: **Vault access policy**

- [ ] Select **Review + create** → **Create**

- [ ] After deployment:
      Select **Go to resource** (or search for the new key vault)

- [ ] On **Overview** tab:
      Note the **Vault URI** (used by applications via REST APIs)

- [ ] Under **Settings**, review:
      Keys  
      Secrets  
      Certificates  
      Access policies  
      Firewalls and virtual networks  

- [ ] Note:
      Your Azure account is currently the only identity authorized;
      modify under **Access policies** if needed


## Task 2 — Add a Secret to the Key Vault

- [ ] In the key vault, under **Settings**:
      Select **Secrets**

- [ ] Select:
      **+ Generate/Import**

- [ ] Configure the secret:
      Upload options: Manual  
      Name: ExamplePassword  
      Value: hVFkk96  

- [ ] Select **Create**

- [ ] After creation:
      Select **ExamplePassword**

- [ ] Verify:
      Status = **Enabled**

- [ ] Note the **Secret Identifier** URL  
      (centrally managed, securely stored password reference)

- [ ] Select **Show Secret Value** to view the stored value


## Optional Cleanup

- [ ] To avoid extra costs:
      In Azure portal → search **Resource groups**
- [ ] Select the resource group used for this lab
- [ ] Click **Delete resource group**
- [ ] Type the resource group name to confirm
- [ ] Click **Delete**
- [ ] Monitor **Notifications** for delete progress
