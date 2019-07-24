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
	HttpClientModule],

providers: [HttpService],

bootstrap: [AppComponent]

})

export class AppModule { }

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTAzNjU2MDUsMjA1MTQzOTE1N119
-->