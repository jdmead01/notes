##  New Angular Project 

 1. [ ] navigate into the folder directory you want your new app to be saved in:
``` terminal 
ng new public
```

 2. [ ] Yes to Routing
 3. [ ] Select CSS
 4. [ ] in server.js 
```terminal
cd public
```
**app.module.ts**
```javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { HttpService } from './http.service';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';

@NgModule({
declarations: [
	AppComponent
	],

imports: [
	BrowserModule,
	HttpClientModule
	],
	providers: [HttpService],
	bootstrap: [AppComponent]
	})
	export class AppModule { }
```
**http.service.ts**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
	providedIn: 'root'
	})

export class HttpService {

	constructor(private _http: HttpClient){
		this.getTasks();
	}

getTasks(){

	// // our http response is an Observable, store it in a variable

	let tempObservable = this._http.get('/tasks');

	// // subscribe to the Observable and provide the code we would like to do with our data from the response

	tempObservable.subscribe(data => console.log("Got our tasks!", data));

	return this._http.get('/tasks');
	}
}
```
**app.components.ts**
```javascript
import { Component, OnInit } from '@angular/core';
import { HttpService } from './http.service';

@Component({
	selector: 'app-root',
	templateUrl: './app.component.html',
	styleUrls: ['./app.component.css']
})

export class AppComponent implements OnInit {
	title = 'TITLE OF PROJECT app.component.ts';
	num: number;
	randNum: number;
	str: string;
	first_name: string;
	snacks: string[];
	loggedIn: boolean;
	
constructor(private _httpService: HttpService){}

// // examples on initialization gives them values
ngOnInit(){
	this.getTasksFromService();
	this.num = 7;
	this.randNum = Math.floor( (Math.random() * 2 ) + 1);
	this.str = 'Hello Angular Developer!';
	this.first_name = 'Alpha';
	this.snacks = ["vanilla latte with skim milk", "brushed suede", "cookie"];
	this.loggedIn = true;
}

tasks = [];
getTasksFromService(){
	// this._httpService.getTasks();
	let observable = this._httpService.getTasks();
	observable.subscribe(data => {
		console.log("Got our tasks!", data);
		// In this example, the array of tasks is assigned to the key 'tasks' in the data object.
		// This may be different for you, depending on how you set up your Task API.
		this.tasks = data['tasks'];
		});
	}
	onButtonClick(): void {
		console.log(`Click event is working`);
	}
	onButtonClickParam(num: Number): void {
		console.log(`Click event is working with num param: ${num}`);
	}
	onButtonClickParams(num: Number, str: String): void {
		console.log(`Click event is working with num param: ${num} and str param: ${str}`);
	}
	onButtonClickEvent(event: any): void {
		console.log(`Click event is working with event: ${event}`);
		}
}
```
**app.component.html**
```html
<!--The content below is only a placeholder and can be replaced.-->
<div  style="text-align:center"></div>
<div  class="header">
	<h1>Welcome to {{ title }}!</h1>
	<a  href="/generate"></a>
		<img  class="ng"  width="40"  alt="Angular Logo"  	src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNTAgMjUwIj4KICAgIDxwYXRoIGZpbGw9IiNERDAwMzEiIGQ9Ik0xMjUgMzBMMzEuOSA2My4ybDE0LjIgMTIzLjFMMTI1IDIzMGw3OC45LTQzLjcgMTQuMi0xMjMuMXoiIC8+CiAgICA8cGF0aCBmaWxsPSIjQzMwMDJGIiBkPSJNMTI1IDMwdjIyLjItLjFWMjMwbDc4LjktNDMuNyAxNC4yLTEyMy4xTDEyNSAzMHoiIC8+CiAgICA8cGF0aCAgZmlsbD0iI0ZGRkZGRiIgZD0iTTEyNSA1Mi4xTDY2LjggMTgyLjZoMjEuN2wxMS43LTI5LjJoNDkuNGwxMS43IDI5LjJIMTgzTDEyNSA1Mi4xem0xNyA4My4zaC0zNGwxNy00MC45IDE3IDQwLjl6IiAvPgogIDwvc3ZnPg==">
<p>Create (app.POST) | Read (app.GET) | Update (app.PUT) | Delete (app.DELETE)</p>
</div>
<div  class="topnav">
<a  href="#">Add New Restaurant</a>

<!-- <a href="#">Link</a>

