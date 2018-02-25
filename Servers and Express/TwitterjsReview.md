## Twitter.js Review Video

First we need to do an npm init, which walks us through generating a package.json file.

Start by doing some scaffolding (creating folders and files), then ```npm install express --save```. ('--save' adds it to the package.json)

Create a '.gitignore' file, and add 'node_modules' - we don't need to add these to GitHub because we can always just redownload all the npm packages (assuming they're saved in our package.json - hence the --save) by running ```npm install```.

Express just sits on top of Node's http library.

Basic http app:
```
var http = require('http');

var server = http.createServer(function(request, response) {
  console.log('made a request!');
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.write('Here is some plain text');
  response.end();
})

server.listen(1337, function(){
  console.log('we are listening on 1337')
})
```

(Same) Basic Express app:
```
var express = require('express');
var app = express();

app.use(function(request, response) {
  response.response('Here is some plain text. KIDDING! It is html');
})

app.listen(1337, function(){
  console.log('we are listening on 1337')
})
```

We can customize with 'app.get()', which will route to a particular function based on the method and uri associated with the request.

You can use 'app.use()' for middleware:
```
app.use(function(req,res,next){
  console.log('you made a request');
  next(); // When you use middleware, you need to explicitly call next to tell it to move down the chain
})
```

Morgan is a tool that lets us create logging middleware:
```
var morgan = require('morgan')

var logger = morgan('dev')

app.use(logger);
```

Nunjucks is a templating language. It takes files that contain mostly HTML, but also have some syntax that looks for data.
```
var nunjucks = require('nunjucks')

nunjucks.configure('views') // this tells nunjucks where our templates are

var locals = {
  title: 'Some Title',
  people: [
    { name: 'Gandalf' },
    { name: 'Hermione' }
    ]
} // this is where our data is

nunjucks.render('index.html', locals, function(err,output) {
  if (err) return console.err
  console.log(output)
})

// Or, using an Express method:

nunjucks.configure('views', {noCache: true })
app.set('view engine', 'html')
app.engine('html', nunjucks.render)

app.get('/', function(res,req){
  res.render('index', locals) // render the index template using the data from locals
})
```

TweetBank Module:
```
var _ = require('lodash')

var data = [];

function add (name, text) {
  data.push({name: name, text: text})
}

function list(){
  return _.cloneDeep(data) // we create a deep clone so that whatever we do, we're not modifying the original array
}

function find(properties) {
  return _.cloneDeep(_.filter(data, properties))
}
```

Modularizing Our Routes:
```
// routes/index.js
var express = require('express');
var router = express.Router() // returns a new router instance
var tweetBank = require('../tweetBank');

router.get('/', function(req, res, next) {
  var tweets = tweetBank.list()
  res.render('index', {title: 'Twitter.js', tweets: tweets})
})

/*
router.get('/stylesheets/style.css', function(req, res, next){
  res.sendFile('/stylesheets/style.css', {root: __dirname + '/../public/'}) // .sendFile takes either an absolute path or a relative path and an options object (which contains the root)
})
*/

```

Static Routing:
```
var fs = require('fs');
var path = require('path');
var mime = require('mime');

// manually written static middleware
app.use(function(req,res,next){
  var mimeType = mime.lookup(req.path) // gives us the file location
  fs.readFile('./public' + req.path, function(err, fileBuffer){
    if (err) return next();
    res.header('Content-Type', mimeType)
    res.send(fileBuffer)
  })
})

// express built-in static middleware
app.use(express.static(__dirname + '/public'))
```

Route to show the tweets for an individual user:
```
router.get('/users/:name', function (req,res,next) { // the part that starts with ":" is a parameter (aka variable)
  // How do we get just the tweets for one user? In our tweetBank, there's a find function
  var tweetsForName = tweetBank.find({ name: req.params.name })
  res.render('index', { title: 'Twitter.js', tweets: tweetsForName })
})
```

Posting a tweet:
```
// Our form shows us posting to /tweets
router.post('/tweets', function(req,res,next) {
  // We have an add function on our tweetBank - so we use that
  // We install the body-parser library allow us to parse the text from the req.body
  tweetBank.add(req.body.name, req.body.text)
  // Once we add it to the tweet bank, we need to send a response
  res.redirect('/') // this instructs the browser to go to a different location - here, the root (where all tweets are listed)
})
```