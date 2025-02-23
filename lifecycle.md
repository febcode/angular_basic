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

Would you like a **real-world example** of lifecycle hooks in a task management app? 🚀
