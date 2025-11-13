# PowerShell Data Importing — Get-Content, CSV, XML, JSON

---

# Get-Content — Read Text Files into PowerShell

Get-Content reads a text file and turns each line into an item in an **array**.

Concept:
- Each line in the file = one item in the array.
- Useful for lists of computers, servers, usernames, etc.

Example description:
$computers = Get-Content C:\Scripts\computers.txt

Now $computers contains multiple items:
- $computers[0]
- $computers[1]
- etc.

You can loop through them with ForEach to perform actions on each computer.

---

# Using Wildcards, Include, and Exclude

Get-Content supports wildcards to read multiple files at once.

Examples (description only):

Get-Content -Path "C:\Scripts\*" -Include "*.txt","*.log"  
→ Reads only .txt and .log files.

You can also use -Exclude to skip certain files.

This is useful for scanning log files for errors.

---

# Limiting Output: -TotalCount and -Tail

-TotalCount → get the first X lines  
-Tail → get the last X lines

Example description:
Get-Content C:\Scripts\computers.txt -TotalCount 10  
→ Reads only the first 10 lines.

---

# Import-Csv — Read CSV Files as Objects

CSV files are common export formats for many apps.

Import-Csv:
- Reads the file
- Uses the first row as **property names**
- Turns each row into an **object**
- Stores the objects in an array

Example (described):
$users = Import-Csv C:\Scripts\Users.csv

If the CSV contains:

First,Last,UserID,Department  
Amelie,Garner,AGarner,Sales  
Evan,Norman,ENorman,Sales  
Siu,Robben,SRobben,Sales  

Then $users has 3 objects, each with properties:
- First  
- Last  
- UserID  
- Department  

You can access data like:
$users[2].UserID

---

# Import-Csv with Other Delimiters or No Header Row

If the file uses something other than commas:
Use -Delimiter.

If there is **no header row**, use -Header to supply your own names.
Every row becomes data.

---

# Import-Clixml — Import Structured XML Data

XML supports nested and complex data.

Import-Clixml:
- Reads XML
- Converts it into objects
- Returns an array of objects if the file contains multiple entries

Example (described):
$users = Import-Clixml C:\Scripts\Users.xml

Use Get-Member to explore the imported structure because XML files can contain layered data.

You can limit imported objects with:
- -First (first X objects)
- -Skip (skip X objects)

---

# ConvertFrom-Json — Work with JSON Data

JSON is lightweight and supports nested data like XML.

PowerShell does not import JSON directly from a file.  
You must:
1. Read it with Get-Content  
2. Convert it with ConvertFrom-Json

Example (described):
$users = Get-Content C:\Scripts\Users.json | ConvertFrom-Json

---

# Invoke-RestMethod — Get JSON Directly from Web APIs

Many web services return data in JSON.

Invoke-RestMethod:
- Calls a web API
- Retrieves data
- Automatically converts JSON into PowerShell objects

Example (described):
$users = Invoke-RestMethod "https://hr.adatum.com/api/staff"

Notes:
- Web service URLs are not standardized; check documentation.
- Invoke-RestMethod can also process XML, RSS, and Atom feeds.

---

# Module Assessment

## 1. Which cmdlet imports data from a text file?

Correct answer: **Get-Content**

---

## 2. Which cmdlet retrieves data from an XML file?

Correct answer: **Import-Clixml**

