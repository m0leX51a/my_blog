# WEB HACKING RECONNAISSANCE chapter

# Manually Walking Through the Target

it will help to first manually walk through
the application to learn more about it.

- uncover every feature in the application that users can access by
    1. browsing through every page and clicking every link
    2. Access the functionalities that you don’t usually use
    3. Sign up for an account at every privilege level like (member, admin , etc ) to reveal all of the application’s features. 
    4. how different users interact with each other
    5. where the data entry and end  points are
    

---

# Google Dorking

advanced Google searches are a powerful technique that hackers
often use to perform recon. Hackers call this Google **DORKING** .

### how is google is useful for hackers?

for ordinary users Google is just a text search tool for finding images, videos, and
web pages. But for the hacker, Google can be a means of discovering valuable
information such as hidden admin portals, unlocked password files,
and leaked authentication keys.

### how to use google Dorking?

Google’s search engine has its own built-inquery language that helps
you filter your searches.

**useful operators for search** 

- **site**
    
    Tells Google to show you results from a certain site only, it useful to find the most reputable source on the topic you searched for.
    
    example  in search box—> print function site:python.org 
    
- **inurl**
    
    Searches for pages with a URL that match the search string.
    
    say you know that the page called course/jumpto.php in a website 
    
    to reach it easily using **inurl** 
    
    inurl:"/course/jumpto.php" site:example.com.
    
- **intitle**
    
    Finds specific strings in a page’s [title](http://title.so), so This is useful because it allows you
    
     to find pages that contain a particular type of content like the index page .
    
    example —> intitle:"index of" site:example.com.
    
- **link**
    
    Searches for web pages that contain links to a specified URL. You can
    use this to find documentation about obscure technologies or vulnerabilities.
    
    it useful when you searching for uncommon topic like denial-of-service (ReDoS) vulnerability
    
    let’s say we searching in Wikipedia so it will be like that 
    
    link:"https://[en.wikipedia.org/wiki/ReDoS](http://en.wikipedia.org/wiki/ReDoS)".
    
- **filetype**
    
    Searches for pages with a specific file extension, often hackers use it to locate 
    
    a sensitive files like (password ,log , etc  ) files 
    
    for example locating log file in website —>
    
    filetype:log site:example.com.   
    
- **Wildcard (*)**
    
    You can use the wildcard operator (*) within searches to mean any character
    or series of characters. For example, the following query will return
    any string that starts with how to hack and ends with using Google
    
    match with strings like how to hack websites using Google, how to hack applications
    using Google, and so on:
    
     "how to hack * using Google".
    
- **Quotes (" ")**
    
    Adding quotation marks around your search terms forces an exact match, 
    
    that mean if i use it in search for example like that “how to hack”
    
    the query will search for pages that contain ( how ) , (hack) not necessarily together like that (how to hack) 
    
- **Or (|)**
    
    can be used to search for one search term or the other, or both at the same time.
    
    for example this query will search for how to hack on either Reddit or Stack Overflow:
    "how to hack" site:([reddit.com](http://reddit.com/) | [stackoverflow.com](http://stackoverflow.com/))
    
- **Minus (-)**
    
    The minus operator (-) excludes certain search results. For example,
    let’s say you’re interested in learning about websites that discuss hacking,
    but not those that discuss hacking PHP. This query will search for pages
    that contain how to hack websites but not php: "how to hack websites" -php.
    

## Advanced using of google Dorking

- **collect target subdomain**
    
    we can use —> site:*.example.com
    
- **looking for special endpoints that can lead to vulnerabilities**
    
    (Kibana is a data visualization tool that displays server operation data such
    as server logs)
    
    accessing kibana dashboard allow to hacker to collect extensive information about a site’s
    operation , it can be in path **app/kibana**
    
    you can  try access it if it unprotected, by using query site:example.com inurl:app/kibana
    
- **finding company resources hosted by a third party online such amazon S3 buckets**
    
    by using query —> site:s3.amazonaws.com COMPANY_NAME
    
- **Look for special extensions that could indicate a sensitive file**
    
    such log files by using query —> site:example.com ext:php
    
    and  php, cfm, asp, jsp ,pl  that extensions often used for scripts file
    
    for example query —> site:example.com ext:asp
    
- **combine search terms for a more accurate search**
    
    for example to get file whose extension (txt) and it’s name (passwords) in site www.example.com
    
    the query —> site: [www.example.com](http://www.example.com) ext: txt  passwords  
    
- **checking out the Google Hacking Database**
    
    ([https://www.exploit-db.com/google-hacking-database/](https://www.exploit-db.com/google-hacking-database/)), 
    
    this site used by hackers to share there Google search queries
    
    so It contains many search queries that helping in (recon) 
    
    you can find in it queries to  look for( files containing passwords, common
    URLs of admin portals, or pages built using vulnerable software).
    

---

# Scope Discovery

**Let’s now dive into recon itself !!**

First, always verify the target’s  scope that mentioned in 

 program  policy to verify the in scope and the out of scope   

the in scope —> (domains, subdomains, and IP addresses can you attack) 

the out of scope —> (domains, subdomains, and IP addresses you’re not allowed to attack)

## WHOIS and Reverse WHOIS

When companies or individuals register a domain name so the domain registrar need 

identifying information, such as( mailing address ,phone number ,email address)

### WHOIS

—> used to query identifying information of the domain

the command  —> whois domai.com

### Reverse WHOIS

some organization and individuals use a third party  service called  “ domain privacy ” to replace the user’s information with that of a forwarding service  so you you can searching a database by using 

any y identifying information ( org name ,phone number ,etc ) so it will show the all domains 

that belong to the same owner 

so you can collect some top level domains of target 

and you can use ([https://viewdns.info/reversewhois/](https://viewdns.info/reversewhois/))

---

## IP Addresses

 you can get  the ip address of the target domain using (nslookup) command 

once you get it you can perform a reverse IP lookup using ([https://viewdns.info/reversewhois/](https://viewdns.info/reversewhois/))

that will show you the  domains that hosted on same server

also using (whois ) command on ip address can show you ranges of the ip that belongs 

to the target by seeing the (NetRange) field.