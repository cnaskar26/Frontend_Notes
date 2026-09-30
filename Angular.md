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

 #. `constructor()`

 The TypeScript constructor runs when the class is instantiated.

 Main purpose:

 - Dependency injection
- Basic class initialization

```
constructor(private userService: UserService) {}
```

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

---

 ## 4\. `ngDoCheck()`

 Called during Angular's change-detection process.

```
ngDoCheck() {
  console.log('Change detection');
}
```

 It can run **very frequently**, so avoid expensive operations here.

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


# Parent–Child Communication in Angular

 Angular provides several ways for a **parent component and child component to communicate**.

 A simple mental model:

```
                Parent
              /        \
         @Input()     @ViewChild()
            ↓              ↓
          Child  ←─────────
            │
         @Output()
            │
            ↓
          Parent
```

 > In modern Angular, you may also see the newer `input()` and `output()` APIs. The concepts are the same; `@Input()` and `@Output()` remain very important for interviews and existing codebases.

---

 ## 1\. `@Input()` — Parent to Child

 `@Input()` is used when the **parent wants to send data to the child**.

 ### Parent component

```
export class ParentComponent {
  userName = 'John';
  age = 25;
}
```

 ### Parent template

```
<app-child
  [name]="userName"
  [userAge]="age">
</app-child>
```

 ### Child component

```
export class ChildComponent {
  @Input() name!: string;
  @Input() userAge!: number;
}
```

 ### Child template

```
<p>Name: {{ name }}</p>
<p>Age: {{ userAge }}</p>
```

 The data flow is:

```
Parent
  │
  │ [name]="userName"
  │ [userAge]="age"
  ↓
Child
  │
  ├── name
  └── userAge
```

 ### What happens when the value changes?

 Suppose:

```
userName = 'John';
```

 Later:

```
userName = 'David';
```

 Angular updates the child's input.

 The child can react to this change using `ngOnChanges()`:

```
@Input() name!: string;

ngOnChanges(changes: SimpleChanges) {
  console.log(changes['name']);
}
```

---

 ## 2\. `@Output()` — Child to Parent

 `@Output()` is used when the **child needs to send an event/data back to the parent**.

 The child doesn't directly modify the parent's property. Instead, it **emits an event**, and the parent listens to it.

 ### Child component

```
import { EventEmitter, Output } from '@angular/core';

export class ChildComponent {

  @Output() userSelected = new EventEmitter<string>();

  selectUser() {
    this.userSelected.emit('John');
  }
}
```

 ### Child template

```
<button (click)="selectUser()">
  Select User
</button>
```

 ### Parent template

```
<app-child
  (userSelected)="onUserSelected($event)">
</app-child>
```

 ### Parent component

```
export class ParentComponent {

  onUserSelected(name: string) {
    console.log('Selected:', name);
  }
}
```

 The flow is:

```
Child
  │
  │ userSelected.emit('John')
  ↓
Parent
  │
  │ (userSelected)
  ↓
onUserSelected($event)
```

 `$event` contains the value emitted by the child.

---

 ## 3\. Why use `EventEmitter`?

 `EventEmitter` allows a child component to **emit an event/value** that the parent can listen to.

 Example:

```
@Output()
save = new EventEmitter<User>();

this.save.emit(user);
```

 Parent:

```
<app-user (save)="saveUser($event)">
</app-user>
```

 This creates a clean communication pattern:

```
Child → Event → Parent
```

---

 ## 4\. `@ViewChild()` — Parent Accessing Child

 `@ViewChild()` is different from `@Input()` and `@Output()`.

 It allows a parent component to **get a reference to a child component or an element in its own template**.

 For example:

 ### Child

```
export class ChildComponent {

  reset() {
    console.log('Child reset');
  }

}
```

 ### Parent template

```
<app-child></app-child>
```

 ### Parent component

```
export class ParentComponent {

  @ViewChild(ChildComponent)
  child!: ChildComponent;

  resetChild() {
    this.child.reset();
  }

}
```

 Now the parent can call the child's method:

```
Parent
  │
  │ @ViewChild
  ↓
Child instance
  │
  ↓
child.reset()
```

---

 ## 5\. Why `@ViewChild()` is different

 Compare the three:

 | Mechanism | Direction | Purpose |
| --- | --- | --- |
| `@Input()` | Parent → Child | Pass data |
| `@Output()` | Child → Parent | Send events/data |
| `@ViewChild()` | Parent → Child | Get child reference and interact with it |

### Example

```
                 Parent
                /      \
          @Input()     @ViewChild()
             ↓             ↓
           Child ←──────────
             │
          @Output()
             │
             ↓
           Parent
```

---

 ## 6\. `@ViewChild()` with Template Reference

 You don't have to use `@ViewChild()` only for components.

 You can reference an element:

