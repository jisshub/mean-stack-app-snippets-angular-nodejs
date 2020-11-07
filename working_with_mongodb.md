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
