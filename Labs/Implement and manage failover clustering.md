### Exercise 1 — Task 1: Connect cluster nodes to iSCSI shared storage

#### Part A — Configure the iSCSI targets (on LON-SVR1)

- [ ] Sign in to **LON-SVR1** as **Contoso\Administrator** with the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] Select **Start**, then open **Server Manager**.

- [ ] In **Server Manager**, in the navigation pane, select **File and Storage Services**.

- [ ] In **File and Storage Services**, select **iSCSI**.

- [ ] In the **iSCSI VIRTUAL DISKS** pane, select **TASKS → New iSCSI Virtual Disk**.

- [ ] In the **New iSCSI Virtual Disk Wizard**, on *Select iSCSI virtual disk location*:

    - Under **Storage location**, select **C:**  
    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk name*:

    - In **Name**, enter:

        ```powershell
        iSCSIDisk1
        ```

    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk size*:

    - In **Size**, enter **5**  
    - Make sure **GB** is selected  
    - Select **Next**.

- [ ] On *Assign iSCSI target*:

    - Select **New iSCSI target**  
    - Select **Next**.

- [ ] On *Specify target name*:

    - In **Name**, enter:

        ```powershell
        lon-svr1
        ```

    - Select **Next**.

- [ ] On *Specify access servers*, select **Add**.

- [ ] In **Select a method to identify the initiator**:

    - Select **Enter a value for the selected type**  
    - In **Type**, choose **IP Address**  
    - In **Value**, enter:

        ```powershell
        172.16.0.22
        ```

    - Select **OK**.

- [ ] On *Specify access servers*, select **Add** again.

- [ ] In **Select a method to identify the initiator**:

    - Select **Enter a value for the selected type**  
    - In **Type**, choose **IP Address**  
    - In **Value**, enter:

        ```powershell
        172.16.0.23
        ```

    - Select **OK**.

- [ ] On *Specify access servers*, select **Next**.

- [ ] On *Enable Authentication*, select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On *View results*, wait until creation is complete, then select **Close**.

---

#### Create iSCSIDisk2 on same target

- [ ] In the **iSCSI VIRTUAL DISKS** pane, select **TASKS → New iSCSI Virtual Disk**.

- [ ] On *Select iSCSI virtual disk location*:

    - Under **Storage location**, select **C:**  
    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk name*:

    - In **Name**, enter:

        ```powershell
        iSCSIDisk2
        ```

    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk size*:

    - In **Size**, enter **5**  
    - Ensure **GB** is selected  
    - Select **Next**.

- [ ] On *Assign iSCSI target*:

    - Select existing target **lon-svr1**  
    - Select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On *View results*, wait for completion, then select **Close**.

---

#### Create iSCSIDisk3 on same target

- [ ] In the **iSCSI VIRTUAL DISKS** pane, select **TASKS → New iSCSI Virtual Disk**.

- [ ] On *Select iSCSI virtual disk location*:

    - Under **Storage location**, select **C:**  
    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk name*:

    - In **Name**, enter:

        ```powershell
        iSCSIDisk3
        ```

    - Select **Next**.

- [ ] On *Specify iSCSI virtual disk size*:

    - In **Size**, enter **5**  
    - Ensure **GB** is selected  
    - Select **Next**.

- [ ] On *Assign iSCSI target*:

    - Select **lon-svr1**  
    - Select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On *View results*, wait until creation is complete, then select **Close**.

---

#### Part B — Connect nodes to the iSCSI targets (LON-SVR2 & LON-SVR3)

##### Connect LON-SVR2 to iSCSI target

- [ ] Sign in to **LON-SVR2** as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Select **Start → Server Manager**.

- [ ] In **Server Manager**, select **Tools → iSCSI Initiator**.

- [ ] In the **Microsoft iSCSI** dialog, select **Yes** to start the service.

- [ ] In **iSCSI Initiator Properties**, select the **Discovery** tab, then select **Discover Portal**.

- [ ] In **IP address or DNS name**, enter:

    ```powershell
    172.16.0.21
    ```

    then select **OK**.

- [ ] Select the **Targets** tab, then select **Refresh**.

- [ ] In **Targets**, select:

    ```text
    iqn.1991-05.com.microsoft:lon-svr1-lon-svr1-target
    ```

    then select **Connect**.

- [ ] Make sure **Add this connection to the list of Favorite Targets** is checked, then select **OK** twice.

---

##### Connect LON-SVR3 to iSCSI target

- [ ] Switch to **LON-SVR3**.

