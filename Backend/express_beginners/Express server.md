
# What is Express.js?

Express.js (or simply Express) is the most popular Node.js web application framework, designed for building web applications and APIs.

# Getting Started with Express

```js
const express = require('express'); //requiring the module
const app = express();//creating a server instance
const port = 8080;  
  
// Define a route for GET requests to the root URL  
app.get('/', (req, res) => {  
  res.send('Hello World from Express!');  
});  
//req is used to request data
//res is used to send data/send a response
  
// Start the server  
app.listen(port, () => {  
  console.log(`Example app listening at http://localhost:${port}`);  
});
```

---
# API -(Application Programming Interface)

A set of rules and protocols that allows two different software programs to communicate and exchange data with each other. It acts as a messenger or intermediary, handling requests and responses between a client application and a server, while hiding the complexity of the underlying systems.

the 2 applications can be the frontend and the backend, so these api are used here to create a communication way between them...
![[REST-API.pdf]]
## Types by Architectural Style/Protocol

These categories define the rules, structure, and constraints for how the API operates and exchanges data.

- **REST (Representational State Transfer) APIs**: The most widely used architectural style for web services, REST APIs are lightweight, flexible, and use standard HTTP methods (GET, POST, PUT, DELETE) to interact with resources (data) typically in JSON or XML format. They are stateless and easy to scale.
- **SOAP (Simple Object Access Protocol) APIs**: A highly structured and protocol-rigid standard that uses XML for message formatting and often relies on WSDL (Web Services Description Language) files to define its operations. Due to its built-in standards for security and error handling, it is often used in enterprise-level applications like banking and healthcare.
- **GraphQL APIs**: A modern API query language that allows clients to request exactly the data they need in a single request to a single endpoint. This helps prevent over- or under-fetching of data, which can occur with REST APIs.
- **RPC (Remote Procedure Call) APIs**: One of the oldest API types, RPC APIs allow a program to execute a function or procedure on a remote server as if it were a local call. Modern implementations include gRPC (Google RPC), which uses HTTP/2 and Protocol Buffers for high performance.
- **Event-Driven APIs**: Unlike the typical request-response model, these APIs use a publish-and-subscribe model where a response is triggered by an event, enabling real-time data exchange. This is ideal for applications requiring instantaneous updates, such as stock trackers or IoT devices.
---

# File Structure followed

```
src/app.js //here u intialize(create) the server
/server.js //used to start the server
```

**`app.js`**
```js
const express = require('express');

cosnt app = express();  

module.exports = app
```

**`server.js`**
```js
///start the server

const app = require('./src/app');
const port = 3000;
app.listen(port, ()=>{
	console.log(`Example app listening at http://localhost:${port}`)
});
```



---
# Basic Routing

```js
app.get()//to fetch data from the server
app.post()//create new data
app.put()//update/replace the data
app.delete()//Delete exisiting data
app.all()//run all methods(usually admin privileges)
```

- `app.get()` - Handle GET requests
- `app.post()` - Handle POST requests
- `app.put()` - Handle PUT requests
- `app.delete()` - Handle DELETE requests
- `app.all()` - Handle all HTTP methods

## Route Parameters

Route parameters are named URL segments that capture values at specific positions in the URL.
They are specified in the path with a colon `:` prefix.
the `:` simply tells the url path that the content after the colon is dynamic and needs to be captured by the variable around it.

**Example:** `/users/:userId/notes/:noteId`

**`app.js`**
```js
// Route with parameters  
app.get('/users/:userId/books/:bookId', (req, res) => {  
  // Access parameters using req.params  
  const userId = req.params.userId;
  res.send(`User ID: ${userId}, Book ID: ${req.params.bookId}`);  
});
```

## Post method

When a client sends a POST request, the data is sent as a **raw stream of bytes** (JSON/text).

Example request:

```json
{  
  "title": "Study",  
  "content": "Revise JS"  
}
```

Express (by default) does **NOT parse this**.

So inside your route:

```js
console.log(req.body); // ❌ undefined (without middleware)
```

### Why Middleware is Required

Middleware like:

```js
app.use(express.json());
```

acts as a **parser** that:

1. Reads incoming raw data
    
2. Converts JSON → JavaScript object
    
3. Attaches it to `req.body`

```js
app.post('/notes', (req, res)=>{
    notes.push(req.body)//now to parse 
    res.status(201).json({
        message: "note created sucessfully",
    })
})
```

## Get Method

```js
app.get('/notes', (req, res)=>{
    res.status(200).json({
        message: "Notes displayed sucessfully",
        notes: notes
    })
})
```

## Delete Method

```js
app.delete('/notes/:index', (req,res)=>{
    const index = req.params.index;
    delete notes[index];
    
    res.status(200).json({
        message: "Note deleted sucessfully",
        notes: notes
    })
})
```


## Patch Method

#### The patch method is used to update the data in the server
for the below example the data was present in the req.body since only the logic for description is saved...

Now here we cant update the title since we haven't programmed it to be updated

```js
app.patch('/notes/:index', (req,res)=>{

    const index = req.params.index;
    const description = req.body.description;
    notes[index].description = description;
    
    res.status(200).json({
        message:`Notes present at ${index} is updated sucessfully`,
        notes: notes
    })
})
```



---





---

# Built-in Middleware

Middleware functions are the backbone of Express applications.
**Middleware** is a function that runs **between the incoming request and the final response**.

```js
function middleware(req, res, next) {  
// do something  
next(); // pass control forward  
}
```

Middleware exists to **modularize and reuse common logic** in the request-response cycle.

Instead of repeating code in every route, you centralize it.

They have access to:

- The request object (`req`)
- The response object (`res`)
- The next middleware function in the stack (`next`)

Middleware can:

- Execute any code
- Modify request and response objects
- End the request-response cycle
- Call the next middleware in the stack

### Analogy

Think of a request like a **sealed package** 📦

- Without middleware → you just receive the package
    
- With middleware → someone **opens it and organizes the contents for you**

