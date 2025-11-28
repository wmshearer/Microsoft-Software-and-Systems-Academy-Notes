# Module 11 — Maintain and Monitor Windows Server Installations

## Exercise 1 — Implement the WSUS Server Role

### Task 1 — Install the WSUS server role (LON-SVR4)

- [ ] Sign in to **LON-SVR4** as **Contoso\Administrator** / **Pa55w.rd**
- [ ] Open **Server Manager**
- [ ] Select **Manage** → **Add Roles and Features**
- [ ] **Before You Begin** → **Next**
- [ ] **Installation type** → ensure **Role-based or feature-based installation** is selected → **Next**
- [ ] **Destination server** → leave default **LON-SVR4** → **Next**

- [ ] **Server roles**:
  - [ ] Check **Windows Server Update Services**
  - [ ] In **Add Roles and Features Wizard** popup → **Add Features**
  - [ ] **Next**

- [ ] **Features** → **Next**
- [ ] **Windows Server Update Services** info page → **Next**

- [ ] **Role services**:
  - [ ] Ensure **WID Connectivity** and **WSUS Services** are both selected
  - [ ] **Next**

- [ ] **Content location selection**:
  - [ ] Folder: `C:\WSUSUpdates`
  - [ ] **Next**

- [ ] **Confirm installation selections** → **Install**
- [ ] Wait for installation to complete (shows **Configuration required**)
- [ ] **Close** the wizard

- [ ] In **Server Manager** → **Tools** → **Windows Server Update Services**
- [ ] In **Complete WSUS Installation** dialog:
  - [ ] Select **Run**
  - [ ] Wait for post-installation to complete
  - [ ] **Close**

- [ ] The **WSUS Configuration Wizard** starts

### Task 2 — Configure WSUS to sync from upstream server (LON-SVR2)

- [ ] In **WSUS Configuration Wizard: LON-SVR4**:
  - [ ] **Next** twice

- [ ] **Choose Upstream Server**:
  - [ ] Select **Synchronize from another Windows Server Update Services server**
  - [ ] **Server name**: `LON-SVR2.Contoso.com`
  - [ ] **Next**

- [ ] **Specify Proxy Server** → no proxy → **Next**

- [ ] **Connect to Upstream Server**:
  - [ ] Click **Start Connecting**
  - [ ] Wait for connection to complete
  - [ ] **Next**

- [ ] **Choose Languages** → leave default → **Next**
- [ ] **Choose Products / Classifications** (accept defaults) → **Next**
- [ ] **Set Sync Schedule** → accept defaults → **Next**

- [ ] **Finished**:
  - [ ] Check **Begin initial synchronization**
  - [ ] **Finish**

- [ ] In **Update Services** console:
  - [ ] Expand **LON-SVR4** → select **Synchronizations**
  - [ ] Wait until latest sync **Progress** shows **100%**
    - This can take several minutes; use **Refresh**

- [ ] In navigation pane → select **Options**
- [ ] In **Options** pane → select **Computers**
- [ ] In **Computers** dialog:
  - [ ] Select **Use Group Policy or registry settings on computers**
  - [ ] **OK**

---

## Exercise 2 — Configure Update Settings

### Task 1 — Configure WSUS computer group (LON-SVR4)

- [ ] On **LON-SVR4**, in **Update Services** console:
  - [ ] Expand **Computers**
  - [ ] Select **All Computers**
  - [ ] In **Actions** pane → **Add Computer Group**
  - [ ] Name: **Research**
  - [ ] **Add**

### Task 2 — Configure GPO to deploy WSUS settings (LON-DC1)

- [ ] On **LON-DC1**, sign in as **Contoso\Administrator** / **Pa55w.rd**
- [ ] Open **Server Manager** → **Tools** → **Group Policy Management**

- [ ] In **Group Policy Management**:
  - [ ] Expand **Forest: Contoso.com → Domains → Contoso.com**
  - [ ] Right-click **Research** OU → **Create a GPO in this domain, and Link it here**
  - [ ] Name: **WSUS Research**
  - [ ] **OK**

