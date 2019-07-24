##  New Angular Project 

 1. [ ] navigate into the folder directory you want your new app to be saved in:
``` terminal 
ng new public
```

 2. [ ] Yes to Routing
 3. [ ] Select CSS
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
**http:service.ts**
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwODUyMjY3NDEsLTc2NDU2MTk1NiwyMD
UxNDM5MTU3XX0=
-->