# Exercise 1 — Create and Manage Groups in AD DS

## Task 1 — Create Groups and Add Members

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] In **Server Manager**:
      Tools → **Active Directory Administrative Center**

- [ ] Maximize the ADAC window

### Create the Enterprise Managers Group

- [ ] In ADAC:
      Select **Contoso (local)** → **Managers**

- [ ] In **Tasks** pane under **Managers**:
      New → **Group**

- [ ] Enter:
      Group name: Enterprise Managers  
      Group scope: Universal  

- [ ] Select **OK**

### Create the Research Mail Distribution Group

- [ ] In ADAC:
      Select **Contoso (local)** → **Research**

- [ ] In **Tasks** pane under **Research**:
      New → **Group**

- [ ] Enter:
      Group name: Research Mail  
      Group type: Distribution  
      E-mail: Research@Contoso.com  

- [ ] In **Managed By**:
      Click **Edit**

- [ ] In Select User dialog:
      Enter: Carlos  
      Click **Check Names** → **OK**

- [ ] Check:
      Manager can update membership list

- [ ] Select **OK**

### Create the Research Managers Group and Add Members

- [ ] Under **Research**:
      New → **Group**

- [ ] Enter:
      Group name: Research Managers

- [ ] Scroll to **Members** → Select **Add**

- [ ] In object names:
      Enter: Carlos; Susan  
      Click **Check Names** → **OK**

- [ ] Select **OK** to create the group


## Task 2 — Configure Group Nesting

- [ ] In ADAC:
      Select **Contoso (local)** → open **Managers** OU

- [ ] Right-click **Enterprise Managers** → **Properties**

- [ ] In navigation pane:
      Select **Members** → **Add**

- [ ] In object names:
      Enter: Managers; Research Managers  
      Click **Check Names** → **OK**

- [ ] Select **OK** to close Enterprise Managers properties


## Task 3 — Convert Group Type from Distribution to Security

- [ ] In ADAC:
      Select **Research**

- [ ] Open **Research Mail** group

- [ ] Change:
      Group type → **Security**

- [ ] Select **OK**


## Result

- Groups created  
- Members added  
- Group nesting configured  
- Distribution group converted to Security  


# Exercise 2 — Create and Configure User Accounts in AD DS

## Task 1 — Create & Configure a User Template for the Research Department

- [ ] In **Active Directory Administrative Center (ADAC)**:
      Select **Research** OU

- [ ] In **Tasks** pane under Research:
      New → **User**

- [ ] In **Create User**:
      First name: _Research Template  
      User UPN logon: ResearchTemplate  
      Password: Pa55w.rd  
      Confirm password: Pa55w.rd  

- [ ] In navigation pane:
      Select **Organization**

      - [ ] Department: Research  
      - [ ] Company: Contoso  
      - [ ] Manager:  
            Select **Edit**  
            Enter: Carlos  
            Click **Check Names** → **OK**

- [ ] Select **Member Of** → **Add**
      Enter: Research  
      Click **Check Names**  
      In **Multiple Names Found**, select **Research**  
      Click **OK** twice

- [ ] Select **Profile**
      Logon script:
      \\LON-DC1\Netlogon\Logon.bat

- [ ] Click **OK** to create the template account

- [ ] Select **_Research Template**
      In **Tasks** pane:
      Select **Disable**

- [ ] Close **Active Directory Administrative Center**


## Task 2 — Create New Users for the Research Branch Office Based on the Template

- [ ] In **Server Manager**:
      Tools → **Active Directory Users and Computers**

- [ ] Expand:
      Contoso.com → Research OU

- [ ] Right-click **_Research Template** → **Copy**

- [ ] In **Copy Object – User**:
      First name: Research  
      Last name: User  
      User logon name: ResearchUser  
      Click **Next**

- [ ] Password: Pa55w.rd  
      Confirm password: Pa55w.rd  

- [ ] Clear:
      Account is disabled

- [ ] Click **Next** → **Finish**


## Task 3 — Validate the Template

- [ ] Open **Research User** (Properties)

### Profile Tab
- [ ] Ensure:
      Logon script = \\LON-DC1\Netlogon\Logon.bat

### Organization Tab
- [ ] Ensure:
      Department = Research  
      Company = Contoso  
      Manager = Carlos Tijerina  

### Member Of Tab
- [ ] Ensure:
      Research group is listed

- [ ] Select **Cancel** to close properties


## Result

- User template created and configured  
- New user created based on the template  
- All inherited attributes validated successfully  

# Exercise 3 — Manage Computer Objects in AD DS

