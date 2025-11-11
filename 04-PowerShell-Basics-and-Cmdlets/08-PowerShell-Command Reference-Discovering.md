# PowerShell Command Reference — Discovering and Exploring PowerShell

This reference organizes your `.ps1` training files into four main learning categories:
1. Searching for Cmdlets  
2. Using About Files (Help Topics)  
3. Working with Aliases  
4. Viewing and Managing Modules  

Each section includes:
- Purpose  
- Typical Usage Scenario  
- Example Commands (with expected behavior)

---

## 1. Search Cmdlets
**Purpose:** Find commands or cmdlets related to a specific keyword.

### Common Scenarios
- When you can’t remember the exact cmdlet name.
- To explore all available PowerShell commands matching part of a word.
- To narrow results to a specific cmdlet category.

### Commands

```powershell
# 1. Search for commands containing "net" in their name
Get-Command *net*

# 2. Search help files for cmdlets that include "net"
Get-Help *net* -Category cmdlet
```

**Explanation:**
- `Get-Command` searches across all cmdlets, functions, workflows, aliases, and scripts available in your PowerShell session.
- `Get-Help` limits search results to the PowerShell help system and filters by category (in this case, cmdlets).

**Expected Output:**
A list of cmdlets related to networking or containing “net,” such as `Get-NetAdapter`, `Get-NetIPAddress`, etc.

---

## 2. Using About Files
**Purpose:** Learn PowerShell concepts, behaviors, and syntax through built-in documentation.

### Common Scenarios
- Reviewing conceptual topics (not command-specific help).
- Discovering help about aliases, event logs, or scripting concepts.

### Commands

```powershell
# 1. List all available "about" help topics
Get-Help about*

# 2. Display help for aliases
Get-Help about_aliases

# 3. Open detailed help for event logs in a separate window
Get-Help about_eventlogs -ShowWindow

# 4. Search for help topics containing "beep"
Get-Help *beep*
```

**Explanation:**
- “About” topics are text-based documentation built into PowerShell that explain **concepts** like variables, loops, execution policy, etc.
- `-ShowWindow` opens the content in a resizable help viewer window.

**Expected Output:**
Text-based help files detailing the requested concept, displayed either in the console or in a separate window.

---

## 3. Using Aliases
**Purpose:** Understand and manage PowerShell aliases (shortcuts to cmdlets).

### Common Scenarios
- Discovering which commands have aliases (e.g., `dir`, `ls`, `cp`).
- Creating or modifying aliases for frequent use.
- Checking definitions and verifying which cmdlet an alias maps to.

### Commands

```powershell
# 1. Use an alias for directory listing
dir

# 2. Equivalent explicit cmdlet
Get-ChildItem

# 3. Find what command the alias 'dir' maps to
Get-Alias dir

# 4. Create a custom alias named 'list' for Get-ChildItem
New-Alias list Get-ChildItem

# 5. Test your new alias
list

# 6. Find all aliases that reference Get-ChildItem
Get-Alias -Definition Get-ChildItem
```

**Explanation:**
- `Get-Alias` shows existing aliases.
- `New-Alias` creates new ones (temporary until PowerShell is restarted).
- Aliases improve efficiency for interactive use but should be avoided in production scripts for clarity.

**Expected Output:**
Lists alias definitions or creates a new alias that works within the current session.

---

## 4. Viewing and Managing Modules
**Purpose:** Load, view, and explore PowerShell modules and their contents.

### Common Scenarios
- Checking what modules are currently loaded.
- Importing additional modules (like Active Directory or Server Manager).
- Listing available modules on the system.

### Commands

```powershell
# 1. Show modules currently loaded in your session
Get-Module

# 2. Run a command that loads a module automatically (e.g., Active Directory)
Get-ADUser Lara

# 3. Verify which modules are now loaded
Get-Module

# 4. List all modules installed and available for loading
Get-Module -ListAvailable

# 5. Manually import a module into your session
Import-Module ServerManager

# 6. Confirm the module has loaded
Get-Module
```

**Explanation:**
- `Get-Module` (no parameters) shows active modules.
- `-ListAvailable` reveals all modules on disk.
- `Import-Module` loads a module manually if it hasn’t autoloaded.

**Expected Output:**
Lists of module names, paths, and versions — confirming module availability or successful import.

---

## Summary Table

| Category | Key Cmdlets | Typical Use |
|-----------|--------------|-------------|
| Search Cmdlets | Get-Command, Get-Help | Find cmdlets or help topics |
| Using About Files | Get-Help about_* | Learn PowerShell concepts |
| Using Aliases | Get-Alias, New-Alias | Manage command shortcuts |
| Viewing Modules | Get-Module, Import-Module | Load and inspect PowerShell modules |

---

## Practical Order for Exploration
If you’re learning or troubleshooting PowerShell:

1. **Search** → Use `Get-Command` or `Get-Help` to find what you need.  
2. **Learn** → Open relevant “about” files for conceptual understanding.  
3. **Simplify** → Create or inspect aliases for convenience.  
4. **Expand** → Load and explore new modules for advanced cmdlets.

---