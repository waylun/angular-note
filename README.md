# Angular Notes
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
| Route         | `ng g command` --routing           |

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

## Pipes

 - AsyncPipe：Uses for Observable or Promise return lastest value
 - CurrencyPipe
 - DatePipe
 - DecimalPipe
 - DeprecatedCurrencyPipe：Use currency to format a number as currency.
 - DeprecatedDatePipe
 - DeprecatedDecimalPipe
 - DeprecatedPercentPipe
 - I18nPluralPipe：Maps a value to a string that pluralizes the value according to locale rules.
 - I18nSelectPipe：Generic selector that displays the string that matches the current value.
 - JsonPipe：Converts value into JSON string.
 - LowerCasePipe
 - UpperCasePipe
 - PercentPipe
 - SlicePipe：Creates a new List or String containing a subset (slice) of the elements.
 - TitleCasePipe：Transforms text to titlecase.

 
 ```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero-birthday',
  template: `<p>The hero's birthday is {{ birthday | date }}</p>`
})
export class HeroBirthdayComponent {
  birthday = new Date(1988, 4, 15); // April 15, 1988
}
 ```
 ```html
 <p>The hero's birthday is {{ birthday | date:"MM/dd/yy" }} </p>
 ```
> MM/dd/yy：04/15/88
```html
{{ birthday | date:'fullDate' | uppercase}}
```
> FRIDAY, APRIL 15, 1988

## Lifecycle Hooks

1. ngOnChanges()	
2. ngOnInit()
3. ngAfterViewInit()	
4. ngOnDestroy()	


> What is different between Constructor and ngOnInit: The constructor of the component is called when Angular constructs components tree. All lifecycle hooks are called as part of running change detection.


```javascript
export class App implements OnInit {
  constructor() {
     // Called first time before the ngOnInit()
  }

  ngOnInit() {
     // Called after the constructor and called  after the first ngOnChanges() 
  }
}
```

## Component Interaction

1. Parent ---> Child 
```javascript
import { Component, Input } from '@angular/core';
 
import { Hero } from './hero';
 
@Component({
  selector: 'app-hero-child',
  template: '
    <h3>{{hero.name}} says:</h3>
    <p>I, {{hero.name}}, am at your service, {{masterName}}.</p>
  '
})
export class HeroChildComponent {
  @Input() hero: Hero;
  @Input('master') masterName: string;
}
```
```javascript
import { Component } from '@angular/core';
 
import { HEROES } from './hero';
 
@Component({
  selector: 'app-hero-parent',
  template: '
    <h2>{{master}} controls {{heroes.length}} heroes</h2>
    <app-hero-child *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </app-hero-child>
  '
})
export class HeroParentComponent {
  heroes = HEROES;
  master = 'Master';
}
```
2. Parent <--- Child
```javascript
import { Component, EventEmitter, Input, Output } from '@angular/core';
 
@Component({
  selector: 'app-voter',
  template: '
    <h4>{{name}}</h4>
    <button (click)="vote(true)"  [disabled]="voted">Agree</button>
    <button (click)="vote(false)" [disabled]="voted">Disagree</button>
  '
})
export class VoterComponent {
  @Input()  name: string;
  @Output() onVoted = new EventEmitter<boolean>();
  voted = false;
 
  vote(agreed: boolean) {
    this.onVoted.emit(agreed);
    this.voted = true;
  }
}
```
```javascript
import { Component }      from '@angular/core';
 
@Component({
  selector: 'app-vote-taker',
  template: '
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>
    <app-voter *ngFor="let voter of voters"
      [name]="voter"
      (onVoted)="onVoted($event)">
    </app-voter>
  '
})
export class VoteTakerComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Mr. IQ', 'Ms. Universe', 'Bombasto'];
 
  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```
3. Parent ---> Child (Local Variable)
```javascript
import { Component, OnDestroy, OnInit } from '@angular/core';

@Component({
  selector: 'app-countdown-timer',
  template: '<p>{{message}}</p>'
})
export class CountdownTimerComponent implements OnInit, OnDestroy {

  intervalId = 0;
  message = '';
  seconds = 11;

  clearTimer() { clearInterval(this.intervalId); }

  ngOnInit()    { this.start(); }
  ngOnDestroy() { this.clearTimer(); }

