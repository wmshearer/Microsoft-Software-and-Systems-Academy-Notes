# Implement Storage Spaces and Data Deduplication

## Exercise 1: Creating a Storage Space

### Task 1 — Create a storage pool from six disks

- [ ] Sign in to **LON-SVR1** as **Contoso\Administrator** (Pa55w.rd).

- [ ] Select **Start → Server Manager**.

- [ ] In Server Manager: **File and Storage Services → Storage Pools**.

- [ ] In **STORAGE POOLS** pane → **TASKS → New Storage Pool**.

- [ ] In the wizard → **Next**.

- [ ] Name: **StoragePool1** → **Next**.

- [ ] Select the first six physical disks → **Next**.

- [ ] Confirm selections → **Create**.

- [ ] Wait for completion → **Close**.

### Task 2 — Create a three-way mirrored virtual disk

- [ ] In **Server Manager**, under **Storage Pools**, select **StoragePool1**.

- [ ] In the **VIRTUAL DISKS** pane, select **TASKS → New Virtual Disk**.

- [ ] In the **Select the storage pool** dialog box, select **StoragePool1**, then select **OK**.

- [ ] In the **New Virtual Disk Wizard**, on the *Before you begin* page, select **Next**.

- [ ] On *Specify the virtual disk name*, in **Name**, enter **Mirrored Disk**, then select **Next**.

- [ ] On *Specify enclosure resiliency*, select **Next**.

- [ ] On *Select the storage layout*, in **Layout**, select **Mirror**, then select **Next**.

- [ ] On *Configure the resiliency settings*, select **Three-way mirror**, then select **Next**.

- [ ] On *Specify the provisioning type*, select **Thin**, then select **Next**.

- [ ] On *Specify the size of the virtual disk*, enter **10** for the size, then select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On *View results*, wait until the task completes.

- [ ] Ensure **Create a volume when this wizard closes** is checked, then select **Close**.

- [ ] In the **New Volume Wizard**, on *Before you begin*, select **Next**.

- [ ] On *Select the server and disk*, in **Disk**, select the **Mirrored Disk** virtual disk, then select **Next**.

- [ ] On *Specify the size of the volume*, accept the default size, then select **Next**.

- [ ] On *Assign to a drive letter or folder*, ensure drive letter **H** is selected, then select **Next**.

- [ ] On *Select file system settings*, set **File system** to **ReFS**, set **Volume label** to **Mirrored Volume**, then select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On the *Completion* page, wait until the creation completes, then select **Close**.
### Task 3 — Copy a file to the volume and verify it displays in File Explorer

- [ ] Select **Start**, type **Command Prompt**, and press **Enter**.

- [ ] At the command prompt, run the following command:

```powershell
Copy C:\Windows\System32\write.exe H:\
```
### Task 3 — Copy a file to the volume and verify it displays in File Explorer

- [ ] Select **Start**, type **Command Prompt**, and press **Enter**.

- [ ] At the command prompt, run the following command:

    ```powershell
    Copy C:\Windows\System32\write.exe H:\
    ```

- [ ] Close **Command Prompt**.

- [ ] Select the **File Explorer** icon on the taskbar.

- [ ] In **File Explorer**, in the navigation pane, select **Mirrored Volume (H:)**.

- [ ] Verify that **write.exe** appears in the file list.

- [ ] Close **File Explorer**.

### Task 4 — Add and remove disks from the storage pool

- [ ] In **Server Manager**, in the **STORAGE POOLS** pane, right-click **StoragePool1** and select **Add Physical Disk**.

- [ ] In the **Add Physical Disk** window, select the **first disk** in the list, then select **OK**.

- [ ] Verify that the disk is added to **StoragePool1** and appears in the **PHYSICAL DISKS** section.

- [ ] In the **PHYSICAL DISKS** section, right-click the **first disk** in the list and select **Remove Disk**.

- [ ] When prompted with **Remove Physical Disk**, select **Yes**.

- [ ] Wait while the disk is removed from the storage pool.

- [ ] In the confirmation message, select **OK**.

# Exercise 2 – Task 1
### Task 1 — Use the Get-PhysicalDisk cmdlet to review all available disks on the system

- [ ] On **LON-SVR1**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell ISE command prompt, enter the following command:

    ```powershell
    Get-PhysicalDisk
    ```
### Task 2 — Create a new storage pool

- [ ] At the **Windows PowerShell ISE** command prompt, enter the following command:

    ```powershell
    $canpool = Get-PhysicalDisk -CanPool $true
    ```

- [ ] At the command prompt, enter the next command:

    ```powershell
    New-StoragePool -FriendlyName "TieredStoragePool" -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks $canpool
    ```

- [ ] Open **File Explorer**, then browse to **E:\Labfiles\Mod04**.

- [ ] Right-click **mod4.ps1**, select **Run with PowerShell**, and if prompted, select **Y**.

