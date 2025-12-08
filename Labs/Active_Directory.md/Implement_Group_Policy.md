# Lab — GPO Creation, Scope, and Loopback (Contoso)

---

## Exercise 1: Create and Configure GPOs

### Scenario

You must ensure:

- When users leave computers unattended for **10 minutes**, the **screen locks automatically**.
- Users are **prevented from using local registry editing tools**.
- You’ll use a **GPO linked to the Contoso.com domain**.

**Main tasks:**

1. Create and edit a GPO  
2. Link the GPO  
3. View the effects of the GPO’s settings  

---

### Task 1: Create and Edit a GPO

#### On LON-DC1

1. Sign in as:
   - `Contoso\Administrator`
   - Password: `Pa55w.rd`
2. **Server Manager** starts automatically.
3. In Server Manager:
   - Select **Tools** > **Group Policy Management**.
4. In **Group Policy Management Console (GPMC)**:
   - Expand **Forest: Contoso.com**
   - Expand **Domains**
   - Expand **Contoso.com**
   - Select **Group Policy Objects**.
5. Right-click **Group Policy Objects** > **New**.
6. In **Name**, type: `Contoso Standards`
   - Select **OK**.
7. In the details pane, right-click **Contoso Standards** > **Edit**.

#### Configure “Prevent access to registry editing tools”

1. In **Group Policy Management Editor**:
   - Expand **User Configuration**
   - Expand **Policies**
   - Expand **Administrative Templates**
   - Select **System**
2. In the right pane:
   - Open **Prevent access to registry editing tools**.
3. In the dialog:
   - Select **Enabled**
   - Select **OK**.

#### Configure Screen Saver Timeout and Password Protection

1. In the left pane:
   - Expand **User Configuration**
   - Expand **Policies**
   - Expand **Administrative Templates**
   - Expand **Control Panel**
   - Select **Personalization**
2. Open **Screen saver timeout**:
   - Select **Enabled**
   - In **Seconds**, enter: `600`
   - Select **OK**
3. Open **Password protect the screen saver**:
   - Select **Enabled**
   - Select **OK**
4. Close **Group Policy Management Editor**.

---

### Task 2: Link the GPO

1. In **GPMC**, in the left pane:
   - Right-click **Contoso.com** (the domain) > **Link an Existing GPO**.
2. In **Select GPO**:
   - Choose **Contoso Standards**
   - Select **OK**.

---

### Task 3: View the Effects of the GPO’s Settings

#### Enable Remote Management on LON-CL1

1. Switch to **LON-CL1**.
2. Sign in as:
   - `Contoso\Administrator`
   - Password: `Pa55w.rd`
3. Select **Start**, type: `firewall`.
4. In the results:
   - Select **Windows Defender Firewall**.
5. In Windows Defender Firewall:
   - Select **Allow an app or feature through Windows Defender Firewall**.
6. In **Allowed apps and features**, check **Domain**, **Private**, and **Public** for:
   - **Remote Event Log Management**
   - **Windows Management Instrumentation (WMI)**
7. Select **OK**.
8. Sign out of **LON-CL1**.

#### Verify User Experience (Contoso\Tonnie)

1. Sign in to **LON-CL1** as:
   - `Contoso\Tonnie`
   - Password: `Pa55w.rd`
2. Select **Start**, type: `screen saver`.
3. In the results:
   - Select **Change screen saver**.
4. In **Screen Saver Settings**:
   - Confirm **Wait** is **dimmed** and cannot be changed.
   - Confirm **On resume, display logon screen** is **selected and dimmed**.
   - If it’s NOT enforced:
     - Open **Command Prompt**
     - Run: `gpupdate /force`
     - Reopen **Change screen saver** and verify again.
5. Select **OK** to close **Screen Saver Settings**.

#### Verify Registry Editor is Blocked

1. Right-click **Start** > **Run**.
2. In **Run**:
   - Type: `regedit`
   - Select **OK**
3. Confirm you see an error:
   - “**Registry editing has been disabled by your administrator.**”
4. In the **Registry Editor** dialog:
   - Select **OK**.
5. Sign out of **LON-CL1**.

**Result:** You created, edited, linked, and verified a domain-level GPO (**Contoso Standards**) that enforces screen saver and registry restrictions.

---

## Exercise 2: Managing GPO Scope

### Scenario

- The **Contoso Standards** GPO enforces a 10-minute screen lock.
- A critical **Research engineering application fails when the screen saver starts**.
  - You must **prevent the Contoso Standards GPO from applying to any member of the Research security group**.