## Task 1 — Reset a Computer Account

- [ ] In **Active Directory Users and Computers**:
      Select **Computers** container

- [ ] In details pane:
      Right-click **LON-CL1** → **Reset Account**

- [ ] In **Active Directory Domain Services** dialog:
      Select **Yes**

- [ ] In confirmation message box:
      Select **OK**


## Task 2 — Observe Behavior When a Client Attempts to Sign In

- [ ] Switch to **LON-CL1**

- [ ] Attempt sign-in as:
      Contoso\Oscar / Pa55w.rd

- [ ] Question:
      What message is displayed?

- [ ] Answer:
      ____________________________________________
      ____________________________________________
      ____________________________________________


## Task 3 — Resolve the Computer Issue

- [ ] Sign in to **LON-CL1** as:
      Contoso\Administrator / Pa55w.rd  
      (cached credentials allow login)

- [ ] Right-click **Start** → **Windows Terminal**

- [ ] In **Administrator: Windows PowerShell**, run:
      Test-ComputerSecureChannel -Repair

- [ ] Close Windows PowerShell / Terminal window

- [ ] Sign out of **LON-CL1**

- [ ] Sign back in as:
      Contoso\Oscar / Pa55w.rd  
      (sign-in should now succeed)

- [ ] Sign out of **LON-CL1**

- [ ] Keep lab environment running for the next lab


## Result

- Computer account reset  
- Secure channel repaired  
- User sign-in restored successfully  

# Lab B: Administer AD DS  
# Exercise 1 — Delegate Administration for OUs

## Task 1 — Create a New OU for the Branch Office

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] In **Server Manager**:
      Tools → **Active Directory Users and Computers**

- [ ] In ADUC:
      Right-click **Contoso.com** → New → **Organizational Unit**

- [ ] Enter:
      Name: London

- [ ] Select **OK**


## Task 2 — Create Groups for Branch Admins & Helpdesk

- [ ] Right-click **London OU** → New → **Group**
      Group name: London Admins  
      Select **OK**

- [ ] Repeat to create:
      Group name: London Helpdesk


## Task 3 — Add Members to Groups

- [ ] Select **IT** OU

### Add Alberto Hermosilla to London Admins

- [ ] Right-click **Alberto Hermosilla** → **Add to a group**
- [ ] Enter: London Admins  
      Click **Check Names** → **OK**  
- [ ] Select **OK** on confirmation

### Add Tonnie Thomsen to London Helpdesk

- [ ] Right-click **Tonnie Thomsen** → **Add to a group**
- [ ] Enter: London Helpdesk  
      Click **Check Names** → **OK**  
- [ ] Select **OK** on confirmation


## Task 4 — Delegate Permissions to Groups

### Enable Advanced Features

- [ ] In ADUC:
      View → **Advanced Features**

### Full Control for London Admins

- [ ] Right-click **London OU** → **Properties**
- [ ] Select **Security** tab → **Add**
- [ ] Enter: London Admins  
      Check Names → **OK**
- [ ] With London Admins selected:
      Allow: **Full Control**
- [ ] Select **OK**

### Delegate Limited Control to London Helpdesk

- [ ] Right-click **London OU** → **Delegate Control**
- [ ] Select **Next**

- [ ] Add:
      Enter: London Helpdesk  
      Check Names → **OK**
- [ ] Select **Next**

- [ ] Select:
      **Create a custom task to delegate**
- [ ] Select **Next**

- [ ] Select:
      **Only the following objects in this folder**
      TAB - User objects  
      TAB - Check:
            - Create selected objects in this folder  
            - Delete selected objects in this folder  

- [ ] Select **Next**

- [ ] Permissions page:
      Select **Full Control**

- [ ] Select **Next** → **Finish**


## Task 5 — Test Permissions

### Install AD DS Tools on LON-SVR1

- [ ] Sign in to **LON-SVR1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] Start → **Server Manager**
      Add roles and features → Next → Next → Next

- [ ] Features:
      Remote Server Administration Tools  
      → Role Administration Tools  
      → AD DS and AD LDS Tools  
      → Check **AD DS Tools**

- [ ] Select **Next** → **Install**
- [ ] When complete → **Close**
- [ ] Sign out

---

## Test Permissions for London Admins (Alberto)

- [ ] Sign in to **LON-SVR1** as:
      Alberto / Pa55w.rd

- [ ] Open:
      Start → Server Manager → Tools → ADUC

- [ ] Expand **Contoso.com** → **Research**
      - Icons to create objects should be **disabled**

- [ ] Select **London** OU
      - Icons to create objects should now be **enabled**

