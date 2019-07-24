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
##### create controller.js
```javascript
const  Task  =  require("./models");
const  bodyParser  =  require('body-parser');
var  moment  =  require('moment');
  
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
eyJoaXN0b3J5IjpbLTE1NzcyNDY4MzgsOTY3NDMxOTk2LC0yMD
g3MjE3OTgyLC0xNjUwMzIzNDYwLC0xNzg2NDkwNjg4LDEyNTg3
NDgyMzEsLTI5NTY1NzkyNyw3MTUzNTA0NjcsNzgyMDI1MjE0LC
0zMzk2OTc5MDEsLTIwMDkzMDgzNjMsMjA0NjE3NDUxLDYxMTU4
NDY1NF19
-->