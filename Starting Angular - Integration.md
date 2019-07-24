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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzAwNzU4NzksLTc2NDU2MTk1NiwyMD
UxNDM5MTU3XX0=
-->