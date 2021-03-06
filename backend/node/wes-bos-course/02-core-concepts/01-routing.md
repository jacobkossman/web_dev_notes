# Core Concept - Routing
* You will deal with routing in any Application you build
* When people go to a URL you need to do stuff
    - query Database
    - filter through list of stores
    - modify that date in some way
    - and finally, when you have all the data you want to send to the user, you send it to them

## `routes/index.js`
1. Import express

`const express = require('express');`

2. Grab router off of express

`const router = express.Router();`

3. Define all your routes

```
router.get('/', (req, res) => {
  res.send('Hey! It works!');
});
```

4. We point to this file in `app.js`

`const routes = require('./routes/index');`

5. We tell express to use our **routes** here in `app.js`

`app.use('/', routes);`

* Anytime someone goes to `/` anything, we will hit the routes file and that file will handle every single URL hit that we get
* You can have multiple route handlers `app.use('/admin', adminRoutes)`

## How the router works
```
router.get('/', (req, res) => {
  res.send('Hey! It works!');
});
```

1. You get the url
2. You have a callback function that runs whenever someone visits that specific URL

### Callback function gives you three things
1. The `req` (request) ---> an object full of information coming in
2. the `res` (response) ---> an object full of methods for sending data back to the user
3.  `next` --> we'll learn more about this later but at times you won't want to send data back and you just want to pass it off to something else and that is the topic of `middleware`

### Stuff we can do with our `res`
* res.send()
* You NEVER want to send data more than once

```
router.get('/', (req, res) => {
  const player = { name: 'Kobe', age: 40, good: true };
  res.send('Hey! It works!');
  res.json(player);
});
```

## Houston we have a problem
We get an error stating `Error: Can't set headers after they are sent`

![header error](https://i.imgur.com/vpowWmg.png)

But if we comment out `res.send()` and we want to send our JSON object

```
router.get('/', (req, res) => {
  const player = { name: 'Kobe', age: 40, good: true };
  // res.send('Hey! It works!');
  res.json(player);
});
```

We will see this:

![json output](https://i.imgur.com/UZVO25T.png)

* Download Chrome extension `JSON formatter` to make your JSON look more readable

### How do we get data that is in URL?
`http://localhost:7777/?name=kobe&age=40&good=true`

* The URL is part of the request

```
router.get('/', (req, res) => {
  const player = { name: 'Kobe', age: 40, good: true };
  // res.send('Hey! It works!');
  // res.json(player);
  res.send(req.query.name);
});
```

Will output to screen `kobe`

### Pass our query back in JSON format
URL: `http://localhost:7777/?team=lakers&good=true`

```
router.get('/', (req, res) => {
  const player = { name: 'Kobe', age: 40, good: true };
  // res.send('Hey! It works!');
  // res.json(player);
  // res.send(req.query.good);
  res.json(req.query);
});
```

`app.js`

```
// Takes the raw requests and turns them into usable properties on req.body
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

* This is middleware
* Before we get to our routes, express will check URL and it will check if use has posted data from a form and this will put all the data in the request so we can easily access it with stuff like `req.query` or `req.body` or `req.params`

### Let's make a new route
* Our route will return the reverse of the name we enter
* How do you put variables in a route? `router.get('/reverse/:name')`

URL -> http://localhost:7777/reverse/bob

```
router.get('/reverse/:name', (req, res) => {
  res.send('it works!');
});
```

Now if we go to that URL it will hit our route and output `it works!`

```
router.get('/reverse/:name', (req, res) => {
  res.send(req.params);
});
```

URL -> http://localhost:7777/reverse/jerry

Will output to screen:

```
{
"name": "jerry"
}
```

### Reverse the name in the URL and send it back to user
URL -> http://localhost:7777/reverse/elvis

```
router.get('/reverse/:name', (req, res) => {
  const reverse = [...req.params.name].reverse().join('');
  res.send(reverse);
});
```

* Try to reverse the name of `racecar`
* req.body --> we'll use for posted parameters

All this and more can be found at the [Express Documentation page](https://expressjs.com/en/4x/api.html)





