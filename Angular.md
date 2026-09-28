## Angular Architecture

 Angular follows a **component-based architecture** where an application is divided into reusable components, services, modules/configuration, and other building blocks. The architecture promotes **separation of concerns, reusability, maintainability, and testability**.

 ### 1\. High-Level Angular Architecture

 A typical Angular application can be viewed as:

```
Angular Application
│
├── Components
│   ├── Template (HTML)
│   ├── Class (TypeScript)
│   └── Styles (CSS/SCSS)
│
├── Services
│   └── Business logic / API calls / shared functionality
│
├── Dependency Injection
│   └── Provides services to components
│
├── Routing
│   └── Navigation between views
│
├── Directives
│   └── Modify DOM behavior/structure
│
├── Pipes
│   └── Transform data for display
│
└── Models / Interfaces
    └── Define application data structures
```

---

 ## 2\. Components

 **Components are the fundamental building blocks of an Angular UI.**

 A component generally consists of:

 - **TypeScript class** — contains component logic and state.
- **HTML template** — defines what is displayed.
- **CSS/SCSS** — defines presentation.
- **Metadata** — tells Angular how to use the component.

 Example:

```
@Component({
  selector: 'app-user',
  template: `<h2>{{ name }}</h2>`
})
export class UserComponent {
  name = 'John';
}
```

 Here:

```
UserComponent
     │
     ├── TypeScript → Logic
     ├── HTML       → View
     └── CSS        → Styling
```

 Components communicate with each other using mechanisms such as **input/output bindings** and shared services.

---

 ## 3\. Templates

 Angular templates are HTML enhanced with Angular features such as:

 - Interpolation: `{{ userName }}`
- Property binding: `[value]="userName"`
- Event binding: `(click)="save()"`
- Two-way binding: `[(ngModel)]="userName"`
- Control flow: `@if`, `@for`, etc.

 Example:

```
<h1>{{ title }}</h1>
<button (click)="save()">Save</button>
```

 The template represents the **view layer** of the component.

---

 ## 4\. Services

 Services contain **reusable application logic** that should not normally be placed directly inside components.

 Common responsibilities include:

 - Calling REST APIs
- Authentication
- Logging
- Sharing data
- Business logic
- State-related operations

 Example:

```
@Injectable({
  providedIn: 'root'
})
export class UserService {
  getUsers() {
    return this.http.get('/api/users');
  }
}
```

 A component can consume this service through **Dependency Injection**.

 ### Interview point

 > Components should primarily manage UI-related concerns, while reusable business or data-access logic can be moved into services.

---

 ## 5\. Dependency Injection (DI)

 Angular has a built-in **Dependency Injection system**.

 Instead of a component creating its own dependencies:

```
const service = new UserService();
```

 Angular creates and provides the dependency:

```
constructor(private userService: UserService) {}
```

 Benefits:

 - Loose coupling
- Easier unit testing
- Better reusability
- Centralized dependency management

---

 ## 6\. Directives

 Directives allow Angular to **change the behavior or appearance of DOM elements**.

 There are three broad categories:

 ### Component directives

 Components are technically directives with a template.

 ### Attribute directives

 Change the behavior or appearance of an existing element.

 Examples:

```
<div [ngClass]="classes"></div>
```

 ### Structural/control-flow directives

 Control whether and how elements are rendered. Modern Angular commonly uses built-in control-flow syntax:

```
@if (isLoggedIn) {
  <p>Welcome</p>
}

@for (user of users; track user.id) {
  <p>{{ user.name }}</p>
}
```

---

 ## 7\. Pipes

 Pipes transform data for display in templates.

 Example:

```
<p>{{ price | currency }}</p>
<p>{{ name | uppercase }}</p>
```

 Angular provides built-in pipes such as:

 - `date`
- `currency`
- `uppercase`
- `lowercase`
- `json`

 You can also create **custom pipes**.

---

 ## 8\. Routing

 Angular Router provides navigation between different views/components without requiring a full browser page reload.

 Example:

```
export const routes: Routes = [
  { path: 'users', component: UsersComponent },
  { path: 'users/:id', component: UserDetailsComponent }
];
```

 A URL such as:

```
/users/10
```

 can display `UserDetailsComponent` for user `10`.

 Important routing concepts include:

 - Route parameters
- Child routes
- Route guards
- Lazy loading
- Resolvers
- Navigation

