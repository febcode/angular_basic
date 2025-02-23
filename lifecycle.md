# Component Lifecycle
|Phase |	Method |	Summary |
|------|----------|----------|
|Creation|	constructor |	Standard JavaScript class constructor . Runs when Angular instantiates the component. |
|Change Detection |ngOnInit	|Runs once after Angular has initialized all the component's inputs.|
||ngOnChanges|	Runs every time the component's inputs have changed.|
||ngDoCheck	|Runs every time this component is checked for changes.|
||ngAfterContentInit |	Runs once after the component's content has been initialized.|
||ngAfterContentChecked |	Runs every time this component content has been checked for changes.|
||ngAfterViewInit |	Runs once after the component's view has been initialized.|
||ngAfterViewChecked |	Runs every time the component's view has been checked for changes. |
|Rendering	| afterNextRender |	Runs once the next time that all components have been rendered to the DOM.|
| |afterRender	| Runs every time all components have been rendered to the DOM.|
| Destruction|	ngOnDestroy	| Runs once before the component is destroyed. |


### **Angular Lifecycle Hooks**
Angular provides **lifecycle hooks** that allow developers to respond to different stages of a component’s lifecycle. These hooks are methods that Angular calls at specific moments during the component’s existence.

---

### **1️⃣ List of Angular Lifecycle Hooks**
| Hook | Description |
|------|------------|
| `ngOnChanges()` | Called when input properties change. |
| `ngOnInit()` | Called once after the component is initialized. |
| `ngDoCheck()` | Runs on every change detection cycle. |
| `ngAfterContentInit()` | Called after content projection (`<ng-content>`) is initialized. |
| `ngAfterContentChecked()` | Runs after content projection is checked. |
| `ngAfterViewInit()` | Runs after the component's views (child components) are initialized. |
| `ngAfterViewChecked()` | Runs after the component's views are checked. |
| `ngOnDestroy()` | Called just before the component is destroyed. Used for cleanup. |

---

### **2️⃣ Example: Lifecycle Hooks in a Component**
```typescript
import { Component, OnInit, OnChanges, DoCheck, AfterViewInit, OnDestroy, Input } from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `<h3>Lifecycle Hooks Demo</h3>`,
})
export class LifecycleDemoComponent implements OnInit, OnChanges, DoCheck, AfterViewInit, OnDestroy {
  
  @Input() inputData: string = ''; // Trigger ngOnChanges when changed

  constructor() {
    console.log('Constructor: Component is being created');
  }

  ngOnChanges() {
    console.log('ngOnChanges: Input property changed');
  }

  ngOnInit() {
    console.log('ngOnInit: Component initialized');
  }

  ngDoCheck() {
    console.log('ngDoCheck: Change detection cycle triggered');
  }

  ngAfterViewInit() {
    console.log('ngAfterViewInit: View initialized');
  }

  ngOnDestroy() {
    console.log('ngOnDestroy: Cleanup before component is destroyed');
  }
}
```

---

### **3️⃣ When to Use Each Hook?**
| Hook | When to Use |
|------|------------|
| `ngOnChanges()` | When working with `@Input()` properties. |
| `ngOnInit()` | When initializing data after component creation. |
| `ngDoCheck()` | When you need custom change detection logic. |
| `ngAfterViewInit()` | When interacting with child components or `ViewChild`. |
| `ngOnDestroy()` | When unsubscribing from Observables, clearing intervals, etc. |


### **Real-World Example: Lifecycle Hooks in a Task Management App**  

In a **Task Management App**, we can use **Angular lifecycle hooks** for various purposes, such as:  
- **Initializing tasks (`ngOnInit`)**  
- **Detecting input changes (`ngOnChanges`)**  
- **Performing cleanup (`ngOnDestroy`)**  

---

### **📌 Example: Task List Component with Lifecycle Hooks**  
#### `task-list.component.ts`
```typescript
import { Component, Input, Output, EventEmitter, OnInit, OnChanges, DoCheck, AfterViewInit, OnDestroy, SimpleChanges } from '@angular/core';
import { TaskService } from '../services/task.service';

@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  styleUrls: ['./task-list.component.css']
})
export class TaskListComponent implements OnInit, OnChanges, DoCheck, AfterViewInit, OnDestroy {
  
  @Input() tasks: any[] = [];
  @Output() taskUpdated = new EventEmitter<any>();

  constructor(private taskService: TaskService) {
    console.log('Constructor: TaskListComponent created');
  }

  /** Called when @Input() `tasks` changes */
  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges: Input changed', changes);
  }

  /** Called once after the component initializes */
  ngOnInit(): void {
    console.log('ngOnInit: Fetching tasks...');
    this.taskService.getTasks().subscribe(tasks => {
      this.tasks = tasks;
    });
  }

  /** Called on every change detection cycle */
  ngDoCheck(): void {
    console.log('ngDoCheck: Change detection cycle triggered');
  }

  /** Called after the component’s view is initialized */
  ngAfterViewInit(): void {
    console.log('ngAfterViewInit: Task List View Initialized');
  }

  /** Called before the component is destroyed */
  ngOnDestroy(): void {
    console.log('ngOnDestroy: Cleaning up resources');
  }

  /** Simulate an update action */
  updateTask(task: any) {
    task.status = 'Completed';
    task.updatedDate = new Date();
    this.taskUpdated.emit(task);
  }
}
```

---

### **📌 Example: Task Service (`task.service.ts`)**
Handles fetching and managing tasks.  
```typescript
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class TaskService {
  
  private tasks = [
    { id: 1, name: 'Design UI', description: 'Create wireframes', status: 'Pending', assignee: 'Alice', createdDate: new Date(), updatedDate: new Date() },
    { id: 2, name: 'Develop API', description: 'Build REST endpoints', status: 'In Progress', assignee: 'Bob', createdDate: new Date(), updatedDate: new Date() }
  ];

  getTasks(): Observable<any[]> {
    return of(this.tasks);
  }
}
```

---

### **📌 Example: Task List UI (`task-list.component.html`)**
```html
<h2>Task List</h2>
<ul>
  <li *ngFor="let task of tasks">
    {{ task.name }} - {{ task.status }} - {{ task.assignee }}
    <button (click)="updateTask(task)">Mark as Completed</button>
  </li>
</ul>
```

---

### **🔹 How Lifecycle Hooks Work Here**
| Hook | What It Does in the Task App |
|------|------------------------------|
| `ngOnChanges()` | Detects changes in `@Input() tasks` from the parent. |
| `ngOnInit()` | Fetches tasks from `TaskService`. |
| `ngDoCheck()` | Detects any manual changes to tasks (useful if not using `OnPush` change detection). |
| `ngAfterViewInit()` | Confirms the view is initialized (useful for animations or `ViewChild`). |
| `ngOnDestroy()` | Cleans up resources before the component is destroyed. |

---

### **✨ What You Get From This?**
✔️ **Proper task initialization** (`ngOnInit`)  
✔️ **Detect input changes** (`ngOnChanges`)  
✔️ **Change detection on updates** (`ngDoCheck`)  
✔️ **Prevent memory leaks** (`ngOnDestroy`)  

