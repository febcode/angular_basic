## **🔹 Angular Interceptors**
An **HTTP Interceptor** in Angular is used to intercept and modify **HTTP requests and responses** before they reach the server or the client.

---

## **1️⃣ Why Use an Interceptor?**
✅ **Modify HTTP Requests** – Add headers (e.g., authentication tokens)  
✅ **Handle Errors Globally** – Manage errors in a single place  
✅ **Logging** – Monitor requests and responses  
✅ **Caching** – Improve performance by storing responses  

---

## **2️⃣ Creating an HTTP Interceptor**
To create an interceptor, implement the `HttpInterceptor` interface.

### **🔹 Step 1: Create an Interceptor**
Run the following command:
```sh
ng generate service interceptors/auth
```
OR manually create `auth.interceptor.ts` in the `interceptors` folder.

### **🔹 Step 2: Implement the Interceptor**
#### **`auth.interceptor.ts`**
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Get token from AuthService
    const authToken = this.authService.getToken();

    // Clone the request and add the Authorization header
    const authReq = req.clone({
      setHeaders: {
        Authorization: `Bearer ${authToken}`
      }
    });

    return next.handle(authReq);
  }
}
```

---

## **3️⃣ Register the Interceptor**
You need to register the interceptor in the `app.module.ts`.

#### **`app.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { AppComponent } from './app.component';
import { AuthInterceptor } from './interceptors/auth.interceptor';
import { AuthService } from './services/auth.service';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [
    AuthService,
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
✅ **The interceptor is now active for all HTTP requests.**

---

## **4️⃣ Handling Errors in an Interceptor**
You can catch and handle HTTP errors inside the interceptor.

#### **Modify `auth.interceptor.ts` to include error handling**
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          console.error('Unauthorized request - Redirect to login');
        } else if (error.status === 500) {
          console.error('Internal server error - Try again later');
        }
        return throwError(() => new Error(error.message));
      })
    );
  }
}
```
✅ **Now, the interceptor will handle errors globally.**

---

## **5️⃣ Logging Requests and Responses**
You can log all HTTP requests and responses using an interceptor.

#### **Modify `logger.interceptor.ts`**
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable, tap } from 'rxjs';

@Injectable()
export class LoggerInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    console.log(`Request: ${req.method} ${req.url}`);

    return next.handle(req).pipe(
      tap(event => {
        console.log('Response:', event);
      })
    );
  }
}
```

✅ **This will log all HTTP requests and responses in the console.**

---

## **🔄 Summary**
| Feature | Purpose |
|---------|---------|
| **Authorization Token** | Attach JWT token to requests |
| **Error Handling** | Catch and process errors globally |
| **Logging** | Track all HTTP requests and responses |
| **Caching** | Store frequently used responses |

---