```
<input #username>
```

 Then:

```
@ViewChild('username')
usernameInput!: ElementRef<HTMLInputElement>;
```

 You could then access the element:

```
this.usernameInput.nativeElement.focus();
```

 However, direct DOM manipulation should generally be minimized in Angular; prefer Angular APIs and bindings where possible.

---

 ## 7\. `@ViewChild()` and `ngAfterViewInit()`

 **"When is `@ViewChild()` available?"**

 For a view child that is initialized with the component's view, you commonly access it in:

```
ngAfterViewInit() {
  this.child.reset();
}
```

 Example:

```
@ViewChild(ChildComponent)
child!: ChildComponent;

ngAfterViewInit() {
  this.child.reset();
}
```

 Why?

 Because the component's view needs to be initialized before Angular can provide the view-child reference.

---

 ## 8\. `@Input()` vs `@ViewChild()`

 ### `@Input()`

 Use when the parent is **passing data**.

```
<app-child [user]="user"></app-child>
```

 This is declarative and is generally preferred for normal parent → child data flow.

 ### `@ViewChild()`

 Use when the parent needs to **access the child instance**.

```
@ViewChild(ChildComponent)
child!: ChildComponent;

this.child.reset();
```

 So:

 > **`@Input()` passes data; `@ViewChild()` gives the parent a reference to the child.**

---

 ## 9\. `@Output()` vs `@ViewChild()`

 Suppose a child has:

```
save() {
  // save logic
}
```

 The parent could call it using:

```
@ViewChild(ChildComponent)
child!: ChildComponent;

this.child.save();
```

 But if the child needs to notify the parent that something happened, use `@Output()`:

```
@Output() saved = new EventEmitter<User>();

this.saved.emit(user);
```

 Parent:

```
<app-child (saved)="handleSave($event)">
</app-child>
```

 ### Difference

```
@ViewChild()
Parent directly controls/accesses Child

@Output()
Child notifies Parent through an event
```

---

 ## 10\. Complete Example

 Imagine we have:

```
ParentComponent
       │
       │ sends user
       ↓
ChildComponent
       │
       │ user saved event
       ↓
ParentComponent
```

 ### Child

```
@Component({
  selector: 'app-child',
  template: `
    <h3>{{ user.name }}</h3>
    <button (click)="save()">Save</button>
  `
})
export class ChildComponent {

  @Input() user!: User;

  @Output() saved = new EventEmitter<User>();

  save() {
    this.saved.emit(this.user);
  }

  reset() {
    console.log('Reset child');
  }
}
```

 ### Parent

```
@Component({
  selector: 'app-parent',
  template: `
    <app-child
      [user]="user"
      (saved)="onSaved($event)">
    </app-child>

    <button (click)="resetChild()">
      Reset Child
    </button>
  `
})
export class ParentComponent {

  user = {
    name: 'John'
  };

  @ViewChild(ChildComponent)
  child!: ChildComponent;

  onSaved(user: User) {
    console.log('User saved:', user);
  }

  resetChild() {
    this.child.reset();
  }
}
```

 Here we're using all three:

```
@Input()
Parent ──────────────→ Child
       user

@Output()
Child ───────────────→ Parent
       saved event

@ViewChild()
Parent ──────────────→ Child
       child reference
```

---

 ## 11\. What if Components Are Not Direct Parent/Child?

 If components are unrelated:

```
Component A       Component B
      │                │
      └──────┬─────────┘
             ↓
       Shared Service
```

 A shared service can be used for communication/state sharing.

 For larger applications, Angular Signals or dedicated state-management patterns can also be used depending on the application's requirements.

---
# Angular Change Detection, `OnPush`, and `ChangeDetectorRef`

---

 ## 1\. What is Change Detection?

 **Change detection is Angular's mechanism for detecting changes in application state and updating the DOM accordingly.**

 For example:

```
name = 'John';

changeName() {
  this.name = 'David';
}
```

 Template:

```
<h2>{{ name }}</h2>
```

 When `name` changes, Angular's change-detection mechanism determines that the template needs to be updated.

 Conceptually:

```
Application state changes
        ↓
Angular runs change detection
        ↓
Angular checks component templates
        ↓
Changed bindings are detected
        ↓
DOM is updated
```

 ### Important point

 Change detection doesn't mean Angular blindly recreates the entire DOM.

 Angular evaluates the relevant bindings and updates the DOM where necessary.

---

 ## 2\. Default Change Detection

 Angular has two main component change-detection strategies:

```
ChangeDetectionStrategy.Default
ChangeDetectionStrategy.OnPush
```

 `Default` is the normal strategy if you don't specify one.

 Example:

```
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.Default
})
```

 With the default strategy, Angular checks the component during normal application change-detection cycles.

 For example, events such as:

