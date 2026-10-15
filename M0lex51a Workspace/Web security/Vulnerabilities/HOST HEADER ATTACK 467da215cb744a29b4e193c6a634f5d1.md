# HOST HEADER ATTACK

Tags: In progress
Status: Not started

## What is host header(demo)

when there is number of domains  hosted on same server (ip) that is mean that it have same ip (server ip) so to make the web server know the requested sites from user  it is through the **(host header )** 

and it can specify sites with port number like that **www.example.com:446** 

## ACCESS LOCAL RESOURCES (virtual host name enumeration)

### what is virtual host names

 when i host 10 site on same server so every site have specific virtual name that carry the location of 

site on server  

![virtual name.png](HOST%20HEADER%20ATTACK/virtual_name.png)

so i can in server config add a virtual name carry a specific location if the user 

give  a unknown host name 

### access local resources attack (virtual host name enumeration)

it happen when the user input in host header unknown value and by default (non common now)

in server config the first virtual name is local host so if the user input unknown value the server 

will redirect the user to the first virtual name (local host)

### exploitation (enumeration)

intercept the request and change the host header parameter 

to unknown value , and try that words 

- localhost
- stage
- dev
- test

### password reset scenario (how to test)

creating a temp account through (TEMPEMAIL.COM) and write  the temp email 

and intercept the request and change the host header value to [localhost](http://localhost) or server i can see on the logs 

so after the vulnerable site send the reset link to the email i should go to the mail and put the mouse on reset link 

if the reset link directed to local host so it is vulnerable to host header attack 

### xss into host header

it happen when the host header value is printed on the page when i request it 

so i can intercept the request and write xss pylod in host header 

### useful tool to test host header

(virtual host scanner)