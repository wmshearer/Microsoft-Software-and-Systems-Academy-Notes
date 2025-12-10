# Lab — AD Trusts & Child Domains (Contoso / Trey Research)

---

## Exercise 1: Implement Forest Trusts

### Scenario

Contoso Corporation is working on several projects with a partner organization named **Trey Research**.  
They’ve deployed a WAN between **London (Contoso)** and **Munich (Trey Research)**.

You must:

- Implement and validate a **forest trust** between the two forests  
- Configure the trust to allow access **only to selected servers in London**

**Main tasks:**

1. Configure **stub zones** for DNS name resolution  
2. Configure a **forest trust** with **selective authentication**  
3. Configure a **server** for selective authentication

---

### Task 1: Configure Stub Zones for DNS Name Resolution

#### On LON-DC1 (Contoso)

1. Sign in to **LON-DC1** as:  
   - User: `Contoso\Administrator`  
   - Password: `Pa55w.rd`
2. **Server Manager** opens automatically.
3. In Server Manager, select **Tools** > **DNS**.
4. In the **DNS** tree:
   - Expand **LON-DC1**
   - Right-click **Forward Lookup Zones** > **New Zone**
5. In the **New Zone Wizard**:
   - Select **Next**
6. **Zone Type** page:
   - Select **Stub zone**
   - Select **Next**
7. **Active Directory Zone Replication Scope** page:
   - Select **To all DNS servers running on domain controllers in this forest: Contoso.com**
   - Select **Next**
8. **Zone name**:
   - Enter: `treyresearch.net`
   - Select **Next**
9. **Master DNS Servers** page:
   - Select **Click here to add an IP Address or DNS Name**
   - Enter: `172.16.10.10`
   - Press **Enter**
   - Select **Next**
10. **Completing the New Zone Wizard**:
    - Select **Next**, then **Finish**
11. Under **Forward Lookup Zones**:
    - Right-click the new stub zone **treyresearch.net** > **Transfer from Master**
    - Right-click **treyresearch.net** again > **Refresh**
12. Confirm the **treyresearch.net** stub zone now contains records.
13. Close **DNS Manager**.

#### On TREY-DC1 (Trey Research)

1. Switch to **TREY-DC1**.
2. Sign in as:
   - User: `TreyResearch\Administrator`
   - Password: `Pa55w.rd`
3. In **Server Manager**, select **Tools** > **DNS**.
4. In the **DNS** tree:
   - Expand **TREY-DC1**
   - Right-click **Forward Lookup Zones** > **New Zone**
5. In the **New Zone Wizard**:
   - Select **Next**
6. **Zone Type** page:
   - Select **Stub zone**
   - Select **Next**
7. **Active Directory Zone Replication Scope** page:
   - Select **To all DNS servers running on domain controllers in this forest: TreyResearch.net**
   - Select **Next**
8. **Zone name**:
   - Enter: `Contoso.com`
   - Select **Next**
9. **Master DNS Servers** page:
   - Select **Click here to add an IP Address or DNS Name**
   - Enter: `172.16.0.10`
   - Press **Enter**
   - Select **Next**
10. **Completing the New Zone Wizard**:
    - Select **Next**, then **Finish**
11. Under **Forward Lookup Zones**:
    - Right-click **Contoso.com** > **Transfer from Master**
    - Right-click **Contoso.com** again > **Refresh**
12. Confirm the **Contoso.com** stub zone now contains records.
13. Close **DNS Manager**.

---

### Task 2: Configure a Forest Trust with Selective Authentication

#### On LON-DC1 (Contoso)

1. In **Server Manager**, select **Tools** > **Active Directory Domains and Trusts**.
2. In the console, right-click **Contoso.com** > **Properties**.
3. On the **Trusts** tab:
   - Select **New Trust**.
4. **New Trust Wizard**:
   - Select **Next**
5. **Trust Name** page:
   - Name: `treyresearch.net`
   - Select **Next**
6. **Trust Type** page:
   - Select **Forest trust**
   - Select **Next**
7. **Direction of Trust** page:
   - Select **One-way: outgoing**
   - Select **Next**
8. **Sides of Trust** page:
   - Select **Both this domain and the specified domain**
   - Select **Next**