- You must **exempt conference-room kiosk computers** from Contoso Standards, but:
  - Ensure kiosk computers always use a **2-hour (7200 seconds) screen saver timeout**, **regardless of who signs in**.
- You’ll use:
  - **Security filtering** to scope GPO to specific users/computers.
  - **Loopback processing** for kiosk/computer-based user settings.

**Main tasks:**

1. Create and link the required GPOs  
2. Verify the order of precedence  
3. Configure GPO scope with security filtering  
4. Configure loopback processing  

---

### Task 1: Create and Link the Required GPOs

#### Create “Research Application Override” GPO

1. On **LON-DC1**, open **Group Policy Management** (GPMC).
2. In the left pane:
   - Expand **Forest: Contoso.com**
   - Expand **Domains**
   - Expand **Contoso.com**
   - Select the **Research** OU.
3. Right-click **Research** OU > **Create a GPO in this domain, and Link it here**.
4. In **New GPO**:
   - Name: `Research Application Override`
   - Select **OK**.
5. In the details pane:
   - Right-click **Research Application Override** > **Edit**.

#### Disable Screen Saver Timeout in Research Application Override

1. In **Group Policy Management Editor**:
   - Expand **User Configuration**
   - Expand **Policies**
   - Expand **Administrative Templates**
   - Expand **Control Panel**
   - Select **Personalization**
2. Open **Screen saver timeout**:
   - Select **Disabled**
   - Select **OK**
3. Close **Group Policy Management Editor**.

---

### Task 2: Verify the Order of Precedence

1. In **GPMC**, select the **Research** OU.
2. Select the **Group Policy Inheritance** tab.
3. Confirm:
   - **Research Application Override** GPO has **higher precedence** than **Contoso Standards**.
   - Result: Research Application Override’s Screen saver timeout setting **overwrites** the domain Contoso Standards setting.
   - Screen saver timeout **will not be enforced** for users in the scope of **Research Application Override**.

---

### Task 3: Configure the Scope of a GPO with Security Filtering

> Goal: Apply **Research Application Override** only to:
> - Members of the **Research** security group
> - And to specific computers (e.g., **LON-CL1**) if required

1. In **GPMC**, under the **Research OU**, select **Research Application Override** GPO.
2. If prompted with **Group Policy Management Console** dialog:
   - Read the message
   - Check **Do not show this message again**
   - Select **OK**
3. In the **Security Filtering** section:
   - Note the default **Authenticated Users** entry.
4. Select **Authenticated Users** > **Remove**.
5. In the **Group Policy Management** dialog:
   - Select **OK** (acknowledge warning).
6. Under **Security Filtering**, select **Add**.
   - In **Select User, Computer, or Group**:
     - In **Enter the object name to select**, type: `Research`
     - Select **OK**  
       (This targets the GPO to members of the **Research** group.)
7. In the details pane, select **Add** again (for computer).
8. In **Select User, Computer, or Group**:
   - Select **Object Types**
   - In **Object Types**, check **Computers**
   - Select **OK**
   - In **Enter Object Names to select**, type: `LON-CL1`
   - Select **OK**

**Result:** Research Application Override GPO now only applies to the **Research** group and **LON-CL1** (instead of all authenticated users).

---

### Task 4: Configure Loopback Processing for Kiosk / Conference Room

#### Create Kiosks and Conference Room OUs

1. In **GPMC**, under **Contoso.com**:
   - Right-click **Contoso.com** > **New Organizational Unit**.
2. In **New Organizational Unit**:
   - Name: `Kiosks`
   - Select **OK**
3. Right-click **Kiosks** > **New Organizational Unit**.
4. Name: `Conference Room`
   - Select **OK**
5. In the left pane:
   - Expand **Kiosks**
   - Select **Conference Room** OU.

#### Create Conference Room Settings GPO

1. Right-click **Conference Room** OU > **Create a GPO in this domain, and Link it here**.
2. In **New GPO**:
   - Name: `Conference Room Settings`
   - Select **OK**
3. In the left pane, under **Conference Room**:
   - Select **Conference Room Settings** GPO.
4. Right-click **Conference Room Settings** > **Edit**.

#### Configure Screen Saver Timeout to 2 Hours (7200 seconds)

1. In **Group Policy Management Editor**:
   - Expand **User Configuration**
   - Expand **Policies**
   - Expand **Administrative Templates**
   - Expand **Control Panel**
   - Select **Personalization**
