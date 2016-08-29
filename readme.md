<!--
Location: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Express Routes & Query Strings

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Hard-coded routes like `/about`, `/home`, and `/contact` will only take a website so far.  Two great strategies save us from hard-coding every valid URL for a site. Parameterized routes like `/users/:name` help give a website the flexibility to grow as data is added.  A route that can handle query strings can read in extra data sent by the client, like `/search?q=epiphany`, and respond appropriately.  These strategies will also help keep code DRY.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Create more flexible, dynamic routes with parameterized route urls.
- Process data sent to a server through query strings.
- Process data sent from named form fields with `POST` requests.


### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Describe or draw how a server and a client participate in the request/response cycle with AJAX.
- Create simple, hard-coded routes in the Express framework.
- Distinguish between JavaScript, Node.js, npm, and Express.


### Review: Creating an Example App

#### 1. Directory and File Setup

Let's start a simple **Express** application.

1. Make a project directory and `server.js`:

  ```bash
  mkdir express_example
  cd express_example
  touch server.js
  ```

1. Use `npm init` to create a `package.json`:

  ```bash
  npm init
  ```

1. Install Express and open the project directory in your text editor:

  ```bash
  npm install --save express
  atom .
  ```

  The folder structure will be:

  ```
  express_example
    ├── node_modules/
        └── express/
    ├── server.js  
    ├── package.json
    ...
  ```


#####Check for Understanding

<details><summary>Summarize the effects of the two `npm` commands above.</summary>

* Node Package Manager keeps track of the various libraries and third-party packages of code used in a Node project.  

* The `npm init` command helps you set up `package.json`, which stores important information about the project.  

* `npm install --save express` tells the Node Package Manager to download and install the Express library for this particular project.  The `--save` flag lets NPM know that we will be downloading and using `express` for this project and to save it into the project's `package.json`.  

The above two `npm` commands will be necessary for every new web application that uses Express.  Other packages can also be installed as above.

</details>


#### 2. Server Setup

1. Write some boilerplate code for a simple server.  Remember to require express, call the `express` function to create a server, and tell the server to  start listening.

  `server.js`
  ```js
  // REQUIREMENTS
  var express = require('express'),
      app = express();

  // ROUTES
  // ... coming soon

  // SERVER START
  app.listen(3000, function () {
    console.log("HTTP server listening at localhost:3000");
  });

  ```

1. Start the server:

  ```bash
  node server.js
  ```

##### Check for Understanding

<details><summary>Visit `localhost:3000/` in your browser. Why don't we see anything yet? How can we show something here?</summary>

There aren't any routes yet! Add the following to the routes section of your `server.js` file and visit `localhost:3000/` again:

```js
app.get("/", function(request, response) {
  // send back the response: 'Hello World'
  response.send('Hello World');
});
```


#### 3. Starter Routes

##### Routes Review

Building an application requires a firm grasp of something  **routing**.  Each **route** is a combination of a **Request Type** and **Path**.  Code for the server lets us dictate how it will respond to requests on each route.


##### More Hard-Coded Routes

Let's build two more routes into our application:

| Request Type | Request Path | Response
| :--- | :--- | :--- |
| `GET` | `/` | `Hello World` |
| `GET` | `/api/burgers` | `Hamburger`, `Cheese Burger`, `Vegetable Burger` |
| `GET` | `/api/tacos` | `Soft Taco`, `Crunchy Taco`, `Super Taco` |



`server.js`
```js
// REQUIREMENTS
var express = require('express'),
  app = express();

//DATA (temporary until we know how to use databases)
var burgers = [
  'Hamburger',
  'Cheese Burger',
  'Vegetable Burger'
];

var tacos = [
  'Soft Taco',
  'Crunchy Taco',
  'Super Taco'
];

// ROUTES
app.get("/", function (request, response) {
  response.send("Hello World");
});

app.get("/api/burgers", function (request, response) {
  //send all the burgers     
  response.json(burgers);
});

app.get("/api/tacos", function (request, response) {
  //send all the tacos       
  response.json(tacos);
});

// SERVER START
app.listen(3000, function () {
  console.log("HTTP server listening at localhost:3000");
});

```

### Parameterized URLs

What if we want to create an app that can dynamically say hello to anyone?

