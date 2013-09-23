---
title: "Your first app in 30 minutes"
layout: "default"
category: "hack"
menuOrder: 2
---

# Your first app in 30 minutes
This first tutorial is an introduction to cozy webapp development. Therefore, as we've mentioned earlier, it is an introduction to webapp development in NodeJS.

## Before we start...

### What you will achieve
* writing a small NodeJS webapp using [ExpressJS](http://expressjs.com/), a framework for NodeJS
* introducing [Jade](http://jade-lang.com/) to write the templates that will be displayed by the browser
* deploying it into Cozy

### What you need to know (or to be familiar with)
* JavaScript / NodeJS
* HTML
* Git

### Get ready
Get the template:
```bash
cd cozy-dev/ # where you start the VM
git clone https://github.com/mycozycloud/cozy-tutorial.git && cd cozy-tutorial
npm install
```

* public/ : this is where we will have our public assets (html pages, styles, images, javascripts)
* package.json : holds your app information and dependencies.
* server.js : where you will write your code

## Step 1: displaying HTML pages
We first need to create a HTTP server that will serve the content on requests:
```javascript
// server.js

// At the root of your website, we show the index.html page
app.get('/', function(req, res) {
    res.sendfile('./public/index.html')
});

/* This will allow Cozy to run your app smoothly but
 it won't break other execution environment */
var port = process.env.PORT || 9250;
var host = process.env.HOST || "127.0.0.1";

// Starts the server itself
var server = http.createServer(app).listen(port, host, function() {
    console.log("Server listening to %s:%d within %s environment",
                host, port, app.get('env'));
});
```
Now start the server by running:
```bash
node server.js
```

And open your browser on http://localhost:9250/ and check the result.

Whoohoo, you've just made your first NodeJS app compatible with Cozy!

You might think "well, it sucks, I actually can't do anything with that". You are absolutely right, let's move on to step 2.

## Step 2: listing the bookmarks from the server
For the step 2, we are going to use a list stored in memory on the server and render it within a template. We are going to use Jade as a template engine. If you don't know Jade, you will find it is not difficult to read and write it.

First, install jade:
```bash
npm install jade --save # save also adds jade to package.json
```

Go back to `server.js` and change it that way:
```javascript
// server.js

/* We add configure directive to tell express to use Jade to
   render templates */
app.configure(function() {
    app.set('views', __dirname + '/public');
    app.engine('.html', require('jade').__express);
});

// Let's define some bookmarks
var bookmarks = []
bookmarks.push({title: "Cozycloud", url: "https://cozycloud.cc"});
bookmarks.push({title: "Cozy.io", url: "http://cozy.io"});
bookmarks.push({title: "My Cozy", url: "http://localhost:9104/"});

// We render the templates with the data
app.get('/', function(req, res) {
    params = {
        "bookmarks": bookmarks
    }
    res.render('index.jade', params, function(err, html) {
        res.send(200, html);
    });
});
```

We also need to change the HTML file for a Jade file:
```jade
doctype 5
html(lang="fr")
  head
    title My Own Bookmarks

  body
    h1 Welcome on My Own Bookmarks
    p This application will help you manage your bookmarks!

    ul
        - for(bookmark in bookmarks) {
            li= bookmarks[bookmark].title
                | &nbsp;- (
                a(href=bookmarks[bookmark].url) link
                | )
        - }
```

Start the server and go to http://localhost:9250/ to make sure the app displays your bookmarks.

Again, this is not very useful because you can't modify the list. Let's fix it!

## Step 3: adding and removing bookmarks

First, add the following before the list in `index.jade`

```jade
form(action="/add", method="post")
    label Title:
    input(type="text", name="title")
    label Url:
    input(type="text", name="url")
    input(type="submit", value="Add a new bookmark")
```

Then we must define the corresponding route in the server with the appropriate


We also need a button to remove a bookmark, let's rewrite the way a bookmark is displayed:

```jade
li
    a(href=bookmarks[bookmark].url)= bookmarks[bookmark].title
    | &nbsp;- (
    a(href="/delete/#{bookmark}") delete
    | )
```

Now we can create the corresponding routes in the server:

```javascript
// We define a new route that will handle bookmark creation
app.post('/add', function(req, res) {
    bookmarks.push(req.body);
    res.redirect('/');
});

// We define another route that will handle bookmark deletion
app.get('/delete/:id', function(req, res) {
    bookmarks.splice(req.params.id, 1);
    res.redirect('/');
});
```

To be able to get the data from the POST request, we need to tell express to process the parameters. Add the following inside the "app.configure" section:

```javascript
// Allows express to get data from POST requests
app.use(express.bodyParser());
```

Et voilà! You can now add and remove bookmarks. But it still sucks, right? Each time you start and stop the server you lose everything. Let's use a database!

## Step 4: using a real database, SQLite

## Step 5 : deploying your app into Cozy
If you want to deploy the app on a Cozy, your first need to put on a public git repository. We use [Github](https://github.com) because it's awesome but you can use any git provider.

When you have published your code, go to http://localhost:9104/#applications and install your app through the dedicated interface.

# What's next ?
You've developed your first Cozy app and you must understand now that it's nothing more than a normal web application.
Next we'll introduce you the Cozy architecture before diving in Cozy specifities.