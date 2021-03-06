# koa-router-resources

`koa-router-resources` is a package to set your resources' routes in koa framework.

# Install

```bash
npm install koa-router-resources
```

# Usage

```js
const app = require('koa')();
var router = require('koa-router-resources');

router.configControllersPath('./app/controllers'); // controllers path
// will load app/controllers/users.js
router.resources('users', {}, function() {
  // will load app/controllers/admin/posts.js
  this.resources('admin/posts', { as: 'articles', only: ['index', 'new'] }, function() {
    this.collections([
      { method: 'get', path: 'test1' }
    ]);
    this.members([
      { method: 'get', path: 'test2' }
    ]);
    // will load comments.js
    this.resources('comments', { as: 'coms', except: ['edit', 'update']});
  });
});

router.apiResources('roles');

// In the version v0.1.0, It supports the method named 'namespace'.

router.namespace('admin', function() {
  this.apiResources('users');
});

// if you want to get `koa-router`, try `router._router`
router._router.get('/others', function(ctx) {
  ctx.body = 'others';
});

app.use(router.routes());
```

This code will generate routes below:

```bash
GET  /users
GET  /users/new
POST /users
GET  /users/:id
GET  /users/:id/edit
PUT  /users/:id
DEL  /users/:id

GET /users/:user_id/articles
GET /users/:user_id/articles/new
GET /users/:user_id/articles/test1
GET /users/:user_id/articles/:id/test2

GET  /users/:user_id/articles/:article_id/coms
GET  /users/:user_id/articles/:article_id/coms/new
POST /users/:user_id/articles/:article_id/coms
GET  /users/:user_id/articles/:article_id/coms/:id
DEL  /users/:user_id/articles/:article_id/coms/:id


GET  /roles
POST /roles
GET  /roles/:id
PUT  /roles/:id
DEL  /roles/:id

GET /others

### With version v0.1.0, using namespace
GET  /admin/users
POST /admin/users
GET  /admin/users/:id
PUT  /admin/users/:id
DEL  /admin/users/:id
```

You can also visit the examples in `test/resources.js`.
