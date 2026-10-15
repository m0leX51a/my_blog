# Master Http Protocol

source : “[https://www.facebook.com/share/p/1E6Zrdde9V/](https://www.facebook.com/share/p/1E6Zrdde9V/)”

# Http Concept

Http protocol : it is way of communication between the server and client where the client send request to server and vice versa

### Example

```go
GET /users/15 HTTP/1.1
// method , request target , http version 
```

method : describe how the server can handle the sent request and the needed operation 

request target  : an identifier that specify what resource in the server or (host) that needed to satisfy the request 

resource : in context of http the resource concept identified base on application design 

- resource can be reached through the URI or the target request
- resource can be users list , image , pdf , tickets , .... etc

<aside>
💡

if we understand the GET method is translated to SELECT sql command  on database , so if the application using Mongodb as database!!! , so the concept is request reach the sever and server handle it based on the implementation or the technology that used  

</aside>

# Http Methods

**Http Methods** : tell the server what operation should be done and the way to operate

## GET Method

GET Method : mean that the client need a representation of the resource that defined in the target request 

- **GET is safe method**
    - **safe method** : it doesn't alter the state of the server. In other words, a method is safe if it leads to a read-only operation
    
    <aside>
    💡
    
    GET is safe method it doesn’t mean that it can’t be vulnerable for example if the request target is “`GET /delete-account`” so it is only reaching this endpoint can delete accounts based on some considerations 
    
    </aside>
    
- **GET is  idempotent**
    - **idempotent :** if the intended effect on the server of making a single request is the same as the effect of making several identical requests**.**
- **Example of 200 ok GET request and response**
    
    ```go
    GET /contact HTTP/1.1
    Host: example.com
    User-Agent: curl/8.6.0
    Accept: */*
    ```
    
    ```go
    HTTP/1.1 200 OK
    Content-Type: text/html; charset=UTF-8
    Date: Fri, 21 Jun 2024 14:18:33 GMT
    Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
    Content-Length: 1234
    
    <!doctype html>
    <!-- HTML content follows -->
    ```
    

## HEAD Method

same as GET method except it doesn’t retrieve the response body as the GET method it retrieve only the headers that can be retrieved through the GET method 

- **Example of 200 ok HEAD request and response**
    
    ```go
    HEAD / HTTP/1.1
    Host: example.com
    User-Agent: curl/8.6.0
    Accept: */*
    ```
    
    ```go
    HTTP/1.1 200 OK
    Content-Type: text/html; charset=UTF-8
    Date: Wed, 04 Sep 2024 10:33:11 GMT
    Content-Length: 1234567
    ```
    

## POST Method

the client request the target resource to handle the representation of request body in the way that already defined by the target resource and it can result operations on the target resource such as (creation , processing , adding data ,  etc … )

- **data in body**
    - based on the predefined representation the data sent in the request body
- **not safe as it self**
    - the https use the tls to encrypt the data where it transit and to ensure of the entity that claim to be authorized
    - sever should make security controls specially in the post requests
- **Example of 200 ok POST request**
    
    ```go
    POST /test HTTP/1.1
    Host: example.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 27
    
    field1=value1&field2=value2
    ```
    

## PUT Method

we can call PUT Method as the fully modification method in which the main semantic of PUT method is the  request representation should be the intended state of resource that predefined in the target 

- **Representation**
    - the intended resource representation should be fully satisfied in the request representation , mean the whole fields should be passed through request not only the required field to change

<aside>
💡

application may have it’s representation  model or additional rules or another way to handle PUT request , all of that based on the application implementation 

</aside>

- **PUT is  idempotent**
- **Example**
    
    request line : `PUT /users/15 HTTP/1.1`  has representation of 3 fields (name , phone , email)
    
    so to change the name field value we need also to send the another field values based on the predefined representation 
    
- **Example of 200 ok PUT request**
    
    ```go
    PUT /new.html HTTP/1.1
    Host: example.com
    Content-type: text/html
    Content-length: 16
    
    <p>New File</p>
    ```
    
    ```go
    HTTP/1.1 201 Created
    Content-Location: /new.html
    ```
    

## PATCH Method

used for partial modification on target resource and not required to send the full representation but only the intended part of representation is enough 

- **the same mentioned example**
    - request line : `PATCH /users/15 HTTP/1.1`  has representation of 3 fields (name , phone , email) if we need to change the name field value  so we don’t have to send the other fields values
- **PATHCH can be idempotent or not idempotent based on the patch document**
- **EXAMPLE**
    - representation
    
    ```json
    {
      "firstName": "Example",
      "LastName": "User",
      "userId": 123,
      "signupDate": "2024-09-09T21:48:58Z",
      "status": "active",
      "registeredDevice": {
        "id": 1,
        "name": "personal",
        "manufacturer": {
          "name": "Hardware corp"
        }
      }
    }
    ```
    
    - request to change “status” field
        
        ```go
        PATCH /users/123 HTTP/1.1
        Host: example.com
        Content-Type: application/json
        Content-Length: 27
        Authorization: Bearer ABC123
        
        {
          "status": "suspended"
        }
        ```
        
    - here the success statues in response  is 204 depending on implementation
        - 204 status means the request was successful and no additional information needs to be sent back to the client:
        
        ```go
        HTTP/1.1 204 No Content
        Content-Location: /users/123
        ETag: "e0023aa4f"
        ```
        

## DELETE Method

the client ask origin server to delete resource from it’s  state that give it the availability 

- **DELETE IS idempotent**
    - NOTE : if response of  2 DELETE requests has difference it doesn’t mean it is not idempotent but it is idempotent because it have the same final state (resources deletion )
- **Not necessary to be physical deletion**
    - not necessary to delete record from database but it  possible the state of entity change
        - for example if delete user operation done and application need to store “deleted users ” records so deletion change field on user entity for example called “status” so based on this operation this user entity association should be changed
- **Example of vulnerability based on misconfiguration on DELETE method**
    - request line : `DELETE /users/15 HTTP/1.1`  if the developer apply authorization on GET method on this endpoint (to fetch user data ) and forget to make authorization on DELETE method so any one can delete users where he doesn’t suppose have the ability of it this vulnerability called “authorization bypass”
- **Example of 200 ok DELTE request and response**
    
    ```go
    DELETE /file.html HTTP/1.1
    Host: example.com
    ```
    
    ```go
    HTTP/1.1 200 OK
    Content-Type: text/html; charset=UTF-8
    Date: Fri, 21 Jun 2024 14:18:33 GMT
    Content-Length: 1234
    
    <html lang="en-US">
      <body>
        <h1>File "file.html" deleted.</h1>
      </body>
    </html>
    ```
    

## OPTIONS Method

The **OPTIONS** HTTP method requests permitted communication options for a given URL or server. This can be used to test the allowed HTTP methods for a request, or to determine whether a request would succeed when making a CORS preflighted request. A client can specify a URL with this method, or an asterisk (`*`) to refer to the entire server.

- **OPTIONS is idempotent**
- **OPTIONS is Safe**
- **Preflight request is important usage of OPTIONS Method**
    - **Preflight request :** is a CORS request that checks to see if the CORS protocol is understood and a server is aware using specific methods and headers.
    - It is an [`OPTIONS`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/OPTIONS) request, using two or three HTTP request headers: [`Access-Control-Request-Method`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Request-Method), [`Origin`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Origin), and optionally [`Access-Control-Request-Headers`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Request-Headers).
- Example of OPTIONS Method request
    
    ```go
    OPTIONS / HTTP/2
    Host: example.org
    User-Agent: curl/8.7.1
    Accept: */*
    ```
    
    ```go
    HTTP/1.1 204 No Content
    Allow: OPTIONS, GET, HEAD, POST
    Cache-Control: max-age=604800
    Date: Thu, 13 Oct 2016 11:45:00 GMT
    Server: EOS (lax004/2813)
    ```
    

## ****Preflighted requests in CORS** from mdn site**

In [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS), a [preflight request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) is sent with the `OPTIONS` method so that the server can respond if it is acceptable to send the request. In this example, we will request permission for these parameters:

- The [`Access-Control-Request-Method`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Request-Method) header sent in the preflight request tells the server that when the actual request is sent, it will have a [`POST`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST) request method.
- The [`Access-Control-Request-Headers`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Request-Headers) header tells the server that when the actual request is sent, it will have the `X-PINGOTHER` and `Content-Type` headers.

```go
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.example
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: https://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type,x-pingother
```

The server now can respond if it will accept a request under these circumstances. In this example, the server response says that:

[`Access-Control-Allow-Origin`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Origin)The `https://foo.example` origin is permitted to request the `bar.example/resources/post-here/` URL via the following:[`Access-Control-Allow-Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Methods)[POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST)`, [`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/GET), and `OPTIONS` are permitted methods for the URL. (This header is similar to the [`Allow`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Allow) response header, but used only for [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS).)[`Access-Control-Allow-Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Headers)X-PINGOTHER` and `Content-Type` are permitted request headers for the URL.[`Access-Control-Max-Age`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Max-Age)The above permissions may be cached for 86,400 seconds (1 day).

```go
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

## CONNECET Method

CONNECT Method operate in different way than GET,POST,…etc so it doesn’t meant to get data representation or edit it . but it does the following →

The **`CONNECT`** HTTP method requests that a [proxy](https://developer.mozilla.org/en-US/docs/Glossary/Proxy_server) establish an HTTP tunnel to a destination server, and if successful, blindly forward data in both directions until the tunnel is closed.

- If a website is behind a proxy and it's enforced via network rules that all external traffic must pass through the proxy, the `CONNECT` method allows you to establish a [TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS) ([HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS)) connection with that website:
- **OPTIONS is not idempotent**
- **OPTIONS is not Safe**
- Example
    
    ```go
    CONNECT server.example.com:80 HTTP/1.1
    Host: server.example.com:80
    Proxy-Authorization: basic aGVsbG86d29ybGQ=
    ```