- [ ] Sign in to **LON-SVR3** as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Open **Server Manager**, then select **Tools → iSCSI Initiator**.

- [ ] In the **Microsoft iSCSI** dialog box, select **Yes**.

- [ ] In **iSCSI Initiator Properties**, select the **Discovery** tab, then **Discover Portal**.

- [ ] In **IP address or DNS name**, enter:

    ```powershell
    172.16.0.21
    ```

    then select **OK**.

- [ ] Select the **Targets** tab, then select **Refresh**.

- [ ] In **Targets**, select:

    ```text
    iqn.1991-05.com.microsoft:lon-svr1-lon-svr1-target
    ```

    then select **Connect**.

- [ ] Ensure **Add this connection to the list of Favorite Targets** is checked, then select **OK** twice.

---

#### Part C — Bring disks online and initialize on LON-SVR2

- [ ] Switch back to **LON-SVR2**.

- [ ] In **Server Manager**, select **Tools → Computer Management**.

- [ ] In **Computer Management**, expand **Storage → Disk Management**.

- [ ] Right-click **Disk 5** (first 5.00 GB unallocated disk), then select **Online**.

- [ ] Right-click **Disk 5** again, then select **Initialize Disk**.

- [ ] In **Initialize Disk**, leave defaults and select **OK**.

- [ ] Right-click the **unallocated space** next to **Disk 5**, then select **New Simple Volume**.

- [ ] In the wizard:

    - *Welcome* → **Next**  
    - *Specify Volume Size* → **Next**  
    - *Assign Drive Letter or Path* → **Next**  
    - *Format Partition* →  
      - **Volume label:** `Data1`  
      - Check **Perform a quick format**  
      - Select **Next**  
    - Select **Finish**.

- [ ] Repeat the same steps for **Disk 6** and **Disk 7**:

    - Use **Data2** as the volume label for Disk 6.  
    - Use **Data3** as the volume label for Disk 7.  
    - Ensure each is **5.00 GB** and fully initialized and formatted.

- [ ] Close **Computer Management** on **LON-SVR2**.

---

#### Part D — Bring disks online on LON-SVR3

- [ ] Switch to **LON-SVR3**.

- [ ] In **Server Manager**, select **Tools → Computer Management**.

- [ ] Expand **Storage → Disk Management**.

- [ ] In **Disk Management**, select **Action → Refresh** (or right-click **Disk Management** and select **Refresh**).

- [ ] Right-click **Disk 3** (5.00 GB), then select **Online**.

- [ ] Right-click **Disk 4** (5.00 GB), then select **Online**.

- [ ] Right-click **Disk 5** (5.00 GB), then select **Online**.

- [ ] Confirm that all three **5.00 GB** disks now show **Online**.

- [ ] Close **Computer Management** on **LON-SVR3**.

### Exercise 1 — Task 2: Install the Failover Cluster feature

- [ ] Switch to **LON-SVR2**.

- [ ] On **LON-SVR2**, open **Server Manager**.

- [ ] Select **Add roles and features**.

- [ ] In the *Before you begin* page, select **Next**.

- [ ] On *Select installation type*, select **Next**.

- [ ] On *Select destination server*, ensure **Select a server from the server pool** is selected,  
      then select **Next**.

- [ ] On *Select server roles*, select **Next**.

- [ ] On *Select features*, check **Failover Clustering**.

- [ ] When prompted with **Add features that are required for Failover Clustering**, select **Add Features**.

- [ ] Select **Next**.

- [ ] On *Confirm installation selections*, select **Install**.

- [ ] When installation completes and displays:

    ```
    Installation succeeded on LON-SVR2.Contoso.com
    ```

    select **Close**.

- [ ] Switch to **LON-SVR3**.

- [ ] Repeat steps **2 through 10** to install the Failover Cluster feature on **LON-SVR3**.

- [ ] Switch to **LON-SVR4** (if required), sign in as:

    ```powershell
    Contoso\Administrator  (password: Pa55w.rd)
    ```

- [ ] Install the Failover Cluster feature on **LON-SVR4** by repeating steps **2 through 10**.

### Exercise 1 — Task 3: Validate the servers for failover clustering

- [ ] Switch to **LON-SVR2**.

- [ ] On **LON-SVR2**, open **Server Manager**.

- [ ] Select **Tools → Failover Cluster Manager**.

- [ ] In **Failover Cluster Manager**, in the **Actions** pane, select **Validate Configuration**.

- [ ] In the *Validate a Configuration Wizard*, select **Next**.