9. **User Name and Password** page (for TreyResearch):
   - User name: `Administrator`
   - Password: `Pa55w.rd`
   - Select **Next**
10. **Outgoing Trust Authentication Level – Local Forest** page:
    - Select **Selective authentication**
    - Select **Next**
11. **Trust Selections Complete**:
    - Select **Next**
12. **Trust Creation Complete**:
    - Select **Next**
13. **Confirm Outgoing Trust**:
    - Select **Next**
14. **Completing the New Trust Wizard**:
    - Select **Finish**
15. Back in **Contoso.com Properties**:
    - On the **Trusts** tab, under **Domains trusted by this domain (outgoing trusts)**:
      - Select **treyresearch.net**
      - Select **Properties**
16. In **treyresearch.net Properties** dialog:
    - Select **Validate**
    - Confirm message:  
      > “The trust has been validated. It is in place and active.”
    - Select **OK**
    - At the prompt, select **No** (don’t reset).
17. Select **OK** to close **treyresearch.net Properties**.
18. Select **OK** again to close **Contoso.com Properties**.
19. Close **Active Directory Domains and Trusts**.

---

### Task 3: Configure a Server for Selective Authentication

#### Grant “Allowed to authenticate” to TreyResearch\IT on LON-SVR2

1. On **LON-DC1**, open **Server Manager**.
2. Select **Tools** > **Active Directory Users and Computers**.
3. In **ADUC**, on the **View** menu:
   - Select **Advanced Features**.
4. Expand **Contoso.com** > **Computers**.
5. Right-click **LON-SVR2** > **Properties**.
6. Go to the **Security** tab, then select **Add**.
7. In **Select Users, Computers, Service Accounts, or Groups**:
   - Select **Locations**
   - Choose **TreyResearch.net**
   - Select **OK**
8. In **Enter the object name to select**, type: `IT`
   - Select **Check Names**
9. When prompted for credentials:
   - User: `TreyResearch\Administrator`
   - Password: `Pa55w.rd`
   - Select **OK**
10. Back in the object picker, select **OK**.
11. In **LON-SVR2 Properties**:
    - Ensure **IT (TreyResearch\IT)** is selected
    - Check **Allow** for **Allowed to authenticate**
    - Select **OK**

#### Share a Folder for TreyResearch\IT on LON-SVR2

1. Switch to **LON-SVR2**.
2. Sign in as:
   - User: `Contoso\Administrator`
   - Password: `Pa55w.rd`
3. On the taskbar, select **File Explorer**.
4. In **This PC**, open **Local Disk (C:)**.
5. In the details pane:
   - Right-click empty space > **New** > **Folder**
   - Name: `IT-Data` and press **Enter**
6. Right-click **IT-Data** > **Properties**.
7. Go to the **Sharing** tab:
   - Select **Advanced Sharing**.
8. In **Advanced Sharing**:
   - Check **Share this folder**
   - Select **Permissions**
9. In **Permissions for IT-Data**:
   - Select **Add**
10. In **Select Users, Computers, Service Accounts, or Groups**:
    - Select **Locations**
    - Choose **TreyResearch.net**
    - Select **OK**
    - In **Enter the object name to select**, type `IT`
    - Select **Check Names**
11. When prompted for credentials:
    - User: `TreyResearch\Administrator`
    - Password: `Pa55w.rd`
    - Select **OK**
12. Select **OK** to add **TreyResearch\IT**.
13. In **Permissions for IT-Data**, configure as needed (at least **Read**, or more as desired), then select **OK**.
14. In **Advanced Sharing**, select **OK**.

#### Test Access from TreyResearch

1. Switch to **TREY-DC1**.
2. Sign **out**, then sign back in as:
   - User: `TreyResearch\Abbi`
   - Password: `Pa55w.rd`
3. Select **Start**, type:
   - `\\LON-SVR2.Contoso.com\IT-Data`
   - Press **Enter**
4. Confirm the **IT-Data** folder opens successfully.
5. Sign out of **TREY-DC1**.

**Result:** Forest trust with **selective authentication** is implemented and validated, and access is restricted to selected servers/folders (LON-SVR2 \ IT-Data) for TreyResearch users.

---

