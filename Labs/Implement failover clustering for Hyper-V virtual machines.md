
### Exercise 1 — Task 1: Connect cluster nodes to iSCSI shared storage

#### Connect LON-SVR2 to the iSCSI target

- [ ] Sign in to **LON-SVR2** as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Select **Start → Server Manager**.

- [ ] In **Server Manager**, select **Tools → iSCSI Initiator**.

- [ ] In the **Microsoft iSCSI** dialog box, select **Yes**.

- [ ] In **iSCSI Initiator Properties**, select the **Discovery** tab → **Discover Portal**.

- [ ] In **IP address or DNS name**, enter:

    ```powershell
    172.16.0.24
    ```

    Select **OK**.

- [ ] Select the **Targets** tab → select **Refresh**.

- [ ] In **Targets**, select:

    ```
    iqn.1991-05.com.microsoft:lon-svr4-svr4target-target
    ```

    Then select **Connect**.

- [ ] Ensure **Add this connection to the list of Favorite Targets** is checked.

- [ ] Select **OK** twice.

---

#### Connect LON-SVR3 to the iSCSI target

- [ ] Sign in to **LON-SVR3** as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Open **Server Manager → Tools → iSCSI Initiator**.

- [ ] In the **Microsoft iSCSI** dialog box, select **Yes**.

- [ ] In **iSCSI Initiator Properties**, select **Discovery → Discover Portal**.

- [ ] In **IP address or DNS name**, enter:

    ```powershell
    172.16.0.24
    ```

    Select **OK**.

- [ ] Select the **Targets** tab → **Refresh**.

- [ ] Select:

    ```
    iqn.1991-05.com.microsoft:lon-svr4-svr4target-target
    ```

    Then select **Connect**.

- [ ] Ensure **Add this connection to the list of Favorite Targets** is checked.

- [ ] Select **OK** twice.

---

#### Initialize and format cluster disks on LON-SVR2

- [ ] Switch back to **LON-SVR2**.

- [ ] In **Server Manager**, select **Tools → Computer Management**.

- [ ] Expand:

    ```
    Storage → Disk Management
    ```

- [ ] Right-click **Disk 5** (20 GB, unallocated) → select **Online**.

- [ ] Right-click **Disk 5** again → **Initialize Disk** → **OK**.

- [ ] Right-click the **unallocated space** next to Disk 5 → **New Simple Volume**.

    Wizard steps:

    - *Welcome* → **Next**  
    - *Volume Size* → **Next**  
    - *Assign Drive Letter* → **Next**  
    - *Format Partition* →  
      - Volume label: `ClusterDisk`  
      - Check **Perform a quick format**  
      - Select **Next**  
    - Select **Finish**

- [ ] Repeat the above steps for:

    - **Disk 6** → Volume label: `ClusterVMs`  
    - **Disk 7** → Volume label: `Quorum`

- [ ] Close **Computer Management**.

---

#### Bring disks online on LON-SVR3

- [ ] Switch to **LON-SVR3**.

- [ ] Open **Server Manager → Tools → Computer Management**.

- [ ] Expand:

    ```
    Storage → Disk Management
    ```

- [ ] Right-click **Disk Management** → **Refresh**.

- [ ] Bring online the three 20 GB disks:

    - Right-click **Disk 3** → **Online**  
    - Right-click **Disk 4** → **Online**  
    - Right-click **Disk 5** → **Online**

- [ ] Confirm all three show **Online** and are **20 GB**.

- [ ] Close **Computer Management** on LON-SVR3.

### Exercise 1 — Task 2: Install the Failover Clustering feature

#### Install Failover Clustering on LON-SVR2

- [ ] Switch to **LON-SVR2**.

- [ ] Select **Start**, then open **Windows PowerShell ISE**.

