
### Exercise 1 — Task 1: Verify and configure the Hyper-V server role

- [ ] On **LON-SVR1**, sign in as **Contoso\Administrator** with the password **Pa55w.rd**.

- [ ] Select **Start**, then select **Server Manager**.

- [ ] In **Server Manager**, select **Tools → Hyper-V Manager**.

- [ ] In **Hyper-V Manager**, select **LON-SVR1**, then select **Hyper-V Settings** in the **Actions** pane.

- [ ] In the **Hyper-V Settings for LON-SVR1** window, select each available setting in the left pane and review its description.

- [ ] In the settings list, select **Virtual Hard Disks**.

- [ ] On the **Virtual Hard Disks** page, select **Browse**.

- [ ] Browse to and select **E:\VirtualMachines**, then select **Select Folder**.

- [ ] In the settings list, select **Virtual Machines**.

- [ ] On the **Virtual Machines** page, select **Browse**.

- [ ] Browse to and select **E:\VirtualMachines**, then select **Select Folder**.

- [ ] Confirm that **E:\VirtualMachines** is now the default location for **Virtual Hard Disks** and **Virtual Machine** files.

- [ ] Select **OK** to close the **Hyper-V Settings for LON-SVR1** window.

### Exercise 2 — Task 1: Create an external network

- [ ] In **Hyper-V Manager**, select **LON-SVR1**.

- [ ] In the **Actions** pane, select **Virtual Switch Manager**.

- [ ] In the **Virtual Switch Manager for LON-SVR1** window, select **New virtual network switch**.

- [ ] In the **Create virtual switch** pane, select **External**, then select **Create Virtual Switch**.

- [ ] In the **Virtual Switch Properties** pane, in the **Name** box, enter **Physical Network**.

- [ ] In the **Connection type** area, select **External network**.

- [ ] In the **External network** drop-down menu, select **Microsoft Hyper-V Network Adapter #3**.

- [ ] Ensure the checkbox **Allow management operating system to share this network adapter** is selected.

- [ ] Select **OK**.

- [ ] In the **Apply Networking Changes** dialog box, review the warning and select **Yes**.

- [ ] Open **Server Manager**, select **Local Server**, and verify that a new network adapter named  
      **vEthernet (Physical Network)** now appears.


### Exercise 2 — Task 2: Create a private network

- [ ] On **LON-SVR1**, open **Hyper-V Manager**.

- [ ] In the **Actions** pane, select **Virtual Switch Manager**.

- [ ] In the **Virtual Switch Manager for LON-SVR1** window, select **New virtual network switch**.

- [ ] In the **Create virtual switch** pane, select **Private**, then select **Create Virtual Switch**.

- [ ] In the **Virtual Switch Properties** pane, in the **Name** box, enter **Isolated Network**.

- [ ] In the **Connection type** area, verify that **Private network** is selected.

- [ ] Select **OK**.

- [ ] Open **Server Manager**, select **Local Server**, and verify that **no new network adapter** appears  
      (because private networks do *not* connect to a physical NIC).

- [ ] Refresh Server Manager if needed to confirm the changes.


### Exercise 2 — Task 3: Create an internal network

- [ ] On **LON-SVR1**, open **Hyper-V Manager**.

- [ ] In the **Actions** pane, select **Virtual Switch Manager**.

- [ ] In the **Virtual Switch Manager for LON-SVR1** window, select **New virtual network switch**.

- [ ] In the **Create virtual switch** pane, select **Internal**, then select **Create Virtual Switch**.

- [ ] In the **Virtual Switch Properties** pane, in the **Name** box, enter **Host Internal Network**.

- [ ] In the **Connection type** area, verify that **Internal network** is selected.

- [ ] Select **OK**.

- [ ] Open **Server Manager**, select **Local Server**, and verify that a new network adapter named  
      **vEthernet (Host Internal Network)** has been created.

- [ ] Confirm that the new adapter is using **DHCP** to obtain an IP address  
      (Refresh Server Manager if necessary).

### Exercise 3 — Task 1: Create a Generation 2 VM

- [ ] On **LON-SVR1**, select **File Explorer** from the taskbar.

- [ ] In File Explorer, browse to **E:\VirtualMachines**.

- [ ] Select the **Home** tab, then select the **New Folder** icon twice to create two new folders.

- [ ] Rename the folders to:

    - **LON-GUEST1**
    - **LON-GUEST2**

- [ ] Close **File Explorer**.

- [ ] In **Hyper-V Manager**, in the **Actions** pane, select **New → Virtual Machine**.

- [ ] In the **New Virtual Machine Wizard**, on the *Before You Begin* page, select **Next**.

- [ ] On *Specify Name and Location*, enable  
      **Store the virtual machine in a different location**, then enter:

    - **Name:** LON-GUEST2  
    - **Location:** E:\VirtualMachines\LON-GUEST2\

    Select **Next**.