**URL parameters** let us add dynamic routes to the application. In Express, parameters in a URL are indicated by a colon `:` and any variable name you want to use. We'll use `:name` for the example below.

```js
app.get("/greet/:name", function (request, response) {
  response.send( "Hello, " + request.params.name );
});
```

In the route above, `:name` is considered a route or URL parameter. Inside the route's callback, the parameter is accessible with `request.params.name`.

| Request Type | Request Path | Response
| :--- | :--- | :--- |
| `GET` | `/greet/:name` | `Hello, :name` |

### Independent Practice: Pick A Color

Create a route that responds with "You picked: blue" or "You picked: green" depending on a parameter in its path. For instance, if I visit `localhost:3000/pick-a-color/orange` in the browser, I should see the name of the color I chose (orange) as the response.

<details><summary>click for answer</summary>
```js
// server.js
// Pick A Color Route
app.get('/color/:choice', function(request, response){
  var choice = request.params.choice;
  response.send('Your color is: ' + choice);
});
```
</details>


### Query String Parameters

When you want even more flexibility, **query string parameters** can be included with each request.

Let's see query params in action. Go to [https://google.com/search?q=kittens&tbm=isch](https://google.com/search?q=kittens&tbm=isch).

#### Query String Review

* `?` marks the beginning of the query parameters.
* `=` indicates a key-value-pair assignment; the text to the left is a key, while the right is its value.
* `&` allows for multiple parameters; it separates the key-value pairs.



#### Query Parameters in Express

In Express, query string parameters are available in the `request.query` object. Let's add our first route to practice query params.

```js
app.get("/thank", function (request, response) {
  console.log(request.query);
  var name = request.query.name;
  response.send('Thank you, ' + name + '!');
});
```

#### Check for Understanding

<details><summary>What URL would you visit to see the results of the route above? Can you make the site say thank you with your name?</summary>


Reset your server and go to [localhost:3000/thank?name=jane](localhost:3000/thank?name=jane), but use your name instead if it's not Jane!
</details>


### Independent Practice: Calculator App

1. Create a `/multiply` route that uses query parameters `x` and `y` to multiple two numbers and send the result back: `"10 is the result"`.

2.  Create an `/add` route, similar to above.

### Choosing Parameter Types

* **request url parameters:** `http://localhost:3000/icecream/:flavor`

* **query string parameters:** `http://localhost:3000/icecream?flavor=SOMEFLAVOR`

Generally if the parameter is identifying a specific _entity_ or _resource_ on it's own, you most likely want request/url parameters.  If it is an optional parameter for a route, you should use query string parameters.

Common cases for **Request URL Parameters**:
* **database/item IDs**            -  GET `/contacts/348`
* **major components of the app**  -  GET `/users/2a98cef`
* **expressing hierarchies**       -  GET `/departments/44/employees/2`

> Request Params are not always nonsense IDs. Names can be used in some systems: `/departments/physics/employees/LutherRichard`

Common cases for **Query Parameters**:
* **searches**             -  GET `/?q=kittens`
* **optional selections**  -  GET `/calculator?lang=en`
* **pagination**           -  GET `/articles?start=1&num=10`
* **other limits**         -  GET `/posts?since=2015-11-29`

Of course, you might combine both in some cases: `/posts/33/comments?limit=50`

### Independent Practice: Which Taco?

1. Hardcode some data in your main server file. This will stand in for database data until we learn how to connect a database.

``` javascript
var tacos = [
                "Soft Taco",
                "Crunchy Taco",
                "Super Taco"
            ];
```

1. We'd like a route that will allow you to access a single taco by its index in the array. For example, sending index 2 would trigger a server response with the text "Super Taco".  What kind of parameters would be most appropriate for this route, query parameters or url parameters?

<details><summary>click for answer</summary>
Since we're identifying a single resource, it's best to use url parameters (aka route parameters).
</details>

1. Create the route you planned in your server file. Following the pattern from the example earlier, your endpoint should include `/api`.

<details><summary>click for answer</summary>
```js
// server.js

// Which Taco?
app.get('/api/tacos/:id', function(request, response){
  var index = request.params.id;
  var selection = tacos[id] || 'Sorry, that\'s not a taco option';
  response.json( selection );
});

```
</details>


### POST