```
Click
Timer
HTTP response
Observable activity
Other async operations
```

 can cause Angular to run change detection.

 A simplified view:

```
Application
     ↓
Angular runs change detection
     ↓
Parent
     ↓
Child
     ↓
Grandchild
     ↓
...
```

---

 ## 3\. What is `OnPush`?

 `OnPush` is a change-detection strategy that allows Angular to **skip unnecessary checking of a component subtree under certain conditions**.

 Example:

```
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserComponent {}
```

 The goal is generally to improve performance by reducing unnecessary checks.

---

 ## 4\. When Does an `OnPush` Component Get Checked?

 An `OnPush` component can be checked when, among other cases:

 ### 1\. An input reference changes

```
<app-user [user]="user"></app-user>
```

 If:

```
this.user = newUser;
```

 the input reference changes, so Angular can check the child.

 But consider:

```
this.user.name = 'David';
```

 The object reference is still the same.

```
Before: user → Object A
After:  user → Object A
```

 So simply mutating the object does **not** provide a new input reference.

 Instead:

```
this.user = {
  ...this.user,
  name: 'David'
};
```

 Now:

```
Before: user → Object A
After:  user → Object B
```

 The reference changed.

---

 ### 2\. An event occurs in the component's subtree

 For example:

```
<button (click)="save()">Save</button>
```

 An event handled within the component/subtree can cause Angular to check the relevant `OnPush` component.

---

 ### 3\. An observable/signal-driven update is used through Angular's reactive mechanisms

 For example, Angular's `async` pipe can mark the relevant view for checking when a new value arrives.

 Signals also integrate with Angular's change-detection system.

---

 ### 4\. You explicitly request checking

 You can use `ChangeDetectorRef`, which we'll discuss below.

---

 ## 5\. Why Object Mutation Is a Common Question

 Consider:

```
user = {
  name: 'John'
};
```

 Child:

```
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() user!: User;
}
```

 Parent does:

```
this.user.name = 'David';
```

 The object reference hasn't changed.

```
user ─────→ Object A
             name = David
```

 A new object is preferable:

```
this.user = {
  ...this.user,
  name: 'David'
};
```

 Now:

```
user ─────→ Object B
```

 This is why **immutable update patterns** work particularly well with `OnPush`.

---

 ## 6\. `ChangeDetectorRef`

 `ChangeDetectorRef` is an Angular API that allows you to **interact with Angular's change-detection mechanism for a view**.

 Inject it:

```
constructor(private cdr: ChangeDetectorRef) {}
```

 It provides methods such as:

```
markForCheck()
detectChanges()
detach()
reattach()
checkNoChanges()
```

 The two most important are:

```
markForCheck()
detectChanges()
```

---

 ## 7\. `markForCheck()`

 `markForCheck()` tells Angular:

 > **"This component/view should be checked during a future change-detection run."**

 Example:

```
constructor(private cdr: ChangeDetectorRef) {}

updateUser() {
  this.user.name = 'David';

  this.cdr.markForCheck();
}
```

 This is particularly useful with `OnPush` when you've changed state in a way where Angular isn't otherwise going to schedule/check the view as needed.

 ### Important distinction

 `markForCheck()` **doesn't immediately perform change detection**.

 It marks the view so that it will be checked when Angular performs its next appropriate change-detection pass.

---

 ## 8\. `detectChanges()`

 `detectChanges()` tells Angular to **immediately perform change detection for the view and its descendants**.

 Example:

```
this.user.name = 'David';

this.cdr.detectChanges();
```

 Conceptually:

```
State changed
    ↓
detectChanges()
    ↓
Check this view/subtree now
    ↓
DOM updated
```

 ### `markForCheck()` vs `detectChanges()`

 | `markForCheck()` | `detectChanges()` |
| --- | --- |
| Marks view as needing checking | Runs change detection immediately |
| Does not immediately check | Immediately checks the view/subtree |
| Waits for a suitable change-detection pass | Explicitly triggers a check |
| Common with `OnPush` | Useful when immediate/local checking is specifically needed |

---

 ## 9\. `detach()` and `reattach()`

 `detach()` removes a view from the normal change-detection tree.

```
this.cdr.detach();
```

 Angular will no longer automatically check that view as part of the normal change-detection traversal.

 You can manually check it:

```
this.cdr.detectChanges();
```

 And later reattach it:

```
this.cdr.reattach();
```

 This can be useful for specialized performance scenarios, but it should not be used casually.

---

 ## 10\. Example: `OnPush` \+ `ChangeDetectorRef`

```
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <h2>{{ user.name }}</h2>
    <button (click)="update()">Update</button>
  `
})
export class UserComponent {

  user = {
    name: 'John'
  };

