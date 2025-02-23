### **Angular Directives**
Directives in Angular are used to modify the **DOM structure, behavior, or appearance** of elements.

---

## **1️⃣ Types of Directives**
There are **three** main types of directives in Angular:

| Type                  | Description | Example |
|----------------------|------------|---------|
| **Structural Directives** | Change the DOM structure (add/remove elements) | `*ngIf`, `*ngFor`, `*ngSwitch` |
| **Attribute Directives** | Change the appearance or behavior of an element | `ngClass`, `ngStyle`, `customDirective` |
| **Custom Directives** | Create your own directives | `@Directive()` |

---

## **2️⃣ Structural Directives**
Structural directives start with `*` and modify the DOM structure.

### **🔹 `*ngIf` (Conditionally Show Elements)**
```html
<p *ngIf="isLoggedIn">Welcome back!</p>
<p *ngIf="!isLoggedIn">Please log in.</p>
```
#### **Component (`app.component.ts`)**
```typescript
isLoggedIn = false;
```
✅ If `isLoggedIn` is `true`, "Welcome back!" is shown; otherwise, "Please log in." appears.

---

### **🔹 `*ngFor` (Loop Through Data)**
```html
<ul>
  <li *ngFor="let task of tasks">{{ task }}</li>
</ul>
```
#### **Component (`app.component.ts`)**
```typescript
tasks = ['Task 1', 'Task 2', 'Task 3'];
```
✅ The `*ngFor` loop iterates over `tasks` and renders each item as a `<li>`.

---

### **🔹 `*ngSwitch` (Multiple Conditions)**
```html
<div [ngSwitch]="userRole">
  <p *ngSwitchCase="'admin'">Admin Dashboard</p>
  <p *ngSwitchCase="'user'">User Dashboard</p>
  <p *ngSwitchDefault>Guest Dashboard</p>
</div>
```
#### **Component (`app.component.ts`)**
```typescript
userRole = 'user';
```
✅ If `userRole = 'user'`, "User Dashboard" will be displayed.

---

## **3️⃣ Attribute Directives**
Attribute directives modify the behavior or styling of elements.

### **🔹 `ngClass` (Dynamically Add CSS Classes)**
```html
<p [ngClass]="{ 'active': isActive, 'disabled': !isActive }">This is a paragraph</p>
```
#### **Component (`app.component.ts`)**
```typescript
isActive = true;
```
✅ If `isActive = true`, the `active` class is applied.

---

### **🔹 `ngStyle` (Dynamically Set Styles)**
```html
<p [ngStyle]="{ 'color': isActive ? 'green' : 'red' }">Styled Text</p>
```
✅ The text color changes based on the value of `isActive`.

---

## **4️⃣ Custom Directives**
You can create custom directives using `@Directive()`.

### **Example: Highlight Directive**
#### **Directive (`highlight.directive.ts`)**
```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.el.nativeElement.style.backgroundColor = 'transparent';
  }
}
```
#### **Usage in Template (`app.component.html`)**
```html
<p appHighlight>Hover over me to see the effect!</p>
```
✅ The background turns **yellow** on hover.

---

## **🔄 Summary**
| Directive Type         | Example                 | Purpose |
|-----------------------|------------------------|---------|
| **Structural**        | `*ngIf`, `*ngFor`, `*ngSwitch` | Modify DOM structure |
| **Attribute**         | `[ngClass]`, `[ngStyle]` | Modify element behavior/style |
| **Custom**           | `@Directive()` | Create custom behavior |

Let me know if you need further explanation! 🚀
