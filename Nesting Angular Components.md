


## Nesting Angular Components

### Build new component
```console
ng generate component {{{component_name}}} - task
```
app tree:
├── app.component.css
├── app.component.html
├── app.component.spec.ts
├── app.component.ts
├── app.module.ts
├── task
│   ├── task.component.css
│   ├── task.component.html
│   ├── task.component.spec.ts
│   └── task.component.ts
├── tasks.service.spec.ts
└── tasks.service.ts

##### app.component.html
* add the selector for the nested component in the root component html
* *ngIf to control display
* `[task_to_show]` (nested variable) collects value from `"selectedTask"` (root variable) __note__: this is a one way connection
```html
...
<app-task *ngIf="selectedTask" [taskToShow]="selectedTask"></app-task>
...
```

##### /task/task.component.ts
```javascript
import { Component, OnInit, Input } from '@angular/core'; // add Input to our imports
@Component({
  selector: 'app-task',
  templateUrl: './task.component.html',
  stylesUrls: ['./task.component.css']
})
export class TaskComponent implements OnInit {
  @Input() taskToShow: any;  // use the @Input decorator to indicate this comes from the parent
  constructor() { }
  ngOninit() { }
}
```

##### task/task.component.html
```html
<h6>{{taskToShow.title}}</h6>
<p>{{taskToShow.description}}</p>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjAzNjk1MTczXX0=
-->
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzgwNjk3NzY0XX0=
-->