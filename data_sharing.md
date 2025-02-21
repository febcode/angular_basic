In Angular, there are multiple ways to share data between components. 
The method you choose depends on whether the components have a **parent-child** relationship or are unrelated.

---

## **1. Parent to Child (Using `@Input()`)**
If you want to pass data from a **parent component** to a **child component**, use `@Input()`.

### **Example**
#### **Parent Component (`app.component.ts`)**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<app-task-list [tasks]="taskList"></app-task-list>`,
})
export class AppComponent {
  taskList = [
    { id: 1, name: 'Task 1', status: 'Pending' },
    { id: 2, name: 'Task 2', status: 'Completed' },
  ];
}
```

#### **Child Component (`task-list.component.ts`)**
```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-task-list',
  template: `<ul><li *ngFor="let task of tasks">{{ task.name }} - {{ task.status }}</li></ul>`,
})
export class TaskListComponent {
  @Input() tasks: any[] = [];
}
```

✅ Now, `task-list` receives `taskList` from `app.component.ts`.

---

## **2. Child to Parent (Using `@Output()` and `EventEmitter`)**
To send data **from child to parent**, use `@Output()`.

### **Example**
#### **Child Component (`task-list.component.ts`)**
```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-task-list',
  template: `<button (click)="sendData()">Send Task to Parent</button>`,
})
export class TaskListComponent {
  @Output() taskEvent = new EventEmitter<string>();

  sendData() {
    this.taskEvent.emit('New Task from Child');
  }
}
```

#### **Parent Component (`app.component.ts`)**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `<app-task-list (taskEvent)="receiveTask($event)"></app-task-list>`,
})
export class AppComponent {
  receiveTask(task: string) {
    console.log('Received from child:', task);
  }
}
```

✅ The parent receives data when the button in the child component is clicked.

---

## **3. Sharing Between Unrelated Components (Using a Service)**
If components don’t have a direct relationship (like **siblings** or **separate parts of the app**), use a **service with RxJS `BehaviorSubject`**.

### **Create a Shared Service (`shared.service.ts`)**
```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class SharedService {
  private taskSource = new BehaviorSubject<string>('Default Task');
  currentTask = this.taskSource.asObservable();

  updateTask(task: string) {
    this.taskSource.next(task);
  }
}
```

### **Component 1 (Sender)**
```typescript
import { Component } from '@angular/core';
import { SharedService } from '../services/shared.service';

@Component({
  selector: 'app-sender',
  template: `<button (click)="changeTask()">Change Task</button>`,
})
export class SenderComponent {
  constructor(private sharedService: SharedService) {}

  changeTask() {
    this.sharedService.updateTask('Task Updated from Sender');
  }
}
```

### **Component 2 (Receiver)**
```typescript
import { Component, OnInit } from '@angular/core';
import { SharedService } from '../services/shared.service';

@Component({
  selector: 'app-receiver',
  template: `<p>Received Task: {{ task }}</p>`,
})
export class ReceiverComponent implements OnInit {
  task: string = '';

  constructor(private sharedService: SharedService) {}

  ngOnInit() {
    this.sharedService.currentTask.subscribe(task => this.task = task);
  }
}
```

✅ Now, when **`SenderComponent`** updates the task, **`ReceiverComponent`** gets the updated value.

---

### **Summary**
| **Method**        | **Use Case**                                      |
|------------------|------------------------------------------------|
| `@Input()`       | Parent → Child data transfer                    |
| `@Output()`      | Child → Parent event communication              |
| Service + RxJS   | Unrelated components (global state management)  |

