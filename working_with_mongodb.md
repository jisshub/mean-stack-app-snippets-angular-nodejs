# adding the mongodb to store posts.

## what is mongodb

> a no sql database that stores _documents_ in _collections_.
> in sql db, _records_ are stored in _tables_.

- enforces no data schema or relations. so individual documents can be arranged based on our need.

- can easily connect with node/express, bt not directly to angular.

## sql and nosql

- NoSQL good for application where structure of data varies.

![image](./screenshots/screen-13.jpg 'image')

## connecting angular to databse

- Y not directly connect mongodb to the angular?
  > Highly insecue to connect drieclty with angular. bcause secure authentication is not possible. we have to authenticate to the db.
  > this authentication is done via credentials which is stored in angular code.
  > that code is compiled to js. and it is loaded in to the browser. thus every user can view those informations. therefore directly connceting mongodb to angular is not a good idea.
  > so here v connect db to angular via node.

## using mongodb atlas as database

- install mongodb, mongodb compass on machine
- connect to the cluster from mongodb compass.
- connection string,

```bash
mongodb+srv://jissmonJose:YsX6ghrRid3aK9Fx@cluster101.wqs3s.mongodb.net/test
```

- copy thie string to compass and click connect.

## installing mongoose

- elegant mongodb object modeling for node.js

```bash
npm i mongoose
```

## installing nodemon

```bash
npm i nodemon
```

- add following in **package.json**

```json
  "start:server": "nodemon server.js"
```

https://mongoosejs.com/

## understanding mongoose schemas and models

1. create _models_ subfolder in backend dir
2. add _post.js_ file in that folder. this holds post models created using _mongoose_.

**backend/models/post.js**

```javascript
const mongoose = require('mongoose');
const postSchema = mongoose.Schema({
  title: {
    type: String,
    required: [true, 'title is required'],
  },
  content: {
    type: String,
    required: [true, 'add content'],
  },
});
// export the model
module.exports = mongoose.model('post', postSchema);

// here post is the model, postSchema is just a blueprint of that model.
// can use this model outide this file.
```

## creating instance of Post model

- using post model to store data to db. bcause post model is a bridge from our nodejs express app to the mongodb database.

**app.js**

```javascript
const Post = require('./models/post');
// create an instance of this Post model - pass data
app.post('/api/posts', (req, res, next) => {
  const post = new Post({
    title: req.body.title,
    content: req.body.content,
  });
  console.log(post);
  return res.status(201).json({
    message: 'Data Posted',
    post,
  });
});
```

- next run,

```bash
npm start
npm run start:server
```

---

## connecting node express app to mongodb

**app.js**

```typescript
// require mongoose
const mongoose = require('mongoose');

// connect to mongodb database
mongoose
  .connect(
    'mongodb+srv://jissmonJose:YsX6ghrRid3aK9Fx@cluster101.wqs3s.mongodb.net/<dbname>?retryWrites=true&w=majority'
  )
  .then(() => {
    console.log('Connected to Database!');
  })
  .catch(() => {
    console.log('Connection Failed!');
  });
```

- now thw connwction is ok, next lets save the data to the db.

---

## storing data to db.

- mongoose automartically creates a query to insert the data in to the db. thats what save() method does.

**app.js**

```javascript
app.post('/api/posts', (req, res, next) => {
  const post = new Post({
    title: req.body.title,
    content: req.body.content,
  });
  post.save();
  console.log(post);
  return res.status(201).json({
    message: 'Data Posted',
    post,
  });
});
```

- next overwrite the default db name in connection string.

```javascript
mongoose.connect(
  'mongodb+srv://jissmonJose:YsX6ghrRid3aK9Fx@cluster101.wqs3s.mongodb.net/node-angular?retryWrites=true&w=majority'
);
```

- here _node-angular_ is the db where data gets stored.
- will automatically creates a collection in node-angular database and stores the first document.

---

## fetching data from db

- using _find()_ method to fetch the data from mongodb.

**app.js**

```typescript
// fetch the post using get request
app.get('/api/posts', (req, res, next) => {
  Post.find().then((document) => {
    // send the response as json - set status as 200 means success
    res.status(200).json({ message: 'post fetched successfully' }, document);
  });
});
```

> just refresh the page, page loads data coming from database.

---

## transforming response dATA

- changing _\_id_ to _id_ of eah document.

- use _map operator_ for this.

- V call _map_ with in _pipe method_

- Import _map_ from _rxjs/operators_ package.

- _map_ allows to transform the elements in an array and
  store them to new array.

- map takes an arrow function, which takes post data
  as argument.

- In _map_, we returns post array after converting every elements based on our intended format. for that v use javascript method _map()_. In _map()_ v transform each element of the array.

**post.service,ts**

```typescript
 getPosts(){
    // return copy of post array, use spread operator, so changes only affected on its copy,
    // not the original array
    this.http.get<{message: string, posts: any}>('http://localhost:3000/api/posts')
      .pipe(map((postData) => {
        return postData.posts.map(post => {
          return {
             title: post.title,
             content: post.content,
             id: post._id
          };
        });
      }))
      .subscribe((postData) => {
        this.posts = postData.posts;
        console.log(this.posts);
        this.postUpdated.next([...this.posts]);
      });
  }
```

## deleting documents

- delete document using document id.

- add a function to delete in click listener. pass post id as argument.

- In _app.js_, set a delete route which takes document id as param. using id v delete that document.

**post-list.component.html**

```html
<button mat-button color="accent" (click)="onDelete(post.id)">DELETE</button>
```

## updating the frontend after deleting post

