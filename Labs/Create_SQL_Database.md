# Create a SQL Database — Step-By-Step Checklist

## Task 1 — Create the Database

- [ ] Sign in to **https://portal.azure.com**
- [ ] Go to **All services** → search **SQL databases**
- [ ] Click **+ Add → + Create → + New**

### **Basics Tab**
- [ ] Subscription: *Default*
- [ ] Resource group: *Default*
- [ ] Database name: **db1**
- [ ] Server → **Create new**
  - Server name: **sqlserverxxxx** (replace *xxxx* with a unique value)
  - Location: **East US**
  - Authentication: **SQL authentication**
  - Server admin login: **sqluser**
  - Password: **Pa$$w0rd1234**
  - [ ] Click **OK**

### **Networking Tab**
- [ ] Connectivity method: **Public endpoint**
- [ ] Allow Azure services to access this server: **Yes**
- [ ] Add current client IP: **No**

### **Security Tab**
- [ ] Microsoft Defender for SQL: **Not now**

### **Additional Settings Tab**
- [ ] Use existing data: **Sample (AdventureWorksLT)**

- [ ] Click **Review + create**
- [ ] Click **Create**
- [ ] Wait ~2–5 minutes for deployment

---

## Task 2 — Test the Database

- [ ] After deployment → Click **Go to resource**
  - OR search **SQL databases** → select **db1**

- [ ] On the db1 blade → Click **Query editor (preview)**

### **Login Attempt**
- [ ] Login:
  - Username: **sqluser**
  - Password: **Pa$$w0rd1234**
- [ ] Login will fail (expected)
- [ ] Read the error → note the IP address that needs firewall access

### **Allow the IP**
- [ ] Return to **db1 → Overview**
- [ ] Click **Set server firewall**
- [ ] Click **+ Add client IP**
  - Ensure the IP from the error is listed (autofilled or paste manually)
- [ ] Click **Save**

### **Run the Query**
- [ ] Return to **Query editor (preview)**
- [ ] Login again (sqluser / Pa$$w0rd1234)
- [ ] Wait 1–2 minutes if the firewall rule is still applying
- [ ] Paste the query:

```sql
SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
FROM SalesLT.ProductCategory pc
JOIN SalesLT.Product p
ON pc.productcategoryid = p.productcategoryid;