2. Open **Screen saver timeout**:
   - Select **Enabled**
   - In **Seconds**, enter: `7200`
   - Select **OK**

#### Enable Loopback Processing (Merge Mode)

1. In the left pane:
   - Expand **Computer Configuration**
   - Expand **Policies**
   - Expand **Administrative Templates**
   - Expand **System**
   - Select **Group Policy**
2. In the right pane:
   - Open **Configure user Group Policy loopback processing mode**.
3. In the dialog:
   - Select **Enabled**
   - In **Mode**, select: **Merge**
   - Select **OK**
4. Close **Group Policy Management Editor**.
5. Close **Group Policy Management Console**.
6. Sign out of **LON-DC1**.

**Result:**  
- **Research Application Override** avoids breaking the Research app by disabling the screen saver timeout for users in that group/OU.  
- **Conference Room Settings** + **loopback (Merge)** ensure **any user** who logs on to **Conference Room kiosk computers** gets a **2-hour (7200 seconds) screen saver timeout**, regardless of their normal user policies.

---
# Lab B — GPO Verification & Troubleshooting (RSoP, Modeling, Events)

---

## Exercise 1: Verify GPO Application

### Scenario

You’ve configured GPOs for:

- Research department
- Conference room kiosk computers

You now need to:

- Verify that policies apply as intended using RSoP (Resultant Set of Policy)
- Use both Group Policy Management Console and client-side GPResult
- Simulate kiosk behavior using Group Policy Modeling
- Review Group Policy–related events in Event Viewer

Main tasks:

1. Perform RSoP analysis  
2. Analyze RSoP with GPResult  
3. Evaluate GPO results with Group Policy Modeling Wizard  
4. Review policy events  

---

### Task 1: Perform RSoP Analysis (GP Results Wizard)

#### On LON-CL1 — Force Policy and Record Time

1. Sign in to **LON-CL1** as:  
   - User: `Contoso\Susan`  
   - Password: `Pa55w.rd`
2. Open **Command Prompt**:  
   - Start → type `cmd` → Enter.
3. Force a policy update:

       gpupdate /force

4. Record the current system time (for later event correlation):

       time

   - Press Enter twice to display the time.
5. Restart **LON-CL1**.
6. Wait for LON-CL1 to reboot, but **do not sign in** yet.

#### On LON-DC1 — Generate Group Policy Results

1. Switch to **LON-DC1**.
2. Sign in as:
   - User: `Contoso\Administrator`
   - Password: `Pa55w.rd`
3. In **Server Manager**:
   - Select **Tools** → **Group Policy Management**.
4. In **Group Policy Management Console (GPMC)**:
   - Expand **Forest: Contoso.com**  
   - Select **Group Policy Results**.
5. Right-click **Group Policy Results** → **Group Policy Results Wizard**.
6. Wizard steps:
   - Welcome page → **Next**
   - **Computer Selection**:
     - Select **Another computer**
     - Enter `LON-CL1`
     - Select **Next**
   - **User Selection**:
     - Select `Contoso\Susan`
     - Select **Next**
   - **Summary of Selections**:
     - Review settings → **Next**
   - Select **Finish**.

#### Review RSoP Report (in GPMC)

1. In the details pane, the **RSoP report** for **Susan on LON-CL1** appears.
2. On the **Summary** tab:
   - Note **Last policy refresh** time for **User** and **Computer**.
   - Review **Allowed** and **Denied** GPOs.
   - Note which components processed policy (e.g., Group Policy Infrastructure, User/Computer Configuration).
3. Select the **Details** tab:
   - Review **User Details** and **Computer Details**.
   - Identify the **Winning GPO** for key settings.
4. Select the **Policy Events** tab:
   - Find the event that corresponds to the `gpupdate /force` run on LON-CL1 (based on the time you recorded).
5. Save the report:
   - On the **Summary** tab → right-click in empty area → **Save Report**.
   - Save to **Desktop** (file like `Susan on LON-CL1.htm`).
6. On the **LON-DC1** desktop:
   - Right-click the `.htm` report → **Open** (in Microsoft Edge).
   - Review, then close Edge.

---

### Task 2: Analyze RSoP with GPResult (Client-side)

#### On LON-CL1 — View RSoP in Command Line

1. Sign in to **LON-CL1** as:
   - User: `Contoso\Susan`
   - Password: `Pa55w.rd`
2. Open **Command Prompt**:
   - Start → type `cmd` → select **Command Prompt**.
3. View basic RSoP summary:

       gpresult /r

   - Note applied GPOs and security groups; this is similar to the **Summary** tab from GPMC.

