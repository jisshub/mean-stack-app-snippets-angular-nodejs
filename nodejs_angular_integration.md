## Introduction

- nodejs - a javascript runtime that runs on the server.

![image](./screenshots/screen-7.jpg 'image')

## creating server with nodejs

- till now v start the server using _ng serve_
- but it only gave us a dev server. development server means it only aimed at angular development. not a production ready server. it doesn't contain the logic that v need to add to the server side.

### two ways of connecting angular and node backend

1. we have a node express app that serves angular spa as part of it. this app have url end points to which one can send the request where it will return the angular spa.

2. Two separated servers,

   1. node express server for business logic, authentication, data storage.

   2. a separate static host that returns the angular spa. static host can be any server that doesn't runs any server side logic, but returns html, js and css files. eg: apache, ngnix.

   > In both cases, we have separated apps. so, angular handles UI, sends background request. and node express handles this background request.

![image](./screenshots/screen-8.jpg 'image')

---

## What is a restful api?

REST stands for Representational State Transfer.

### In traditional apps,

![image](./screenshots/screen-9.jpg 'image')

- client sends a request, server gives a response in form of http request.

### In Angular SPA,

![image](./screenshots/screen-10.jpg 'image')

> Here in our case _Client Browser with SPA_. here not renders a second html page. because everything is rendered by js in that single page v have.

- **restful api** exposes couple of url end points also called paths to which client sends request

- this paths are _/users, /products, /customer_

- client send request to _domain/users_ or _domain/posts_, _domain/products_

- depending on which path request is sent, gets different response.

- handling request is done by http methods,

1.  GET
2.  POST
3.  UPDATE
4.  PATCH

_Example_: send a **POST** request to _domain/users_. will get a response which can be used in client side app.

- this how restful api works.

- here client and server communication is done with _JSON_ data, not html, xml...

![image](./screenshots/screen-11.jpg 'image')

---

## Adding the Node Backend

- here v use _ng serve_ to serve the angular app.

- and built a separate node backend.

- and backend code in a separate folder. that folder made in root folder.

- create a js file for server in root folder.

- we cab then run this server.js file using node.

```bash
node server.js
```

- import inbuilt http package from node and store its content to a constant.

- later create server.

- listen to port.

**server.js**

```javascript
const http = require('http');
// create server and store it
const server = http.createServer((req, res) => {
  // end the response
  res.end('this is my first response');
});
// make server listen to a port number
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`App runs in a PORT ${PORT}`);
});
```

---

## adding the express framework

- a nodejs framework.

```bash
npm i express
```

- add express app and all files belonging to it to _backend_ folder.

**app.js**

```javascript
const express = require('express');

// initialize express app
const app = express();

// use express router - create a middleware
app.use((req, res, next) => {
  console.log('First Middleware');
  next(); // movie to next middleware
});

// second middleware
app.use((req, res, next) => {
  res.send('hello from express app');
});

// export the app
module.exports = app;
```

**server.js**

```javascript
const http = require('http');
// import the express app exported
const app = require('./backend/app');

// make server listen to a port number
const PORT = process.env.PORT || 3000;
// set port for the app
app.set('port', PORT);
// create server and store it
const server = http.createServer(app);
// listen to a port
server.listen(PORT);
```

```bash
node server.js
```

---

## fetching Initial Posts

- fetch posts when client sends a request.
- v use rest api _/api/posts_.

**app.js**

```javascript
// set api
app.use('/api/posts', (req, res, next) => {
  const posts = [
    {
      id: 'post10101',
      title: 'This is first title',
      content: 'This is second content',
    },
    {
      id: 'post10111',
      title: 'This is second title',
      content: 'This is third content',
    },
  ];
  // send the response as json - set status as 200 means success
  res.status(200).json({ message: 'post fetched successfully', posts });
});
```

> naivgate to browser, type

localhost:3000/api/posts/

---

## using angular http client
- used to connect angular app to express server.
- Send request to fetch the posts. store the posts in post variable.
- In _getPosts()_ method, v send an http request.
- first have to import _HttpClientModule_ from
  http module. later add the same to imports array.

**app.module.ts**

```typescript
import { HttpClientModule } from '@angular/common/http';
```

- inject _HttpClient_ module to _post service_.
- send the **GET** request.

- angular http client uses _observables_.
- but this observable didn't send request unless u listen to it.
- to listen to it, v _subscribe_

**post-service.ts**

```typescript

import { HttpClient } from '@angular/common/http';

constructor(public http: HttpClient) { }

 getPosts(){
    // return copy of post array, use spread operator, so changes only affected on its copy,
    // not the original array
    this.http.get<{message: string, post: Post[]}>('http://localhost:3000/api/posts')
      .subscribe((postData) => {
        this.posts = postData.post;
        this.postUpdated.next([...this.posts]);
      })
  }
```

**post-create.component.ts**

```typescript
onAddPost(form: NgForm) {
    if (form.invalid) {
      return;
    }
    this.postService.addPost(form.value.title, form.value.content);
    form.resetForm();
  }
```

---

## Understanding CORS

### Cross-Origin-Resource-Sharing

![image](./screenshots/screen-12.jpg 'image')

- client & server communicates when they r in same host.

- communication not possible if they r in different host. for eg, client in _localhost:3000_ and server in _localhost:4000_.
  this causes CORS error.

**app.js**

```typescript
// set header
app.use((req, res, next) => {
  // allow which domain to access the resources.
  res.setHeader('Access-Control-Allow-Origin', '*');
  // only allow domains sending request with certain set of header.
  res.setHeader(
    'Access-Control-Allow-Header',
    'Origin, X-Requested-With, Content-Type, Accept'
  );
  // only allow domains sending request with a set of http methods.
  res.setHeader(
    'Access-Control-Allow-Methods',
    'GET, POST, PUT, UPDATE, PATCH, OPTIONS'
  );
  next();
});
```

---

## Adding the post request, parsing json body

**app.js**

```typescript
// Used to parse incoming JSON object
app.use(express.json());

// parse url encoded bodies
app.use(express.urlencoded());

// post request
app.post('/api/posts', (req, res, next) => {
  const post = req.body;
  console.log(post);
  return res.status(201).json({
    message: 'Data Posted',
    post,
  });
});
```

---

## Adding the Angular

```typescript
/**
   *
   * @param title : post title
   * @param content : post content
   */
  addPost(title: string, content: string){
    const post: Post = {id: null, title, content};
    this.http.post<{message: string}>('http://localhost:3000/api/posts', post)
        .subscribe((resData) => {
          console.log(resData.message);
          this.posts.push(post);
          // call next() on subject - pass copy of posts array as argument.
          this.postUpdated.next([...this.posts]);
        });
  }
```

---
