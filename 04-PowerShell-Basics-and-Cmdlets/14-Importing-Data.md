## Importing Data Into PowerShell

### Using ConvertFrom-Json  
*Analogy:* JSON is like a tightly packed suitcase — you have to **unpack** it before you can use what’s inside.

- JSON is a lightweight data format often used by web APIs.
- PowerShell can **convert** JSON → objects using `ConvertFrom-Json`.
- But PowerShell **cannot import JSON directly** (you must `Get-Content` first).

```powershell
# Convert a local JSON file into objects
$users = Get-Content C:\Scripts\Users.json | ConvertFrom-Json
```

- You can also retrieve JSON **directly from a website** using `Invoke-RestMethod`.

```powershell
# Pull JSON data from a web service (API)
$users = Invoke-RestMethod "https://hr.adatum.com/api/staff"
```

#### Additional Example (JSON Placeholder API)
*This API is perfect for practice — no authentication required.*

```powershell
# Retrieve fake user data for testing
$users = Invoke-RestMethod "https://jsonplaceholder.typicode.com/users"
```

```powershell
# View names of users returned from the API
$users.name
```

```powershell
# Show specific properties
$users | Select-Object name, email, phone
```

```powershell
# Loop through and display formatted output
ForEach ($u in $users) {
    Write-Host "User: $($u.name) | Email: $($u.email)"
}
```


### Using Import-Clixml  
*Analogy:* CLIXML files are like “PowerShell save files” — they preserve full objects, not just text.*

- XML can store more complex data than CSV.
- `Import-Clixml` restores full PowerShell objects, including nested properties.

```powershell
# Import XML as PowerShell objects
$users = Import-Clixml C:\Scripts\Users.xml
```

```powershell
# View all available properties in the imported object
$users | Get-Member
```

```powershell
# Limit results (first 5 users)
$users | Select-Object -First 5
```


### Using Import-Csv  
*Analogy:* CSVs are like spreadsheets saved as plain text.*

- First row = headers (become property names)
- Each line becomes one object in an array

```powershell
# Import CSV as objects
$users = Import-Csv C:\Scripts\Users.csv
```

```powershell
# Custom delimiter CSV
$users = Import-Csv C:\Scripts\Users.csv -Delimiter ";"
```

```powershell
# If the CSV is missing headers, define them manually
$users = Import-Csv C:\Scripts\Users.csv -Header Name,Department,Email
```


### Using Get-Content  
*Analogy:* `Get-Content` reads a file **line by line** into an array.*

- Retrieves text from files  
- Each line becomes one item in an array

```powershell
# Read a list of computers into an array
$computers = Get-Content "C:\Scripts\computers.txt"
```

```powershell
# Import multiple files using wildcards
Get-Content -Path "C:\Scripts\*" -Include "*.txt","*.log"
```

```powershell
# Get the first 10 lines
Get-Content "C:\Scripts\computers.txt" -TotalCount 10
```

```powershell
# Get the last lines (like tail)
Get-Content "C:\Scripts\computers.txt" -Tail 20
```

