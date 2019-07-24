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
	title = 'TITLE OF PROJECT app.com';
	num: number;
	randNum: number;
	str: string;
	first_name: string;
	snacks: string[];
	loggedIn: boolean;
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0ODg5NTQyNSwyMDUxNDM5MTU3XX0=
-->