<a href="#">Link</a> -->
<a  href="#"  style="float:right">Link</a>
</div>
<div  class="row">
<div  class="leftcolumn">
<div  class="card">
<h2>Lets Eat!</h2>
<h5>current Date</h5>
<!-- <div class="fakeimg" style="height:200px;">Image</div> -->
<table>
<caption>Something Table</caption>
<thead>
<tr>
<th  scope="col">Restaurant</th>
<th  scope="col">Cuisine</th>
<th  scope="col">Actions Available</th>
</tr>
</thead>
<tbody>
<tr>
<td  data-label="Account">{{restaurant.name}}</td>
<td  data-label="Due Date">04/01/2016</td>
<td  data-label="Amount">$1,190</td>
</tr>
</tbody>
</table>
</div>
</div>
<div  class="rightcolumn">
<div  class="card">
<h2>Restaurant Name</h2>
<div  class="fakeimg"  style="height:100px;">Image</div>
<p>Review</p>
</div>
<div  class="card">
<h3>Popular Restaurants</h3>
<div  class="fakeimg"><p>Image</p></div>
<div  class="fakeimg"><p>Image</p></div>
<div  class="fakeimg"><p>Image</p></div>
</div>
</div>
</div>
<div  class="footer">
<h2>Another Failed Belt Exam</h2>
</div>
// // loops for actionables in displaying content located in the databse
<p  *ngIf="loggedIn">You are logged in!</p>
<p  *ngFor="let snack of snacks">{{snack}}</p>
<p  *ngIf="snacks.length < 3">You need more snacks.</p>
// // button actionables examples 
<button  (click)="onButtonClick()" >Click me!</button>
<button  (click)="onButtonClickParam(5)">Click me!</button>
<button  (click)="onButtonClickParams(5, 'hello')">Click me!</button>
<button  (click)="onButtonClickEvent($event)">Click me!</button>
```
** app.component.css **
``` css
* {
	box-sizing: border-box;
}
body {
	font-family: Arial;	
	padding: 10px;
	background: #f1f1f1;
	
/* Header/Blog Title */

.header {

padding: 30px;

text-align: center;

background: white;

}

  

.header  h1 {

font-size: 50px;

}

  

/* Style the top navigation bar */

.topnav {

overflow: hidden;

background-color: #333;

}

  

/* Style the topnav links */

.topnav  a {

float: left;

display: block;

color: #f2f2f2;

text-align: center;

padding: 14px  16px;

text-decoration: none;

}

  

/* Change color on hover */

.topnav  a:hover {

background-color: #ddd;

color: black;

}

  

/* Create two unequal columns that floats next to each other */

/* Left column */

.leftcolumn {

float: left;

width: 75%;

}

  

/* Right column */

.rightcolumn {

float: left;

width: 25%;

background-color: #f1f1f1;

padding-left: 20px;

}

  

/* Fake image */

.fakeimg {

background-image: url('https://proxy.duckduckgo.com/iu/?u=https%3A%2F%2Fgffoodservice.org%2Fwp-content%2Fuploads%2F2015%2F03%2Frestaurant-e1456862749354.jpg&f=1');

height: 100px;

background-color: #aaa;

width: 100%;

padding: 20px;

}

  

/* Add a card effect for articles */

.card {

background-color: white;

padding: 20px;

margin-top: 20px;

}

  

/* Clear floats after the columns */

.row:after {

content: "";

display: table;

clear: both;

}

  

/* Footer */

.footer {

padding: 20px;

text-align: center;

background: #ddd;

margin-top: 20px;

}

  

/* Responsive layout - when the screen is less than 800px wide, make the two columns stack on top of each other instead of next to each other */

@media  screen  and (max-width: 800px) {

.leftcolumn, .rightcolumn {

width: 100%;

padding: 0;

}

}

  

/* Responsive layout - when the screen is less than 400px wide, make the navigation links stack on top of each other instead of next to each other */

@media  screen  and (max-width: 400px) {

.topnav  a {

float: none;

width: 100%;

}

}

body {

font-family: "Open Sans", sans-serif;

line-height: 1.25;

}

  

table {

border: 1px  solid  #ccc;

border-collapse: collapse;

margin: 0;

padding: 0;

width: 100%;

table-layout: fixed;

}

  

table  caption {

font-size: 1.5em;

margin: .5em  0  .75em;

}

  

table  tr {

background-color: #f8f8f8;

border: 1px  solid  #ddd;

padding: .35em;

}

  

table  th,

table  td {

padding: .625em;

text-align: center;

}

  

table  th {

font-size: .85em;

letter-spacing: .1em;

text-transform: uppercase;

}

  

@media  screen  and (max-width: 600px) {

table {

border: 0;

}

  

table  caption {

font-size: 1.3em;

}

  

table  thead {

border: none;

clip: rect(0  0  0  0);

height: 1px;

margin: -1px;

overflow: hidden;

padding: 0;

position: absolute;

width: 1px;

}

  

table  tr {

border-bottom: 3px  solid  #ddd;

display: block;

margin-bottom: .625em;

}

  

table  td {

border-bottom: 1px  solid  #ddd;

display: block;

font-size: .8em;

text-align: right;

}

  

table  td::before {

/*

* aria-label has no advantage, it won't be read inside a table

content: attr(aria-label);

*/

content: attr(data-label);

float: left;

font-weight: bold;

text-transform: uppercase;

}

  

table  td:last-child {

border-bottom: 0;

}

}
```
** Run Mongo DB **
```terminal
sudo mongod
mongo
```
**Run Node **
navigate to the root of the project where server.js lives 
```terminal 
nodemon server.js
```
**Run Angular Server **
Navigate to the angular folder 'public'
```terminal
ng build --watch
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk2NzYyODY3NywtMjAyNDgyNDExMCwzMD
A5NTE3NTcsLTc2NDU2MTk1NiwyMDUxNDM5MTU3XX0=
-->