4. View verbose output (more detail):

       gpresult /v | more

   - Press **Spacebar** to scroll.
   - Review which GPOs provided which settings.

5. View maximum detail:

       gpresult /z | more

   - Press **Spacebar** to scroll through the full detailed report.

6. Generate an HTML RSoP report on the desktop:

       gpresult /h %userprofile%\Desktop\RSOP.html

7. Open `RSOP.html` from Susan’s desktop and compare:
   - Formatting and content vs the GPMC-generated RSoP report.

8. Sign out of **LON-CL1**.

---

### Task 3: Evaluate GPO Results with Group Policy Modeling Wizard

You will **simulate** how policy applies if **LON-CL1** were a **Conference Room kiosk** computer with loopback processing.

#### On LON-DC1 — Run Group Policy Modeling

1. On **LON-DC1**, in **Group Policy Management**, select **Group Policy Modeling**.
2. Right-click **Group Policy Modeling** → **Group Policy Modeling Wizard**.

3. Wizard steps:
   - **Welcome** → **Next**
   - **Domain Controller Selection** → leave defaults → **Next**
   - **User and Computer Selection**:
     - Under **User information**:
       - Select **User**
       - Select **Browse**
       - Enter `Susan` → Enter → select user → OK
     - Under **Computer information**:
       - Select **Computer**
       - Select **Browse**
       - Enter `LON-CL1` → Enter → select computer → OK
     - Select **Next**
   - **Advanced Simulation Options**:
     - Check **Loopback Processing**
     - Mode: **Merge**
     - Select **Next**
   - **Alternate Active Directory Paths**:
     - Under **Computer location** → **Browse**
     - In **Choose Computer Container**:
       - Expand **Contoso**
       - Expand **Kiosks**
       - Select **Conference Room**
       - Select **OK**
     - Select **Next**
   - **User Security Groups** → **Next**
   - **Computer Security Groups** → **Next**
   - **WMI Filters for Users** → **Next**
   - **WMI Filters for Computers** → **Next**
   - **Summary of Selections** → Review → **Next**
   - Select **Finish**

#### Review Modeling Results

1. In the details pane, select the **Details** tab.
2. Under **User Details**:
   - Expand **Group Policy Objects** → **Applied GPOs**.
   - Confirm **Conference Room Settings** is listed as **Applied**.
3. Still under **User Details**:
   - Expand **Settings** → **Policies** → **Administrative Templates** → **Control Panel/Personalization**.
   - Confirm:
     - **Screen saver timeout = 7200 seconds (2 hours)**.
     - This is from **Conference Room Settings** GPO and overrides the 600 seconds from **Contoso Standards**.
4. Close the Group Policy Modeling results and any open windows on **LON-DC1**.

---

### Task 4: Review Policy Events in Event Viewer

#### On LON-CL1 — System and Group Policy Logs

1. Sign in to **LON-CL1** as:
   - User: `Contoso\Administrator`
   - Password: `Pa55w.rd`
2. Right-click **Start** → **Event Viewer**.
3. In the left pane:
   - Expand **Windows Logs**
   - Select **System**
4. Click the **Source** column header to sort by source.
5. Look for events with:
   - Source: **GroupPolicy**
   - Event ID: **1502** or **1503**
6. Review their details:
   - These describe Group Policy application to the system.

7. In the left pane:
   - Expand **Applications and Services Logs**
   - Expand **Microsoft** → **Windows** → **GroupPolicy**
   - Select **Operational**
8. Find the first event corresponding to the **gpupdate /force** you ran earlier (based on time).
9. Review that event and subsequent related events to see:
   - Policy refresh start  
   - Policy processing  
   - Success/failure status  
10. Sign out of **LON-CL1**.

**Result:** You verified GPO application using RSoP, GPResult, Group Policy Modeling, and event logs.

---

## Exercise 2: Troubleshooting GPOs

### Scenario

User **Susan Kemp** reports that Research configuration no longer applies.

- She now has a **fixed 10-minute screen saver timeout**.
- This causes a **Research application** to fail once the screen saver starts.
- Tier 1 helpdesk couldn’t resolve the issue; you must fix it.

### Incident Record 604531 (Summary)

- **Incident Reference Number:** 604531  
- **Date of Call:** July 15  
- **User:** Susan Kemp  
- **Incident Details:** Research configuration doesn’t apply anymore.  
- **Additional Information:** Screen saver locked at 10 minutes; breaks Research app.

### Plan of Action (high level)

