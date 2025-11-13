## Accepting User Input in PowerShell Scripts

*Analogy:*  
Scripts are like tools. At first, you hard-code values (like measuring tape set to a fixed length).  
But when you reuse the tool, you want it to be adjustable — that’s where **user input** helps.

---

### Identifying Values That Might Change  
Some script values stay the same at first, like:

- Find users who haven’t signed in for 30 days  
- Find specific event IDs on domain controllers  

But when scripts are reused, you may want those values to change without editing the script.

To make scripts flexible:

- Put changeable values in **variables** at the top  
- Or better — **ask the user for input**

---

### Passing Parameters to a Script  
A `Param()` block lets the script accept input when it runs — like giving your script knobs to adjust.

```powershell
Param(
    [string]$ComputerName,
    [int]$EventID
)
```

Calling the script with parameters:

```powershell
.\GetEvent.ps1 -ComputerName LON-DC1 -EventID 5772
```

Why define variable types?

- Prevents errors from invalid data  
- `[switch]` is a true/false toggle  
- You can set defaults or request input if none is provided

Example with a default value:

```powershell
[string]$ComputerName = "LON-DC1"
```

Example accepting user input if not supplied:

```powershell
[int]$EventID = Read-Host "Enter event ID"
```

---

### Using Out-GridView  
*Analogy:*  
It’s like giving the user a pop-up menu they can click on instead of typing.

You can present data in a graphical list and let the user choose an item:

```powershell
$selection = $users | Out-GridView -PassThru
```

`-OutputMode` gives more control:

- **None** – selection disabled  
- **Single** – one row allowed  
- **Multiple** – multiple rows allowed  

---

### Using Read-Host  
`Read-Host` asks the user a question directly in the terminal.

```powershell
$answer = Read-Host "How many days"
```

Removing the default colon:

```powershell
Write-Host "How many days? " -NoNewline
$answer = Read-Host
```

Secure or hidden input:

```powershell
$password = Read-Host "Enter password" -AsSecureString
```

Or hide typing visually:

```powershell
$secret = Read-Host "Enter secret value" -MaskInput
```
