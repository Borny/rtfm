# Angular Cheat Sheet

- Angular CLI
- Types
- Binding
- HostBinding
- HostListener
- Directives
- Services
- Routing
- Forms
- Pipes
- HTTP request
- Authentication
- NGXS / NGRX
- Test

---

## Angular CLI

---

### Creating a new project

`ng new --minimal=true|false --routing=true|false --style=css|sass|scss|less [projectName}`  
Flags available for creating a new project/component/service/etc :

- --minimal=true|false : testing frameworks
- --routing=true|false : routing
- --style=css|sass|scss|less : styles files

### Creating a component

`ng g c --skipTests=true|false --style=css|sass|scss|less —-viewEncapsulation=Emulated|Native|None|ShadowDom [componentName]`
--skipTests=true|false : no testing files
— viewEncapsulation=Emulated|Native|None|ShadowDom : the view encapsulation strategy

_ViewEncapsulation_ : définie la façon dont un component sera créé.  
**Emulated**: par défaut, le style du component ne pourra pas être écrasé par le reste du projet  
**ShadowDom**: Angular va créé un Web Component

## Types

Types are needed in Angular as it uses Typescript. They make developping easier by highlighting errors.
Types:

- string
- number
- boolean
- array[]
- Observable
- Promise
- etc...

### Generic type

A generic type is a type of which we know the returned value of:
`<{valueName: valueType}>`

---

## Binding

---

Property binding : [childProperty]="[parentProperty]"  
`@Input() [propertyName]: [type] = [defaultValue]`

Event binding : (childEvent)="[parentFunction()]"  
`@Output() [propertyName$]: EventEmitter<void> = new EventEmitter();`

### EventEmitter vs Subject

**EventEmitter** should be used when dealing with eventBinding: from child to parent via the @Output() decorator.
**Subject** should be used when passing data to any component in the app.

Two way data binding : [(ngModel)]="[propertyName]"
Needs to be declared in the TS file.

Binding to element :  
`@ViewChild('[elementName]', { static: true }) propertyName: ElementRef;`

---

## HostBinding

---

Decorator that binds to an element of the DOM. Imported form @angular/core

- Binding to **class** `@HostBinding('class.[className]') propertyName: [type] = [defaultValue];`
- Binding to **style** `@HostBinding('style.[CSSPropertyName]') propertyName: [type] = [defaultValue];`

---

## HostListener

---

Decorator that listens to event on a given host. Imported form @angular/core

- Template
  `@HostListener('[host]', ['$event']) [functionName](event:[eventType]) { }`

- Window event with key pressed:  
  `@HostListener('window:keyup', ['$event']) [functionName](event:KeyboardEvent) { }`
- Document event with click:  
  `@HostListener('document:click', ['$event']) [functionName](event:Event) { }`
- Document event with mouse:  
  `@HostListener('mouseenter') [functionName](event:Event) { }`

### ElementRef

Gives access to the element the directive sits on
Declared in the constructor and imported form @angular/core  
i.e: `this.elementRef.nativeElement`

### Renderer2

Better than ElementRef alone as it can be used when no DOM is present: ServiceWorkers, etc...
Declared in the constructor and imported form @angular/core
Template : `this.renderer.[method]([elementTarget],...)`
i.e :`this.renderer.setStyle(this.elRef.nativeElemnt)`

---

## Directives

---

### Structural directives

Removes/replace element from the DOM : *ngIf, *ngFor

### \*ngFor

Renders a list from an array.

Utilities :
index : `let i = index`
first : `let first = first`
last : `let last = last`
even : `let even = even`
odd : `let odd = odd`

trackBy :
Used to improve performances of the \*ngFor. It will not rerender the whole list in case of a modification but only the affected items.
Template : `trackBy: [trackByCustomFunctionName]`
TS: `public trackByCustomFunctionName(index, item){return index or item.id}`

### Attribute directives

Changes/modifies elements from the DOM : ngStyle, ngClass

---

## Services

---

Declared with the @Injectable() decorator, needed to import another service

Declared in the app.module : can be accessed by all components/services/directives
Declared in the app.component : can be accessed by all components
Declared in a component : can be accessed by the child components