---

 ## 9\. Modules vs Modern Angular

 This is an important **interview topic**.

 Older Angular applications commonly organized functionality using **NgModules**:

```
AppModule
 ├── Components
 ├── Directives
 ├── Pipes
 └── Services
```

 Modern Angular supports and encourages **standalone components**, which can reduce the need for NgModules.

 Example:

```
@Component({
  selector: 'app-user',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './user.html'
})
export class UserComponent {}
```

 So, in an interview, avoid saying **"every Angular application must use NgModules."** Modern Angular can be built using standalone components and related standalone APIs.

---

 ## 10\. Data Flow

 Angular supports several ways of moving data through an application.

 For parent → child communication:

```
Parent
  │
  │ @Input
  ▼
Child
```

 For child → parent communication:

```
Child
  │
  │ output/event
  ▼
Parent
```

 For unrelated components, a **shared service** or an appropriate state-management approach can be used.

---

 ## 1 HTTP and API Communication

 Angular provides `HttpClient` for communicating with backend APIs.

 Typical flow:

```
Component
    ↓
Service
    ↓
HttpClient
    ↓
REST API
    ↓
Backend
```

 Example:

```
getUsers() {
  return this.http.get<User[]>('/api/users');
}
```

 This separation keeps API-related logic out of the UI component.

---

 ## 12\. Change Detection

 Angular needs to determine when the application's state has changed and update the DOM accordingly.

 Conceptually:

```
State changes
     ↓
Angular detects changes
     ↓
Template is updated
     ↓
DOM reflects new state
```

 Modern Angular also provides **Signals**, which enable fine-grained reactive state management and can help Angular track dependencies more precisely.

 Example:

```
count = signal(0);

increment() {
  this.count.update(value => value + 1);
}
```

 Template:

```
<p>{{ count() }}</p>
```

---

 ## 13\. Typical Angular Project Structure

 A project might be organized like:

```
src/
├── app/
│   ├── core/
│   │   ├── services/
│   │   ├── guards/
│   │   └── interceptors/
│   │
│   ├── shared/
│   │   ├── components/
│   │   ├── directives/
│   │   └── pipes/
│   │
│   ├── features/
│   │   ├── users/
│   │   └── products/
│   │
│   ├── app.component.ts
│   └── app.routes.ts
│
├── assets/
└── main.ts
```

 This is an organizational convention rather than a mandatory Angular architecture.

 **Component → Template → Service → DI → Routing → Directives → Pipes → HttpClient → Change Detection → Signals → Standalone Components**

 ## Angular Component Lifecycle

 The **Angular component lifecycle** is the sequence of stages a component goes through from **creation to destruction**.

 Angular provides **lifecycle hooks** that allow us to execute code at specific points in a component's life.

 ### Lifecycle flow

```
Component Created
       ↓
constructor()
       ↓
ngOnChanges()
       ↓
ngOnInit()
       ↓
ngDoCheck()
       ↓
ngAfterContentInit()
       ↓
ngAfterContentChecked()
       ↓
ngAfterViewInit()
       ↓
ngAfterViewChecked()
       ↓
   Component runs
       ↓
ngOnDestroy()
       ↓
Component Destroyed
```

 > Not every hook runs in exactly this sequence on every change-detection cycle. Some hooks run only once, while others run repeatedly.

---

 ## 1\. `constructor()`

 The TypeScript constructor runs when the class is instantiated.

 Main purpose:

 - Dependency injection
- Basic class initialization

```
constructor(private userService: UserService) {}
```

 ### Interview point

 Don't use `constructor()` for Angular initialization logic that depends on inputs or the initialized view.

---

 ## 2\. `ngOnChanges()`

 Called when an **input property** (`@Input`) changes.

```
@Input() userId!: number;

ngOnChanges(changes: SimpleChanges) {
  console.log(changes);
}
```

 For example:

```
Parent
  ↓
@Input() userId
  ↓
Child
  ↓
ngOnChanges()
```

 ### Important

 `ngOnChanges()` runs before `ngOnInit()` when the component has initial input values.

 It runs again whenever Angular detects a change to an input binding.

---

 ## 3\. `ngOnInit()`

 Called **once**, after Angular has initialized the component's input properties.

 Common uses:

 - Initial API calls
- Initial component setup
- Loading data
- Setting initial state