- [ ] In the **Administrator: Windows PowerShell ISE** window, run:

    ```powershell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

- [ ] Wait for the installation to complete on **LON-SVR2**.

---

#### Prepare to remotely install Failover Clustering on LON-SVR3

- [ ] On **LON-SVR2**, open a **new** Windows PowerShell ISE session  
      (leave the first one open or reopen ISE again).

- [ ] In the new ISE window, run:

    ```powershell
    $cred = Get-Credential
    ```

- [ ] When prompted, enter:

    - **Username:** `Contoso\Administrator`  
    - **Password:** `Pa55w.rd`

- [ ] Create a PowerShell remoting session to **LON-SVR3**:

    ```powershell
    $sess = New-PSSession -Credential $cred -ComputerName lon-svr3.contoso.com
    ```

- [ ] Enter the remote session:

    ```powershell
    Enter-PSSession $sess
    ```

- [ ] Confirm that the prompt changes to:

    ```
    lon-svr3.contoso.com:\>
    ```

---

#### Install Failover Clustering on LON-SVR3 (through remoting)

- [ ] While connected to **LON-SVR3** via PowerShell remoting, run:

    ```powershell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

- [ ] Wait for installation to complete successfully on **LON-SVR3**.

---

#### Finalize

- [ ] Close both Windows PowerShell ISE windows on **LON-SVR2**.

### Exercise 1 — Task 3: Validate and create a failover cluster

#### Validate cluster configuration

- [ ] On **LON-SVR2**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell command prompt, run the cluster validation test:

    ```powershell
    Test-Cluster LON-SVR2, LON-SVR3
    ```

- [ ] Wait several minutes for all tests to complete.  
      *Some warnings are expected, but there must be **no errors***.

---

#### Create the failover cluster (initial node)

- [ ] In the same Windows PowerShell ISE session, create the cluster:

    ```powershell
    New-Cluster -Name VMCluster -Node lon-svr2 -StaticAddress 172.16.0.126
    ```

- [ ] Verify the output shows:

    ```
    Name: VMCluster
    ```

- [ ] Confirm that **no errors** were reported during creation.

---

#### Add LON-SVR3 as a second cluster node

- [ ] Still in Windows PowerShell ISE, run:

    ```powershell
    Add-ClusterNode -Name LON-SVR3
    ```

- [ ] Verify the command completes successfully with **no errors**.

---

- [ ] Close **Windows PowerShell ISE**.

### Exercise 1 — Task 4: Configure disks for a failover cluster

- [ ] On **LON-SVR2**, open **Server Manager**.

- [ ] Select **Tools → Failover Cluster Manager**.

- [ ] In Failover Cluster Manager, expand:

    ```
    VMCluster.Contoso.com → Storage → Disks
    ```

- [ ] Verify that the following disks are **present and online**:

    - **Cluster Disk 1**
    - **Cluster Disk 2**
    - **Cluster Disk 3**

---

#### Add Cluster Disk 1 to Cluster Shared Volumes (CSV)

- [ ] Right-click **Cluster Disk 1**, then select:

    ```
    Add to Cluster Shared Volumes
    ```

---

#### Configure cluster quorum settings

- [ ] Right-click **VMCluster.Contoso.com**, select:

    ```
    More Actions → Configure Cluster Quorum Settings
    ```

- [ ] On *Before You Begin*, select **Next**.

- [ ] On *Select Quorum Configuration Option*, select:

    ```
    Use default quorum configuration
    ```

    Then select **Next**.

- [ ] On *Confirmation*, select **Next**.

- [ ] On *Summary*, select **Finish**.

---

#### Ensure Disk Ownership

- [ ] In **Failover Cluster Manager**, confirm that **LON-SVR2** is the **Owner Node** of  
      the disk you added to Cluster Shared Volumes.

- [ ] If **LON-SVR2 is NOT the owner**, perform these steps:

    - Right-click **Cluster Disk 1**  
    - Select **Move → Select Node**  
    - Choose:

        ```
        LON-SVR2
        ```

    - Select **OK**

- [ ] Verify that **Cluster Disk 1** now shows **Owner Node = LON-SVR2**.


### Exercise 2 — Task 1: Move VM storage to the Cluster Shared Volume

