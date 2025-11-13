# PowerShell Loop & Decision Constructs — Study Notes

## The For construct
The **For** loop runs a block of code a specific number of times.  
Think of it like a numbered checklist: *start at number X, stop at number Y, move one step each time*.

A For loop is controlled by three parts:
- Initial value  
- Condition (when to stop)  
- Action (how to move to the next number)

```powershell
# Runs 10 times, creating users 1 through 10
For($i = 1; $i -le 10; $i++) {
    Write-Host "Creating User $i"
}
```

## The ForEach construct
**ForEach** loops run a block of code **once for every item in a list or array**.  
Think of it like: *“Do this action for every item in the grocery bag.”*

Key ideas:
- You don't need to know how many items are in the array.
- `$user` (or whatever variable name you choose) temporarily holds the current item.
- Indentation is only for readability.
- Use meaningful variable names.

```powershell
# Processes each user in the $users array
ForEach ($user in $users) {
    Set-ADUser $user -Department "Marketing"
}
```

## The If construct
The **If** statement lets PowerShell make decisions.  
Think of it like: *“If this is true, do this. Otherwise, try something else.”*

Important points:
- You can have zero or more `ElseIf` statements.
- `Else` is optional.
- Only the first matching condition runs.

```powershell
# Simple disk space check with multiple conditions
If ($freeSpace -le 5GB) {
    Write-Host "Free disk space is less than 5 GB"
}
ElseIf ($freeSpace -le 10GB) {
    Write-Host "Free disk space is less than 10 GB"
}
Else {
    Write-Host "Free disk space is more than 10 GB"
}
```

## The While loop
A **While** loop keeps running **as long as** the condition is true.  
Think of it like: *“Keep stirring the pot while the soup is boiling.”*  
Note: The script block may **never run** if the condition starts false.

```powershell
# Runs only while the answer stays "go"
While ($answer -eq "go") {
    Write-Host "Script block to process"
}
```

## Do..While loops
A **Do..While** loop always runs **once**, then checks the condition.  
Think of it like: *“Do this at least one time, then keep doing it if needed.”*

```powershell
# Always runs once, then repeats while answer is "go"
Do {
    Write-Host "Code block to process"
} While ($answer -eq "go")
```

## Do..Until loops
A **Do..Until** loop is the opposite of Do..While — it repeats until the condition becomes true.

Think of it like:  
*“Keep doing this thing until the stop sign appears.”*

```powershell
# Runs until answer becomes "stop"
Do {
    Write-Host "Code block to process"
} Until ($answer -eq "stop")
```

## The Switch construct
**Switch** compares one value against many possible matches.  
Think of it like ordering food at a kiosk:  
*“If you pick 1, show menu 1. If you pick 2, show menu 2…”*

You can also use wildcards or regex.

```powershell
# Prints a message depending on which menu item was chosen
Switch ($choice) {
    1 { Write-Host "You selected menu item 1" }
    2 { Write-Host "You selected menu item 2" }
    3 { Write-Host "You selected menu item 3" }
    Default { Write-Host "You did not select a valid option" }
}
```
## Understanding Break and Continue

*Easy analogy:*  
Think of a loop like going through a stack of papers:

- **Continue** = “Skip this paper and move to the next one.”  
- **Break** = “Stop going through the stack entirely.”

### Continue  
**Continue** stops processing the *current* iteration and moves immediately to the next item.

```powershell
# Skip Administrator accounts, process everything else
ForEach ($user in $users) {
    If ($user.Name -eq "Administrator") { Continue }
    Write-Host "Modify user object"
}
```

### Break  
**Break** completely stops the loop — nothing else in the list is processed.

```powershell
# Stop the loop once the counter reaches the max value
ForEach ($user in $users) {
    $number++
    Write-Host "Modify User object $number"
    If ($number -ge $max) { Break }
}
```