- [ ] In the **Enter Name** box, type:

    ```
    LON-SVR2
    ```

    then select **Add**.

- [ ] In the **Enter Name** box, type:

    ```
    LON-SVR3
    ```

    then select **Add**.

- [ ] Select **Next**.

- [ ] Ensure the option **Run all tests (recommended)** is selected.

- [ ] Select **Next**.

- [ ] On the *Confirmation* page, select **Next** to begin validation.

- [ ] Wait **5–7 minutes** for all validation tests to complete.

- [ ] On the *Summary* page of the report, scroll through and:

    - Confirm **no errors** are present  
    - Note that **some warnings are expected** and acceptable

- [ ] Select **Finish** to complete validation.

### Exercise 1 — Task 4: Create the failover cluster

- [ ] On **LON-SVR2**, in **Failover Cluster Manager**, go to the **Actions** pane.

- [ ] Select **Create Cluster**.

- [ ] In the *Before You Begin* page, select **Next**.

- [ ] On the *Select Servers* page, in **Enter server name**, type:

    ```
    LON-SVR2
    ```

    then select **Add**.

- [ ] In **Enter server name**, type:

    ```
    LON-SVR3
    ```

    then select **Add**.

- [ ] Select **Next**.

- [ ] On *Access Point for Administering the Cluster*, enter:

    - **Cluster Name:**  
      ```
      Cluster1
      ```

    - **Address:**  
      ```powershell
      172.16.0.125
      ```

- [ ] Select **Next**.

- [ ] On the *Confirmation* page, select **Next** to begin cluster creation.

- [ ] Wait for the process to complete.

- [ ] On the *Summary* page, select **Finish**.