- [ ] Expand **Research** OU → right-click **WSUS Research** → **Edit**

#### Configure Automatic Updates

- [ ] In **GPO Editor**:
  - [ ] Navigate:  
    `Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Update`
  - [ ] In **Settings** pane, double-click **Configure Automatic Updates**
  - [ ] Set to **Enabled**
  - [ ] Under **Configure automatic updating**, choose:  
    **4 - Auto download and schedule the install**
  - [ ] **OK**

#### Configure WSUS server location

- [ ] In **Settings** pane, double-click **Specify intranet Microsoft update service location**
  - [ ] Set to **Enabled**
  - [ ] Both fields:
    - **Set the intranet update service for detecting updates**
    - **Set the intranet statistics server**
    - Value for both:  
      `http://LON-SVR4.Contoso.com:8530`
  - [ ] **OK**

#### Enable client-side targeting

- [ ] In **Settings** pane, double-click **Enable client-side targeting**
  - [ ] Set to **Enabled**
  - [ ] **Target group name for this computer**: `Research`
  - [ ] **OK**

- [ ] Close **Group Policy Management Editor** and **Group Policy Management**

#### Move client into Research OU

- [ ] In **Server Manager** → **Tools** → **Active Directory Users and Computers**
- [ ] Expand **Contoso.com → Computers**
- [ ] Right-click **LON-CL1** → **Move**
- [ ] Select **Research** OU → **OK**
- [ ] Close **AD Users and Computers**

### Task 3 — Verify GPO applied (LON-CL1)

- [ ] Restart **LON-CL1**
- [ ] Sign in as **Contoso\Administrator** / **Pa55w.rd**

#### Check Windows Update UI

- [ ] Start → type **Updates** → **Windows Update settings**
- [ ] **Advanced options** → under **Additional options** → **Configured update policies**
  - [ ] Confirm GPO-based policies are listed
- [ ] Close **Settings**

#### Check gpresult

- [ ] Start → type **cmd** → open **Command Prompt**
- [ ] Run:
```powershell
gpresult /r
```
 #### In output under Computer Settings, confirm WSUS Research is listed under Applied Group Policy Objects

### Task 4 — Initialize Windows Update (client + WSUS)
- [ ] On LON-CL1, in Command Prompt, run:
```powershell
wuauclt.exe /detectnow /reportnow
```

 - [ ]Switch to LON-SVR4

 - [ ] In Update Services console:

    - [ ] Expand Computers → All Computers → Research

    - [ ] Status drop-down: Any

    - [ ] Click Refresh until LON-CL1.contoso.com appears
(may take several minutes; if not, repeat the wuauclt command and refresh)

 - [ ] Once LON-CL1 appears, verify that update status is reported (approximately 17 updates needed)

## Exercise 3 — Approve and Deploy an Update via WSUS
### Task 1 — Approve WSUS updates for Research group (LON-SVR4)

  - [ ] On LON-SVR4, in Update Services console:

     - [ ] Expand Updates → All Updates

     - [ ] Status drop-down: Needed → Refresh

     - [ ] Find:
2022-09 Cumulative Update for .NET Framework 3.5, 4.8, and 4.8.1 for Windows 11 for x64 (KB50117497)

     - [ ] Right-click it → Approve

     - [ ] In Approve Updates dialog:

         - [ ] For Research group, set to Approved for Install

         - [ ] OK → Close

### Task 2 — Trigger update on LON-CL1

  - [ ] On LON-CL1, open Command Prompt and run:
```powershell
wuauclt.exe /detectnow /reportnow
```
 - [ ] Start → type Windows Update → open

 - [ ] Click Check for updates

 - [ ] Wait for updates to appear; cumulative update should download and install

   - [ ]  If device shows up to date, wait ~5 minutes and Check for updates again

    - [ ] LON-SVR4 must finish downloading from LON-SVR2 first

- [ ]  After installation completes, select Restart now

 - [ ] After reboot, sign back in as Contoso\Administrator / Pa55w.rd

