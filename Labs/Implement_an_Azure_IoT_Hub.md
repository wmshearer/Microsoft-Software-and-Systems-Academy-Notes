# Implement an Azure IoT Hub — Step-By-Step Checklist

## Task 1 — Create an IoT Hub

- [ ] Sign in to **https://portal.azure.com**
- [ ] Go to **All services → IoT Hub**
- [ ] Click **+ Add → + Create → + New**

### **Basics Tab**
- [ ] Subscription: *Default*
- [ ] Resource group: *Default*
- [ ] IoT Hub name: **my-hub-group57040122**
- [ ] Region: **East US**
- [ ] Tier: **Free (F1)**

- [ ] Click **Review + create**
- [ ] Click **Create**
- [ ] Wait until deployment completes

---

## Task 2 — Add an IoT Device

- [ ] Click **Go to resource**  
  *Or manually search: IoT Hub → select your hub*

- [ ] In the left menu → **Device management → Devices**
- [ ] Click **+ Add Device**

### **New Device**
- [ ] Device ID: **myRaspberryPi**
- [ ] Click **Save**

- [ ] Refresh if needed
- [ ] Select **myRaspberryPi**
- [ ] Copy **Primary Connection String**  
  *(You'll need this for the simulator)*

---

## Task 3 — Test the Device with Raspberry Pi Simulator

- [ ] Open: **https://aka.ms/RaspPi**
- [ ] Close the popup window

### **Update the Code**
- [ ] In the code editor, find:  
  `const connectionString = '...'`
- [ ] Replace the entire string with the **Primary Connection String** you copied  
  *(Includes DeviceId + SharedAccessKey)*

- [ ] Click **Run**
  - Verify sensor data and messages appear in the console  
    *(Each LED flash = message sent)*

- [ ] Click **Stop** when finished

### **Confirm Data in Azure**
- [ ] Return to Azure Portal
- [ ] Go to your IoT Hub → **Overview**
- [ ] Scroll to **IoT Hub Usage**
- [ ] Change “Show data for last…” to **Last hour**
- [ ] View message metrics from the simulator

---

## Optional Cleanup
- [ ] Go to **Resource groups**
- [ ] Select the resource group used for this lab
- [ ] Click **Delete resource group**
- [ ] Type the name to confirm → **Delete**

