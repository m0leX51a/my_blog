# API Concepts

# **What is an API**

An **API (Application Programming Interface)** is a set of rules and protocols that allows different software applications to communicate with each other. It defines how requests and responses should be structured so that systems can exchange data efficiently.

### **Key Concepts:**

- **Interface**: A bridge between two systems.
- **Protocol**: Rules for communication (e.g., HTTP, REST, GraphQL).

# **Types of APIs**

## **By Availability**

| **Type** | **Description** | **Example** |
| --- | --- | --- |
| **Public (Open) API** | Free to use, often requires an API key | Twitter API, Google Maps API |
| **Private (Internal) API** | Used within a company | Internal employee management system |
| **Partner API** | Shared with specific business partners | Payment gateway APIs (Stripe, PayPal) |

## **By Architecture**

| **Type** | **Description** | **Example** |
| --- | --- | --- |
| **REST API** | Uses HTTP methods (GET, POST, PUT, DELETE) | Twitter API, GitHub API |
| **SOAP API** | XML-based, strict standards | Banking systems, legacy enterprise APIs |
| **GraphQL API** | Allows clients to request only needed data | Facebook API, Shopify API |
| **gRPC API** | High-performance, uses Protocol Buffers | Microservices, IoT devices |

# **How APIs Work (Request & Response)**

### **API Request Components**

1. **Endpoint (URL)** – Where the API is located (e.g., **`https://api.example.com/users`**).
2. **HTTP Method** – Defines the action:
    - **`GET`** → Retrieve data
    - **`POST`** → Send data
    - **`PUT`** → Update data
    - **`DELETE`** → Remove data
3. **Headers** – Extra info (e.g., **`Authorization: Bearer token`**, **`Content-Type: application/json`**).
4. **Body (Payload)** – Data sent to the server (for **`POST`**, **`PUT`**).
5. **Query Parameters** – Filters in the URL (e.g., **`?user_id=123`**).

### **API Response Components**

- **Status Code** (e.g., **`200 OK`**, **`404 Not Found`**, **`500 Server Error`**).
- **Response Body** (usually JSON or XML).
- **Headers** (metadata like rate limits).

# **Authentication Methods in APIs**

| **Method** | **Description** | **Example** |
| --- | --- | --- |
| **API Keys** | Simple token passed in headers/query | **`?api_key=abc123`** |
| **OAuth 2.0** | Token-based (used by Google, Facebook) | **`Authorization: Bearer token`** |
| **JWT (JSON Web Token)** | Encrypted token with user data | Used in login systems |
| **Basic Auth** | Username & password in headers | **`Authorization: Basic base64(user:pass)`** |