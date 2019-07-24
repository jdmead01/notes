
##  New Node Project
```console
mkdir {{project_name}}
cd {{project_name}}
mkdir static static/images static/css views
npm init -y
npm install express ejs express-session body-parser express-flash mongoose moment mongoose socket.io
code .
```
##### create server.js
```javascript
const express = require("express")
const bp = require("body-parser")
const path = require("path")
const session = require("express-session")
const flash = require("express-flash")
var app = express()

app.use(bp.urlencoded({extended: true}))
app.use(express.static(path.join(__dirname, './static')))
app.use(session({
    secret: 'quotes',
    resave: false,
    saveUninitialized: true,
    cookie: { maxAge: 60000 }
}))
app.use(flash())

app.set('views', path.join(__dirname, './views'))
app.set('view engine', 'ejs')
  
require('./routes')(app)

app.listen(8000, (err)=>{
    if (err){
        console.log(err)
    } else {
        console.log("listening on port 8000...")
    }
})
```
##### create controller.js
```javascript
const User = require("./models")

module.exports = {
    index : (req, res)=>{
        res.render('index')
    },
    users : (req, res)=>{
        console.log("POSTDATA", req.body
        var user =  new User({name: req.body.name, age: req.body.age});
        // Try to save that new user to the database (this is the method that actually inserts into the db) and run a callback function with an error (if any) from the operation.
        user.save(function(err) {
        // if there is an error console.log that something went wrong!
            if(err) {
	        console.log('something went wrong');
            } else { // else console.log that we did well and then redirect to the root route
                console.log('successfully added a user!');
            }
        res.redirect('/')
        })
    }
}
```
##### create routes.js
```javascript
const controller = require("./controller");
module.exports = function(app){
  app.get('/', controller.index)
  app.get('/cars', controller.cars)
  app.get('/cars/new', controller.newcar)
  app.get('/cats', controller.cats)
  app.get('/cats/:catID', controller.cat_show)
}
```
##### models.js
```javascript
const mongoose = require('mongoose')

mongoose.connect('mongodb://localhost/quotes_db')

var QuoteSchema =  new mongoose.Schema({
name: String,
quote: String,
},
{timestamps : true})  //https://stackoverflow.com/a/15147350/5248397

module.exports = mongoose.model('Quote', QuoteSchema)
```

#### Mongo
```console
sudo mongod
mongo
```
#### project tree
.
├── controller.js
├── models.js
├── routes.js
├── server.js
├── static
│   └── css
│       └── style.css
└── views
    └── index.ejs
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDkxODczMTY0XX0=
-->