
#web 


- [ ] RESTful API ➕ 2025-06-14 


A RESTful API is an architectural style for designing networked applications. It stands for **REpresentational State Transfer** and provides a set of rules and constraints for creating web services that are simple, scalable, and reliable. Think of it as a translator that allows two different software applications to communicate with each other over the internet.

At its core, a RESTful API uses standard HTTP methods (the same ones your web browser uses) to interact with resources. A **resource** can be any piece of information, such as a user profile, a product, or a collection of data.

---

### How it Works: The Client-Server Model

REST operates on a **client-server** model. The client is the application that requests information, and the server is the application that provides it. The key is that the client and server are independent of each other. The client only needs to know the location (URL) of the resource; it doesn't need to know anything about how the server is implemented.

This separation of concerns allows for greater flexibility and scalability. The server can be updated or changed without affecting the client, as long as the way the data is presented remains consistent.


---

### Key Principles of REST

For an API to be considered RESTful, it must adhere to several guiding principles:

- **Stateless:** Every request from a client to a server must contain all the information needed to understand and process the request. The server does not store any information about the client's state between requests. This makes the system more reliable and easier to scale.
    
- **Uniform Interface:** This is a fundamental concept in REST and has four main aspects:
    
    - **Resource Identification:** Each resource is uniquely identified by a stable identifier, typically a URL (Uniform Resource Locator).
        
    - **Resource Manipulation through Representations:** When a client interacts with a resource, it does so through a representation of that resource. This representation is usually in a format like JSON (JavaScript Object Notation) or XML (eXtensible Markup Language).
        
    - **Self-Descriptive Messages:** Each message includes enough information to describe how to process it.
        
    - **Hypermedia as the Engine of Application State (HATEOAS):** The server's response should include links that guide the client on what actions it can take next.
        
- **Cacheable:** To improve performance, responses from the server can be marked as cacheable or non-cacheable. This allows the client to reuse a response for subsequent identical requests.
    
- **Layered System:** A client cannot ordinarily tell whether it is connected directly to the end server or to an intermediary along the way. This allows for the introduction of layers like load balancers and proxies to improve scalability and security.
    
- **Code on Demand (Optional):** This allows a server to temporarily extend or customize the functionality of a client by transferring logic that it can execute.
    

---

### Common HTTP Methods in RESTful APIs

RESTful APIs use standard HTTP methods to perform actions on resources:

- **GET:** Retrieves a representation of a resource.
    
- **POST:** Creates a new resource.
- **PUT:** Updates an existing resource.
    
- **DELETE:** Removes a resource.
    
- **PATCH:** Partially updates a resource.
    

For example, to get information about a user with the ID `123`, a client would send a `GET` request to a URL like `https://api.example.com/users/123`. To create a new user, it would send a `POST` request to `https://api.example.com/users` with the user's data in the request body.



-------


First, to clarify: a **RESTful API is not a protocol**. It's an **architectural style**.

Think of it this way: a protocol is like a strict set of rules for a conversation, dictating the exact language and grammar you must use (like TCP/IP or HTTP). An architectural style, on the other hand, is like a set of guidelines or a blueprint for how to structure a building. It doesn't tell you what color to paint the walls, but it suggests where the doors and windows should go for the building to be functional and efficient.

REST uses the HTTP protocol to work, but it imposes a set of principles (like statelessness and a uniform interface) on top of HTTP to create a system that is predictable, scalable, and easy to work with.

Now, let's look at how RESTful APIs compare to other major API styles you'll encounter.

### Comparative API Styles

Here’s a breakdown of the main alternatives to REST and how they differ:

---

### 1. SOAP (Simple Object Access Protocol)

This is the more traditional counterpart to REST.

|Feature|REST|SOAP|
|:--|:--|:--|
|**Type**|Architectural Style|Protocol|
|**Data Format**|Flexible (usually **JSON**, but can be XML, HTML, plain text)|Strictly **XML**|
|**Transport**|Primarily uses HTTP/HTTPS.|Can use any transport protocol (HTTP, SMTP, TCP, etc.).|
|**Structure**|Resource-oriented. You interact with resources via URLs (e.g., `GET /users/123`).|Function-oriented. You call specific procedures (e.g., `GetUser(UserID: "123")`).|
|**Flexibility**|Very flexible and less verbose.|Very rigid and structured with a strict contract (WSDL file).|
|**Performance**|Generally faster and requires less bandwidth due to lightweight JSON payloads.|Slower and more bandwidth-intensive due to the verbosity of XML.|
|**Security**|Relies on transport-level security like HTTPS (SSL/TLS).|Has its own comprehensive security standard (WS-Security) which is more robust but also more complex.|
|**Best For**|Public-facing APIs, mobile applications, and modern web services where performance and flexibility are key.|Enterprise environments, financial services, and legacy systems that require high security and a strict contract between client and server.|

