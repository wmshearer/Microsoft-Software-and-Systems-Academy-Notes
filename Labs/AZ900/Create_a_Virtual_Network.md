# Create a Virtual Network — Step-By-Step Checklist

## Task 1 — Create a Virtual Network

- [ ] Sign in to **https://portal.azure.com**
- [ ] Go to **All services** → search **Virtual networks**
- [ ] Select **+ Add** → **+ Create** → **+ New**
- [ ] **Basics** tab:
  - Subscription: *Leave default*
  - Resource Group: *Use default supplied*
  - Name: **vnet1**
  - Region: **East US**
- [ ] Click **Review + create**
- [ ] Ensure validation passes
- [ ] Click **Create**

---

## Task 2 — Create Two Virtual Machines

### VM1

- [ ] Go to **All services** → **Virtual machines**
- [ ] Select **+ Add** → **+ Create** → **Virtual Machine**
- [ ] **Basics** tab:
  - Subscription: default
  - Resource group: default
  - Virtual machine name: **vm1**
  - Region: **East US**
  - Image: **Windows Server 2019 Datacenter – Gen2**
  - Username: **azureuser**
  - Password: **Pa$$w0rd1234**
  - Public inbound ports: **Allow selected ports**
  - Selected inbound ports: **RDP (3389)**
- [ ] **Networking** tab: ensure **vnet1** is selected
- [ ] Click **Review + create**
- [ ] After validation, click **Create**

### VM2

- [ ] Repeat the VM creation steps with:
  - Virtual machine name: **vm2**
  - Virtual network: **vnet1**
  - Public IP: **vm2-ip**
- [ ] Wait until both VMs show **Status: Running**

---

## Task 3 — Test the Connection (Ping VM1 → VM2)

### Connect to VM1

- [ ] Open **vm1** → **Overview**
- [ ] Click **Connect** → **RDP**
- [ ] Click **Download RDP File**
- [ ] Open RDP file → **Connect**
- [ ] Login:
  - Username: **azureuser**
  - Password: **Pa$$w0rd1234**
- [ ] Accept certificate warning if prompted

### Disable Firewalls in Both VMs

Perform these steps inside **vm1** and **vm2**:

- [ ] Start Menu → **Settings**
- [ ] **Network & Internet**
- [ ] Locate **Windows Firewall**
- [ ] Disable **Public** and **Private** firewall

### Ping VM2 from VM1

- [ ] Open **PowerShell (Run as administrator)**
- [ ] Run:

```powershell
ping vm2
```