### Exercise 1 — Task 5: Add the file-server application to the failover cluster

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Storage → Disks
    ```

- [ ] Verify that the following disks are **present and online**:

    - **Cluster Disk 1**  
    - **Cluster Disk 2**  
    - **Cluster Disk 3**

- [ ] Right-click **Roles**, then select **Configure Role**.

- [ ] In the *Before You Begin* page, select **Next**.

- [ ] On the *Select Role* page, choose:

    ```
    File Server
    ```

    then select **Next**.

- [ ] On the *File Server Type* page, select:

    ```
    File Server for general use
    ```

    then select **Next**.

- [ ] On the *Client Access Point* page, enter:

    - **Name:**  
      ```
      ContosoFS
      ```

    - **Address:**  
      ```powershell
      172.16.0.130
      ```

    Select **Next**.

- [ ] On the *Select Storage* page, check:

    ```
    Cluster Disk 2
    ```

    then select **Next**.

- [ ] On the *Confirmation* page, select **Next**.

- [ ] Wait for the configuration to finish.

- [ ] On the *Summary* page, select **Finish**.

### Exercise 1 — Task 6: Add a shared folder to a highly available file server

- [ ] Switch to **LON-SVR3**.

- [ ] On **LON-SVR3**, open **Server Manager**.

- [ ] Select **Tools → Failover Cluster Manager**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] Right-click **ContosoFS**, then select **Add File Share**.

- [ ] In the *New Share Wizard*, on **Select the profile for this share**, choose:

    ```
    SMB Share - Quick
    ```

    then select **Next**.

- [ ] On *Select the server and path for this share*, select **Next**.

- [ ] On *Specify share name*, enter:

    ```
    Docs
    ```

    then select **Next**.

- [ ] On *Configure share settings*, review available options (no changes required), then select **Next**.

- [ ] On *Specify permissions to control access*, select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On the *View results* page, select **Close**.

### Exercise 1 — Task 7: Configure failover and failback settings

- [ ] On **LON-SVR3**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] Right-click **ContosoFS**, then select **Properties**.

- [ ] In the **ContosoFS Properties** dialog, select the **Failover** tab.

- [ ] Select **Allow failback**.

- [ ] Select **Failback between**, and configure the window to:

    ```
    4 and 5 hours
    ```

- [ ] Select the **General** tab.

- [ ] Under **Preferred owners**, select both:

    - **LON-SVR2**
    - **LON-SVR3**

- [ ] Select **LON-SVR3**, then select **Up** so that it becomes the **first** preferred owner.

- [ ] Select **OK** to close the properties dialog.

### Exercise 1 — Task 8: Validate the highly available file-server deployment

- [ ] Switch to **LON-DC1** and sign in as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] On **LON-DC1**, open **File Explorer**.

- [ ] In the address bar, enter:

    ```
    \\ContosoFS
    ```

    then press **Enter**.

- [ ] Verify that the **ContosoFS** location opens and that the **Docs** folder is visible.

- [ ] Open the **Docs** folder.

- [ ] Create a new text document inside the folder named:

    ```
    test.txt
    ```

---

- [ ] Switch to **LON-SVR2**.

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] In the **Owner Node** column, observe which server currently owns **ContosoFS**  
      (either **LON-SVR2** or **LON-SVR3**).

- [ ] Right-click **ContosoFS**, select **Move**, then select **Select Node**.

- [ ] In the **Move Clustered Role** dialog, select the *other* node  
      (whichever is NOT currently the owner), then select **OK**.

- [ ] Verify that **ContosoFS** moves to the new owner node in the console.

---

- [ ] Switch back to **LON-DC1**.

- [ ] Open **File Explorer**.

- [ ] In the address bar, enter:

    ```
    \\ContosoFS
    ```

    then press **Enter**.

- [ ] Verify that the file share is still accessible  
      and the **Docs** folder opens successfully.

### Exercise 1 — Task 9: Validate the failover and quorum configuration for the File Server role

- [ ] Switch to **LON-SVR2**.

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] In the **Owner Node** column, identify which node currently owns **ContosoFS**  
      (either **LON-SVR2** or **LON-SVR3**).

- [ ] In the navigation pane, select **Nodes**.

- [ ] Select the node that is the **current owner** of **ContosoFS**.

- [ ] Right-click the node → **More Actions → Stop Cluster Service**.

- [ ] Return to **Roles** and verify:

    - The **ContosoFS** clustered role is still **running**.  
    - Ownership has automatically moved to the **other node**.

---

- [ ] Switch to **LON-DC1**.

- [ ] Open **File Explorer**.

- [ ] In the address bar, enter:

    ```
    \\ContosoFS
    ```

    then press **Enter**.

- [ ] Verify that the file server share is **still accessible**.

---

- [ ] Switch back to **LON-SVR2**.

- [ ] In **Failover Cluster Manager**, select **Nodes**.

- [ ] Right-click the **stopped node**, select **More Actions → Start Cluster Service**.

---

#### Validate the quorum witness disk

- [ ] Expand:

    ```
    Storage → Disks
    ```

- [ ] In the center pane, locate the disk assigned as the **Disk Witness in Quorum**  
      (shown in the **Assigned To** column).

- [ ] Right-click the **disk witness**, then select **Take Offline** → **Yes**.

- [ ] Switch to **LON-DC1**.

- [ ] Open **File Explorer**.

- [ ] In the address bar, enter:

    ```
    \\ContosoFS
    ```

    then press **Enter**.

- [ ] Verify that the share is still accessible even with the **witness disk offline**.

---

#### Re-enable the quorum witness and configure settings

- [ ] Switch back to **LON-SVR2**.

- [ ] In **Failover Cluster Manager**, expand:

    ```
    Storage → Disks
    ```

- [ ] Right-click the disk that is **Offline**, select **Bring Online**.

- [ ] Right-click **Cluster1.Contoso.com**, select:

    ```
    More Actions → Configure Cluster Quorum Settings
    ```

- [ ] On *Before You Begin*, select **Next**.

- [ ] On *Select Quorum Configuration Option*, choose:

    ```
    Advanced quorum configuration
    ```

    then select **Next**.

- [ ] On *Select Voting Configuration*, review available settings  
      (no changes required), then select **Next**.

- [ ] On *Select Quorum Witness*, make sure:

    ```
    Configure a disk witness
    ```

    is selected, then select **Next**.

- [ ] On *Configure Storage Witness*, select:

    ```
    Cluster Disk 3
    ```

    then select **Next**.

- [ ] On the *Confirmation* page, select **Next**.

- [ ] On the *Summary* page, select **Finish**.

### Exercise 2 — Task 1: Remotely connect to a cluster

- [ ] Sign in to **LON-CL1** as **Contoso\Administrator** using the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] Select **Start → Windows Tools**.

- [ ] Open **Failover Cluster Manager**.

- [ ] In the left pane, right-click **Failover Cluster Manager**, then select:

    ```
    Connect to Cluster
    ```

- [ ] In the **Select Cluster** dialog box, in the **Cluster name** field, enter:

    ```
    Cluster1.Contoso.com
    ```

- [ ] Select **OK**.

- [ ] Expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

### Exercise 2 — Task 2: Check the assigned votes in the Nodes section

- [ ] Switch to **LON-SVR2**.

- [ ] Select **Start**, then open **Windows PowerShell ISE**.

- [ ] In the PowerShell ISE console, run the following cmdlet:

    ```powershell
    Get-ClusterNode | Select Name, NodeWeight, ID, State
    ```

- [ ] Verify that each cluster node has:

    ```
    NodeWeight = 1
    ```

    This confirms the node **has a quorum vote assigned** and is being managed correctly by the cluster.

### Exercise 2 — Task 3: Verify the status of the disk witness

- [ ] On **LON-SVR2**, in **Windows PowerShell ISE**, run the following cmdlet:

    ```powershell
    Get-ClusterQuorum | Select Cluster, QuorumResource, QuorumType
    ```

- [ ] Review the output to confirm:

    - The **QuorumResource** shows the current Disk Witness  
    - The **QuorumType** reflects that the cluster is using a **disk witness** quorum model

### Exercise 2 — Task 4: Add a node to the cluster

- [ ] On **LON-SVR2**, open **Failover Cluster Manager**.

- [ ] In the left pane, select **Nodes**.

- [ ] In the **Actions** pane, select:

    ```
    Add Node
    ```

- [ ] In the *Before You Begin* page, select **Next**.

- [ ] On the *Select Servers* page, in **Enter server name**, type:

    ```
    LON-SVR4
    ```

    then select **Add**.

- [ ] Select **Next**.

- [ ] On the *Validation Warning* page, select **Next**.

- [ ] Allow the validation tests to run with the default settings.

- [ ] On the *Summary* page of the **Validate a Configuration** Wizard, select **Finish**.

- [ ] In the **Add Node Wizard**, on the *Confirmation* page, select **Next**.

- [ ] On the *Summary* page, select **Finish**.

- [ ] Verify LON-SVR4 now appears as a **node** in the cluster.

### Exercise 2 — Task 5: Verify the assigned votes

- [ ] On **LON-SVR2**, open **Windows PowerShell ISE**.

- [ ] In the PowerShell ISE console, run the following cmdlet:

    ```powershell
    Get-ClusterNode | Select Name, NodeWeight, ID, State
    ```

- [ ] Verify that the newly added node **LON-SVR4** now appears in the list.

- [ ] Confirm that **NodeWeight = 1** for all nodes, indicating:

    - The node has a quorum vote assigned  
    - The cluster is correctly managing quorum participation

- [ ] Close **Windows PowerShell ISE**.

### Exercise 3 — Task 1: Simulate server failure

- [ ] Switch to **LON-SVR2** and open **Server Manager**.

- [ ] From **Server Manager**, select **Tools → Failover Cluster Manager**.

- [ ] In **Failover Cluster Manager**, expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] In the **Owner Node** column, identify which server currently owns **ContosoFS**  
      (either **LON-SVR2** or **LON-SVR3**).

- [ ] If the **current owner is NOT LON-SVR3**, then:

    - Right-click **ContosoFS**
    - Select **Move → Select Node**
    - Choose:

        ```
        LON-SVR3
        ```

    - Select **OK**

- [ ] Shut down **LON-SVR3** to simulate a server failure.

---

### Exercise 3 — Task 2: Verify functionality and file availability in Cluster1

- [ ] Sign in to **LON-DC1** as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Open **File Explorer** on LON-DC1.

- [ ] In the address bar, enter:

    ```
    \\ContosoFS\
    ```

    then press **Enter**.

- [ ] Verify that the **Docs** folder is accessible.

- [ ] Inside the **Docs** folder, create a new text document named:

    ```
    test2.txt
    ```

---

### Exercise 3 — Task 3: Validate whether the file is still available

- [ ] Start the **LON-SVR3** VM.

- [ ] Wait for **LON-SVR3** to fully start.

- [ ] Sign in as **Contoso\Administrator** with:

    ```powershell
    Pa55w.rd
    ```

- [ ] Switch to **LON-SVR2**.

- [ ] In **Failover Cluster Manager**, expand:

    ```
    Cluster1.Contoso.com → Roles
    ```

- [ ] Right-click **ContosoFS**, select **Move → Select Node**.

- [ ] Choose:

    ```
    LON-SVR3
    ```

    then select **OK**.

- [ ] Switch back to **LON-DC1**.

- [ ] Open **File Explorer** → enter:

    ```
    \\ContosoFS\
    ```

- [ ] Verify that the **Docs** folder is still accessible.

- [ ] Inside **Docs**, create another test file:

    ```
    test3.txt