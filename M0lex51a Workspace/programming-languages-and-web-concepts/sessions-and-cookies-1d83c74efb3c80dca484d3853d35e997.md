# Sessions And Cookies 1d83c74efb3c80dca484d3853d35e997

## Sessions And Cookies

## Authentication & Authorization Methods

### Stateful Auth

which the method depend on storing the state (sessionId) in database

then the meta data in each request should contain (sessionId) so for example Cookies Base Authentication

#### Cookies Base Authentication Life Cycle

1. user attempt to login using his credentials
2. server verify this credentials
3. once it true server generate random unique identifier called sessionId
4. server store this sessionId in new entry in database
5. server send sessionId back to user and browser store it in cookies
6. where user is logged in and try to use any function of application it make request included the sessionId that stored in cookies already
7. previous step for server to identify whether the user who send the request is authorized to do that or not
8. when user is logout the session is expired then user have to login again to get a new sessionId

### Stateless

Metadata is self-contained state

* For example: user ID, operation, cart items... etc
* Must be signed to prevent tampering, and possibly also encrypted
* Example: JWT, OAuth tokens

Token : simply it contain all user data + unique signature then when any manipulating done on signature or any one try t pretend the user identity the server refuse request based on the true user signature

for example JWT

[TOKENS](../book-notes/bug-bounty-bootcamp-book/tokens-624c3fdcb3f64af0b65c0ced4d2bb988.md)

## Where to store metadata?

o Cookies on browsers o Local storage on browsers or other clients (cli. mobile apps„ etc)

### Cookies

* Bags of keys and values stored on browser, tagged with a few fields: o Domain o Path on domain o Expiration time
* Browsers send them automatically with every request
* Limited in size
* Security features(`HttpOnly` ,`Secure` ,`SameSite`)
* Can be set by server

cookies header returned from server example

```jsx
Set-Cookie: sessionId-a24a3fWa; Expires-in 42 days;
Set-Cookie: encodedJwt•a18760aid87qwea24a3fWa; Expires-in 42 days;
```

### Local Storage

* More flexible API
* Bigger, or sometimes not limited in size
* _Be careful securing the data_: o Device disk is usually shared o Browser local storage is accessible by page scripts
* Client must manually manage the data and send it with requests