- Confirm Susan’s **effective policy** via RSoP (Group Policy Results).
- Check if **Research Application Override** GPO is still applied.
- Verify **GPO link state** on the **Research OU** (not disabled).
- Correct any misconfiguration and verify behavior again.

---

### Task 1: Read Incident & Simulate the Problem

1. Review Incident 604531 details above.
2. On **LON-DC1**:
   - Open **File Explorer** from the taskbar.
3. In File Explorer:
   - Expand **Allfiles (E:)** → **Labfiles** → **Mod05**.
4. In the details pane:
   - Right-click **Mod05-1.ps1** → **Run with PowerShell**.
   - If prompted, type `Y` and press Enter.
   - This script prepares the lab state for the incident.

---

### Task 2: Determine Plan of Action

From the record and symptoms:

- Verify Susan’s actual configuration using **Group Policy Results** in GPMC.
- Verify that **Research Application Override** GPO exists and is intended to disable forced screen saver timeout.
- Confirm whether:
  - The GPO link is **disabled**, or
  - Security filtering / OU structure prevents it from applying.

---

### Task 3: Troubleshoot and Resolve the Problem

#### Step 1 — Confirm User Symptoms on LON-CL1

1. On **LON-CL1**, sign in as:
   - User: `Contoso\Susan`
   - Password: `Pa55w.rd`
2. Start → type `screen saver` → select **Change screen saver**.
3. In **Screen Saver Settings**:
   - Confirm **Wait** is **dimmed** (not editable) and set to **10 minutes**.
   - This indicates the **Contoso Standards** GPO is controlling the setting.
4. Sign out of **LON-CL1**.

#### Step 2 — Diagnose with Group Policy Results (GPMC)

1. Switch to **LON-DC1**.
2. Open **Server Manager** → **Tools** → **Group Policy Management**.
3. In **GPMC**, select **Group Policy Results**.
4. Right-click **Group Policy Results** → **Group Policy Results Wizard**.
5. Wizard steps:
   - Welcome → **Next**
   - **Computer Selection**:
     - Select **Another computer**
     - Enter `LON-CL1`
     - Select **Next**
   - **User Selection**:
     - Select `Contoso\Susan`
     - Select **Next**
   - **Summary of Selections**:
     - Review → **Next**
   - Select **Finish**

6. In the **Details** tab:
   - Select **Show all** to expand all sections if needed.

7. Under **User Details**:
   - Locate **Settings** → **Control Panel/Personalization**.
   - Confirm:
     - **Screen saver timeout = 600 seconds (10 minutes)**.
     - **Winning GPO = Contoso Standards**.

8. Under **User Details**, look at **Denied GPOs**:
   - Confirm that **Research Application Override** appears in the **Denied GPOs** list.
   - Reason: **Disabled Link**.
   - Conclusion: The **Research Application Override** GPO is not applying because its link is disabled.

#### Step 3 — Re-enable the GPO Link for Research

1. In the GPMC left pane:
   - Select the **Research** OU.
2. Right-click the **Research** OU → **Refresh**.
3. Expand **Research** OU.
4. Note that **Research Application Override** link appears **disabled**.
5. Right-click **Research Application Override** (under the Research OU) → select **Link Enabled**.
   - This re-enables the link so the GPO can apply to Research users again.

#### Step 4 — Re-test Behavior on LON-CL1

1. Switch to **LON-CL1**.
2. Sign in as:
   - User: `Contoso\Susan`
   - Password: `Pa55w.rd`
3. Start → type `screen saver` → select **Change screen saver**.
4. Verify:
   - **Wait** is no longer dimmed (user can edit it).
   - It may still show **10 minutes**, but it’s now **user-configurable**.
   - This indicates the **Research Application Override** GPO is successfully overriding the enforced timeout.

5. If the **Wait** value is still dimmed:
   - Restart and try again:
     1. Right-click Start → **Shut down or sign out** → **Restart**.
     2. Sign back in as `Contoso\Susan` with `Pa55w.rd`.
     3. Run **Change screen saver** again and verify.

6. Sign out of **LON-CL1**.

---

### Resolution Summary

- Root cause: The **Research Application Override** GPO link for the **Research OU** was **disabled**.
- Effect: Susan received only **Contoso Standards** GPO settings (fixed 10-minute timeout), breaking her Research app.
- Fix: Re-enabled the **Research Application Override** GPO link on the **Research OU**.
- Result: Research configuration applies again, and the screen saver timeout is no longer enforced as a fixed 10 minutes for Susan.

---