Export to Sheets

**In a Nutshell:** REST is simpler, more flexible, and faster. SOAP is more standardized and has more built-in security features, but at the cost of complexity and performance.

---

### 2. GraphQL

Developed by Facebook, GraphQL is a more modern approach that addresses some of the limitations of REST.

|Feature|REST|GraphQL|
|:--|:--|:--|
|**Type**|Architectural Style|Query Language and a runtime for fulfilling those queries.|
|**Endpoints**|Multiple endpoints for different resources (e.g., `/users`, `/posts`).|Typically a **single endpoint** that handles all queries.|
|**Data Fetching**|**Over-fetching and Under-fetching**. You get a fixed data structure from an endpoint. If you need more data, you have to make another request. If you need less, you get more than you need.|**Precise Data Fetching**. The client specifies exactly what data it needs in a single query, and that's exactly what it gets back. No more, no less.|
|**Data Format**|Usually JSON.|Always returns **JSON**.|
|**Performance**|Can be less efficient due to multiple round trips to the server.|Highly efficient as it can fetch all required data in a single request.|
|**Flexibility**|Less flexible for the client, as the server defines the response structure.|Very flexible for the client, allowing them to shape the response data.|
|**Best For**|Applications with complex data requirements, mobile apps where bandwidth is a concern, and microservice architectures.|Simpler APIs where the data needs are well-defined and unlikely to change frequently.|

Export to Sheets

**In a Nutshell:** GraphQL gives more power to the client, allowing for very efficient data fetching, especially in complex applications. REST is often simpler to implement for basic use cases.

---

### 3. gRPC (Google Remote Procedure Call)

Developed by Google, gRPC is a high-performance framework that takes a different approach from the text-based nature of REST and GraphQL.

|Feature|REST|gRPC|
|:--|:--|:--|
|**Type**|Architectural Style|A framework based on the Remote Procedure Call (RPC) paradigm.|
|**Protocol**|Typically uses HTTP/1.1.|Uses **HTTP/2**, which allows for more advanced features like bidirectional streaming and multiplexing.|
|**Data Format**|Text-based (usually JSON).|Binary format using **Protocol Buffers (Protobuf)**. This is much smaller and faster to serialize/deserialize than JSON.|
|**Communication**|Classic request-response model.|Supports unary (request-response), server streaming, client streaming, and **bidirectional streaming**.|
|**API Contract**|Defined by documentation (like OpenAPI/Swagger).|Defined by a strict `.proto` file that specifies the available services and message formats.|
|**Performance**|Good, but slower than gRPC due to text-based data and HTTP/1.1.|**Extremely high performance** and low latency due to HTTP/2 and binary data format.|
|**Browser Support**|Natively supported by all browsers.|Limited native browser support, often requiring a proxy layer (like gRPC-Web).|
|**Best For**|Public APIs and web applications where human readability and broad compatibility are important.|Internal microservice-to-microservice communication, real-time applications, and situations where performance is the absolute top priority.|

Export to Sheets

**In a Nutshell:** gRPC is all about maximum performance and is ideal for communication between internal services. REST is more suitable for public-facing APIs due to its use of standard, human-readable formats and its universal browser support.




---


Here are conceptual code examples for SOAP, GraphQL, and gRPC to illustrate how they work in practice. These examples will show both the client-side request and what the corresponding server-side data or definition looks like.

For context, let's imagine we want to fetch information about a specific user with an ID of `"123"`.

---

### 1. SOAP (Simple Object Access Protocol) Example

SOAP relies on a verbose XML structure and a rigid contract. The client must formulate a request that precisely matches the function call defined on the server.

#### Client Request (HTTP POST body)

The client sends a `POST` request to a single endpoint (e.g., `/userservice`). The body of the request contains the SOAP "envelope," which wraps the specific function call.

XML

```
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
               xmlns:usr="http://www.example.com/userservice">
   <soap:Header/>
   <soap:Body>
      <usr:GetUserDetails>
         <usr:UserID>123</usr:UserID>
      </usr:GetUserDetails>
   </soap:Body>
</soap:Envelope>
```

