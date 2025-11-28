# Module 10 — Create & Manage Deployment Images  
## **Exercise 1: Configure MDT**

### **Task 1 — Configure the Deployment Share**
- [ ] Sign in to **LON-SVR1**  
  - User: **Contoso\Administrator**  
  - Pass: **Pa55w.rd**

- [ ] Open **File Explorer** → browse to **E:\ISOs**
- [ ] Right-click **Win2022_Eval.iso** → **Mount**  
  - Mounts as **DVD Drive F:**

- [ ] Close File Explorer

- [ ] Start Menu → **Microsoft Deployment Toolkit** → **Deployment Workbench**

- [ ] Right-click **Deployment Shares** → **New Deployment Share**

### **New Deployment Share Wizard**
- [ ] **Path:** `E:\DeploymentShare` → **Next**
- [ ] **Share name:** leave default → **Next**
- [ ] **Descriptive Name:** leave default → **Next**

### **Options**
- [ ] **Ask to set the local Administrator password** → **Enabled**
- [ ] All other boxes **Disabled**
- [ ] **Next → Next → Finish**

### **Verify Nodes**
You should now see under *MDT Deployment Share*:
- Applications  
- Operating Systems  
- Out-of-Box Drivers  
- Packages  
- Task Sequences  
- Advanced Configuration  
- Monitoring  

---

# **Exercise 2 — Create & Deploy an Image**

## **Task 1 — Add OS Files to Deployment Share**
- [ ] In Deployment Workbench → **Operating Systems**
- [ ] Right-click → **Import Operating System**

### **Import OS Wizard**
- [ ] OS Type: **Full set of source files**
- [ ] Source directory: **F:\\**
- [ ] Destination directory: **Windows Server 2022**
- [ ] **Next → Next → Finish**

- [ ] Verify **4 OS objects** appear

---

## **Task 2 — Create an MDT Task Sequence**
- [ ] Go to **Task Sequences**
- [ ] Right-click → **New Task Sequence**

### **General Settings**
- [ ] Task Sequence ID: **001**
- [ ] Name: **Deploy Windows Server 2022**

### **Template**
- [ ] **Standard Client Task Sequence**

### **Select OS**
- [ ] Choose **Windows Server 2022 SERVERDATACENTER**

### **Product Key**
- [ ] **Do not specify a product key**

### **OS Settings**
- [ ] Full Name: **Admin**  
- [ ] Organization: **Contoso Ltd.**  
- [ ] IE Home Page: **about:blank**

### **Admin Password**
- [ ] Use the specified password: **Pa55w.rd**

- [ ] **Next → Next → Finish**

### **Modify Task Sequence Properties**
- [ ] Right-click task sequence → **Properties**
- [ ] **Task Sequence** tab
- [ ] Expand **Validation** → Select **Validate**

- [ ] Uncheck:
  - **Ensure minimum memory**
  - **Ensure minimum processor speed**

- [ ] Ensure current OS to be refreshed is: **Server**
- [ ] **OK**

---

## **Task 3 — Configure Deployment Share & Windows PE**
- [ ] Right-click **MDT Deployment Share** → **Properties**

### **General tab**
- [ ] Uncheck **x86**
- [ ] Leave **x64** checked

### **Rules tab**
- [ ] Review (do not modify)

### **Windows PE tab**
- [ ] Platform: **x86**  
  - Uncheck **Generate a Lite Touch bootable ISO image**

- [ ] Change Platform → **x64**
- [ ] Scratch space size: **64 MB**

### **Features tab**
Enable:
- [ ] DISM Cmdlets  
- [ ] Windows PowerShell  
- [ ] MDAC/ADO Support  

### **Monitoring tab**
- [ ] Enable monitoring

- [ ] **OK**

### **Update Deployment Share**
- [ ] Right-click **MDT Deployment Share** → **Update Deployment Share**

- [ ] Options: **Optimize the boot image updating process**
- [ ] **Next → Next → Finish**

---

## **Task 4 — Deploy Windows Server 2022 via MDT**

### **Create Hyper-V Virtual Switch**
- [ ] Open **Hyper-V Manager**
- [ ] **Virtual Switch Manager**
- [ ] New virtual network switch → **External**
- [ ] Name: **External network**
- [ ] Connection: **External network → Microsoft Hyper-V Network Adapter #3**
- [ ] **OK → Yes**

### **Create VM: LON-SVR7**
- [ ] New → **Virtual Machine**
- [ ] Name: **LON-SVR7**
- [ ] Store in different location: `E:\VirtualMachines`
- [ ] Generation: **1**
- [ ] Memory: **8192 MB**
- [ ] Network: **External Network**
- [ ] Create VHDX:
  - Name: **LON-SVR7.vhdx**
  - Location: `E:\VirtualMachines\LON-SVR7\Virtual Hard Disks\`
  - Size: **60 GB**
- [ ] Install from bootable ISO:
  - **E:\DeploymentShare\Boot\LiteTouchPE_x64.iso**
- [ ] **Finish**

### **Start the VM**
- [ ] Connect → **Start**
- [ ] MDT Deployment Wizard appears

### **MDT Wizard Steps**
- [ ] Run Deployment Wizard  
- [ ] Credentials:
  - Username: **Administrator**
  - Password: **Pa55w.rd**
  - Domain: **Contoso**

- [ ] Task Sequence: **Deploy Windows Server 2022**

- [ ] Computer name: **LON-SVR7**

- [ ] Join domain: **Contoso.com**

- [ ] Locale & Time: **Next**

- [ ] Administrator Password: **Pa55w.rd**

- [ ] **Begin**

### **Monitor Deployment**
- [ ] Deployment Workbench → Deployment Share → **Monitoring**
- [ ] Right-click **LON-SVR7** → **Properties** (watch progress)

### **After Completion**
- [ ] LON-SVR7 reboots automatically
- [ ] Finalize setup
- [ ] Server Manager opens
- [ ] Verify:
  - Computer Name → **LON-SVR7**
  - Domain Joined → **Contoso.com**

- [ ] Shut down VM
- [ ] Close VM window

---

# ✔ Results
You have:
- Configured MDT  
- Added Windows Server 2022 OS  
- Created a Task Sequence  
- Customized Windows PE  
- Created a VM  
- Successfully deployed Windows Server 2022 using MDT  

---