- [ ] On *Specify Generation*, select **Generation 2**, then select **Next**.

- [ ] On *Assign Memory*, enter **1024 MB** for Startup memory, then select **Next**.

- [ ] On *Configure Networking*, select **Isolated Network**, then select **Next**.

- [ ] On *Connect Virtual Hard Disk*, choose **Create a virtual hard disk**, then enter:

    - **Name:** LON-GUEST2.vhdx  
    - **Location:** E:\VirtualMachines\LON-GUEST2  
    - **Size:** 127 GB  

    Select **Finish**.

- [ ] In Hyper-V Manager, right-click **LON-GUEST2** and select **Settings**.

- [ ] In **Settings**, select **SCSI Controller → DVD Drive → Add**.

- [ ] Under **DVD Drive**, choose **Image file** and enter:

    ```powershell
    E:\ISOs\Win2022_Eval.iso
    ```

    Select **Apply**.

- [ ] In the **Hardware** section, select **Firmware**.

- [ ] In **Boot order**, select **Network Adapter**, then **Move Down** twice.

- [ ] Select **DVD Drive**, then **Move Up** until it is **first** in the list.  
      Select **OK**.

- [ ] Right-click **LON-GUEST2** and select **Connect**.

- [ ] In the VM window, select **Start**.

- [ ] When prompted with “Press any key to boot from CD or DVD”:

    - Select the **Ctrl-Alt-Delete** button in the VM window.  
    - Then press any key on your keyboard.

- [ ] In the Windows Setup screen, select **Next → Install now**.

- [ ] Choose **Windows Server 2022 Datacenter Evaluation**, then select **Next**.

- [ ] Accept the license terms → **Next**.

- [ ] Select **Custom: Install Windows only (advanced)**.

- [ ] Select **Drive 0 Unallocated Space**, then select **Next**.

- [ ] Wait for installation and automatic restarts.

- [ ] When prompted to change the Administrator password, select **OK**.

- [ ] Enter the password:

    ```powershell
    Pa55w.rd
    ```

- [ ] Press **Enter** twice to confirm the change.

- [ ] After login, wait for **sconfig.exe** to appear.

- [ ] Shut down the VM: inside VM → **Shut Down → Shut Down**.

- [ ] Close the VM connection window.

### Exercise 3 — Task 2: Create a Generation 1 VM

- [ ] In **Hyper-V Manager**, in the **Actions** pane, select **New → Hard Disk**.

- [ ] In the **New Virtual Hard Disk Wizard**, on *Before You Begin*, select **Next**.

- [ ] On *Choose Disk Format*, select **VHD**, then select **Next**.

- [ ] On *Choose Disk Type*, select **Differencing**, then select **Next**.

- [ ] On *Specify Name and Location*, enter:

    - **Name:** LON-GUEST1.vhd  
    - **Location:** E:\VirtualMachines\LON-GUEST1\

    Then select **Next**.

- [ ] On *Configure Disk*, enter the parent disk path:

    ```powershell
    E:\VirtualMachines\ServerCoreBase.vhd
    ```

    Then select **Finish**.

- [ ] Select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell command prompt, create the VM by running:

    ```powershell
    New-VM -Name LON-GUEST1 -MemoryStartupBytes 1024MB -VHDPath "E:\VirtualMachines\LON-GUEST1\LON-GUEST1.vhd" -SwitchName "Isolated Network"
    ```

- [ ] Close **Windows PowerShell ISE**.

### Exercise 3 — Task 3: Configure VMs

- [ ] On **LON-SVR1**, open **Hyper-V Manager**.

- [ ] Right-click **LON-GUEST1**, then select **Settings**.

- [ ] In **Settings for LON-GUEST1**, review the list of hardware.

- [ ] In the **Hardware** section, select **Memory**.

- [ ] Check **Enable Dynamic Memory**.

- [ ] In **Maximum RAM**, enter **4096**.

- [ ] In the **Hardware** section, select **Network Adapter**.

- [ ] In **Bandwidth Management**, check **Enable bandwidth management**.

- [ ] In **Minimum bandwidth**, enter **10**.

- [ ] In **Maximum bandwidth**, enter **100**.

- [ ] In the **Management** section, select **Integration Services**.

- [ ] Check **Guest services**, then select **OK**.

---

- [ ] Ensure **LON-GUEST2** is completely shut down before continuing.

- [ ] In **Hyper-V Manager**, right-click **LON-GUEST2**, then select **Settings**.

- [ ] In **Settings for LON-GUEST2**, review the hardware list and note differences from LON-GUEST1.

- [ ] In the **Hardware** section, select **Security**, and review available options.

- [ ] In **Memory**, verify **Enable Dynamic Memory** is **not** selected.

- [ ] In the **Hardware** section, expand **Hard Drive**, then select **Quality of Service**.

