# PowerShell Scripting Constructs — Study Notes

---

# ForEach — Loop Through Items in an Array

ForEach is used to process each item in an array, one at a time.  
Think of it like going through a stack of papers: pick one, work on it, move to the next.

Syntax and behavior description:

ForEach ($user in $users) {  
    Set-ADUser $user -Department "Marketing"  
}

- $users is the array.  
- $user becomes each item in the array during the loop.  
- First loop = $users[0], second loop = $users[1], etc.  
- You can place multiple commands inside the braces.  
- Use meaningful variable names ($users → $user).  

This is the preferred loop when you don’t know how many items are in the array.

---

# Parallel ForEach (PowerShell 7+)

You can process multiple objects at once using ForEach-Object -Parallel.

Example description:

$users | ForEach-Object -Parallel { Set-ADUser $user -Department "Marketing" }

- Runs several operations at the same time.  
- Default parallel tasks = 5.  
- Can change with -ThrottleLimit.  
- Useful for large-scale scripts.  

---

# If — Make Decisions in Scripts

If lets you choose what code to run based on conditions.

Analogy: traffic light — red = stop, yellow = caution, green = go.

Syntax explanation:

If ($freeSpace -le 5GB) {  
   Write-Host "Free disk space is less than 5 GB"  
}  
ElseIf ($freeSpace -le 10GB) {  
   Write-Host "Free disk space is less than 10 GB"  
}  
Else {  
   Write-Host "Free disk space is more than 10 GB"  
}

- PowerShell checks conditions top-to-bottom.  
- Only the first true condition runs.  
- ElseIf is optional.  
- Else runs when nothing else matches.  
- Use ElseIf chains instead of nested If statements for clarity.  

---

# Switch — Cleaner Alternative to Many If/ElseIf Blocks

Switch evaluates one value against many possible matches.  
Think of it like phone menu logic: “Press 1 for Sales, 2 for Support…”

Syntax description:

Switch ($choice) {  
   1 { Write-Host "Menu item 1" }  
   2 { Write-Host "Menu item 2" }  
   3 { Write-Host "Menu item 3" }  
   Default { Write-Host "Invalid option" }  
}

- Each case is checked.  
- Default runs if none match.  

## Pattern Matching in Switch

Switch can match patterns using -Wildcard or -Regex.

Example description:

Switch -WildCard ($ip) {  
   "10.*"     { Write-Host "Internal network" }  
   "10.1.*"   { Write-Host "London" }  
   "10.2.*"   { Write-Host "Vancouver" }  
   Default    { Write-Host "Not internal" }  
}

Important behavior:
- Pattern matching may trigger **multiple results**.  
  For example, "10.1.x.x" matches both "10.*" and "10.1.*".  

If $ip is an array, each item is processed individually.

---

# For — Loop a Specific Number of Times

Use For when you need to loop a specific number of times.

Analogy: “Do this 10 times.”

Syntax description:

For ($i = 1; $i -le 10; $i++) {  
   Write-Host "Creating User $i"  
}

Parts explained:
- $i = 1 → starting value  
- $i -le 10 → keep looping as long as condition is true  
- $i++ → increment $i by 1 each loop  

Runs exactly 10 times.

Use Case:
- Creating test accounts  
- Repeating tasks a defined number of times  

For arrays, ForEach is typically preferred because it auto-detects the number of items.

---

# Other Loop Constructs (Do..While, Do..Until, While)

These loops repeat until a condition changes, but differ in structure.

## Do..While  
- Always runs **at least once**.  
- Repeats **while** the condition remains true.

Example description:

Do {  
   Write-Host "Processing…"  
} While ($answer -eq "go")

## Do..Until  
- Always runs **at least once**.  
- Repeats **until** the condition becomes true.

Example description:

Do {  
   Write-Host "Processing…"  
} Until ($answer -eq "stop")

## While  
- Checks the condition **before** running.  
- Might run zero times.

Example description:

While ($answer -eq "go") {  
   Write-Host "Processing…"  
}

Loop summary:  
- Do..While → repeat WHILE condition stays true  
- Do..Until → repeat UNTIL the condition becomes true  
- While → repeat WHILE condition is true (may run 0 times)  

---

# Break and Continue

Break and Continue modify loop behavior.

## Continue  
Skips the rest of the current iteration and moves to the next.

Example description:

ForEach ($user in $users) {  
   If ($user.Name -eq "Administrator") { Continue }  
   Write-Host "Modify user object"  
}

Use case: skip invalid or special objects.

## Break  
Stops the loop entirely.

Example description:

ForEach ($user in $users) {  
   $number++  
   Write-Host "Modify User object $number"  
   If ($number -ge $max) { Break }  
}

Use case: stop after processing a limit.

---

# Module Assessment

## 1. Which construct should you use to process an array when you're not sure how many items are in it?

Correct answer: **ForEach**

Explanation:  
ForEach automatically loops through all items, regardless of array size.

---

## 2. Which loop construct supports processing multiple objects simultaneously?

Correct answer: **ForEach-Object -Parallel**

Explanation:  
Parallel processing is available only in the ForEach-Object cmdlet in PowerShell 7, not in standard loop constructs.

