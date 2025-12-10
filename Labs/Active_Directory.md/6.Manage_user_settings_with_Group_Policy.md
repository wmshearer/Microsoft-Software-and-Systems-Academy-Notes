# EXERCISE 1 — USE ADMINISTRATIVE TEMPLATES TO MANAGE USER SETTINGS

## TASK 1: IMPORT MICROSOFT OFFICE ADMIN TEMPLATES
1. Sign in to **LON-DC1** as `Contoso\Administrator` / `Pa55w.rd`.
2. Open File Explorer → Allfiles (E:) → Labfiles → Mod06.
3. Run: admintemplates_x64_5287-1000_en-us.exe
4. Accept license → extract to **Desktop**.
5. Open Desktop → admx folder → CTRL+A → Copy.
6. Navigate to: C:\Windows\PolicyDefinitions → Paste.

## TASK 2: CONFIGURE OFFICE SETTINGS
1. Server Manager → Tools → Group Policy Management.
2. Expand:
   Forest: Contoso.com → Domains → Contoso.com → Group Policy Objects
3. Create new GPO → name: **Office settings**.
4. Edit **Office settings** GPO.

### Enable Developer Tab in Excel
User Configuration → Policies → Administrative Templates → Microsoft Excel 2016 → Excel Options → Customize Ribbon  
Enable: **Display Developer tab in the Ribbon**

### Set default save location
Enable: **Default file location**  
Path: `%userprofile%\Desktop`

5. Link **Office settings** GPO to **Contoso.com**.

## TASK 3: APPLY & VERIFY SETTINGS ON CLIENT
1. Sign in to **LON-CL1** as Administrator.
2. Run the gpupdate command:

gpupdate /force

3. Open Excel → Accept → Blank Workbook.
4. Verify **Developer** tab appears.
5. Save file → verify default location = Desktop.

-------------------------------------------------------------

# EXERCISE 2 — GROUP POLICY PREFERENCES

## TASK 1: SET UP ENVIRONMENT
1. On LON-DC1: File Explorer → Allfiles (E:) → Labfiles → Mod06.
2. Run: Mod06-1.ps1 (choose Y if prompted).
3. Copy BranchScript.cmd.

### Add script to Branch1 GPO
User Configuration → Policies → Windows Settings → Scripts (Logon/Logoff)
→ Logon → Show Files → Paste script  
Add → Browse → select BranchScript.cmd

## TASK 2: TEST MAPPED DRIVE
1. Restart **LON-CL1**.
2. Log in as **Tonnie**.
3. Verify S: appears in This PC.  
If missing, run:

gpupdate /force
shutdown /r /t 0

## TASK 3: CREATE “PREFERENCES” GPO
1. On LON-DC1 → ADUC → create group: **Computer Administrators** inside IT.
2. Delete old Branch1 GPO.
3. Create new GPO: **Preferences** (linked at Contoso.com).

### Create Notepad shortcut
User Configuration → Preferences → Windows Settings → Shortcuts  
- Action: Create  
- Name: Notepad  
- Location: All Users Desktop  
- Target: C:\Windows\System32\Notepad.exe  
Common tab → disable “Run in user context”  
Enable Item-level Targeting → Security Group = IT

### Create mapped drive (Preferences)
User Config → Preferences → Windows Settings → Drive Maps  
- Location: \\LON-DC1\Branch1  
- Drive Letter: S  
- Label: Drive for Branch Office 1  
- Reconnect: Yes  
Common tab → Enable “Run in logged-on user’s security context”  
Item-level Targeting → OU = Branch Office 1

### Add local group membership (Preferences)
Computer Config → Preferences → Control Panel Settings → Local Users and Groups  
- Group Name: Administrators  
- Add → Computer Administrators  
Item-level Targeting → OS = Windows Server 2022 Family

## TASK 4: TEST PREFERENCES ON CLIENT
1. Restart **LON-CL1**.
2. Sign in as **Tonnie**.
3. Verify:
   - S: drive exists with correct label  
   - Notepad shortcut appears on desktop  
If missing:

gpupdate /force
shutdown /r /t 0

4. Computer Management → Local Users and Groups → Administrators  
Confirm **Computer Administrators** is NOT listed (correct).

-------------------------------------------------------------

# EXERCISE 3 — CONFIGURE FOLDER REDIRECTION

## TASK 1: CREATE SHARE FOR REDIRECTION
1. On LON-DC1 → C:\ → create folder **Branch1Redirect**.
2. Right-click → Give Access To → Specific people → Everyone → Read/Write.

## TASK 2: CREATE & LINK GPO
Create GPO under Branch Office 1 → name: **Folder Redirection**.

## TASK 3: CONFIGURE REDIRECTION
User Config → Policies → Windows Settings → Folder Redirection

### Documents
- Setting: Basic  
- Target folder location: Create a folder for each user  
- Root path:  
  \\LON-DC1\Branch1Redirect

### Pictures
- Setting: Follow the Documents folder

### Music
- Setting: Follow the Documents folder

## TASK 4: TEST ON CLIENT
1. On LON-CL1, log in as Tonnie.
2. Run:

gpupdate /force

3. Sign out + sign in again.
4. Right-click Documents → Properties → verify location:

\\LON-DC1\Branch1Redirect\Tonnie

5. Open Documents → verify **Music** and **Pictures** folders exist.

-------------------------------------------------------------

# END OF LAB — FORMATTED CORRECTLY IN ONE TERMINAL BLOCK