```
ngOnInit() {
  this.loadUsers();
}
```

 ### Interview question

 **Q: `constructor()` vs `ngOnInit()`?**

 **Answer:**

 > The constructor is a JavaScript/TypeScript class initialization mechanism and is primarily used for dependency injection. `ngOnInit()` is an Angular lifecycle hook and is used for initialization after Angular has initialized the component's inputs.

---

 ## 4\. `ngDoCheck()`

 Called during Angular's change-detection process.

```
ngDoCheck() {
  console.log('Change detection');
}
```

 It can run **very frequently**, so avoid expensive operations here.

 ### Interview point

 Use it only when you need custom change-detection behavior that Angular's normal mechanisms don't cover.

---

 ## 5\. `ngAfterContentInit()`

 Called **once** after Angular projects external content into the component.

 This is related to **content projection** using:

```
<ng-content></ng-content>
```

 Example:

```
<app-card>
  <p>Hello</p>
</app-card>
```

 The `<p>` is projected into the card's content.

---

 ## 6\. `ngAfterContentChecked()`

 Called after Angular checks the **projected content**.

 It may run multiple times during change detection.

 Therefore, avoid expensive logic here.

---

 ## 7\. `ngAfterViewInit()`

 Called **once** after the component's view and child views have been initialized.

 This is useful when you need access to view-related elements.

 For example:

```
@ViewChild('input') input!: ElementRef;

ngAfterViewInit() {
  this.input.nativeElement.focus();
}
```

 Template:

```
<input #input>
```

 ### Interview distinction

- `ngAfterContentInit()` → projected content (`ng-content`)
- `ngAfterViewInit()` → component's own view and child views

---

 ## 8\. `ngAfterViewChecked()`

 Called after Angular checks the component's view and child views.

 It can run frequently, so avoid heavy operations.

---

 ## 9. `ngOnDestroy()`

 Called **once before Angular destroys the component**.

 Typical uses:

- Cleanup subscriptions
- Remove event listeners
- Clear timers
- Release resources

```
ngOnDestroy() {
  this.subscription.unsubscribe();
}
```

 Modern Angular also provides utilities such as `DestroyRef` and `takeUntilDestroyed()` that can simplify cleanup.

---

 # Most Important Hooks

| Hook | When it runs | Common use |
| --- | --- | --- |
| `constructor()` | Class creation | Dependency injection |
| `ngOnChanges()` | Input changes | React to `@Input` changes |
| `ngOnInit()` | Once after inputs initialized | Initial setup/API calls |
| `ngDoCheck()` | During change detection | Custom change detection |
| `ngAfterContentInit()` | Projected content initialized | `ng-content` |
| `ngAfterViewInit()` | View initialized | `@ViewChild`, DOM-related setup |
| `ngOnDestroy()` | Before destruction | Cleanup |


## Constructor vs `ngOnInit()`

 ### Key difference between constructor and ngOnInit()

| `constructor()` | `ngOnInit()` |
| --- | --- |
| TypeScript/JavaScript class feature | Angular lifecycle hook |
| Runs when the class is instantiated | Runs after Angular initializes the component's inputs |
| Mainly used for **Dependency Injection** | Used for **component initialization logic** |
| Runs before Angular lifecycle hooks | Runs after the first `ngOnChanges()` |
| Doesn't mean Angular component is fully initialized | Component inputs are available |

## Dependency Injection (DI) and DI Hierarchy in Angular

 This is another **very common Angular interview topic**.

 ## 1. What is Dependency Injection?

 **Dependency Injection is a design pattern where a class receives the objects/services it depends on from an external system instead of creating them itself.**

 Without DI:

```
export class UserComponent {
  userService = new UserService();
}
```

 The component is responsible for creating `UserService`.

 With Angular DI:

```
export class UserComponent {
  constructor(private userService: UserService) {}
}
```

 Angular creates/provides `UserService` and injects it into `UserComponent`.

 ### Why use DI?

 - Reduces tight coupling
- Improves testability
- Promotes code reuse
- Centralizes dependency creation/configuration
- Makes it easier to replace implementations

---

 ## 2\. How Angular DI Works

 Consider:

```
@Injectable({
  providedIn: 'root'
})
export class UserService {
}
```

 And:

```
@Component({
  selector: 'app-user'
})
export class UserComponent {

  constructor(private userService: UserService) {}

}
```

 The flow is roughly:

