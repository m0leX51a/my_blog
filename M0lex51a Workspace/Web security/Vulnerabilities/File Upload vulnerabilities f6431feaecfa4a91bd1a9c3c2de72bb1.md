# File Upload vulnerabilities

Tags: Done
Status: Not started

# What is File Upload Vulnerabilities

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead.

# What It’s Impact

1. can lead the attacker to get a web shell on server in case there no validation on file content or it’s name 
2. can lead to “Path Traversal” in case there is no validation on file name
3. can lead to “stored XSS” or “XXE” when the function allow ‘SVG’ files 
4. can lead “DOS” attack whereby the attacker fills the available disk space.

# [**How do web servers handle requests for static files?**](https://portswigger.net/web-security/file-upload#:~:text=How%20do%20web%20servers%20handle%20requests%20for%20static%20files%3F)

since static files such as images and html_pages  files stored on server the server handle requests with sequence of process the file passing by 

**Firstly :** the server parses the path in the request to identify the file extension. It then uses this to determine the type of the file being requested, typically by comparing it to a list of preconfigured mappings between extensions and MIME types, What happens next depends on the file type and the server's configuration —>

- If this file type is non-executable, such as an image or a static HTML page, the server may just send the file's contents to the client in an HTTP response.
- If the file type is executable, such as a PHP file, **and** the server is configured to execute files of this type, it will assign variables based on the headers and parameters in the HTTP request before running the script. The resulting output may then be sent to the client in an HTTP response.
- If the file type is executable, but the server **is not** configured to execute files of this type, it will generally respond with an error. However, in some cases, the contents of the file may still be served to the client as plain text. Such misconfigurations can occasionally be exploited to leak source code and other sensitive information

# How to test file upload functions

before diving into test steps we will explore the architecture of the **file upload function request** .

![file upload request.png](File%20Upload%20vulnerabilities/file_upload_request.png)

  

the request structure consists of 3 parts : 

1. “ filename “ parameter 
2. ‘” Content -Type “ header which tells the server the MIME type of the data that was submitted using this input. 
3. file contents it self 

so  the method we will follow in  testing process  is trying to manipulate  one part of the 3 parts , when sending the request and observing the response, if it changed at one time so we will focus on these part that we manipulated to bypass.

   

# Bypass Restrictions On File Upload Function

After determine which part that the server validate and put it’s restrictions next we will try to bypass those restrictions based on our case 

## No Restrictions on file upload function

If the application doesn’t validate on any part of request we can upload a simple PHP code to get a web shell on server for example 

```php
<?php

echo system($_GET["paramter_name"]) ;

?>
```

after uploading file containing this code simply we will go to to the file path on server and add the parameter to the path and write os commands such 

<aside>
💡 target.com/files/file.php?paramter_name=whoami

</aside>

## Validating only on  “ Content -Type “ header

if the server has a restrictions only on “Content-Type“ value and trust it whether the user manipulate it or not  and neglect The other parts  

the first step to determine what the content types the application accept , we can do it by look on “Accept ” header or by guessing it 

so after upload our shell we should change the “Content -Type“ header value to acceptable value instead of “application/octet-stream” value 

for example an application accept only “image/jpeg” so when uploading my shell i changed the  “Content -Type“ header value  to “image/jpeg” 

![Untitled](File%20Upload%20vulnerabilities/Untitled.png)

## **Web shell upload via path traversal**

In these case developers put some restrictions on the directory that files uploaded to ,

so  servers generally only run scripts whose MIME type they have been explicitly configured to execute and according to these case the server return a error message or handle the php code as plain text .

then if these endpoint is vulnerable to “path traversal ” we can exploit it by upload our shell to another directory 

for example if the file upload function save files to “/files/avatars/shell.php” and sever handle it as a plain text we can upload shell to “/files” directory instead of “/files/avatars/” by adding “../” to file name and don’t forget to encode “/”   

![Untitled](File%20Upload%20vulnerabilities/Untitled%201.png)

then we can browse “targe.com/files/shell.php?cmd=whoami” to access our shell 

## **Insufficient blacklisting of dangerous file types**

blacklisting is a way developer some time uses to prevent the user to upload from uploading malicious code to server , here we can bypass blacklisting by using tow technics 

1. **Overriding the server configuration**
2. **Obfuscating file extensions**

