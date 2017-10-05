## koa-mongo

koa-mongo is a mongodb middleware for koa, support connection pool.

### Install

    npm i koa-mongo --save

### Usage

```
app.use(mongo({
  host: 'localhost',
  port: 27017,
  user: 'admin',
  pass: '123456',
  db: 'test',
  max: 100,
  min: 1,
  timeout: 30000,
  log: false,
  reconnectTries: 30,
  reconnectInterval: 1000
}));
```

or

```
app.use(mongo({
  uri: 'mongodb://admin:123456@localhost:27017/test', //or url
  max: 100,
  min: 1,
  timeout: 30000,
  log: false
}));
```

### Example

```
'use strict';

var koa = require('koa');
var mongo = require('koa-mongo');

var app = koa();

app.use(mongo());
app.use(function* (next) {
  var result = yield this.mongo.db('test').collection('users').insert({ name: Date.now() });
  var userId = result.ops[0]._id.toString();
  this.body = yield this.mongo.db('test').collection('users').find().toArray();
  this.mongo.db('test').collection('users').remove({
    _id: mongo.ObjectId(userId)
  });
});
app.listen(3000, function() {
  console.log('listening on 3000.');
});
```

### License

MIT
