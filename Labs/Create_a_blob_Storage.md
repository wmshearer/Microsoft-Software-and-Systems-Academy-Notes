# Create a Storage Account — Step-By-Step Checklist

## Task 1 — Create a Storage Account

- [ ] Sign in to **https://portal.azure.com**
- [ ] Go to **All services** → search **Storage accounts**
- [ ] Click **+ Add** → **+ Create** → **+ New**
- [ ] **Basics** tab:
  - Subscription: *Leave provided default*
  - Resource group: *Use default supplied*
  - Storage account name: **storageaccountxxxxx** (replace *xxxxx* with unique letters/numbers)
  - Location: **East US**
  - Performance: **Standard**
  - Redundancy: **Locally redundant storage (LRS)**
- [ ] Click **Review + Create**
- [ ] Ensure validation passes
- [ ] Click **Create**
- [ ] After deployment, return to **Home → Storage accounts**
- [ ] Confirm your new storage account appears in the list

---

## Task 2 — Work With Blob Storage

- [ ] Open your storage account
- [ ] Scroll to **Data storage** → click **Containers**
- [ ] Click **+ Container**
  - Name: **container1**
  - Public access level: **Private (no anonymous access)**
- [ ] Click **Create**
- [ ] Open a new browser tab → search Bing for an image of a **flower**
- [ ] Right-click → **Save image as** → save to your VM
- [ ] Back in Azure Portal → open **container1**
- [ ] Click **Upload**
- [ ] Browse → select the flower image → click **Upload**
- [ ] Expand **Advanced** options (review only, leave defaults)
- [ ] Verify file appears in the container
- [ ] Right-click the uploaded blob → review options:
  - View/edit
  - Download
  - Properties
  - Delete
- [ ] (Optional) Explore **Files**, **Tables**, and **Queues** sections of the storage account

---

## Task 3 — Monitor the Storage Account

- [ ] Return to your **Storage account** blade
- [ ] Click **Diagnose and solve problems**
- [ ] Explore available troubleshooters
- [ ] Scroll to **Monitoring** → **Insights**
- [ ] Review categories:
  - Failures
  - Performance
  - Availability
  - Capacity

---

## Optional Cleanup

- [ ] Go to **Resource groups**
- [ ] Select your resource group
- [ ] Click **Delete resource group**
- [ ] Type the resource group name to confirm → **Delete**

