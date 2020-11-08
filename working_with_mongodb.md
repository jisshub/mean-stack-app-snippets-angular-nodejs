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

## storing posted data to db

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

---
