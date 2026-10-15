# Tips from bug bounty hunters

- Allam
    
    If you come across a PDF file upload function, you can test it for XSS by using this PDF: (drive.google.com/file/d/1JQ_DVG…)
    
- [**Ankita Dhakar** @expankita](https://twitter.com/expankita)
    
    **sqli checklist** 
    
    1. To find SQLi, you need to identify all the places where user input is used to construct SQL queries. This includes form fields, URL parameters, cookies, and headers
    2. Start with manual testing: enter payloads like ' OR '1'='1 or '; DROP TABLE users; to see if the application executes them. An unexpected result or error may indicate SQLi
    3. Automated tools can help too! Use scanners like SQLmap, OWASP ZAP, or Burp Suite to identify potential injection points
        
        Pay attention to error messages. They can reveal insights into the database structure and guide an attacker.15[653](https://twitter.com/expankita/status/1728020642262307042/analytics)
        
    4. ook for time-based SQLi, where a delay in the response indicates a vulnerability. For instance, using payloads like '; WAITFOR DELAY '00:00:05'.
    5. Blind SQLi doesn't return data but alters the application's content or behavior. Detect this by changing a query logically and observing changes
    
    finaly 
    
    Use parameterized queries, also known as prepared statements, to mitigate SQLi risks. They ensure the database treats input as data, not commands.
    
    ORM frameworks can also safeguard against SQLi by avoiding raw SQL queries altogether. They're not foolproof, but they help!
    
- [Ryan John](https://www.youtube.com/@ryan_phdsec)
    - look at last time that the subdomain had updated
    - look for used CMS in subdomain and it’s pluggeins