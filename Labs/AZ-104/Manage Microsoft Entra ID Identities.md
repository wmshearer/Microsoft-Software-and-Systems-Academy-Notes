LAB 01 – Manage Microsoft Entra ID Identities

ESTIMATED TIME
30 minutes

LAB SCENARIO
Your organization is building a new pre-production lab environment.
Engineers will manage virtual machines and services.
You must provision Microsoft Entra ID users and groups so engineers can authenticate.
Group membership should be automatically manageable based on job roles.

JOB SKILLS
- Create and configure user accounts
- Create groups and assign members

--------------------------------------------------------------------

TASK 1: Create and configure user accounts

- [ ] Sign in to the Azure portal
  URL: https://portal.azure.com

- [ ] If prompted with "Welcome to Azure", select `Cancel`

- [ ] Open Microsoft Entra ID
  Action: Search for and select `Microsoft Entra ID`

- [ ] Explore the Overview blade
  Action: Select `Overview`
  Then select the `Manage tenants` tab

  Note:
  A tenant represents a specific Microsoft Entra ID instance containing users and groups.
  You can create multiple tenants and switch between them.

- [ ] Return to the Microsoft Entra ID page
  Action: Use browser back button or breadcrumb navigation

- [ ] (Optional) Explore additional features
  Examples:
  - Licenses
  - Password reset

--------------------------------------------------------------------

CREATE A NEW USER

- [ ] In Microsoft Entra ID, open Users
  Path: Manage → Users

- [ ] Create a new user
  Action: `New user` → `Create new user`

- [ ] Configure user settings

  User principal name: `az104-user1`
  Display name: `az104-user1`
  Auto-generate password: checked
  Account enabled: checked

- [ ] Open the Properties tab and configure:
  Job title: `IT Lab Administrator`
  Department: `IT`
  Usage location: `United States`

- [ ] Review and create user
  Action: `Review + create` → `Create`

- [ ] Refresh the Users page
  Verify `az104-user1` was created successfully

--------------------------------------------------------------------

INVITE AN EXTERNAL USER

- [ ] In Users, select `New user`
  Action: `Invite an external user`

- [ ] Configure invitation settings
  Email: your email address
  Display name: your name
  Send invite message: checked
  Message: `Welcome to Azure and our group project`

- [ ] Open the Properties tab and configure:
  Job title: `IT Lab Administrator`
  Department: `IT`
  Usage location: `United States`

- [ ] Send invitation
  Action: `Review + invite` → `Invite`

- [ ] Refresh the Users page
  Verify the guest user was created

  Note:
  You should receive an email invitation shortly.

--------------------------------------------------------------------

TASK 2: Create groups and add members

- [ ] Open Microsoft Entra ID
  Action: Search for and select `Microsoft Entra ID`

- [ ] Open Groups
  Path: Manage → Groups

- [ ] Review group management options
  Expiration: controls group lifetime
  Naming policy: controls prefixes, suffixes, and blocked words

--------------------------------------------------------------------

CREATE A SECURITY GROUP

- [ ] Create a new group
  Action: `+ New group`

- [ ] Configure group settings

  Group type: `Security`
  Group name: `IT Lab Administrators`
  Group description: `Administrators that manage the IT lab`
  Membership type: `Assigned`

  Note:
  Dynamic membership requires Microsoft Entra ID Premium P1 or P2.

- [ ] Assign group owners
  Action: `No owners selected`
  Select yourself as owner
  Confirm selection

- [ ] Assign group members
  Action: `No members selected`
  Add:
  - az104-user1
  - invited guest user

- [ ] Create the group
  Action: `Create`

- [ ] Refresh the Groups page
  Verify the group was created

- [ ] Open the new group
  Review:
  - Members
  - Owners

--------------------------------------------------------------------

KEY TAKEAWAYS

- A tenant represents your organization in Microsoft Entra ID
- Microsoft Entra ID supports internal users and external guest users
- Groups help manage access to resources efficiently
- Security groups can have assigned or dynamic membership
- Group ownership is important for lifecycle management
