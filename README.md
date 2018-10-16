# Mulperi's Angular Handbook

Learn Angular basics quickly. This documentation is a work in progress and meant to be used when training new Angular developers along with the official documentation, examples and exercices.

# What is Angular?

Angular is a rapidly evolving JavaScript framework/platform for modern web application development. It is open source and led by Google's Angular Team. To be able to understand and do Angular development, you should know the basics of HTML, CSS and JavaScript.

-   https://angular.io/
-   TypeScript/JavaScript, HTML, CSS/SCSS

# Single Page Application

Single page application or SPA is a web page/application that offers a desktop application-like user experience. The content is updated dynamically with JavaScript so there is no page reload when you switch to another page or sub page. This is what you make with Angular.

# Building blocks of an Angular app

-   Module
-   Component
-   Template
-   Service
-   Routing
-   Store (optional)

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

Angular applications are modular and always have at least one module (root module). Modules can import data from other modules and declare components they are going to use. Modules are classes with @NgModule() decorator and .module.ts file extension.

If you have a large application, you can split features into modules and load them "lazily" - in other words: only when an user selects the feature. This greatly reduces your application's initial loading time. It is good practice to only load modules that are needed.

## Component and a view

Components are reusable custom elements you throw in the html. You define a component's selector (element name in the html-template) in the component decorator metadata. You can change the component selector prefix inside angular.json settings file, it's app by default.

Below is An example of some child components of "app component":

    <!-- app.component.html -->
    <app-header></app-header>
    <app-item-list></app-item-list>
    <app-footer></app-footer>

-   A component defines part of the application logic in it’s class
-   A template defines component’s view
-   A template is regular html with Angular template-syntax
-   A component is it’s **.ts**, **.html** and **.scss** files
-   .ts (class and component logic) & .html (template) & .scss (styles)

## Service

A service is a class with distinct purpose. A component delegates certain actions to a service. Like getting data from a server for example. If you are using a store in your application you usually want to call service from an **effect** and not from the component.

A service is a class with @Injectable() decorator. In it's metadata you tell Angular where you want to provide the service.

    @Injectable({
         providedIn: 'root'
    })
    export class DataService {}

When Angular creates an instance of a component, it checks the component constructor function for what dependencies the component needs. This is dependency injection: the component is depending on the service to be able to construct itself.

    @Component({...})
    export class AppComponent {
        constructor(private service: DataService) { }
    }

# Folder structure

-   Divide components to **Container components** and **Presentational components**
-   Container components take care of the logic of a view
-   Presentational components take care of how a view looks
-   Makes it easier to maintain code and find a specific application logic

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

-   Interpolation – Bind component data to template **{{data}}**
-   Property binding – Bind component data to a child component property, pass data to child component **[property]**
-   Event binding – Bind component to react a user event like mouse click or custom event **(event)**
-   Two-way binding – Component reacts to template event and also the template reflects what is happening in the component’s TypeScript-file **[(data)]**

Example code with data binding:

    INTERPLATION:       <li>{{ hero.nam e}}</li>
    PROPERTY BINDING:   <app-hero-detail [hero]="selectedHero"></app-hero-detail>
    EVENT BINDING:      <li (click)="selectHero(hero)"></li>

# Input() and Output()

If you want to pass down data to a child component with property binding, you need to use Input() decorator in the child component's class.

If on the other hand you want to send data back to the parent component, the child component needs to emit the event and send data inside the $event object with EventEmitter.

    export class UserlistComponent {
        @Input()
        data: User[];

        @Output()
        itemClick: EventEmitter<User> = new EventEmitter();

        onClick(item: User) {
            this.itemClick.emit(item);
        }
    }

When creating a custom event ("itemClick" in this case), you then need to listen and react to it in the parent component:

    <app-userlist (itemClick)="onItemClick($event)"></app-userlist>

The $event object content depends on the event that is used. In this case it will be a "User" and it will be passed as an argument to a function in the parent component called "onItemClick" and now the parent component handles what to do with the selected user.

# Directives and pipes

Directives modify the template dynamically. They are classes with @Directive() decorator. You can create your own directives or use Angular's default ones like *ngFor or *ngIf.
Structural directives alter the DOM and are marked with \*.

    <li *ngFor="let item of items">{{ item.name }}</li>
    <app-item-details *ngIf="itemSelected"></app-item-details>

Pipes alter values in the template like date for example

    <p>My birthday is {{ dateObject | date }}</p>

# Routing

Angular router enables you to navigate between views. Router takes the URL and navigates to corresponding component. Routing can be integrated to the root module in a simple application or it can be taken to it's own routing module which is usually preferred.

You can also route to a lazily loaded module that has it's own routing configurations.

# Observable

Reactive programming is handling asynchronous data streams which can emit many values over time. You can create a stream from almost anything.