- [ ] Right-click **London** → New → **Organizational Unit**
      Name: Laptops  
      Select **OK**
      (Should succeed)

- [ ] Sign out


## Test Permissions for London Helpdesk (Tonnie)

- [ ] Sign in to **LON-SVR1** as:
      Tonnie / Pa55w.rd

- [ ] Start → Server Manager → Tools → ADUC

- [ ] Expand **Contoso.com** → **London**
      - Only **Create User** icon should be available

- [ ] Sign out


## Result

- London OU created  
- London Admins & London Helpdesk groups created  
- Members added to groups  
- Delegated permissions applied correctly  
- Permissions validated on LON-SVR1  

# Lab B: Administer AD DS  
# Exercise 1 — Delegate Administration for OUs

## Task 1 — Create a New OU for the Branch Office

- [ ] Sign in to **LON-DC1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] In **Server Manager**:
      Tools → **Active Directory Users and Computers**

- [ ] In ADUC:
      Right-click **Contoso.com** → New → **Organizational Unit**

- [ ] Enter:
      Name: London

- [ ] Select **OK**


## Task 2 — Create Groups for Branch Admins & Helpdesk

- [ ] Right-click **London OU** → New → **Group**
      Group name: London Admins  
      Select **OK**

- [ ] Repeat to create:
      Group name: London Helpdesk


## Task 3 — Add Members to Groups

- [ ] Select **IT** OU

### Add Alberto Hermosilla to London Admins

- [ ] Right-click **Alberto Hermosilla** → **Add to a group**
- [ ] Enter: London Admins  
      Click **Check Names** → **OK**  
- [ ] Select **OK** on confirmation

### Add Tonnie Thomsen to London Helpdesk

- [ ] Right-click **Tonnie Thomsen** → **Add to a group**
- [ ] Enter: London Helpdesk  
      Click **Check Names** → **OK**  
- [ ] Select **OK** on confirmation


## Task 4 — Delegate Permissions to Groups

### Enable Advanced Features

- [ ] In ADUC:
      View → **Advanced Features**

### Full Control for London Admins

- [ ] Right-click **London OU** → **Properties**
- [ ] Select **Security** tab → **Add**
- [ ] Enter: London Admins  
      Check Names → **OK**
- [ ] With London Admins selected:
      Allow: **Full Control**
- [ ] Select **OK**

### Delegate Limited Control to London Helpdesk

- [ ] Right-click **London OU** → **Delegate Control**
- [ ] Select **Next**

- [ ] Add:
      Enter: London Helpdesk  
      Check Names → **OK**
- [ ] Select **Next**

- [ ] Select:
      **Create a custom task to delegate**
- [ ] Select **Next**

- [ ] Select:
      **Only the following objects in this folder**
      TAB - User objects  
      TAB - Check:
            - Create selected objects in this folder  
            - Delete selected objects in this folder  

- [ ] Select **Next**

- [ ] Permissions page:
      Select **Full Control**

- [ ] Select **Next** → **Finish**


## Task 5 — Test Permissions

### Install AD DS Tools on LON-SVR1

- [ ] Sign in to **LON-SVR1** as:
      Contoso\Administrator / Pa55w.rd

- [ ] Start → **Server Manager**
      Add roles and features → Next → Next → Next

- [ ] Features:
      Remote Server Administration Tools  
      → Role Administration Tools  
      → AD DS and AD LDS Tools  
      → Check **AD DS Tools**

- [ ] Select **Next** → **Install**
- [ ] When complete → **Close**
- [ ] Sign out

---

## Test Permissions for London Admins (Alberto)

- [ ] Sign in to **LON-SVR1** as:
      Alberto / Pa55w.rd

- [ ] Open:
      Start → Server Manager → Tools → ADUC

- [ ] Expand **Contoso.com** → **Research**
      - Icons to create objects should be **disabled**

- [ ] Select **London** OU
      - Icons to create objects should now be **enabled**

- [ ] Right-click **London** → New → **Organizational Unit**
      Name: Laptops  
      Select **OK**
      (Should succeed)

- [ ] Sign out


## Test Permissions for London Helpdesk (Tonnie)

- [ ] Sign in to **LON-SVR1** as:
      Tonnie / Pa55w.rd

- [ ] Start → Server Manager → Tools → ADUC

- [ ] Expand **Contoso.com** → **London**
      - Only **Create User** icon should be available

- [ ] Sign out


## Result

- London OU created  
- London Admins & London Helpdesk groups created  
- Members added to groups  
- Delegated permissions applied correctly  
- Permissions validated on LON-SVR1  