**Easier way** : `@Injectable({providedIn: 'root'})` : can be accessed by all components/services/directives

---

## Routing

---

### Router Module

Registers the routes for the application

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: '',
    redirectTo: '/[routeName]',
    pathMatch: 'full',
  },
  {
    path: '[routeName]',
    component: [componentName],
  },
  {
    path: '[:param]',
    component: [componentDetailName],
  },
  {
    path: '**',
    component: [componentName],
  },
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule],
})
export class AppRouting {}
```

---

### Lazy Loading

---

Improves performance by only loading the requested components. The initial bundle is then lighter. Each components(could be the views) that needs to be loaded needs to have its own module and routing module.

**In the app routing module:**

```typescript
const appRoutes: Routes = [
  {
    path: '',
    redirectTo: '/auth',
    pathMatch: 'full'
  },
  {
    path: '**',
    loadChildren: () => import('./views/view-404/view-404.module').then(m => m.View404Module)
  },
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes),
  ],
  exports: [RouterModule]
})
```

In the component routing module:

```typescript
const [componentRoutes]: Routes = [
  {
    path: '',
    component: ViewHomeComponent,
    canActivate: [AuthGuard]
  },
]

@NgModule({
  imports: [RouterModule.forChild([[componentRoutes]])],
  exports: [RouterModule],
})

export class [ComponentRouting] {}
```

In the template :

- `<router-outlet></router-outlet>` will load the desired routes
- `routerLink="/[routeName]"` replaces the href="" and will load the requested view/component without refreshing the entire page leading to a better performance and UX
- `routerLinkActive="classToAddWhenTheLinkIsActive` will add a class to the active route

### Nested routes

When a component has some child routes
Add **children**

### Absolute path

Will add the path to the main route(i.e: **localhost:4200** or **www.website.com** )

### Relative path

Will add the path to the current route

### Programmatic navigation : navigating from the ts file

import { Router } from '@angular/router'
`this.route.navigate('/routeName')`

### Get params from the URL

Use the ActivatedRoute module

#### Route param

In the route module:

```typescript
path: ':[paramName]';
```

```typescript
const param = this.route.snapshot.params('[paramName]');
```

**or**

```typescript
const param = this.route.params.subscribe((param: Params) => {this.[customProperty] = param})
```

#### Other params

=> Setting the param

```typescript
this.route.navigate([`./[pathName]`], {
  queryParams: { [paramKeyName]: `[paramValueName]` },
});
```

=> Getting the param

```typescript
this.route.snapshot.queryParamMap.get('[paramKeyName]');
```

**or**

```typescript
const param = this.route.queryParamMap.subscribe((param: Params) => {this.[customProperty] = param})
```

### Preloading strategy

Add this code to the appRouting Module:

```typescript
@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes, { preloadingStrategy: PreloadAllModules }),
  ],
  exports: [RouterModule]
})
```

### Route Parameters

_pathToLoad/:whateverParameterYouWant_

### Fetching Route Parameters

