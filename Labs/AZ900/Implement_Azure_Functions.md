# Implement Azure Functions — Step-By-Step Checklist

## **Log Analytics Workspace Setup (Required for Monitoring)**
- [ ] In Azure Portal, search **Log Analytics Workspace**
- [ ] Click **+ Create**
- [ ] Subscription: *Default*
- [ ] Resource group: **myRGFunction-lodxxxxxxxxx**
- [ ] Workspace name: *(your choice)*
- [ ] Region: **East US**
- [ ] Click **Review + Create → Create**
- [ ] Wait for deployment to finish

---

# **Task 1 — Create a Function App**

- [ ] Sign in: **https://portal.azure.com**
  - Username: `Az900User-57040974@cloudslice.onmicrosoft.com`
  - TAP: `t48#drf&.`

- [ ] Search **Function App**
- [ ] Click **+ Create**

### **Hosting Option**
- [ ] Select: **Consumption**
- [ ] Click **Select**

### **Basics Tab**
- [ ] Subscription: *Default*
- [ ] Resource group: *Default*
- [ ] Function App name: **function-xxxx** *(must be globally unique)*
- [ ] Runtime stack: **.NET**
- [ ] Version: **8 (LTS), in-process**
- [ ] Region: **East US**
- [ ] OS: **Windows**

- [ ] Click **Review + Create**
- [ ] After validation → **Create**

- [ ] Once deployed → **Go to resource**
- [ ] Confirm Status: **Running**

---

# **Task 2 — Create an HTTP Trigger Function & Test It**

- [ ] Inside the Function App → click **Functions**
- [ ] Click **Create function**

### **Template**
- [ ] Select **HTTP trigger**
- [ ] Click **Next**
- [ ] Leave defaults
- [ ] Click **Create**

- [ ] You should now be on **Code + Test**
- [ ] Review auto-generated code  
  *(The function logs the request and returns “Hello” with a name)*

### **Get Function URL**
- [ ] Click **Get Function URL**
- [ ] Select **Default (Host key)**
- [ ] Click **Copy**

### **Test in Browser**
- [ ] Paste URL into new browser tab  
  → Should output: *“Please pass a name on the query string…”*

- [ ] Append your name:  
```bash
&name=yourname
```

### Example:  
```bash
https://function-xxxx.azurewebsites.net/api/HttpTrigger1?code=ABC123&name=Cindyhttps://function-xxxx.azurewebsites.net/api/HttpTrigger1?code=ABC123&name=Cindy
```

- [ ] Press Enter → Should return **Hello, yourname**

### **View Traces**
- [ ] Go back to Function App → **HttpTrigger1**
- [ ] Click **Invocations**
- [ ] Select a timestamp
- [ ] Click **Run query in Application Insights**  
*(Only works if workspace/monitoring is connected)*

---

# **Optional Cleanup**
- [ ] Go to **Resource groups**
- [ ] Select the resource group used for this lab
- [ ] Click **Delete resource group**
- [ ] Type the name to confirm → **Delete**