Up until now, we've used the HTTP method `GET` to _retrieve_ data from web APIs - either third-party APIs or a few very simple ones we've created.  

_But what if we want to let our users store data?_

When one wants to store data - or **create new resources** on a server, the standard is to use `POST`.


#### Client-side `POST` Review

You can use `POST` directly from an HTML form.

```html
<body>
  <form method="POST" action="http://localhost:3000/api/cities">
    <input id="cityName" name="name" type="text" />
    <input id="cityDesc" name="description" type="text" />
    <input type="submit" />
  </form>
</body>
```

You can also make a `POST` request with AJAX:

```js
// app.js
$.ajax({
  method: "POST",
  url: "http://localhost:3000/api/cities",
  data: $('#city-form').serialize(),
  success: handleCityCreateSuccess,
  error: handleCityCreateError
});

function handleCityCreateSuccess(responseData) {
  console.log("city was successfully created!");
  console.log("the name of the new city entered is ", responseData.name);
  // render book to page
}

function handleError(jqXHR, status, error){
  console.log('error:', error);
}
```

#### Server-side `POST` Request Handling

In our Express server, we'll use `app.post` rather than `app.get` to handle `POST` requests.  This way, we can make two routes that use the same path.

In other words, **both the request type and request path must match for the server and client to communicate correctly**.

```js
// server.js
app.post('/api/cities', function citiesCreate(request, response) {
  // save the posted data
});
```

### RESTful Routing

Let's look at some routes for a cities **resource**.  

| HTTP Verb | Route       | RESTful description | Purpose |
| :-------- | ----------- | ------------------- | --------------------|
| GET       | /api/cities     | citiesIndex         | Listing all cities. |
| GET       | /api/cities/:id | citiesShow          | Details of one city. |
| POST      | /api/cities     | citiesCreate        | Create a new city.

**REST** is a convention for writing routes in a standard way to make it easier to work with **resources** across the web.  We'll talk more about this, as it's an important theme in modern API design.


### Middleware and `body-parser`

Middleware allows us to intercept the request and response before they're processed by their actual route handler.

POST requests don't use query parameters like GET requests do.  Instead, they submit data in the body of the request.  This looks similar on the client side (the data can still be sent as `data` in jQuery's `ajax` method).

On the server side, we'll use middleware called **body-parser** to help us parse and make use of data from the body of a request.


#### Setting Up `body-parser`

To add the `body-parser` middleware to your app:  

1. Install the `body-parser` module for the project:

  ```bash
  npm install --save body-parser
  ```

2. Require `body-parser` in your server file:

  ```js
  var bodyParser = require('body-parser');
  ```

3. Include the middleware in the app with `app.use`. (The url encoding is configuration.)

  ```js
  app.use(bodyParser.urlencoded({ extended: false }));
  ```

#### Using `body-parser`

In any routes receiving `POST`ed data, access that data using `request.body`.

```js
app.post('/api/cities', function citiesCreate(request, response) {
  var name = request.body.name;
  var desc = request.body.description;
  var newCity = { name: name, description: desc };
  // if we have a cities array in our app (pre-database):
  cities.push(newCity);
  response.json(cities);
});
```

You can [read more about middleware here](middleware-reading.md).

> Note: for most of our Express apps, we'll include `body-parser` as part of the _boilerplate_ of the app.


### Closing Thoughts

* Use dynamic url parameters, i.e. `/api/burgers/:index` and `/api/tacos/:index`, to request data about a specific resource. Access them on the server in the `request.params` object.
* Use query string parameters for dynamic requests to serve up dynamic responses. Access them on the server in the `request.query` object.
* Use `POST` with named form inputs to send data to our Express servers, and use `body-parser` to access that data as part of the `request.body` object.

This will be essential knowledge for building and interacting with applications that contain multiple resources.  

We'll use `PUT` or `PATCH` to send data to update item information on the server side (instead of `POST`), and we'll use `DELETE` to delete items on the server side.


### Additional Resources

0. [In-depth HTTP Intro](
* [HTTP Intro](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177)
1. [Starting an Express Project](http://expressjs.com/starter/installing.html)
2. [Express Hello World](http://expressjs.com/starter/hello-world.html)
3. [Express Basic Routing](http://expressjs.com/starter/basic-routing.html)
