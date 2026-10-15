# Web Concepts summraized form Deepstrike Blog

blog link : “[https://deepstrike.io/blog](https://deepstrike.io/blog)”

# **CSR vs SSR vs SSG**

### Client Side Rendering (CSR)

Imagine you go to a restaurant, but instead of getting a ready meal, the restaurant gives you all the ingredients and the recipe to cook the food yourself at home. This is like CSR.

**CSR life cycle**

- User requests a site, The user enters a URL or clicks a link, and the browser sends an HTTP request to the server.
- Server sends an initial HTML file with JavaScript links, The server responds with a minimal HTML file. This file usually contains a root `<div>` (e.g., `<div id="root"></div>`) and links to the JavaScript bundles.
    
    <aside>
    💡
    
    - It acts as the **entry point** for a JavaScript-based single-page application (SPA).
    - The JavaScript code will **inject** the entire app inside this **`<div>`** by manipulating the DOM.
    </aside>
    
- Browser downloads the CSS and JS files ,As the browser parses the initial HTML, it starts downloading the linked CSS and JavaScript files.
- Browser executes the JavaScript code, Once the JavaScript is downloaded, the browser executes it. This script is responsible for rendering the app in the DOM. Frameworks like React, Angular, or Vue typically handle this process.
- JavaScript fetches data from the server (API calls):After the app initializes, the JavaScript may make API calls to fetch dynamic data (using `fetch`, `axios`, etc.).
- Browser parses the fetched data and constructs the DOM (Virtual DOM in some cases):The fetched data is used to update the UI. The JavaScript framework/library constructs and updates the DOM dynamically, often using techniques like the Virtual DOM in React.

**Conclusion** 

the requested site send initial html file which usually static one with some dynamic data inside , this minimal html contain js code that fetches data from server and it render and executed on browser. 

 

### **Server-Side Rendering (SSR)**

Now imagine going to the same restaurant, but this time, the food is ready, hot, and served on a plate. This is like SSR.

In SSR, the server prepares the whole page with all its content and sends it to the browser. So when you request the page, the server sends a full HTML file with everything inside.

**SSR life cycle**

- User requests a site, The user types a website link or clicks on one, and the browser sends a request to the server.
- Server creates the full HTML page, The server gets the request, collects any needed data (like fetching data from a database), and builds a full HTML page with all the content already in it.
- Server sends the complete HTML to the browser ,The server sends back the ready-made HTML page to the browser. The user can see the content right away.
- Browser downloads CSS and JavaScript files ,While showing the page, the browser also downloads any CSS and JavaScript files.

**Conclusion** 

in SSR the user request site and site prepare the full html file that user need and send it to user  .

<aside>
💡

there are fewer chances to find DOM-based bugs because JavaScript does less in the browser.

</aside>

### **Static Site Generation (SSG)**

Now imagine the restaurant cooks all the meals at once, packs them in boxes, and stores them. When someone orders food, they just give them a ready box. This is like SSG.

In SSG, the pages are built one time during the build process and saved as ready HTML files. Every request gives back the ready file immediately, it good for blogs .

# **Caching**

A cache is temporary storage that saves data to help websites load faster when you revisit them. The cache provides identical responses to users who make similar requests.

### Content Delivery Network (CDN)

A CDN (Content Delivery Network) stores copies of your website's content on servers around the world. When someone visits your site, the CDN delivers the content from the server closest to them. This makes the website load faster and reduces the load on the main server.

<aside>
💡

the cache gives the same response to users who make the same request. These saved (cached) responses are kept for a short time.

</aside>

### Types of Cashing

**Client-side (local):** This means the browser saves some data on the user’s computer, so the website can load faster the next time they visit.

**Server-side:** This means the server saves the responses to some requests for a short or long time. When many users ask for the same thing, the server can give them the saved response instead of processing the request again. This helps the website run faster because the server doesn’t get too busy.

### Cash Keys

Creating a unique **fingerprint** for each request. If the cache finds a matching fingerprint, it delivers the saved response. If there’s no match, it sends the request to the web server to get a new response.

Common cash keys 

```json
**Request** **URL**: The full address of the page or resource being requested (e.g., https://example.com/page).
**Host Header**: Specifies the domain name of the server (e.g., example.com).
**HTTP Method**: The type of request made, like GET, POST, PUT, or DELETE. Usually, only GET requests are cached.
**Query Parameters**: Any additional data in the URL after a question mark (e.g., ?id=123&sort=asc).
**Cookies**: Small pieces of data stored by the browser, which can affect the content shown.
```

# **DOM**

The DOM (Document Object Model) is a programming interface for web documents. It represents the structure of a web page as a tree of objects, where each object corresponds to a part of the page, such as elements, attributes, or text.

### CSR and Dom

in CSR architecture the JS manipulate the DOM dynamically after the page loads. This means the content you see on the page isn’t coming directly from the server-rendered HTML but is being created or updated in real-time through the DOM API , this means it allows scripts (like JavaScript) to dynamically access and update the content, structure, and style of a document.

### **The DOM Tree**

- **Document Node:** The root of the tree.
- **Element Nodes:** Represent HTML tags.
- **Text Nodes:** Represent the text inside elements.
- **Attribute Nodes:** Represent attributes of elements.

### Manipulate DOM

1. access elements by `getElementById`
2. change element content by `element.innerHTML`
3. adding and removing elements `appendChild` , `removeChild`

### **Dom Main Concepts**

**Sources :**are the places where browser(referrer header) or user input  (search parameter ) enters the application 

example:`const userInput = location.hash;` // Source: Getting data from the URL hash

**Sinks :** are the places in the code where the user input is inserted or executed

Common Sinks in the DOM:

- `innerHTML`
- `outerHTML`
- `document.write()`
- `eval()`
- `setAttribute()`
- `innerText` / `textContent`
- `window.location` (for redirects)

example : `document.getElementById('output').innerHTML = userInput;` // Sink: Inserting data into the DOM

**Conclusion** : if user input inserted in page content and it isn’t well sanitized this can lead to vulnerabilities such as (**DOM XSS**) 

# CSP

CSP is like a set of rules for your website that tells the browser which scripts are allowed to run and which are not. Even if an attacker manages to inject a script, CSP can block it from running. 

so to apply CSP we can add header to application 

`Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-source.com;` to make only scripts from (trusted-source.com) allowed 

# **Inspect Elements and Viewing Source Code**

**In CSR** :

- **Source Code :** show will see the initial html
- **DOM :** show the **fully built page** after JavaScript has finished running.

**In SSR** :

- **Source Code :** show **most or all** of the content because it’s rendered on the server.
- **DOM :** will look **very similar** to the source code, with only minor changes from JavaScript

<aside>
💡

- The **source code** doesn’t show:
    - Content loaded via **APIs** or **AJAX**.
    - Dynamic elements generated by **JavaScript**.
    - Any updates from **user interactions**.
</aside>

# **Cross-Site Requests**

we can simply define it that the request go from a origin to another origin 

### Origin

it's important to note that the HTTP **Origin** request header specifies the origin (including the scheme, hostname, and port) that **initiated** the request.

so it is defined with 3 components 

```csharp
//protocol // domain //port 
http://ex.example.com:80
```

### Same-Origin Policy (SOP)

This is a security measure that restricts how documents or scripts loaded from one origin can interact with resources from another origin. This policy helps prevent malicious websites from accessing sensitive data from other sites you might be logged into. 

### Cross-Origin Request

so for example when sharing youtube vide on facebook then facebook return the video name and thumbnail then it called COR when server or domain request a data from another one  

# **AJAX Request**

AJAX is a way for web pages to send and receive data from a server **without reloading the page**. It allows you to update parts of a webpage (like showing new content or data) dynamically, making the user experience smoother and faster.

For example Code 

```jsx
<!DOCTYPE html>
<html>
<head>
    <title>Simple AJAX Example</title>
</head>
<body>
    <h1>Click the Button to Load Data</h1>
    <button id="loadDataBtn">Load Data</button>

    <div id="output"></div> <!-- This is where the data will appear -->

    <script>
        document.getElementById('loadDataBtn').addEventListener('click', function() {
            // Create a new AJAX request
            const xhr = new XMLHttpRequest();

            // Set up the request: GET method, target URL
            xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts/1', true);

            // What happens when data is successfully loaded
            xhr.onload = function() {
                if (xhr.status === 200) {
                    const data = JSON.parse(xhr.responseText); // Convert JSON to object
                    document.getElementById('output').innerHTML = `
                        <h3>${data.title}</h3>
                        <p>${data.body}</p>
                    `;
                } else {
                    document.getElementById('output').innerHTML = 'Error loading data.';
                }
            };

            // Send the request
            xhr.send();
        });
    </script>
</body>
</html>
```

**What Happens Here?**

- When you click the **"Load Data"** button, an AJAX request is sent to a free API (`https://jsonplaceholder.typicode.com/posts/1`).
- The server responds with **JSON data** (a post's title and body).
- The webpage **displays the data** inside the `<div>` without reloading the page.

<aside>
💡

we didn’t get CORS error because of the called API allow external origins to make request with it  

</aside>

# **Preflight Request**

A **preflight request** is an additional HTTP request that a web browser sends to the server before making a cross-origin AJAX request. The purpose of a preflight request is to check with the server whether the actual request (the one that fetches the resource) is safe to send.

### **When Does a Preflight Request Happen?**

If the AJAX request uses methods other than `GET`, `POST`, or `HEAD`, includes custom headers, or uses non-standard content types, the browser automatically sends an HTTP **OPTIONS** request to the server before the actual request. This is the **preflight request**.

### Preflight Request Life Cycle

request sent included the following headers 

- **Origin:** Specifies the source of the request (e.g., `https://your-website.com`).
- **Access-Control-Request-Method:** Indicates the HTTP method intended for the actual request (e.g., `PUT`, `DELETE`).
- **Access-Control-Request-Headers:** Lists any custom headers that will be sent in the actual request (e.g., `Authorization`, `X-Custom-Header`).

the the server respond with the correct CORS headers to indicate whether the actual request is allowed

headers include 

- **Access-Control-Allow-Origin:** Specifies which origin(s) are allowed to access the resource.
- **Access-Control-Allow-Methods:** Lists the HTTP methods the server permits (e.g., `GET`, `POST`, `PUT`, `DELETE`).
- **Access-Control-Allow-Headers**

![image.png](Web%20Concepts%20summraized%20form%20Deepstrike%20Blog/image.png)

# **AJAX and XHR, Fetch Request**

<aside>
💡

AJAX is a technique to request data without loading page where it using **XHR** or **Fetch API** as a tool to achieve that . 

</aside>

### **XMLHttpRequest (XHR)**

Is a tool in JavaScript that lets you send HTTP or HTTPS requests to a server and receive data back without reloading the page. Even though its name mentions "XML," XHR can be used to fetch **any type of data**, not just XML. You can use it to get JSON, HTML, or plain text as well.

### **Fetch API**

it as same as XHR it’s all under AJAX technique 

example code 

```jsx
function loadUser() {
    fetch('user.json') //Get data from `user.json`
        .then(res => res.json())  // Convert response to JSON
        .then(data => { //Show the data on the webpage
            document.getElementById('user').innerHTML = `
                <h3>Name: ${data.name}</h3>
                <p>Email: ${data.email}</p>
                <p>Age: ${data.age}</p>
            `;
        })
        .catch(() => {
            document.getElementById('user').innerHTML = 'Error loading data';
        });
}
```

# **postMessage**

way of communication that is secure and safe. Basically, it lets different pages or windows send messages to each other even if they’re from different origins.

## Scenarios of using postMessage

### **1. A Page Talking to a Popup It Opened**

Imagine you're on a website, and you click **"Login with Google"**. A **popup window** opens where you log in. Once you're logged in, the popup needs to tell the original page that you're logged in. 

### **2. A Page Talking to an Iframe**

Let’s say you embed a **YouTube video** on your blog using an **iframe**. The YouTube player might need to send messages to your blog, like telling it when the video starts playing or when it’s paused.

### Scenario to exploit if if not used properly

Let's suppose we have an `index.html` file, which we’ll call the parent page.

```jsx
<iframe id="myFrame" src="child.html" style="width:300px; height:200px;"></iframe>

<script>
// Listen for messages from the iframe
window.addEventListener('message', function(event) {
    // No origin check, blindly trusting any incoming message
    document.body.innerHTML = event.data;  // Directly injecting message into the DOM (vulnerable)
});
</script>
```

attacker create malicious Iframe (`child.html`) we call it child page

```jsx
<button onclick="sendMaliciousMessage()">Send Malicious Script</button>

<script>
function sendMaliciousMessage() {
    // Sending malicious script to the parent page
    parent.postMessage('<script>alert("XSS Attack!")<\/script>', '*');
}
</script>
```