- [ ] Check **Enable Quality of Service management**.

- [ ] In **Minimum**, enter **10**.

- [ ] In the **Management** section, select **Integration Services**.

- [ ] Check **Guest services**, then select **OK**.

### Exercise 3 — Task 4: Create checkpoints

- [ ] On **LON-SVR1**, open **Hyper-V Manager**.

- [ ] Right-click **LON-GUEST2**, then select **Checkpoint**.

- [ ] Verify a checkpoint appears in the **Checkpoints** pane.

- [ ] Right-click **LON-GUEST2**, then select **Start**.

- [ ] Right-click **LON-GUEST2**, then select **Connect**.

- [ ] In the VM window, sign in as **Administrator** using:

    ```powershell
    Pa55w.rd
    ```

- [ ] Switch back to **LON-SVR1**.

- [ ] In **Hyper-V Manager**, right-click **LON-GUEST2**, then select **Settings**.

- [ ] In **Settings for LON-GUEST2**, in the **Hardware** section, select **Add Hardware**.

- [ ] In **Add Hardware**, select **Network Adapter**, then select **Add**.

- [ ] In **Network Adapter** settings, set **Virtual switch** to:

    ```
    Host Internal Network
    ```

    Then select **OK**.

- [ ] Back in **Hyper-V Manager**, right-click **LON-GUEST2**, then select **Checkpoint**.

- [ ] Review the prompt in **Virtual Machine Checkpoint** window, then select **OK**.

- [ ] In the **Checkpoints** pane, right-click the **most recent checkpoint**, then select **Apply**.

- [ ] In the **Apply Checkpoint** dialog box, select **Apply**.

- [ ] Confirm that **LON-GUEST2** now shows **Status: Off**  
      (Production checkpoints power the VM off when applied).

### Exercise 3 — Task 5: Enable host resource protection

- [ ] On **LON-SVR1**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell command prompt, run the following command:

    ```powershell
    Set-VMProcessor LON-GUEST2 -EnableHostResourceProtection $true
    ```

- [ ] Close **Windows PowerShell ISE**.

### Exercise 3 — Task 6: Export a VM

- [ ] On **LON-SVR1**, open **Hyper-V Manager**.

- [ ] Right-click **LON-GUEST2**, then select **Export**.

- [ ] In the **Export Virtual Machine** dialog box, in the **Location** field, enter:

    ```powershell
    E:\VirtualMachines\Guest2-Bak
    ```

- [ ] Select **Export** to begin the export process.

- [ ] Wait for the VM export to complete before proceeding.

## Exercise 4 — Task 1: Enable nested virtualization

- [ ] On **LON-SVR1**, select **Start**, then open **Windows PowerShell ISE**.

- [ ] At the PowerShell prompt, run the following command:

    ```powershell
    Set-VMProcessor -VMName LON-GUEST1 -ExposeVirtualizationExtensions $true
    ```

- [ ] Then run the next command to enable MAC address spoofing:

    ```powershell
    Get-VMNetworkAdapter -VMName LON-GUEST1 | Set-VMNetworkAdapter -MacAddressSpoofing On
    ```

- [ ] Then run the next command to increase the VM’s startup memory:

    ```powershell
    Set-VM -VMName LON-GUEST1 -MemoryStartupBytes 4GB
    ```

## Exercise 4 — Task 2: Enable Hyper-V on LON-GUEST1

- [ ] On **LON-SVR1**, at the **Windows PowerShell ISE** prompt, start the VM by running:

    ```powershell
    Start-VM LON-GUEST1
    ```

- [ ] Wait for **LON-GUEST1** to fully start.

- [ ] At the PowerShell prompt on **LON-SVR1**, enter the VM session:

    ```powershell
    Enter-PSSession -VMName LON-GUEST1
    ```

- [ ] When prompted, sign in as **Contoso\Administrator** using:

    ```powershell
    Pa55w.rd
    ```

- [ ] Inside the VM session, install Hyper-V by running:

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeAllSubFeature -IncludeManagementTools -Restart
    ```

- [ ] Wait for **LON-GUEST1** to restart.  
      (This may take several minutes and the VM may restart multiple times.)

- [ ] After restart, open the VM connection window and sign in as **Administrator** using:

    ```powershell
    Pa55w.rd
    ```

- [ ] When **sconfig.exe** loads, enter:

    ```powershell
    15
    ```

      to exit to the PowerShell command line.

- [ ] At the VM command prompt, verify Hyper-V installation:

    ```powershell
    Get-WindowsFeature Hyper*
    ```

      Confirm that **Hyper-V** shows an **X** (installed).

- [ ] In the VM window, select **Shut Down**, then confirm shutdown.

- [ ] Close the **LON-GUEST1 on LON-SVR1** Virtual Machine Connection window.
