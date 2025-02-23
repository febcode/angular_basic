🔹 Understanding ng-template, ng-container, ng-content, and ngTemplateOutlet in Angular

These directives help with dynamic content rendering and structural directives in Angular.


---

1️⃣ ng-template

ng-template is an Angular template container that does not render in the DOM unless used with *ngIf, *ngFor, or ngTemplateOutlet.

🔹 Example: Using ng-template with *ngIf

<div *ngIf="showMessage; else elseBlock">
  <p>Welcome to Angular!</p>
</div>

<ng-template #elseBlock>
  <p>Sorry, you are not logged in.</p>
</ng-template>

✅ If showMessage is false, it renders the elseBlock template.


---

2️⃣ ng-container

ng-container is a logical grouping element that helps avoid unnecessary div elements in the DOM.

🔹 Example: Using ng-container with *ngIf

<ng-container *ngIf="isLoggedIn">
  <h1>Welcome, User!</h1>
  <p>Here is your dashboard.</p>
</ng-container>

✅ Without ng-container, you might need an extra div, which isn't ideal for clean HTML.


---

3️⃣ ng-content (Content Projection)

ng-content allows a parent component to pass custom content into a child component.

🔹 Example: Using ng-content in a Child Component

alert-box.component.html

<div class="alert-box">
  <ng-content></ng-content>
</div>

Usage in a Parent Component

<app-alert-box>
  <p>This is a custom alert message!</p>
</app-alert-box>

✅ The content inside <app-alert-box> is injected into <ng-content>.


---

4️⃣ ngTemplateOutlet (Dynamic Template Rendering)

ngTemplateOutlet allows dynamic reusing of ng-template blocks.

🔹 Example: Using ngTemplateOutlet

<ng-template #loading>
  <p>Loading, please wait...</p>
</ng-template>

<ng-template #success>
  <p>Data loaded successfully!</p>
</ng-template>

<ng-container *ngTemplateOutlet="isLoading ? loading : success"></ng-container>

✅ This dynamically switches templates based on isLoading.


---

🚀 Summary Table


---

🔥 Now you can manage Angular templates efficiently!

