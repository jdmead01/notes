## Overview 

##  New Node Project

 - [ ] Navigate to a folder you want to create your project
 - [ ] create project using terminal 
 ```terminal
mkdir {{project_name}}
```
 - [ ] Navigate into the newly created project using terminal:
 ```terminal
cd {{project_name}}
```
 - [ ] within the project run the following commands in the terminal
 ```terminal
npm init -y
npm install express body-parser mongoose 
code .
```

code . should open project up in VS Code 
##### create server.js
```javascript
/// if you're only using angular all you need is body parser express 
const  express  =  require("express")
const  bp  =  require("body-parser")
const  path  =  require("path")
var  app  =  express()

app.use(bp.json());
app.use(express.static( __dirname +  '/public/dist/public' ));

require('./routes')(app)

app.listen(8000, (err)=>{
	if (err){
		console.log(err)
	} else {
		console.log("listening on port 8000...")
	}
})
```
##### routes.js
```javascript
const controller = require("./controller")

module.exports = function(app){
    app.get('/api/authors', controller.authors)
    app.get('/api/authors/:id', controller.author)
    app.post('/api/authors', controller.create)
    app.put('/api/authors/:id', controller.update)
    app.delete('/api/authors/:id', controller.delete)
    app.all("*", controller.all) // Typing into the nav bar and hitting enter or making the browser refresh will trigger the Express routes first and Angular routes second.
}
// this route will be triggered if any of the routes above did not match
app.all("*", (req,res,next) => {
  res.sendFile(path.resolve("./public/dist/public/index.html"))
});
```
##### create controller.js
```javascript
const  Widget  =  require("./models");
const  bodyParser  =  require('body-parser');
  
module.exports  = {
	// GET: Retrieve all Tasks
	home: (req,res)=>{
		console.log("in Home route")
		Task.find({}, function(err, data) {
			if(err){
				console.log("home Returned error", err);
				res.json({message: "Error", error: err});
			}else {
				console.log("Data: ");
				console.log(data);
				res.json(data); //message: "Success", data:
			}
		})
	},
	id: (req, res)=>{
		// GET: Retrieve a Task by ID
		console.log("in Id route");
		console.log("req.params.id: ");
		console.log(req.params.id);
		Task.findById({_id: req.params.id}, function(err, data){
			if(err){
				console.log("name Returned error", err);
				res.json({message: "Error", error: err});
			}else {
				console.log("Data: ");
				console.log(data);
				res.json({message: "Success", data: data});
			}
		})
	},
	create: (req, res)=>{
		// POST: Create a Task
		console.log("in Create route");
		console.log("req.params.id: ");
		console.log(req.params.id);
		console.log("req.body: ");
		console.log(req.body);
		req.body = {
			'title': "something",
			'description': "something else"
			}
		Task.create(req.body, function(err, data){
			if(err){
				console.log("new Returned error", err);
				res.json({message: "Error", error: err});
			}else {
				console.log("Data: ");
				console.log(data);
				res.json({message: "Success", data: data});
			}
		})
	},
	update: (req, res)=>{
		// PUT: Update a Task by ID
		console.log("in Update route");
		console.log("req.params.id: ");
		console.log(req.params.id);
		console.log("req.body: ");
		console.log(req.body);
		Task.findOneAndUpdate(req.params.id, req.body, function(err, data){
			if(err){
				console.log("new Returned error", err);
				res.json({message: "Error", error: err});
			} else {
				console.log("Data: ");
				console.log(data);
				res.json({message: "Success", data: data});
			}
		})
	},
	delete: (req, res)=>{
		// DELETE: Delete a Task by ID
		console.log("in Delete route");
		console.log("req.params.id: ");
		console.log(req.params.id);			
		Task.findByIdAndDelete({_id: req.params.id}, function(err, data){
			if(err){
				console.log("remove Returned error", err);
				res.json({message: "Error", error: err});
			}else {
				console.log("Data: ");
				console.log(data);
				res.json({message: "Success", data: data});
			}
		})
	},
	generate: (re,res)=>{
		console.log("in GENERATE data")
		// Task.tasks.drop()
		// Pandas.collection.drop()
		Task.collection.drop();
		
		var  temp_data  = [
			{ 'title': "Think about stuff more thoroughly", 'description': "Too much stuff"},
			{ 'title': "Deck of Cards", 'description': "Building methods"},
			{ 'title': "Coding in Angular", 'description': "Wheeeeeeeee"},
			{ 'title': "White Boarding MergeSort", 'description': "Now that is Fun!"}
		];
		
Task.create(temp_data, onInsert);

function  onInsert(err, docs) {
	if (err) {
		console.log("error: ");
		console.log(err);
	}else {
		console.info('%d tasks were successfully stored.', docs.length);
	}
}
res.redirect('/');
}
}
```

 - [ ] temp_data is data that will be automatically generated. Now you can go to VS Code within the controller.js file and change all instances of 'Task' to 'Widget'