## Exercise 2: Implement Child Domains in AD DS

### Scenario

Contoso Corporation is deploying a new **child domain** in the **Contoso.com** forest for **North America**.  
First DC is in **Toronto**; new domain name: **na.Contoso.com**.

You must:

- Install a **domain controller** in the new child domain  
- Verify the **default trust configuration**

**Main tasks:**

1. Install a DC in a **child domain**  
2. Verify the **default trust** configuration

---

### Task 1: Install a Domain Controller in a Child Domain

#### On TOR-DC1

1. Sign in to **TOR-DC1** as:
   - User: `Contoso\Administrator`
   - Password: `Pa55w.rd`
2. Open **Server Manager**.
3. Select **Manage** > **Add Roles and Features**.
4. **Before you begin**:
   - Select **Next**
5. **Select installation type**:
   - Ensure **Role-based or feature-based installation** is selected
   - Select **Next**
6. **Select destination server**:
   - Verify **Select a server from the server pool** is chosen
   - Ensure **TOR-DC1.Contoso.com** is highlighted
   - Select **Next**
7. **Select server roles**:
   - Check **Active Directory Domain Services**
8. On the **Add features that are required for Active Directory Domain Services?** dialog:
   - Select **Add Features**
9. Back on **Select server roles**:
   - Select **Next**
10. **Select features**:
    - Select **Next**
11. **Active Directory Domain Services** page:
    - Select **Next**
12. **Confirm installation selections**:
    - Select **Install**
13. After AD DS binaries install:
    - Select **Promote this server to a domain controller**

#### Promote TOR-DC1 to DC in Child Domain

1. In **Deployment Configuration**:
   - Select **Add a new domain to an existing forest**
   - Confirm **Select domain type** = **Child Domain**
   - Confirm **Parent domain name** = `Contoso.com`
   - **New domain name**: `na`
2. Confirm credentials:
   - **Supply the credentials to perform this operation** should be `Contoso\Administrator (Current user)`
   - If not, click **Change** and enter:
     - User: `Contoso\Administrator`
     - Password: `Pa55w.rd`
   - Select **Next**
3. **Domain Controller Options**:
   - Domain functional level: **Windows Server 2016**
   - Check **Domain Name System (DNS) server**
   - Check **Global Catalog (GC)**
   - Site name: **Default-First-Site-Name**
   - Directory Services Restore Mode (DSRM) password:
     - `Pa55w.rd` (both boxes)
   - Select **Next**
4. **DNS Options**:
   - Select **Next**
5. **Additional Options**:
   - Select **Next**
6. **Paths**:
   - Select **Next**
7. **Review Options**:
   - Select **Next**
8. **Prerequisites Check**:
   - Confirm no blocking issues
   - If warning about **“Allow cryptography algorithms compatible with Windows NT 4.0”**, you can ignore.
   - Select **Install**
9. Allow configuration to complete; **server restarts automatically**.

---

### Task 2: Verify the Default Trust Configuration

#### On TOR-DC1 (NA Domain)

1. Sign in to **TOR-DC1** as:
   - User: `NA\Administrator`
   - Password: `Pa55w.rd`
2. Open **Server Manager**.
3. In Server Manager, select **Tools** > **Active Directory Domains and Trusts**.
4. In the console:
   - Expand **Contoso.com**
   - Right-click **na.Contoso.com** > **Properties**
5. In **na.Contoso.com Properties**:
   - Select the **Trusts** tab
   - Under **Domains trusted by this domain (outgoing trusts)**, select **Contoso.com**
   - Select **Properties**
6. In **Contoso.com Properties**:
   - Select **Validate**
   - When prompted, choose **Yes, validate the incoming trust**
7. Enter credentials:
   - User name: `administrator`
   - Password: `Pa55w.rd`
   - Select **OK**
8. Confirm message:
   > “The trust has been validated. It is in place and active.”
   - Select **OK**
9. If you receive an error about the **secure channel**:
   - Choose to **repair the trust**
   - Then repeat the validation (step 8).
10. Select **OK** twice to close the **Contoso.com Properties** dialog boxes.

**Result:** You have successfully implemented a **child domain** `na.Contoso.com` and verified the **default parent/child trust** in AD DS.

---
