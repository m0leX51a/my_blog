# AJAX

**AJAX Request Example:**

```bash
// Sample AJAX request using jQuery
$.ajax({
  url: "/api/login",
  type: "POST",
  dataType: "json",
  data: {
    username: "exampleUser",
    password: "securePassword"
  },
  success: function(response) {
    console.log("Authentication successful:", response);
  },
  error: function(xhr, status, error) {
    console.error("Authentication failed:", error);
  }
});
```

the request has sent 

```bash
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.1234.567 Safari/537.36
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest ***(This header is often used to indicate that the request is an AJAX request.)***
Request Body:

username=exampleUser&password=securePassword
```

In this example, the presence of the **`X-Requested-With: XMLHttpRequest`** header is a strong indicator that the request is an AJAX request. Many JavaScript frameworks and libraries automatically include this header when making asynchronous requests, helping the server distinguish between regular and AJAX requests.