# OAuthV2 1d93c74efb3c80e8b623fc6e767b1df5

## OAuthV2

## Definition

is an open-standard protocol that lets apps securely access your info without you spilling your passwords everywhere. It’s all about giving limited access so your data stays safe.

**Usecase**

its about authorization. It lets one app access your data from another app without sharing your creds. Like letting Instagram post to your Facebook without giving Instagram your Facebook password

## **OAuth Actors**

![image.png](<../.gitbook/assets/image (1).png>)

#### **1. Resource Owner (You, the User)**

* Owner is the person who owns the data or resources that are being accessed
* user give permission for your data to be accessed by other apps without handing over your password

#### **2. Client (The App)**

* **Who They Are:** This is the app or service that wants to access your data. It could be anything from a social media app to a third-party tool.
* **What They Do:** The Client requests access to your data from the Resource Owner. It’s like that friend asking to borrow your stuff but in a secure, controlled way.

#### **3. Authorization Server (The Bouncer)**

* **Who They Are:** The gatekeeper of your data. This server is responsible for authenticating the Resource Owner and issuing tokens.
* **What They Do:** When the Client asks for access, the Authorization Server checks if everything’s legit and then hands out tokens that grant specific access levels.

#### **4. Resource Server**

* **Who They Are:** The actual vault where your data resides. This server holds the protected resources and only allows access via valid tokens.
* **What They Do:**
  * **Receive Requests with Tokens**: The client (e.g., a web or mobile application) makes requests to the Resource Server, typically including an **access token** in the `Authorization` header.
  * **Validate Tokens**: The Resource Server confirms that the token is valid. This can happen in several ways:
    * **Local Validation** (e.g., verifying a JWT’s signature and claims using a public key).
    * **Introspection** (calling back to the Authorization Server to confirm the token’s status, scope, and expiration).
  * **Check Scope/Claims**: Even if the token is valid, the Resource Server needs to check whether the **scope** or **claims** in the token permit the requested action.
  * **Grant or Deny Access**: If the token is valid and the request is authorized, the Resource Server **returns the requested data** or **performs the requested operation**. Otherwise, it denies the request.

## **Authorization Code Grant Life Cycle**

![image.png](<../.gitbook/assets/image 1 (2).png>)

* **When to Use It:** Best for server-side applications where the Client can keep a secret (like web apps with a backend server).
* **How It Works:**
  * **User Authorization:** The user logs in and grants permission to the Client.
  * **Authorization Code:** The Authorization Server sends back a one-time code to the Client.
  * **Token Exchange:** The Client exchanges this code for Access and Refresh Tokens.