  start() { this.countDown(); }
  stop()  {
    this.clearTimer();
    this.message = `Holding at T-${this.seconds} seconds`;
  }

  private countDown() {
    this.clearTimer();
    this.intervalId = window.setInterval(() => {
      this.seconds -= 1;
      if (this.seconds === 0) {
        this.message = 'Blast off!';
      } else {
        if (this.seconds < 0) { this.seconds = 10; } // reset
        this.message = `T-${this.seconds} seconds and counting`;
      }
    }, 1000);
  }
}
```
```javascript
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-lv',
  template: `
  <h3>Countdown to Liftoff (via local variable)</h3>
  <button (click)="timer.start()">Start</button>
  <button (click)="timer.stop()">Stop</button>
  <div class="seconds">{{timer.seconds}}</div>
  <app-countdown-timer #timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownLocalVarParentComponent { }
```
4. Parent <--- Child (@ViewChild())
>The local variable approach is simple and easy. But it is limited because the parent-child wiring must be done entirely within the parent template. The parent component itself has no access to the child.

>You can't use the local variable technique if an instance of the parent component class must read or write child component values or must call child component methods.

>When the parent component class requires that kind of access, inject the child component into the parent as a ViewChild.

```javascript
import {Component} from 'angular2/core';

@Component({
    selector: 'my-child',
    template: `
        <div>Child Component</div>
    `
})
export class ChildComponent {
    name:string = 'childName';
}
```
```javascript
import {Component,ViewChild,AfterViewInit} from 'angular2/core';
import {ChildComponent}         from './child';

@Component({
    selector: 'my-parent',
    template: `
       <div>Parent Component</div>
       <my-child></my-child>
    `,
    directives:[ChildComponent]
})
export class ParentComponent implements AfterViewInit{
    @ViewChild(ChildComponent)
    private child:ChildComponent;

    ngAfterViewInit() {
        console.log(this.child)
    }
}
```
Parent <---> Child (Observables & RxJS)
```javascript
import { Injectable } from '@angular/core';
import { Subject }    from 'rxjs';

@Injectable()
export class MissionService {

  // Observable string sources
  private missionAnnouncedSource = new Subject<string>();
  private missionConfirmedSource = new Subject<string>();

  // Observable string streams
  missionAnnounced$ = this.missionAnnouncedSource.asObservable();
  missionConfirmed$ = this.missionConfirmedSource.asObservable();

  // Service message commands
  announceMission(mission: string) {
    this.missionAnnouncedSource.next(mission);
  }

  confirmMission(astronaut: string) {
    this.missionConfirmedSource.next(astronaut);
  }
}
```
```javascript
import { Component }          from '@angular/core';
import { MissionService }     from './mission.service';

@Component({
  selector: 'app-mission-control',
  template: `
    <h2>Mission Control</h2>
    <button (click)="announce()">Announce mission</button>
    <app-astronaut *ngFor="let astronaut of astronauts"
      [astronaut]="astronaut">
    </app-astronaut>
    <h3>History</h3>
    <ul>
      <li *ngFor="let event of history">{{event}}</li>
    </ul>
  `,
  providers: [MissionService]
})
export class MissionControlComponent {
  astronauts = ['Lovell', 'Swigert', 'Haise'];
  history: string[] = [];
  missions = ['Fly to the moon!',
              'Fly to mars!',
              'Fly to Vegas!'];
  nextMission = 0;

  constructor(private missionService: MissionService) {
    missionService.missionConfirmed$.subscribe(
      astronaut => {
        this.history.push(`${astronaut} confirmed the mission`);
      });
  }

  announce() {
    let mission = this.missions[this.nextMission++];
    this.missionService.announceMission(mission);
    this.history.push(`Mission "${mission}" announced`);
    if (this.nextMission >= this.missions.length) { this.nextMission = 0; }
  }
}
```
[View More about Subject](https://ithelp.ithome.com.tw/articles/10188677)


## Others
- Module
- Route
- Unit Test 
- Git Commands
- Gitlab Flows

> Reference:  
https://angular.io/  
https://ithelp.ithome.com.tw/articles/10188861  
https://blog.johnwu.cc/article/angular-4-%E6%95%99%E5%AD%B8-data-binding.html  
https://ithelp.ithome.com.tw/articles/10195275  
https://ithelp.ithome.com.tw/m/articles/10194798  