import ActivatedRoute
this.activatedRoute.snapshot.params['paramName]

### Reactive Route Params

Subscribe : `this.activatedRoute.params.subscribe()`

### Preserve the params

Add _queryParamsHandling_ to the navigate method

---

## Forms

---

### Template Driven Approach

#### Setting up the form in the template

In the module:
`import { FormsModule } from '@angular/forms'`

In the template  
`<form>` tag:

- `#[formName]="ngForm"` will bind the form
- `(ngSubmit)="[onSubmitFunctionName([formName])]"`

`<input>` tag:

- `ngModel` will bind the form-control to the form object
- `name="[formControlName]"` act as a selector

#### Grouping controls

Use _ngModelGroup="[formControlGroupName]" #formControlGroupName="ngModelGroup"_ in a surrounding div around the form-control you want to group

**Important** Only one button should be of type **submit** as it will be the button that the form is listening to for the ngSubmit method. All other buttons should be of type **button**

#### Accessing the form in the ts file

`onSubmitFunctionName(form: NgForm){}`
**or**
use `@ViewChild('formName') formNameProperty: NgForm` to access the form before submitting it

#### Validators

Add _required, email, password..._ directly to the form-control tag  
**ngNativeValidate** will enable HTML5 validation as Angular disables it by default

#### Resetting the form

`this.[formName].reset()`  
**or**  
`this.[formName].setValue([customValues])` to reset the form with specific values

### Reactive Approach

In the module: `import {ReactiveFormControl}`

In the ts file:  
`import { FormGroup } from '@angular/forms'`  
create the form : `[formName]: FormGroup` and initialize the form in the OnInit interface :

```typescript
this.[formName] = new FormGroup(
  '[controlName]' = new FormControl('[defaultValue]', [Validators.required, Validators.email, ...]),
)
```

In the template:  
`<form>` tag:

- `[formGroup]="[formName]"`
- `(ngSubmit)="[onSubmitFunctionName()]"`

`<input>` tag:

- `formControl="[controlName]"`

#### Nested controls - FormGroup

Add a new **FormGroup** to the existing FormGroup:

```typescript
this.[formName] = new FormGroup({
  '[formGroupControlName]' = new FormGroup({
    '[controlName]' = new FormControl('[defaultValue]', [Validators.required, Validators.email, ...]),
  }),
})
```

In the template, wrap the controls in a new div with the FormGroupName directive:`formGroupName="[formGroupControlName]"`

#### Form Array

In the template:
`*ngFor= let control of getControlsArray(); let i = index`

in the ts file:  
'[arrayControlName]' = new FormArray([])

```
  getControlsArray(){
    return (<FormArray>this.[formName].get('[arrayControlName]')).controls
  }
```

=> alternative use, **Getter**:  
In the template:  
`*ngFor= let control of controlsArray; let i = index`

In the ts file:

```javascript
  get controlsArray(){
    return (this.[formName].get('[arrayControlName]') as FormArray).controls
  }
```

#### setValue()

Will set the values of **all** the controls
`this.[formName].setValue()`

#### patchValue()

Will set the values of the designated control **only**
`this.[formName].patchValue()`
`this.[formName].get('formControlName').up`

#### Custom Validators

...

#### Value and Status changes - hooks observables

`this.[formName].valueChanges.subscribe(value => console.log(value))`  
`this.[formName].statusChanges.subscribe(status => console.log(status))`

---

## Pipes

---

Transforms output in the Template.  
`{{ output | [pipe] }}`

### Chaining pipes

`{{ output | [pipe1] | [pipe2]}}`  
**The order is important as some pipes accept a certain type of output**

### Parameters

`{{ output | [pipe]: ['parameter']}}`

### Custom Pipe

```typescript
import { Pipe, PipeTransform} from '@angular/core';
@Pipe({
  name: '[customPipeName]'
})
export class [CustomPipeClassName] implement PipeTransform{
  transform(value: any, parameter: any){
    return [valueTransformed]...
  }
}
```

---

## HTTP request

---

### Anatomy of a request

- Http Verb : post, get, put, delete, patch
- URL (API Endpoint) : http://www.../posts/1
- Headers (Metadata) : {"Content-Type": "application/json"}
- Body (post, put, patch and delete methods only): Object that will be send with the request

---

## Interceptors

---

Functions that will run with any outgoing request.

---

## Authentication

---

### Sign Up

```typescript
this.http.post<**ResponseType**>(
  url,
  {requestBody}
)
```

### Authentication Guard

- create an Authentication Guard file
- implements **CanActivate** interface
- Then add **canActivate()** method. It will return a boolean or an UrlTree, either primitive, Promise or Observable.
- This is how it will decide if the route can be **activated**. If a guard returns a UrlTree then the current navigation will be cancelled and a new navigation will kick off

```javascript
import { Injectable } from '@angular/core';
import {
  CanActivate,
  ActivatedRouteSnapshot,
  RouterStateSnapshot,
  UrlTree,
} from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class AuthGuard implements CanActivate {
  canLoad(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ):
    | Observable<boolean | UrlTree>
    | Promise<boolean | UrlTree>
    | boolean
    | UrlTree {
    return true;
  }
}
```

### canActivate

It will prevent the user from reaching the page but will **still load** the module

### canload

Will prevent the user from reaching the page but will **not load** the module. _Better in case of lazy loading_

---

## Animations

---

```javascript
animations: [
  trigger('[CustomAnimationName]', [
    transition('* => *', [
      // each time the binding value changes
      query(
        ':leave',
        [
          // :leave is equal to *=> void
          stagger(100, [animate('0.5s', style({ opacity: 0 }))]),
        ],
        { optional: true }
      ),
      query(
        ':enter',
        [
          // :enter is equal to void => *
          style({ opacity: 0 }),
          stagger(100, [animate('0.5s', style({ opacity: 1 }))]),
        ],
        { optional: true }
      ),
    ]),
  ]),
];
```

```html
<div [@CustomAnimationName]></div>
```

---

## Local Storage

---

- localStorage
- **.setItem**('[dataKeyName]', JSON.stringify([objectToStore]));
- JSON.parse(**.getItem**('[dataKeyName]'));
- **.removeItem**('[dataKeyName]');

---

## AOT vs JIT

---

---

## SSR

---

- ng add @nguniversal/express-engine

---

## PWA - Service Workers

---

- ng add @angular/pwa
  Installs the following files:
- manifest.json : loads the icons that will be displayed on the home screen
- ngsw-config.json : **not sure** configures the service worker.
- Adds serviceWorker module to the app module : it acts like a **proxy**, it catches the outgoing request and treats them.... **to complete**

The **service worker** will cache certain resources that will contain a _hash_. Then all new prod build will update the service worker and update the resources

Add this property to the ngsw-config.json file:

```json
"dataGroups": [
  {
    "name": "[**name**]",
    "urls": [**urls that will fetch data**],
    "cacheConfig": {
      "maxSize": 5,
      "maxAge": "6h",
      "timeout": "10s",
      "strategy": "freshness" // or performance
    }
  }
]
```

## NGXS / NGRX

State management

### NGRX:

https://medium.com/angular-in-depth/adding-ngrx-to-your-existing-applications-3175d2227672

## Test

- Integration test
- Unit test
- Suites
- Specifications
- Structure of a test
- Filter testing
- Fixture => wraps the component
- DebugElement => wraps the native DOM element (the HTML template: app-component-name)

### Integration test

An **Integration test** includes ('integrates') the dependencies.

### Suites

An **Unit test** replaces the dependencies with fakes in order to isolate the code under test.  
Replacing a dependency is called **stubbing** or **mocking**.

### Suites

A Test consists of one or more **suites**. A **suite** is declared with a **describe** block:  
**describe** takes two parameters:

- a string to describe the test
- a function containing the suite definition

```typescript
describe('suite description', () => {});
```

**describe** methods can be nested:

```typescript
describe('suite description', () => {
  describe('suite description', () => {});
});
```

### Specifications

Each **suit** consists of one or more **specifications** or **specs**.  
A **spec** is declared with an **it** block:

```typescript
describe('suite description', () => {
  it('Spec description', () => {});
});
```

### Structure of a test

A test consists generally of three phases:

- Arrange
- Act
- Assert

#### Arrange

Preparation and set up of the test.  
A set up that is shared in multiple specs can be defined once in a function:

- beforeAll()
- afterAll()
- beforeEach()
- afterEach()

```typescript
describe('suite description', () => {
  beforeAll(() => {
    // this function will be called before each specification
    console.log('something interesting...');
  });
  it('Spec 1 description', () => {});
  it('Spec 2 description', () => {});
});
```

#### Act

Interaction with the code happens: method called, button clicked...

#### Assert

The code behavior is checked and verified: actual output compared to the expected output

### Filter testing

- the 'f' prefix
- with ng test

#### the 'f' prefix

Adding the **f** prefix will only run this test.  
It can be added to the _describe_ or _it_ methods:

```typescript
describe('suite description', () => {
  fit('Spec 1 description', () => {
    // only this specification will run
  });
  it('Spec 2 description', () => {});
});
```

#### with ng test

Add the **--include** flag and the path of the files to test to the `ng test` command to only include certain spec files:  
`ng test --include **/some-component.component.spec.ts` => only one test file will run