- [ ] On **LON-SVR2**, open **Hyper-V Manager** from the taskbar.

- [ ] In **Hyper-V Manager**, under **Virtual Machines**, select:

    ```
    LON-SVR5
    ```

- [ ] In the **Actions** pane (right side), under **LON-SVR5**, select:

    ```
    Move
    ```

    This opens the **Move "LON-SVR5" Wizard**.

- [ ] On the *Before You Begin* page, select **Next**.

- [ ] On *Choose Move Type*, select:

    ```
    Move the virtual machine's storage
    ```

    Then select **Next**.

- [ ] On *Choose Options for Moving Storage*, select:

    ```
    Move all of the virtual machine's data to a single location
    ```

    Then select **Next**.

- [ ] On *Choose a new location for virtual machine*, in the **Folder** box, enter:

    ```powershell
    C:\ClusterStorage\Volume1\
    ```

    Then select **Next**.

- [ ] On the *Completing Move Wizard* page, select **Finish**.

- [ ] Wait for the storage move to fully complete.

- [ ] Close **Hyper-V Manager**.

### Exercise 2 — Task 2: Configure the VM as highly available

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    VMCluster.Contoso.com → Roles
    ```

- [ ] Right-click **Roles**, then select:

    ```
    Configure Role
    ```

    This launches the **High Availability Wizard**.

- [ ] On the *Before You Begin* page, select **Next**.

- [ ] On the *Select Role* page, choose:

    ```
    Virtual Machine
    ```

    Then select **Next**.

- [ ] On the *Select Virtual Machine* page, select:

    ```
    LON-SVR5
    ```

    Then select **Next**.

- [ ] On the *Confirmation* page, select **Next**.

- [ ] On the *Summary* page, verify the result is **successful**, then select **Finish**.

- [ ] Confirm that **LON-SVR5** now appears under:

    ```
    VMCluster.Contoso.com → Roles
    ```

- [ ] Right-click **LON-SVR5**, then select:

    ```
    Start
    ```

- [ ] Verify that **LON-SVR5** starts successfully as a **clustered role**.

### Exercise 2 — Task 3: Failover a highly available VM

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    VMCluster.Contoso.com → Roles
    ```

- [ ] Right-click **LON-SVR5**, then select:

    ```
    Move → Live Migration → Select Node
    ```

- [ ] In the **Move Virtual Machine** dialog box, select:

    ```
    LON-SVR3
    ```

    Then select **OK**.

- [ ] Wait for the Live Migration to complete.

    - The **Owner Node** column should change from **LON-SVR2** → **LON-SVR3**.

- [ ] After the migration completes, right-click **LON-SVR5**, then select:

    ```
    Shut Down
    ```

- [ ] Verify the VM shuts down cleanly under cluster management.

### Exercise 2 — Task 4: Configure drain on shutdown

#### Verify current DrainOnShutdown setting

- [ ] On **LON-SVR2**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell command prompt, run:

    ```powershell
    (Get-Cluster).DrainOnShutdown
    ```

- [ ] Confirm the output returns:

    ```
    1
    ```

    This means **Drain on Shutdown is enabled** (cluster will live-migrate roles off a node before shutdown).

---

#### Test drain-on-shutdown behavior with a clustered VM

- [ ] Switch to **Failover Cluster Manager** on **LON-SVR2**.

- [ ] Select:

    ```
    VMCluster.Contoso.com → Roles
    ```

- [ ] Confirm that **LON-SVR5** is currently **owned by LON-SVR3**.

    - If not, manually migrate it using:
        - Right-click **LON-SVR5**
        - Select **Move → Live Migration → Select Node**
        - Choose **LON-SVR3**

- [ ] Shut down **LON-SVR3**.

- [ ] On **LON-SVR2**, watch **Failover Cluster Manager** and verify:

    - **LON-SVR5** live-migrates **back to LON-SVR2 BEFORE** LON-SVR3 fully shuts down.  
    - This confirms that **DrainOnShutdown = 1** is functioning.