- **`Envelope`**: The mandatory root element that wraps the entire message.
- **`Body`**: Contains the actual RPC (Remote Procedure Call) information.
- **`GetUserDetails`**: The specific function we are calling on the server.
- **`UserID`**: The parameter being passed to the function.

#### Server Response (HTTP response body)

The server responds with another XML document, following the same SOAP envelope structure.

XML

```
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
               xmlns:usr="http://www.example.com/userservice">
   <soap:Header/>
   <soap:Body>
      <usr:GetUserDetailsResponse>
         <usr:User>
            <usr:UserID>123</usr:UserID>
            <usr:Name>Kim Min-jun</usr:Name>
            <usr:Email>minjun.kim@example.com</usr:Email>
            <usr:MembershipStatus>Gold</usr:MembershipStatus>
         </usr:User>
      </usr:GetUserDetailsResponse>
   </soap:Body>
</soap:Envelope>
```

**Key Takeaway:** Notice how rigid and verbose this is. You call a specific function, and the structure is strictly defined by XML schemas.

---

### 2. GraphQL Example

GraphQL allows the client to ask for exactly what it needs. The client sends a "query" that mirrors the shape of the desired JSON response.

#### Client Request (HTTP POST body)

The client sends a `POST` request to a single endpoint (e.g., `/graphql`). The body contains the GraphQL query. Here, we're asking for only the `name` and `email` of the user.

GraphQL

```
query {
  user(id: "123") {
    name
    email
  }
}
```

- **`query`**: The type of operation.
- **`user(id: "123")`**: The query "field" we are targeting, passing an ID as an argument.
- **`{ name, email }`**: The specific data fields we want in the response. We are intentionally leaving out `membershipStatus`.

#### Server Response (JSON in the HTTP response body)

The server responds with a JSON object that exactly matches the structure of the client's query.

JSON

```
{
  "data": {
    "user": {
      "name": "Kim Min-jun",
      "email": "minjun.kim@example.com"
    }
  }
}
```

**Key Takeaway:** It's highly efficient. The client gets exactly what it asked for—nothing more, nothing less—in a single request. This is perfect for mobile apps where minimizing data usage is crucial.

---

### 3. gRPC (Google Remote Procedure Call) Example

gRPC is not human-readable like the others because it uses a compiled binary format (Protocol Buffers). The process starts with a service definition in a `.proto` file.

#### 1. Service Definition (`.proto` file)

First, you define the "service" and the "message" structures. This file acts as the strict contract for the API.

Protocol Buffers

```
// The syntax is for "proto3"
syntax = "proto3";

// Define the service
service UserService {
  // Define the remote procedure call (RPC)
  rpc GetUserDetails (UserRequest) returns (UserResponse) {}
}

// Define the structure for the request message
message UserRequest {
  string user_id = 1;
}

// Define the structure for the response message
message UserResponse {
  string user_id = 1;
  string name = 2;
  string email = 3;
  string membership_status = 4;
}
```

#### 2. Client Code (Conceptual example in Python)

From the `.proto` file, gRPC tools automatically generate client and server code. The client code looks like a local function call, making it very intuitive for developers.

Python

```
import grpc
import user_service_pb2 # Code generated from the .proto file
import user_service_pb2_grpc # Code generated from the .proto file

# Establish a connection (channel) to the gRPC server
with grpc.insecure_channel('localhost:50051') as channel:
    # Create a client stub (like a remote object)
    stub = user_service_pb2_grpc.UserServiceStub(channel)
    
    # Create a request message
    request = user_service_pb2.UserRequest(user_id='123')
    
    # Make the remote procedure call, it feels just like calling a local method
    response = stub.GetUserDetails(request)
    
    # Use the response data
    print("User ID:", response.user_id)
    print("Name:", response.name)
    print("Email:", response.email)
```

- **The actual transmission over the network is a highly compressed, binary format.** It is not human-readable.

#### Server Response (Handled by the framework)

The gRPC server receives the binary request, decodes it using the `.proto` definition, executes its logic, and sends back a `UserResponse` message, also in the efficient Protocol Buffers binary format. The client library then decodes this binary data back into the Python `response` object shown above.

**Key Takeaway:** gRPC is about raw performance and a strict, code-first contract. The developer experience is simplified to making what looks like a local function call, while the framework handles the complex, high-speed serialization and transport over HTTP/2.