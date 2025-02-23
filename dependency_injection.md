### **Dependency Injection (DI) in Angular 🚀**  

Dependency Injection (DI) is a **design pattern** used in Angular to manage dependencies efficiently by **injecting** them instead of manually creating instances.  

---

## **1️⃣ Why Use Dependency Injection?**
✅ Reduces code duplication.  
✅ Makes services **reusable** across components.  
✅ Improves **testability** and maintainability.  

---

## **2️⃣ Example of Dependency Injection in Angular**  
Let’s say we have a **TaskService** that manages tasks.

### **Step 1: Create a Service (Dependency)**
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // ✅ This makes the service available globally
})
export class TaskService {
  getTasks() {
    return [
      { id: 1, name: 'Task 1', status: 'Pending' },
      { id: 2, name: 'Task 2', status: 'Completed' }
    ];
  }
}
```
💡 `@Injectable({ providedIn: 'root' })` tells Angular to **automatically provide** this service across the app.

---

### **Step 2: Inject Service into a Component**
Now, let's use **TaskService** inside a component.

```typescript
import { Component, OnInit } from '@angular/core';
import { TaskService } from '../services/task.service';

@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  styleUrls: ['./task-list.component.css']
})
export class TaskListComponent implements OnInit {
  tasks: any[] = [];

  // ✅ Inject TaskService in the constructor
  constructor(private taskService: TaskService) {}

  ngOnInit(): void {
    this.tasks = this.taskService.getTasks();  // ✅ Get data from service
  }
}
```

---

## **3️⃣ Providing Dependencies in Different Scopes**
There are **three ways** to provide dependencies:

### **(A) Global Scope (Recommended)**
✅ Use `providedIn: 'root'` in the service.  
```typescript
@Injectable({
  providedIn: 'root'
})
export class TaskService {}
```
🔹 This registers the service **once** at the root level.

---

### **(B) Module-Level Scope**
If you want to provide a service **only in a specific module**, use the `providers` array in `app.module.ts`:

```typescript
import { TaskService } from './services/task.service';

@NgModule({
  providers: [TaskService]  // ✅ Available only in this module
})
export class AppModule {}
```

---

### **(C) Component-Level Scope**
To provide a service **only to a specific component**, use `providers` in the component decorator:

```typescript
@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  providers: [TaskService]  // ✅ This creates a new instance for this component only
})
export class TaskListComponent {}
```
⚠️ **Warning:** This will create a **new instance** of `TaskService`, isolating it from the rest of the app.

---

## **4️⃣ Injecting Other Dependencies**
### **(A) Injecting HTTP Client**
To use **API calls**, inject `HttpClient` inside a service:

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TaskService {
  constructor(private http: HttpClient) {}

  fetchTasks() {
    return this.http.get('https://jsonplaceholder.typicode.com/todos');
  }
}
```

---

### **(B) Injecting a Custom Service**
If `SharedService` is another service, inject it into `TaskService`:

```typescript
import { Injectable } from '@angular/core';
import { SharedService } from './shared.service';

@Injectable({
  providedIn: 'root'
})
export class TaskService {
  constructor(private sharedService: SharedService) {}

  shareTask(taskName: string) {
    this.sharedService.updateTask(taskName);
  }
}
```

---

## **5️⃣ Injecting Dependencies Manually with `@Inject()`**
In some cases, you may need to inject dependencies manually using `@Inject()`:

```typescript
import { Inject, Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LoggerService {
  constructor(@Inject('AppName') private appName: string) {
    console.log(`Logging for ${appName}`);
  }
}
```
You would then **provide the value** like this:

```typescript
providers: [{ provide: 'AppName', useValue: 'Task Manager App' }]
```

---

## **6️⃣ Summary**
🔹 **Dependency Injection** in Angular makes code more modular and testable.  
🔹 Use `@Injectable({ providedIn: 'root' })` to **register services globally**.  
🔹 Inject services into components via the **constructor**.  
🔹 `providers: [TaskService]` can be used at **module or component level** for scoping.  
🔹 Use `@Inject()` for **manual injection** when needed.

---