In Angular, HTTP-requests return an observable that can be subscribed and only then is the request actually made

The selectors in ngrx/Store also return observables from the state tree. This way when your components subscribe to state changes, you can immediately see data change in the state reflecting to the component and the view.

RxJS is included in Angular.

-   RxJS - A JavaScript library for reactive programming using observables
-   Observable - A way to communicate between two parties: publisher and subscriber
-   Operators - tools to modify the stream like combine or concatenate several data streams for example

Read more about RxJS and observables below.

-   RxJS Overview: https://rxjs-dev.firebaseapp.com/guide/overview
-   Observables: https://rxjs-dev.firebaseapp.com/guide/observable

# Tools

Getting an Angular app running is very easy. Install the following tools to start development.

-   Chrome & Redux Devtools extension
-   Node.js (& npm)
-   Angular CLI

## Node.js and npm

Angular uses npm - "world's largest software registry" - for dependency management. First install Node.js to get access to npm.

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

# Exercises

## Exercise 1

-   Get to know the tools
-   Create new Angular project
-   Study the boilerplate (root folder, config files)
-   Study a module and a component
-   Generate a new component manually or with CLI and use the component
-   Import and declare components in a module from a “barrell” file

## Exercise 2

-   Create new service
-   Create mock data (JSON) in to assets folder and request it with HttpClient inside the service
-   Use event binding in a component
-   Use service inside a component
-   Subscribe to a observable inside a component or a template and compare the differences (remember to unsubscribe if you subscribe in component)
-   Use structural directives *ngFor and *ngIf
-   Use conditional styling in a template [class.active]="boolean”

## Exercise 3

-   Try communication between components using @Input and @Output
-   Use property binding to send data to a child component
-   Use EventEmitter to emit a custom event and react to it in the parent component
-   Study observable: https://rxjs-dev.firebaseapp.com/guide/observable

## Exercise 4

-   Build a routing
-   Test a route guard
-   Create a module that is lazily loaded

# i18n

"___Internationalization__ is the process of designing and preparing your app to be usable in different languages. __Localization__ is the process of translating your internationalized app into specific languages for particular locales._" - Angular.io

Angular has built in internalization tools that are very easy to use.

Example of an usage of i18n attribute in a template:

    <h1 i18n="site header|An introduction header for this sample@@introductionHeader">Hello i18n!</h1>

    <!-- cheatsheet -->
    <!-- i18n="context|information@@customid" -->

## Exercise

- Study Angular i18n tools (https://angular.io/guide/i18n)
- Use i18n syntax in a template 

Extract the translation file, translate it and run development server with language configuration:

    ng xi18n --i18n-locale fr --output-path src/locale
    ng serve --configuration=fr --aot

# ngrx/Store

ngrx/Store is a Redux inspired (https://redux.js.org/introduction) state management for Angular. In a larger application it is preferred to use something like ngrx to manage the state of the application.

Imagine the application state as a JavaScript object (key-value pairs) or a tree-like structure. Each key or branch in the state tree is a slice or feature of the state.

In addition to the state object, the store consists of:

-   Actions
-   Reducers
-   Effects
-   Selectors

## Why store?

One of the main benefits of the store is that the application state is in one place and is abstracted. This additional layer of abstraction of the state helps to keep the code clean and prevents too complex communication "chains" where components pass data to child components.

For example, normally you would pass the data down to a child component with property binding like this:

<child-component [data]="data"></child-component>

With store, you dispatch an action in the parent component that updates the state with the data. In the child component you subscribe to the particular "slice" of the state that holds the data that component is interested in.

**There is nothing wrong with property binding or emitting events to parent component! But if it's happening a lot, it may reduce the ability to reuse your components.**

## Updating application state

To update the state, you dispatch an **action**. **Reducer** listens to actions and return a new state if needed.

**Effects** also listen to actions and do asynchronous tasks on the background. Like getting data from the server for example. Effects usually return new actions like "DATA_LOAD_SUCCESS".

With **selectors** you define wich part of the state tree you want to subscsribe to.

    ngrx/Store data flow (roughly)

    Action > Reducer > Store > View
           > Effect > Service > Action > Reducer > Store > View

## Excercise

    npm i @ngrx/store @ngrx/router-store @ngrx/effects @ngrx/store-devtools ngrx-store-freeze

-   Install Chrome Redux DevTools extension https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd
-   Study the ngrx packages and how to apply them to your Angular project https://github.com/ngrx/store
-   Build a Store and make sure that router-reducer can be seen in the state with DevTools
-   Make a new branch (feature) to the state and actions, reducer, effects and selectors to it

# Good practices

-   Separating container and presentational components
-   Block Element Modifier naming convention for CSS classes (http://getbem.com/introduction/)
