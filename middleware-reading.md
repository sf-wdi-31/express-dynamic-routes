# Middleware


###What is middleware?

In terms of Express, [middleware](http://expressjs.com/guide/using-middleware.html) is a function with access to the request object (`request`), the response object (`response`), and the next middleware in the application’s request-response cycle, commonly denoted by a variable named `next`.

Middleware can:

* Execute any code.
* Make changes to the request and the response objects.
* End the request-response cycle.
* Call the next middleware in the stack.

You can create your own middleware, or use third-party modules. There are tons of useful middleware modules that are already out there which you can use to handle common challenges like authentication, validation, and parsing.

The [`body-parser`](https://github.com/expressjs/body-parser) module is an example of some middleware that makes Express awesome. You can use it to parse out data from an HTTP request's body. This provides a different way to collect data instead of relying on URL or query parameters.


##### Including Middleware on One Route

`server.js`
```js
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

// bodyParser.urlencoded() returns a function that will be called later in the app.post() route
var parseUrlencoded = bodyParser.urlencoded({extended: false});

// store for cities in memory (as long as server is running)
var cities = [];

app.get('/cities', function(req, res) {
  res.json({cities: cities});
})

// passing multiple middleware functions to this route; they are executed sequentially
app.post('/cities', parseUrlencoded, function (request, response) {
  //                ^- middleware -^
  var city;
  var name = request.body.name;
  var description = request.body.description;
  city = { name: name, description: description}
  cities.push(city);

  // sending json
  response.json({ cities: cities});
});
```

##### Including Middleware on All Routes

Another way to include middleware is via `app.use`.  This will include it on *all* routes.

```js
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

// store for cities in memory (as long as server is running)
var cities = [];

// body parser config for all routes
app.use(bodyParser.urlencoded({ extended: false }));
//    ^              middleware            ^

app.get('/cities', function(req, res) {
  res.json({cities: cities});
})

app.post('/cities', function (request, response) {
  var city;
  var name = request.body.name;
  var description = request.body.description;
  city = { name: name, description: description }
  cities.push(city);

  // sending json
  response.json({ cities: cities });
});

```

#### Check for Understanding

1. What will the client see when it `GET`s `/cities`?

  <details><summary>click for answer</summary>
  ```
  {
    cities: []  
  }
  ```
  </details>

2. Write code for an HTML form that would `POST` to the `/cities` route.

  <details><summary>click for answer</summary>


  ```html
  <html>
  <body>
    <form method="POST" action="http://localhost:3000/cities">
      <label for"cityName">city</label>
      <input id="cityName" name="name" type="text" />
      <label for"cityDesc">description</label>
      <input id="cityDesc" name="description" type="text" />
      <input type="submit" />
    </form>
  </body>
  </html>
  ```
  </details>

### Custom Middleware

We can write our own middleware. Let's say we want to make some alteration to the request parameters so that further down the chain those alterations can be used.  


```js
// express will call this function on every route with the param of 'name'
app.param('name', function(request, response, next) {
  // get name from params
  var name = request.params.name;
  // capitalize the name
  var capitalizedName = name[0].toUpperCase() + name.slice(1).toLowerCase();
  // set the value of the name to the capitalized version
  request.params.name = capitalizedName;
  // pass control to the next middleware function
  next();
})

app.get("/greet/:name", function (req, res) {
  res.send( "Hello, " + req.params.name );
});
```

Now every name will be capitalized, both for the `GET /greet/:name` route above and for any others with a name parameter.

### Additional Resources

1. [body-parser](https://github.com/expressjs/body-parser) on Github.
