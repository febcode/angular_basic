### **Change Detection in Angular** 🚀  

Change detection in Angular determines when and how the UI updates in response to data changes.  

---

## **1️⃣ Change Detection Strategies**  
Angular provides two strategies for detecting changes:

1. **Default (`ChangeDetectionStrategy.Default`)**  
   - Runs automatically whenever any data bound to the template changes.  
   - Detects **all** changes, even if they don’t impact the component.  
   - More expensive in terms of performance.

2. **OnPush (`ChangeDetectionStrategy.OnPush`)**  
   - Runs only when **Input properties** change or an `Observable` emits new values.  
   - Improves performance by avoiding unnecessary re-renders.

---

## **2️⃣ Using `ChangeDetectionStrategy.OnPush` for Performance Optimization**
You can improve performance by setting change detection to **OnPush**:

```typescript
import { ChangeDetectionStrategy, Component, Input } from '@angular/core';

@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  styleUrls: ['./task-list.component.css'],
  changeDetection: ChangeDetectionStrategy.OnPush  // ✅ Use OnPush strategy
})
export class TaskListComponent {
  @Input() tasks: any[] = [];
}
```

With `OnPush`, Angular will **only** update when:  
✅ The `tasks` array reference changes (not just the values).  
✅ An `Observable` emits a new value.  
✅ A UI event (click, input, etc.) occurs.  

---

## **3️⃣ Forcing Change Detection Manually**  
If Angular **doesn’t detect changes**, you can trigger it manually using `ChangeDetectorRef`:

```typescript
import { ChangeDetectorRef, Component } from '@angular/core';

@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  styleUrls: ['./task-list.component.css']
})
export class TaskListComponent {
  tasks: any[] = [];

  constructor(private cdr: ChangeDetectorRef) {}

  addTask(newTask: any) {
    this.tasks.push(newTask);
    this.cdr.detectChanges();  // ✅ Manually trigger change detection
  }
}
```

---

## **4️⃣ `markForCheck()` vs. `detectChanges()`**
🔹 **`markForCheck()`** – Marks the component **and its ancestors** for change detection in the next cycle.  
🔹 **`detectChanges()`** – Triggers **immediate** change detection in the component.  

```typescript
this.cdr.markForCheck();  // ✅ Updates in next cycle
this.cdr.detectChanges(); // ✅ Updates immediately
```

Use `markForCheck()` with `OnPush` when updates are **asynchronous** (e.g., after an API call).

---

## **5️⃣ When to Use `OnPush`?**
✅ When using `@Input()` properties.  
✅ When working with immutable objects (`{ ...task }`).  
✅ When dealing with `Observables` (RxJS).  
✅ When optimizing performance in **large applications**.  

---

## **6️⃣ Example: `OnPush` with Immutable Updates**
Since `OnPush` detects changes **only if the reference changes**, modify the array like this:

```typescript
editTask(updatedTask: any): void {
  this.tasks = this.tasks.map(task => 
    task.id === updatedTask.id ? { ...task, name: updatedTask.name } : task
  );
}
```

This **replaces** the `tasks` array reference, ensuring change detection triggers.

---

### **🔥 TL;DR**
- **Default change detection** → Runs on every change.  
- **OnPush** → Only updates when inputs change.  
- **Use `ChangeDetectorRef.detectChanges()`** → Forces immediate updates.  
- **Use `markForCheck()`** → Schedules updates in the next cycle.  
- **Use immutable updates** (`{ ...task }`) to ensure `OnPush` detects changes.  

Would you like a **working example** of OnPush in your project? 🚀
