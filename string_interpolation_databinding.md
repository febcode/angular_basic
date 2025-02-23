### **String Interpolation & Data Binding in Angular**  
These are core concepts used to display data and bind properties/events in Angular templates.

---

## **1️⃣ String Interpolation (`{{ }}`)**
String interpolation is used to **bind component properties** to the template. It **only supports one-way binding (from the component to the view).**

### **Example**
#### **Component (`app.component.ts`)**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Task Management App';
  user = 'John Doe';
}
```

#### **Template (`app.component.html`)**
```html
<h1>{{ title }}</h1>
<p>Welcome, {{ user }}!</p>
```
✅ Output:
```
Task Management App
Welcome, John Doe!
```

---

## **2️⃣ Property Binding (`[property]` Binding)**
Property binding is used to **bind component properties** to **DOM properties** in HTML elements.

### **Example**
#### **Component (`app.component.ts`)**
```typescript
buttonDisabled = true;
```

#### **Template (`app.component.html`)**
```html
<button [disabled]="buttonDisabled">Click Me</button>
```
✅ The button will be **disabled** because `buttonDisabled = true`.

---

## **3️⃣ Event Binding (`(event)` Binding)**
Event binding is used to **handle user events** like `click`, `keyup`, `input`, etc.

### **Example**
#### **Component (`app.component.ts`)**
```typescript
clickCount = 0;

increaseCount() {
  this.clickCount++;
}
```

#### **Template (`app.component.html`)**
```html
<button (click)="increaseCount()">Click Me</button>
<p>Button clicked {{ clickCount }} times.</p>
```
✅ Every time the button is clicked, `clickCount` increases.

---

## **4️⃣ Two-Way Binding (`[()]` Banana-in-a-Box)**
Two-way binding allows **data synchronization** between the component and the UI using `ngModel`.

### **Example**
#### **Component (`app.component.ts`)**
```typescript
userName = '';
```

#### **Template (`app.component.html`)**
```html
<input type="text" [(ngModel)]="userName" placeholder="Enter name">
<p>Hello, {{ userName }}!</p>
```
✅ As the user types in the input box, `userName` updates automatically in real-time.

👉 **Make sure you import `FormsModule` in `app.module.ts` for `ngModel` to work.**
```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [FormsModule]
})
```

---

## **🔄 Summary**
| Binding Type        | Syntax             | Direction | Example |
|--------------------|------------------|------------|----------|
| **String Interpolation** | `{{ property }}` | Component → View | `<p>{{ title }}</p>` |
| **Property Binding** | `[property]="value"` | Component → View | `<img [src]="imageUrl">` |
| **Event Binding** | `(event)="handler()"` | View → Component | `<button (click)="increaseCount()">Click</button>` |
| **Two-Way Binding** | `[(ngModel)]="value"` | View ↔ Component | `<input [(ngModel)]="userName">` |

Let me know if you need a deeper explanation of any of these! 🚀
