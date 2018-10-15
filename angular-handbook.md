# Mulperi's Angular Handbook

Learn Angular basics quickly. This documentation is a work in progress and meant to be used in Angular training.

# What is Angular?

Angular is a rapidly evolving JavaScript framework/platform for modern web application development. It is open source and led by Google's Angular Team. To be able to understand and do Angular development, you should know the basics of HTML, CSS and JavaScript.

- https://angular.io/
- TypeScript/JavaScript, HTML, CSS/SCSS

# Single Page Application

Single page application or SPA is a web page/application that offers a desktop application-like user experience. The content is updated dynamically with JavaScript so there is no page reload when you switch to another page or sub page. This is what you make with Angular.

# Building blocks of an Angular app

- Module
- Component
- Template
- Service
- Routing
- Store

## Decorator

A decorator and it's metadata tells Angular what to do with the class: is it a component, a module or a service etc.

Example of different kinds of decorators:

    @NgModule({
        declarations: [AppComponent],
        imports: [BrowserModule]
        bootstrap: [AppComponent] // Only for root module
    }) 
    Export class AppModule {}

    @Component({
        selector: 'app-list-item',
        templateUrl: './app-list-item.component.html',
        styleUrls: ['./app-list-item.component.scss']
    })
    Export class ListItemComponent {}

    @Injectable({
        providedIn: 'root'
    })
    Export class MyDataService {}

## Module

Angular applications always have at least one module (root module). Modules can import other modules and declare components they are going to use. Modules are classes with @NgModule() decorator and .module.ts file extension.

If you have a large application, you can split features into modules and load them "lazily" - in other words: only when an user selects the feature. This greatly reduces your application's initial loading time. It is good practice to only load modules that are needed.

## Component and a view

Components are reusable custom elements you throw in the html. You define a component's selector (element name in the html-template) in the component decorator metadata. You can change the component selector prefix inside angular.json settings file, it's app by default.

    <!-- app.component.ts -->
    <app-header></app-header>
    <app-item-list></app-item-list>
    <app-footer></app-footer>

- A component defines part of the application logic in it’s class
- A template defines component’s view
- A template is regular html with Angular template-syntax
- A component is it’s __.ts__, __.html__ and __.scss__ files
- .ts (class and component logic) & .html (template) & .scss (styles)

## Service

A service is a class with distinct purpose. A component delegates certain actions to a service. Like getting data from a server for example. If you are using a store in your application you usually want to call service from an __effect__ and not from the component.

A service is a class with @Injectable() decorator. In it's metadata you tell Angular where you want to provide the service. 

    @Injectable({
         providedIn: 'root'
    })
    export class DataService {}

When Angular creates an instance of a component, it checks the component constructor function for what dependencies the component needs. This is dependency injection.

    @Component({...})
    export class AppComponent {
        constructor(private service: DataService) { }
    }

# Folder structure

- Divide components to __Container components__ and __Presentational components__
- Container-components take care of the logic of a view
- Presentational components take care of how a view looks
- Makes it easier to maintain code and find a specific application logic

An example of a project directory

    src/
        app/
            components/
                header/
                footer/
            containers/
                main-page/
                settings-page/
            app.module.ts

# Data binding

Data binding is a way to reflect data changes in one place to another place. There are few different kinds of data binding with each having it's own syntax.

- Interpolation – Bind component data to template __{{data}}__
- Property binding – Bind component data to an element property, send data to child component __[property]__
- Event binding – Bind component to react a user event like mouse click or custom event __(event)__
- Two-way binding – Component reacts to template event and also the template reflects what is happening in the component’s TypeScript-file __[(data)]__

Example code with data binding:

    INTERPLATION:       <li>{{ hero.nam e}}</li>
    PROPERTY BINDING:   <app-hero-detail [hero]="selectedHero"></app-hero-detail>
    EVENT BINDING:      <li (click)="selectHero(hero)"></li>

# Input() and Output()

# Directives and pipes

Directives modify the template dynamically. They are classes with @Directive() decorator. You can create your own directives or use Angular's default ones like *ngFor or *ngIf.
Structural directives alter the DOM and are marked with *.

    <li *ngFor="let item of items">{{ item.name }}</li>
    <app-item-details *ngIf="itemSelected"></app-item-details>

Pipes alter values in the template like date for example

    <p>My birthday is {{ dateObject | date }}</p>

# Routing

Angular router enables you to navigate between views. Router takes the URL and navigates to corresponding component. Routing can be integrated to the root module in a simple application or it can be taken to it's own routing module which is usually preferred.

# Observable

- RxJS - A JavaScript library for reactive programming using observables
- Observable - A way to communicate between two parties: publisher and subscriber
- Asynchronous, temporary or never ending stream which can emit many values over time
- Operators - tools to modify the stream like combine or concatenate several data streams for example
- In Angular, HTTP-requests return an observable that can be subscribed and only then is the request actually made

Read more about RxJS and observables below.
- RxJS Overview: https://rxjs-dev.firebaseapp.com/guide/overview
- Observables: https://rxjs-dev.firebaseapp.com/guide/observable

# Tools
## Node.js and npm

Angular uses npm - "world's largest software registry" - for dependency management. First install Node.js.

    http://nodejs.com/

## Angular CLI

Angular has a good command line tool for managing your project. Install it globally via npm.

    npm install -g @angular/cli
    ng new myapp --style scss --prefix myapp
    cd myapp
    ng serve

## Chrome and Redux DevTools

For an application that utilizes ngrx/Store use Chrome extension called Redux DevTools to be able to view the state and what is happening in the store.

    https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd

 # ngrx/Store

 ngrx/Store is a Redux inspired (https://redux.js.org/introduction) state management for Angular. In a larger application it is preferred to use something like ngrx to manage the state of the application.
 
 This additional layer of abstraction of the state helps to keep the code clean and prevents too complex communication "chains" where components pass data to child components. 
 
 For example, normally you would pass the data down to a child component with property binding like this:
    
    <child-component [data]="data"></child-component>

With store, you dispatch an action in the parent component that updates the state with the data. In the child component you subscribe to the particular "slice" of the state that holds the data that component is interested in.

__There is nothing wrong with property binding or emitting events to parent component! But if it's happening a lot, it may reduce the ability to reuse your components.__

In addition to the state object, the store consists of:

- Actions
- Reducers
- Effects
- Selectors

# Good practices

- Separating container and presentational components
- Block Element Modifier naming convention for CSS classes (http://getbem.com/introduction/)