##### models.js
Take a look at the provided wireframe and outline what you need in order to make your database work properly within the scope of the project. 

If you identify more than one schema (1 to many relationship) ensure the schema that is nested within the main schema appears ABOVE the exported schema 
```javascript
const mongoose = require("mongoose");
mongoose.Promise = global.Promise;
mongoose.connect('mongodb://localhost/widget_db');

var FeatureSchema = new mongoose.Schema({
	feature: {type: String, required: true, minlength: 5},
	completed: { type: Boolean, default: false}
	}, {timestamps: true });
	
var WidgetSchema = new mongoose.Schema({
	title: { type: String, required: true, minlength: 2},
	description: { type: String, maxlength: 255, default: ""},
	feature: [FeatureSchema],
	completed: { type: Boolean, default: false}
}, {timestamps: true });

mongoose.model('Widget', WidgetSchema);
var Widget = mongoose.model('Widget')

module.exports = Widget;
```

 - [ ] Ensure you are referencing the correct Database in the mongoose.connect portion of your models.js. mongoose.connect('mongodb://localhost/**task_db**');

#### Mongo
```console
sudo mongod
mongo
```
#### Run Nodemon
navigate to the project root folder 
```Terminal
nodemon server.js
```
Some errors experienced
```errors
/Users/jdmead/Library/Mobile Documents/com~apple~CloudDocs/Coding Dojo/M.E.A.N/belt_exam/routes.js:12
app.all("*", (req,res,next) => {
^

ReferenceError: app is not defined
    at Object.<anonymous> (/Users/jdmead/Library/Mobile Documents/com~apple~CloudDocs/Coding Dojo/M.E.A.N/belt_exam/routes.js:12:1)
    at Module._compile (internal/modules/cjs/loader.js:776:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:787:10)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Module.require (internal/modules/cjs/loader.js:690:17)
    at require (internal/modules/cjs/helpers.js:25:18)
    at Object.<anonymous> (/Users/jdmead/Library/Mobile Documents/com~apple~CloudDocs/Coding Dojo/M.E.A.N/belt_exam/server.js:21:1)
    at Module._compile (internal/modules/cjs/loader.js:776:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:787:10)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:829:12)
    at startup (internal/bootstrap/node.js:283:19)
[nodemon] app crashed - waiting for file changes before starting...
```
**Use Postman To check if the routes work**

 - app.get('/api/widgets', controller.widgets)
 - app.get('/api/widgets/:id', controller.author)
 - app.post('/api/widgets', controller.create)
 - app.put('/api/widgets/:id', controller.update)
 - app.delete('/api/widgets/:id', controller.delete)
 - app.all("*", controller.all) // Typing into the nav bar and hitting enter or making the browser refresh will trigger the Express routes first and Angular routes second.

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
eyJoaXN0b3J5IjpbLTQyMzYyNzE2NCw2NTcxOTE3OTUsLTEyNj
A2MDk4NDAsMTE5Mzc4MzA1Miw5Njc0MzE5OTYsLTIwODcyMTc5
ODIsLTE2NTAzMjM0NjAsLTE3ODY0OTA2ODgsMTI1ODc0ODIzMS
wtMjk1NjU3OTI3LDcxNTM1MDQ2Nyw3ODIwMjUyMTQsLTMzOTY5
NzkwMSwtMjAwOTMwODM2MywyMDQ2MTc0NTEsNjExNTg0NjU0XX
0=
-->