### Task 3 — Review the media types

- [ ] On **LON-SVR1**, at the **Windows PowerShell ISE** command prompt, enter the following command:

    ```powershell
    Get-StoragePool -FriendlyName TieredStoragePool | Get-PhysicalDisk | Select FriendlyName, MediaType, Usage, BusType
    ```
### Task 4 — Specify the media type for the sample disks, and verify the media type is changed

- [ ] On **LON-SVR1**, at the **Windows PowerShell ISE** command prompt, enter the following:

    ```powershell
    Set-PhysicalDisk -FriendlyName PhysicalDisk1 -MediaType SSD
    ```

- [ ] Then enter the next command:

    ```powershell
    Set-PhysicalDisk -FriendlyName PhysicalDisk2 -MediaType HDD
    ```

- [ ] Verify the change by running:

    ```powershell
    Get-PhysicalDisk | Select FriendlyName, MediaType, Usage, BusType
    ```
### Task 5 — Create pool-level storage tiers by using Windows PowerShell

- [ ] On **LON-SVR1**, at the **Windows PowerShell ISE** command prompt, enter the following command:

    ```powershell
    New-StorageTier -StoragePoolFriendlyName TieredStoragePool -FriendlyName HDD_Tier -MediaType HDD
    ```

- [ ] Then enter the next command:

    ```powershell
    New-StorageTier -StoragePoolFriendlyName TieredStoragePool -FriendlyName SSD_Tier -MediaType SSD
    ```
 
### Task 6 — Create a new virtual disk with storage tiering by using the New Virtual Disk Wizard

- [ ] In **Server Manager**, in the **Storage Pools** pane, select **Refresh**, then select **TieredStoragePool**.

- [ ] In the **VIRTUAL DISKS** pane, select **TASKS → New Virtual Disk**.

- [ ] In the **Select the storage pool** dialog box, choose **TieredStoragePool**, then select **OK**.

- [ ] In the **New Virtual Disk Wizard**, on the *Before you begin* page, select **Next**.

- [ ] On the *Specify the virtual disk name* page, in **Name**, enter **TieredVirtDisk**.  
      Check **Create storage tiers on this virtual disk**, then select **Next**.

- [ ] On *Specify enclosure resiliency*, select **Next**.

- [ ] On *Select the storage layout*, choose **Simple**, then select **Next**.

- [ ] On *Specify the provisioning type*, select **Next**.

- [ ] On *Specify the size of the virtual disk*, enter **2** in both size boxes, then select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On *View results*, wait until the task completes.

- [ ] Ensure **Create a volume when this wizard closes** is selected, then select **Close**.

- [ ] In the **New Volume Wizard**, on *Before you begin*, select **Next**.

- [ ] On *Select the server and disk*, in the **Disk** pane, select **TieredVirtDisk**, then select **Next**.

- [ ] On *Specify the size of the volume*, accept the default selection, then select **Next**.

- [ ] On *Assign to a drive letter or folder*, ensure **R** is selected as the drive letter, then select **Next**.

- [ ] On *Select file system settings*, set **Volume label** to **Tiered Volume**, then select **Next**.

- [ ] On *Confirm selections*, select **Create**.

- [ ] On the *Completion* page, wait until the volume is created, then select **Close**.

- [ ] In **Server Manager**, right-click the virtual disk you created, then select **Properties**.

- [ ] Review the **General** and **Health** tabs, then select **OK**.

# Exercise 3 — Install Data Deduplication

### Task 1 — Install the Data Deduplication role service

- [ ] Sign in to **LON-SVR1** as **Contoso\Administrator** (Pa55w.rd).

- [ ] Select **Start → Server Manager**.

- [ ] In the navigation pane, select **Dashboard**.

- [ ] In the details pane, select **Add roles and features**.

- [ ] In the *Before you begin* page, select **Next**.

- [ ] On the *Select installation type* page, select **Next**.

- [ ] On the *Select destination server* page, select **Next**.

- [ ] On the *Select server roles* page:  
      Expand **File and Storage Services (4 of 12 installed)**.  
      Expand **File and iSCSI Services (3 of 11 installed)**.  
      Check **Data Deduplication**.  
      Select **Next**.

- [ ] On the *Select features* page, select **Next**.

- [ ] On the *Confirm installation selections* page, select **Install**.

- [ ] When installation completes, select **Close**.

### Task 2 — Check the status of Data Deduplication

- [ ] Open **Windows PowerShell ISE**.

- [ ] At the command prompt, run:

    ```powershell
    Get-DedupVolume
    ```

- [ ] Then run:

    ```powershell
    Get-DedupStatus
    ```

### Task 3 — Verify the VM performance

- [ ] In **Windows PowerShell ISE**, run:

    ```powershell
    Measure-Command -Expression {Get-ChildItem -Path E:\ -Recurse}
    ```

- [ ] Record the values returned by the command for later comparison.