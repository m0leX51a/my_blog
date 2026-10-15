# Broken Access Control

Tags: Done
Status: Not started

# **What is access control?**

- **Authentication** identifies the user and confirms that they are who they say they are.
- **Session management** identifies which subsequent HTTP requests are being made by that same user.
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.

# Access Control categories

### **Vertical access controls**

Vertical access controls are mechanisms that restrict access to sensitive functionality that is not available to other types of users. 

for example the administrator have the access to delete user functionality when the normal user don’t  

### **Horizontal access controls**

Horizontal access controls are mechanisms that restrict access to resources to the users who are specifically allowed to access those resources.

it’s users have the same functionality access (same privileges) but every user could only  show his information like banking account information

  

### **Context-dependent access controls**

Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the user's interaction with it.

Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.

# **Examples of broken access controls**

Broken access control vulnerabilities exist when a user can in fact access some resource or perform some action that they are not supposed to be able to access.

## **Vertical privilege escalation**

if normal user could access the administrator functionality 

### **Unprotected functionality**

a user might simply be able to access the administrative functions by browsing directly to the relevant admin URL

for example

```jsx
https://insecure-website.com/admin
https://insecure-website.com/robots.txt
```

in some cases the admin panel could be access by URL but the URL could be less predictable for example 

```jsx
https://insecure-website.com/administrator-panel-yb556
```

some times the right URL could be disclosed in java script files like the following 

```jsx
<script>
var isAdmin = false;
if (isAdmin) {
	...
	var adminPanelTag = document.createElement('a');
	adminPanelTag.setAttribute('https://insecure-website.com/administrator-panel-yb556');
	adminPanelTag.innerText = 'Admin panel';
	...
}
</script>
```

### **Parameter-based access control methods**

Some applications determine the user's access rights or role at login based on parameter that comes from client side and user can modify this parameter to gain unpredicted access 

```jsx
https://insecure-website.com/login/home.jsp?admin=false //turn it into "true" to be admin
https://insecure-website.com/login/home.jsp?role=2 //turn it into "1" to be admin 
```

Note!! some times in state changing endpoints for example changing email functionality the application send the data via Json type so when sending it check the request and response and try to simulate it to get more access . 

### **Broken access control resulting from platform misconfiguration**

For example an application might configure rules like the following:

```jsx
DENY: POST, /admin/deleteUser, managers
```

This rule denies access to the `POST` method on the URL `/admin/deleteUser`

so there is (`X-Original-URL` , `X-Rewrite-URL`) these headers lets us to override the URL in the original request and it will help us ho bypass it by request like that 

```jsx
POST / HTTP/1.1
	X-Original-URL: /admin/deleteUser 
```

sometimes An alternative attack can arise in relation to the HTTP method used in the request

because some web sites are tolerant of alternate HTTP request methods when performing an action, so we could bypass it by trying to change request method to (GET,POST,POSTX,… etc).

### **Broken access control resulting from URL-matching discrepancies**

some times the application match on lowercase only so   /Admin can points to /admin 

we could add slash at the end of url like /admin/ .

## **Horizontal privilege escalation**

it means to get access to resources of another user 

For example, a user might ordinarily access their own account page using a URL like the following:

> `https://insecure-website.com/myaccount?id=123`
> 

when changing the id value to 122 for example it will show you the page of user who have id=**122** 

sometimes an application use globally unique identifiers (GUIDs) to identify users so the id will be un predictable so now trying to browse the application like discover posts open users profile trying to get users ids 

In some cases, an application does detect when the user is not permitted to access the resource

so it will redirect you to another page so check the redirection response trying to get some some important information about user like API key . 

## **Horizontal to vertical privilege escalation**

it means that if there is that end point like the following  

> `https://insecure-website.com/myaccount?id=456`
> 

so when changing the id value to 455 i will get access to user functionality who has the 455 id 

so if the administrator have id =1 so i could access it by changing id to id =1  

## Insecure direct object references

the insecure direct object references (IDOR) is kind of Broken access control attack so  

you can see the reference [https://www.notion.so/IDOR-76ccdc8d53b64d2c8095be065b8836c4?pvs=4](https://www.notion.so/IDOR-76ccdc8d53b64d2c8095be065b8836c4?pvs=4) 

## **Access control vulnerabilities in multi-step processes**

Many web sites implement important functions over a series of steps. for example changing users privileges require 3 steps so some Developers put access control rules on the first 2 steps only 

so the attacker can bypass it by go to the third step directly 

## **Referer-based access control**

Some websites base access controls on the `Referer` header submitted in the HTTP request .

for example there is main page /admin   and there subpage admin/delete_user so to access this subpage the `Referer` header should come from  /admin  page 

## **Location-based access control**

Some web sites enforce access controls over resources based on the user's geographical location. 

and it could bypass by using web proxies or VPNs or  manipulation of client-side geolocation mechanisms

## **How to prevent access control vulnerabilities**

- Never rely on obfuscation alone for access control.
- Unless a resource is intended to be publicly accessible, deny access by default.
- Wherever possible, use a single application-wide mechanism for enforcing access controls.
- At the code level, make it mandatory for developers to declare the access that is allowed for each resource, and deny access by default.
- Thoroughly audit and test access controls to ensure they are working as designed.