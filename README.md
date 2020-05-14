# Angular Note
## Angular CLI

| Scaffold      | Usage                              | 
| ------------- |:----------------------------------:| 
| Component     | ng g component my-new-component    | 
| Directive     | ng g directive my-new-directive    | 
| Pipe          | ng g pipe my-new-pipe              | 
| Service       | ng g service my-new-service        | 
| Class         | ng g class my-new-class            | 
| Interface     | ng g interface my-new-interface    | 
| Enum          | ng g enum my-new-enum              | 
| Module        | ng g module my-module              |
| Route         | `ng g command` --routing             |

| Scaffold         | Usage                    | 
| ---------------- |:------------------------:| 
| Build            | ng build                 | 
| Unit test        | ng test                  | 
| End-to-End tests | ng e2e                   | 
| TSLint           | ng lint                  | 
| Custom Scripts   | npm run `custom-scripts` | 

## Data-Binding

<img src="https://blog.johnwu.cc/images/a/196.png" width="450" />

- Interpolation

```javascript
export class AppComponent {
    name: string = "John";
}
```
```html
<span>{{name}}</span>
```

- Property Binding

```javascript
export class AppComponent {
    color: string = "blue";
}
```
```html
<div [ngClass]="color">Font color</div>
```

> Difference between interpolation and property binding: Angular evaluates all expressions in double curly braces, converts the expression results to strings, and concatenates them with neighboring literal strings. Finally, it assigns this composite interpolated result to an element or directive/component property.

> Property binding does not convert the expression result to a string. So if you need to bind something other than a string to your directive/component property, you must use property binding.


1. Element property
```html
<img [src]="heroImageUrl">
```
2. Component property
```html
<hero-detail [hero]="currentHero"></hero-detail>
```
3. Directive property
```html
<div [ngClass]="{special: isSpecial}"></div>
```
4. Attr property
```html
<button [attr.aria-label]="help">help</button>
```
5. Class property
```html
<div [class.special]="isSpecial">Special</div>
```
6. Style property
```html
<button [style.color]="isSpecial ? 'red' : 'green'">
```

- Event Binding

```javascript
export class AppComponent {
    onClick(value: string): void {
        console.log("Hello " + value);
    }
}
```
```html
<button (click)="onClick('World')">Font color</button>
```
- Two-way Data Binding
```html
<input [(ngModel)]="name" />
```
```javascript
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { FormsModule } from '@angular/forms'

import { AppComponent } from './app.component'

@NgModule({
  imports: [BrowserModule, FormsModule],
  declarations: [AppComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```
```html
<input [ngModel]="name" (ngModelChange)="name = $event" />
```
[Example](https://blog.johnwu.cc/images/a/196.gif)

## Directive
1. Components — directives with a template.
2. Structural directives — change the DOM layout by adding and removing DOM elements.
> [ngClass]="condition"
3. Attribute directives — change the appearance or behavior of an element, component, or another directive.
> *ngIf, *ngFor…

```javascript
import {Directive, ElementRef, Renderer, Input} from '@angular/core';

@Directive({
  selector: '[Icheck]',
})
export class RadioCheckbox {
   // custom logic here...
}
```
```html
<span Icheck>HEllo Directive</span>
```

Pipes
Lifecycle hooks
Component interaction
Observables & RxJS
Module & Routing
Unit Test 

Git Commands
Gitlab Flows
Gitlab & CI/CD

1.  
2.

https://blog.johnwu.cc/article/angular-4-%E6%95%99%E5%AD%B8-data-binding.html


[Google 首頁](https://google.com.tw)