  constructor(private cdr: ChangeDetectorRef) {}

  update() {
    this.user.name = 'David';

    this.cdr.markForCheck();
  }
}
```

 Here:

 1. Component uses `OnPush`.
2. The object is mutated.
3. `markForCheck()` tells Angular the view needs checking.
4. Angular checks it during the next appropriate change-detection pass.

 However, an even cleaner approach is often to use immutable updates:

```
this.user = {
  ...this.user,
  name: 'David'
};
```

---

 ## 11\. Change Detection Flow

 A simplified model is:

```
          Application
               │
               ↓
       Change Detection
               │
       ┌───────┴────────┐
       ↓                ↓
   Component A       Component B
       │
       ↓
     Child
       │
       ↓
      DOM
```

 With `OnPush`, Angular can skip checking certain subtrees when their relevant inputs/state haven't indicated that they need checking.

```
Application
    │
    ├── Component A
    │       ↓
    │    Checked
    │
    └── Component B
            ↓
        OnPush + no trigger
            ↓
          Skipped
```

 This is the main performance advantage of `OnPush`.

---

 ## 12\. Default vs `OnPush`

| Default | OnPush |
| --- | --- |
| More broadly participates in change detection | Allows Angular to skip checks when appropriate |
| Easier to reason about initially | More explicit/reactive approach |
| Can perform more checks | Can reduce unnecessary checks |
| Less sensitive to object mutation | Works best with immutable state/reference changes |
| Good default for many applications | Useful for performance-sensitive/componentized applications |

---

```
Change Detection
      ↓
Keeps UI synchronized with state

OnPush
      ↓
Reduce unnecessary checking

ChangeDetectorRef
      ↓
Control/interact with checking

markForCheck()
      ↓
"Check me in the next appropriate cycle"

detectChanges()
      ↓
"Check me now"
```

## Observable vs Promise

 ### 1\. Observable

 An **Observable** represents a stream of values that can arrive **over time**.

```
const observable$ = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
});
```

 To receive the values, you **subscribe**:

```
observable$.subscribe(value => {
  console.log(value);
});
```

 Output:

```
1
2
3
```

 ### 2\. Promise

 A **Promise** represents **one eventual result**—either success or failure.

```
const promise = new Promise(resolve => {
  resolve(10);
});

promise.then(value => {
  console.log(value);
});
```

 Output:

```
10
```

 ### Differences

 | Observable | Promise |
| --- | --- |
| Can emit **multiple values** | Produces **one value** |
| Can be synchronous or asynchronous | Usually represents an asynchronous result |
| Starts when subscribed to (cold observable) | Starts immediately when created |
| Can be cancelled using `unsubscribe()` | Native Promise cannot be cancelled |
| Supports operators like `map`, `filter`, `switchMap`, `debounceTime` | Uses `then`, `catch`, `finally` |
| Can emit `next`, `error`, and `complete` | Resolves or rejects |
| Common in Angular/RxJS | Common JavaScript API |

### Example

 Think of:

 **Promise:**

 > "Give me the result of this HTTP request."

 **Observable:**

 > "Keep giving me values whenever they occur."

 For example, a search box can produce:

```
"a"
"an"
"ang"
"angu"
"angular"
```

 An Observable is naturally suited to this continuous stream.

---

 # Subject vs BehaviorSubject

 Both are **RxJS Subjects**.

 A Subject is special because it is both:

 - an **Observable** — you can subscribe to it.
- an **Observer** — you can call `next()` on it.

```
const subject = new Subject<number>();

subject.subscribe(value => console.log("A:", value));

subject.next(10);
subject.next(20);
```

 Output:

```
A: 10
A: 20
```

 ## Subject

 A normal `Subject` **does not store the latest value**.

```
const subject = new Subject<number>();

subject.next(10);

subject.subscribe(value => {
  console.log(value);
});
```

 The subscriber receives **nothing**, because `10` was emitted before it subscribed.

---

 ## BehaviorSubject

 A `BehaviorSubject` **stores the latest value** and immediately gives that value to a new subscriber.

 It **requires an initial value**.

```
const subject = new BehaviorSubject<number>(0);

subject.next(10);

subject.subscribe(value => {
  console.log(value);
});
```

 Output:

```
10
```

 The subscriber immediately receives the latest value.

 ### Example

```
const user$ = new BehaviorSubject<string>("Guest");

user$.subscribe(user => {
  console.log(user);
});

user$.next("John");
```

 Output:

```
Guest
John
```

 ### Main difference

| Subject | BehaviorSubject |
| --- | --- |
| Doesn't retain current value | Retains latest value |
| No initial value required | Initial value required |
| New subscriber gets only future emissions | New subscriber immediately gets latest value |
| `new Subject()` | `new BehaviorSubject(initialValue)` |
