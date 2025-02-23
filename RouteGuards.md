## **🔹 Angular Route Guards**
Angular **Route Guards** help protect routes from unauthorized access. They allow you to control navigation based on conditions like authentication, user roles, or unsaved changes.

---

## **1️⃣ Types of Route Guards**
| **Guard** | **Purpose** |
|-----------|------------|
| `CanActivate` | Prevents unauthorized access to a route |
| `CanActivateChild` | Restricts access to child routes |
| `CanDeactivate` | Prevents navigation away if unsaved changes exist |
| `Resolve` | Fetches data before activating a route |
| `CanLoad` | Restricts lazy-loaded module loading |

---

## **2️⃣ Implementing `CanActivate` (Authentication Guard)**  
This guard ensures users are authenticated before they can access specific routes.

### **🔹 Step 1: Create a Guard**
Run the command:
```sh
ng generate guard guards/auth
```
OR manually create `auth.guard.ts` in the `guards` folder.

### **🔹 Step 2: Implement `CanActivate` in `auth.guard.ts`**
```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    } else {
      this.router.navigate(['/login']); // Redirect if not logged in
      return false;
    }
  }
}
```
✅ **This guard checks if the user is authenticated. If not, it redirects them to `/login`.**

---

## **3️⃣ Using `CanActivate` in Routes**
Apply the guard to protected routes.

#### **Modify `app.routes.ts`**
```typescript
import { Routes } from '@angular/router';
import { DashboardComponent } from './dashboard/dashboard.component';
import { LoginComponent } from './login/login.component';
import { AuthGuard } from './guards/auth.guard';

export const routes: Routes = [
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
  { path: 'login', component: LoginComponent },
  { path: '**', redirectTo: 'login' } // Redirect unknown routes
];
```
✅ **Now, users must be authenticated to access `/dashboard`.**

---

## **4️⃣ Implementing `CanActivateChild` (Protecting Child Routes)**
Use this to protect nested routes.

#### **Modify `auth.guard.ts`**
```typescript
import { CanActivateChild } from '@angular/router';

export class AuthGuard implements CanActivate, CanActivateChild {
  canActivateChild(): boolean {
    return this.canActivate();
  }
}
```
#### **Apply it to child routes in `app.routes.ts`**
```typescript
{
  path: 'admin',
  component: AdminComponent,
  canActivate: [AuthGuard],
  canActivateChild: [AuthGuard],
  children: [
    { path: 'users', component: UserListComponent },
    { path: 'settings', component: SettingsComponent }
  ]
}
```
✅ **Both `users` and `settings` will be protected.**

---

## **5️⃣ Implementing `CanDeactivate` (Prevent Navigation if Unsaved Changes Exist)**
Use this to prevent users from leaving a page if they have unsaved changes.

#### **Create `unsaved-changes.guard.ts`**
```typescript
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { Observable } from 'rxjs';

export interface CanComponentDeactivate {
  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

@Injectable({
  providedIn: 'root'
})
export class UnsavedChangesGuard implements CanDeactivate<CanComponentDeactivate> {
  canDeactivate(component: CanComponentDeactivate): boolean {
    return component.canDeactivate ? component.canDeactivate() : true;
  }
}
```

#### **Apply it to a Component (`edit-profile.component.ts`)**
```typescript
export class EditProfileComponent implements CanComponentDeactivate {
  unsavedChanges = true;

  canDeactivate(): boolean {
    return this.unsavedChanges || confirm('You have unsaved changes. Leave anyway?');
  }
}
```
#### **Apply the Guard to a Route in `app.routes.ts`**
```typescript
{ path: 'edit-profile', component: EditProfileComponent, canDeactivate: [UnsavedChangesGuard] }
```
✅ **Now, users will get a warning if they try to leave without saving.**

---

## **6️⃣ Implementing `Resolve` (Fetching Data Before Route Activation)**
Use this to fetch data **before** loading a route.

#### **Create `user-resolver.service.ts`**
```typescript
import { Injectable } from '@angular/core';
import { Resolve } from '@angular/router';
import { UserService } from '../services/user.service';

@Injectable({
  providedIn: 'root'
})
export class UserResolver implements Resolve<any> {
  constructor(private userService: UserService) {}

  resolve() {
    return this.userService.getUserData(); // Fetch user data before navigation
  }
}
```
#### **Apply the Resolver to a Route in `app.routes.ts`**
```typescript
{ path: 'profile', component: ProfileComponent, resolve: { user: UserResolver } }
```
✅ **Now, the `ProfileComponent` will receive user data before loading.**

---

## **7️⃣ Implementing `CanLoad` (Restrict Lazy-Loaded Modules)**
Use this to **prevent** a module from loading until a condition is met.

#### **Modify `auth.guard.ts`**
```typescript
import { CanLoad, Route, UrlSegment } from '@angular/router';

export class AuthGuard implements CanActivate, CanLoad {
  canLoad(route: Route, segments: UrlSegment[]): boolean {
    return this.authService.isAuthenticated();
  }
}
```
#### **Apply it to a Lazy-Loaded Module in `app.routes.ts`**
```typescript
{ path: 'admin', loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule), canLoad: [AuthGuard] }
```
✅ **This prevents unauthorized users from even downloading the `AdminModule`.**

---

## **🔄 Summary of Route Guards**
| **Guard** | **Purpose** |
|-----------|------------|
| `CanActivate` | Protects a route based on conditions (e.g., authentication) |
| `CanActivateChild` | Protects child routes |
| `CanDeactivate` | Prevents navigation if conditions aren’t met (e.g., unsaved changes) |
| `Resolve` | Fetches data before activating a route |
| `CanLoad` | Prevents lazy-loaded modules from loading |

---