### Task 3 — Verify update deployment (Event Viewer)
 - [ ] On LON-CL1, open Event Viewer

 - [ ] Navigate:

        Applications and Services Logs → Microsoft → Windows → WindowsUpdateClient → Operational

- [ ]  Review events and confirm entries corresponding to the approved update (download / install / success)

 - [ ] Close all windows and sign out of LON-CL1

# Module 11 — Maintain and Monitor Windows Server Installations  
## Performance & Centralized Logging Labs Cheat Sheet

---

## Exercise 1 — Establish a Performance Baseline (LON-SVR1)

### Task 1 — Create and start a Data Collector Set

- [ ] Sign in to **LON-SVR1** as `Contoso\Administrator` / `Pa55w.rd`
- [ ] Start → type **Perfmon** → open **Performance Monitor**
- [ ] In left pane: expand **Data Collector Sets → User Defined**
- [ ] Right-click **User Defined** → **New → Data Collector Set**

**Create new Data Collector Set wizard**

- [ ] Name: `LON-SVR1 Performance`
- [ ] Select **Create manually (Advanced)** → **Next**
- [ ] On **What type of data…** → check **Performance counter** → **Next**
- [ ] On **Which performance counters…** → click **Add**

Add these counters:

1. **Processor**
   - Expand **Processor**
   - Select **% Processor Time**
   - Click **Add >>**
2. **Memory**
   - Expand **Memory**
   - Select **Pages/sec**
   - **Add >>**
3. **PhysicalDisk**
   - Expand **PhysicalDisk**
   - Select **% Disk Time** → **Add >>**
   - Still under **PhysicalDisk**, select **Avg. Disk Queue Length** → **Add >>**
4. **System**
   - Expand **System**
   - Select **Processor Queue Length** → **Add >>**
5. **Network Interface**
   - Expand **Network Interface**
   - Select **Bytes Total/sec** → **Add >>**

- [ ] Click **OK**
- [ ] Back on **Which performance counters…**:
  - [ ] **Sample interval**: `1` (seconds)
  - [ ] **Next**
- [ ] **Where to save data** → accept default → **Next**
- [ ] **Create the data collector set?** → select **Save and close** → **Finish**

Start the collector:

- [ ] In **User Defined**, right-click **LON-SVR1 Performance** → **Start**

---

# Module 11 — Maintain and Monitor Windows Server Installations

## Task 2 — Create Additional Workload on the Server (LON-SVR1)

- [ ] On LON-SVR1, open Windows PowerShell ISE
- [ ] Select **Open** → browse to:
      E:\Labfiles\Mod11\StressTest.ps1
- [ ] Select **Run Script (F5)**
- [ ] Wait for script to finish running
- [ ] Close **Windows PowerShell ISE**

## Task 3 — Remove Workload and Review Performance Data (LON-SVR1)

- [ ] Switch to **Performance Monitor** on **LON-SVR1**
- [ ] In navigation pane, right-click **LON-SVR1 Performance** → select **Stop**
- [ ] In navigation pane, expand:
      Reports → User Defined → LON-SVR1 Performance
- [ ] Select latest report:
      LON-SVR1_DateTime-000002
- [ ] On toolbar, change graph type to **Report**
- [ ] Review the report data and record:

### Report Values

- [ ] Memory — **Pages/sec**: ______________________
- [ ] Network Interface — **Bytes Total/sec**: ______________________
- [ ] PhysicalDisk — **% Disk Time**: ______________________
- [ ] PhysicalDisk — **Avg. Disk Queue Length**: ______________________
- [ ] Processor — **% Processor Time**: ______________________
- [ ] System — **Processor Queue Length**: ______________________

### Question — Compared with your previous report, which values have changed?

- [ ] Notes: _________________________________________________
- [ ] Notes: _________________________________________________
- [ ] Notes: _________________________________________________

### Question — What would you recommend?

- [ ] Notes: _________________________________________________
- [ ] Notes: _________________________________________________
- [ ] Notes: _________________________________________________

- [ ] Close **Performance Monitor**

## Exercise 3 — Review and Configure Centralized Event Logs