we will explore the tow technics in detail as follow 

### **Overriding the server configuration**

demo :

as we talk that the server has configuration that controls what MIME types that allowed to run on server  ,for example on “apache” servers  the developer can set a global configurations for all the server files through “`/etc/apache2/apache2.conf` ” file , and  can set  configurations for specific directory through “.htaccess”  file or on IIS servers using a `web.config` file 

for example in apache servers will use the following configuration to make the server to allow  a specific extension name as a PHP code 

```php
AddType application/x-httpd-php .MX
```

so if the uploaded file in “MX”  extension the server will handle it as a PHP code 

lets take an example an upload function saves files to “target.com/files/avatars/file.php”  and server set “avatars ” configurations through “htaccess” file and configurations prevents user from uploading PHP files so we can bypass it by overriding “.htaccess” file and make it allow to uploading PHP files 

![Untitled](File%20Upload%20vulnerabilities/Untitled%202.png)

then the server will allow to excute a PHP code and we can get a web shell

![Untitled](File%20Upload%20vulnerabilities/Untitled%203.png)

### **Obfuscating file extensions**

in blacklisting developers doesn’t allow extensions that could be runs on server in some condition, for example the following code put some regex on file extension to prevent PHP files.

```php
foreach($files as $file)

{
if(preg_match('^.*\.(php|phpl|php2|php3|php4|php5|php6|php7|phtml|exe)$') , $file))
echo "Bad Hacker!" ;
header("Location: http://badHacker.com/");
}
```

but the developer doesn’t validate it well because it still can by bypassed with add “pHp” extension instead of “php” because it doesn’t match on upper case, or bypassed by adding unpopular extension such “phar” or “pht”

there is many technics to bypass blacklisting prevention as follow:

1. trying upper and lower case 
2. Provide multiple extensions such “exploit.php.jpg”
3. Add trailing characters “exploit.php.”
4. Try using the URL encoding (or double URL encoding) for dots “exploit%2Ephp”
5. Add semicolons or URL-encoded null byte characters before the file extension “exploit.php%00.jpg”
6. Try using multibyte unicode characters,
7. include extension in extension when stripping “exploit.p.phphp”

for more technics “[https://book.hacktricks.xyz/pentesting-web/file-upload#:~:text=%3A .yaws-,Bypass file extensions checks,-If they apply](https://book.hacktricks.xyz/pentesting-web/file-upload#:~:text=%3A%20.yaws-,Bypass%20file%20extensions%20checks,-If%20they%20apply)”

## **Flawed validation of the file's contents**

in these case server validate on the content part of file by checking the magic byte of the file and missing the filename.

so what is magic byte of files?

it the first few bytes of a file which is used to recognize a file. It is not visible if you open the file. But can be seen using some special tools such “exiftool” 

here a list of file types and it’s magic bytes: “[https://en.wikipedia.org/wiki/List_of_file_signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)”  

so we can bypass this case by add to our shell file a magic bytes of file types that server accept, here an example of image include our shell code 

![Untitled](File%20Upload%20vulnerabilities/Untitled%204.png)

## **Exploiting file upload vulnerabilities without remote code execution**

Exploiting file upload vulnerabilities not limited to get RCE but we can get another vulnerability such “XSS” or “XXE” as we will see.

### **Uploading malicious client-side scripts**

if we can upload HTML or SVG files and the file will showing to another users and file will appear to them then we can imbed a “<script>” tag in SVG file to get “stored XSS” as below 

![Untitled](File%20Upload%20vulnerabilities/Untitled%205.png)

or we can Set **filename** to `<svg onload=alert(document.domain)>` if no validation on filename to achieve a XSS 

### **Exploiting vulnerabilities in the parsing of uploaded file**

For example ,if you know that the server parses XML-based files, such as Microsoft Office `.doc` or `.xls` files, this may be a potential vector for XXE injection attacks.

## **Uploading files using PUT**

we can try sending `OPTIONS` requests to different endpoints to test for any that advertise support for the `PUT` method

if we found it we can provide an alternative means of uploading malicious files, even when an upload function isn't available via the web interface.

```php
PUT /images/exploit.php HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-httpd-php
Content-Length: 49

<?php echo file_get_contents('/path/to/file'); ?>
```

## what can reach in case you upload

![Untitled](File%20Upload%20vulnerabilities/Untitled%206.png)