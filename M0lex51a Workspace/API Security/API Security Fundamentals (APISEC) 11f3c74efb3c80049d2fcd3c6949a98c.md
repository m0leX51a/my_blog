# API Security Fundamentals (APISEC)

# API Security Fundamentals Content

### **1. Introduction — Why API Security?**

### **2. Real-World API Breaches**

### **3. OWASP API Security Top 10**

### **4. 3 Pillars of API Security**

### **5. Application Security Technology Landscape**

### **6. Conclusion & Best Practices**

## **Introduction — Why API Security?**

---

### What is APIs?

(Application Programming Interfaces) enable software systems and applications to communicate and share data .

### **API attacks often involve far fewer steps than classic attacks**

![image.png](API%20Security%20Fundamentals%20(APISEC)/image.png)

## **Real-World API Breaches**

examples from real world of APIs attacks

 

### CoinBase

**What happened:**
• User scraped API calls from web UI
• Identified 4 key parameters for any
Coin base transaction
• Manipulated the parameters via API
  calls
• Sold crypto they DID NOT own

![image.png](API%20Security%20Fundamentals%20(APISEC)/image%201.png)

**Root cause** 

the application doesn’t validate submitting the transaction  owned the account related to the transaction leading “broken object level flow”

![image.png](API%20Security%20Fundamentals%20(APISEC)/image%202.png)

### US Postal Service

**What happened:**

- USPS relied on traditional code and web scanners
- Found missing API authentication
- USPS added authentication
- Left out authorization
- User A able to access User B details (any of 60M accounts)

**It was vulnerable to :**

- Broken Object Level Authorization

### Peloton

**What happened:**

- Open API allowed requests for user details with NO authentication
- 4M user account details exposed (including Joe Biden)
- Including accounts marked private
- Researcher reported to Peloton
- No response after 90 days
- "Fixed" vulnerability by adding authentication
- But hackers could still access all records, just needed to authenticate

**It was vulnerable to :**

- Broken Object Level
- Broken Authentication Authorization

### Venmo

**What happened:**

- Venmo homepage presented live feed of transactions
- Hacker sniffed traffic and identified API calls
- Wrote 20-line script, using 2 IPs
- Pulled 115K transactions/day — even with rate limiting in place
- API returned ALL transaction details
- 207M transactions harvested

**It was vulnerable to :**

- Broken Object Property Level Authorization
- Broken Authentication
- Unrestricted Resource Consumption

### Instagram

**what happened :**

- Account reset requires 6-digit code
- Researcher found API to submit reset code guesses
- Guesses limited to 200 per IP
- Researcher demonstrated could rotate through 5,000 IPs in second
- Enables takeover of ANY account

**It was vulnerable to :**

- Broken Object Property Level Authorization
- Broken Authentication

### Bumble

**what happened :**

- API permitted access to 95M user account details w/o authentication
- Incremental IDs allowed easy scraping of entire database
- Enabled calculation of users' exact location via triangulation
- API allowed paid features to be enabled without proper privileges

**It was vulnerable to :**

- Broken Object Property Level Authorization
- Broken Authentication
- Broken Function Authorization

### T-Mobile

**what happened :**

The preliminary result from our investigation indicates that the bad actor(s) obtained data from this API for approximately 37 million current postpaid and prepaid customer accounts.

**It was vulnerable to :**

- Broken Authentication

### Optus

**what happened :**

- API endpoint required no authentication to access
- Attacker harvested 9.8M user details and threatened $1M ransom
- Data included driver's license, Medicare IDs, name, phone, email

**It was vulnerable to :**

- Broken Object Property Level Authorization
- Broken Authentication
- Unrestricted Resource Consumption

### Experian

**what happened :**

- Experian partner site offered loan eligibility feature
- Feature used Experian API for lenders to automate credit score lookup
- Attacker sniffed API calls
- API accessible with no authentication
- Results delivered with name, address and *any* value for date of birth

 

**It was vulnerable to :**

- Broken Object Property Level Authorization
- Broken object Parameter Level Authorization
- Improper Inventory Management