```
UserComponent
      │
      │ asks for UserService
      ▼
Angular DI system
      │
      │ searches for provider
      ▼
UserService instance
      │
      ▼
Injected into UserComponent
```

 The important concept is the **provider**.

 A provider tells Angular:

 > "When someone requests `UserService`, this is what should be provided."

---

 ## 3\. What is a Provider?

 A provider defines how Angular should create or supply a dependency.

 For example:

```
providers: [UserService]
```

 This essentially tells Angular that `UserService` can be provided through DI.

 You can also use:

```
providers: [
  {
    provide: UserService,
    useClass: UserService
  }
]
```

 Other provider types include:

 - `useClass`
- `useValue`
- `useFactory`
- `useExisting`

 Example:

```
providers: [
  {
    provide: API_URL,
    useValue: 'https://api.example.com'
  }
]
```

---

 ## 4\. DI Hierarchy

 Angular uses a **hierarchical dependency injection system**.

 The important idea is:

 > Angular can have different injectors at different levels, and the injector hierarchy determines where a dependency is searched for and which instance is returned.

 A simplified view is:

```
Application / Environment Injector
             │
             ▼
      Route Injector
             │
             ▼
     Element Injector
             │
             ▼
        Component
```

 The exact injector structure can be more nuanced in modern Angular, but this model is useful for interviews.

---

 ## 5\. Root-Level Provider

 The most common approach is:

```
@Injectable({
  providedIn: 'root'
})
export class UserService {}
```

 This registers the service with the application's root-level injector.

 Typically, the service behaves like a **singleton for that application injector**.

```
Application
    │
    ├── Component A ──┐
    │                 │
    ├── Component B ──┼──> Same UserService instance
    │                 │
    └── Component C ──┘
```

 ### Why `providedIn: 'root'`?

 It also allows Angular's build tooling to remove the service when it isn't used (**tree-shaking**).

---

 ## 6\. Component-Level Provider

 You can provide a service directly on a component:

```
@Component({
  selector: 'app-user',
  providers: [UserService]
})
export class UserComponent {}
```

 Now Angular creates a service instance associated with that component's injector.

```
Root Injector
     │
     ├── UserComponent
     │       │
     │       └── UserService #1
     │
     └── AdminComponent
             │
             └── UserService #2
```

 Therefore, `UserComponent` and `AdminComponent` can receive **different instances**.

 This is useful when the service's state should be isolated to a particular component/subtree.

---

 ## 7\. Child Components and DI Hierarchy

 Suppose:

```
@Component({
  providers: [CounterService]
})
export class ParentComponent {}
```

 And:

```
ParentComponent
      │
      ├── ChildComponent
      │
      └── AnotherChildComponent
```

 If the children request `CounterService`, they can receive the instance provided by the parent's injector.

```
ParentComponent
      │
      │ CounterService instance #1
      │
      ├── ChildComponent ────────┐
      │                          │
      └── AnotherChildComponent ─┘
```

 This allows a service instance to be **shared within a component subtree**.

---

 ## 8\. How Angular Searches for a Dependency

 This is particularly important for interviews.

 Suppose a component requests:

```
constructor(private userService: UserService) {}
```

 Angular looks for a matching provider through the relevant injector hierarchy.

 Conceptually:

```
Component Injector
       ↓
Parent Injector
       ↓
Higher-level Injector
       ↓
Root/Application Injector
```

 Angular uses the appropriate provider it finds according to the injector hierarchy.

 If no provider can be found, Angular throws an error such as:

```
NullInjectorError
```

---

 ## 9\. Example of Different Instances

 Consider:

```
@Injectable({
  providedIn: 'root'
})
export class CounterService {
  count = 0;
}
```

 Normally, components using the root-provided service can share the same instance.

 But:

```
@Component({
  selector: 'app-counter',
  providers: [CounterService]
})
export class CounterComponent {}
```

 creates a component-level provider.

 So:

```
Root
 └── CounterService #1

CounterComponent
 └── CounterService #2
```

 `CounterComponent` gets its own instance rather than using the root instance.

---

 ## 10\. `providedIn: 'root'` vs Component `providers`

 | `providedIn: 'root'` | `providers: [Service]` |
| --- | --- |
| Application/root scope | Component/subtree scope |
| Usually one shared instance | New instance for that component injector |
| Common for application-wide services | Useful for isolated state |
| Supports tree-shaking | Provider exists for that component subtree |

---