## Task 1 — Configure Subscription Prerequisites (LON-DC1 & LON-CL1)

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] Open **Command Prompt** on LON-DC1
- [ ] Run:
      winrm quickconfig
- [ ] If prompted, select **Y**

- [ ] Open **Active Directory Users and Computers**
- [ ] Navigate:
      Contoso.com → Builtin
- [ ] Right-click **Administrators** → **Properties**
- [ ] Select **Members** tab
- [ ] Select **Add**
- [ ] Select **Object Types** → check **Computers** → **OK**
- [ ] Enter:
      LON-CL1
- [ ] Select **OK** → **OK**

- [ ] Switch to **LON-CL1**
- [ ] Sign in as:
      Contoso\Administrator / Pa55w.rd

- [ ] Open **Command Prompt** on LON-CL1
- [ ] Run:
      Wecutil qc
- [ ] If prompted, select **Y**

## Task 2 — Create a Subscription (LON-CL1)

- [ ] On **LON-CL1**, open **Event Viewer**
- [ ] In navigation pane, select **Subscriptions**
- [ ] Right-click **Subscriptions** → **Create Subscription**

- [ ] In Subscription Properties:
      Subscription name: LON-DC1 Events
- [ ] Select **Collector initiated**
- [ ] Select **Select Computers**

- [ ] In Computers dialog:
      Select **Add Domain Computers**
- [ ] In Select Computer:
      Enter: LON-DC1
- [ ] Select **OK** → **OK**

- [ ] In Subscription Properties, select **Select Events**

- [ ] In Query Filter:
      Logged: **Last 7 days**
- [ ] Check:
      Critical, Warning, Information, Verbose, Error

- [ ] In Event logs:
      Applications and Services Logs →
      Microsoft →
      Windows →
      Diagnosis-PLA →
      **Operational**
- [ ] Select **OK**

- [ ] Select **OK** to finish subscription setup

## Task 3 — Configure a Performance Counter Alert (LON-DC1)

- [ ] Switch to **LON-DC1**
- [ ] Open **Performance Monitor**

- [ ] In navigation pane:
      Expand **Data Collector Sets**
      Select **User Defined**

- [ ] Right-click **User Defined** → **New** → **Data Collector Set**

- [ ] Name:
      LON-DC1 Alert
- [ ] Select:
      Create manually (Advanced)
- [ ] Select **Next**

- [ ] Select:
      Performance Counter Alert
- [ ] Select **Next**

- [ ] Select **Add**
- [ ] In Available counters:
      Expand **Processor**
      Select **% Processor Time**
      Select **Add >>**
- [ ] Select **OK**

- [ ] Alert when: **Above**
- [ ] Limit: **10**
- [ ] Select **Next**

- [ ] Select **Finish**

- [ ] In navigation pane:
      Expand **User Defined**
      Select **LON-DC1 Alert**

- [ ] Right-click **DataCollector01** → **Properties**
- [ ] Sample interval: **1**
- [ ] Select **Alert Action** tab
- [ ] Check:
      Log an entry in the application event log
- [ ] Select **OK**

- [ ] Right-click **LON-DC1 Alert** → **Start**


## Task 4 — Introduce Additional Workload on the Server (LON-DC1)

- [ ] On **LON-DC1**, open **Windows PowerShell ISE**
- [ ] Select **Open** → browse to:
      E:\Labfiles\Mod11\StressTest.ps1
- [ ] Select **Run Script (F5)**
- [ ] Wait for script to finish
- [ ] Close **Windows PowerShell ISE**


## Task 5 — Verify the Results (LON-CL1)

- [ ] Switch to **LON-CL1**
- [ ] Open **Event Viewer**
- [ ] In navigation pane:
      Expand **Windows Logs**
      Select **Forwarded Events**

### Question — Are there any performance-related alerts?

- [ ] Notes: _________________________________________________
- [ ] Notes: _________________                        ________________________________
- [ ] Notes: _________________________________________________


## End of Exercise Results

- [ ] Centralized event log subscription configured
- [ ] Performance counter alert created and triggered
- [ ] Forwarded alerts verified on LON-CL1
