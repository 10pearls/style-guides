# AngularJS styleguide (ES2015)

### Architecture, file structure, components, one-way dataflow and best practices

---

*A sensible style-guide for teams inspired by [@toddmotto](//twitter.com/toddmotto)*

This architecture and style-guide has been rewritten from the ground up for ES2015, the changes in Angular 1.5+ for future-upgrading your application to Angular 2. This guide includes new best practices for one-way data flow, event delegation, component architecture and component routing.

## Table of Contents

  1. [Modular architecture](#modular-architecture)
    1. [Theory](#module-theory)
    1. [Root module](#root-module)
    1. [Component module](#component-module)
    1. [Common module](#common-module)
    1. [Low-level modules](#low-level-modules)
    1. [File naming conventions](#file-naming-conventions)
    1. [Scalable file structure](#scalable-file-structure)
  1. [Components](#components)
    1. [Theory](#component-theory)
    1. [Supported properties](#supported-properties)
    1. [Controllers](#controllers)
    1. [One-way dataflow and Events](#one-way-dataflow-and-events)
    1. [Stateful Components](#stateful-components)
    1. [Stateless Components](#stateless-components)
    1. [Routed Components](#routed-components)
  1. [Directives](#directives)
    1. [Theory](#directive-theory)
    1. [Recommended properties](#recommended-properties)
    1. [Constants or Classes](#constants-or-classes)
  1. [Services](#services)
    1. [Theory](#service-theory)
    1. [Classes for Service](#classes-for-service)
  1. [Linting](#linting)
  1. [ES2015 and Tooling](#es2015-and-tooling)
  1. [State management](#state-management)
  1. [Resources](#resources)
  1. [Documentation](#documentation)
  1. [Contributing](#contributing)

# Modular architecture

Each module in an Angular app is a module component. A module component is the root definition for that module that encapsulates the logic, templates, routing and child components.

### Module theory

The design in the modules maps directly to our folder structure, which keeps things maintainable and predictable. We should ideally have three high-level modules: root, component and common. The root module defines the base module that bootstraps our app, and the corresponding template. We then import our component and common modules into the root module to include our dependencies. The component and common modules then require lower-level component modules, which contain our components, controllers, services, directives, filters and tests for each reusable feature.

**[Back to top](#table-of-contents)**

### Root module

A root module begins with a root component that defines the base element for the entire application, with a routing outlet defined, example shown using `ui-view` from `ui-router`.

```js
// app.component.js
const AppComponent = {
  template: `
    <header>
        Hello world
    </header>
    <div>
        <div ui-view></div>
    </div>
    <footer>
        Copyright MyApp 2016.
    </footer>
  `
};

export default AppComponent;
```

A root module is then created, with `AppComponent` imported and registered with `.component('app', AppComponent)`. Further imports for submodules (component and common modules) are made to include all components relevant for the application.

```js
// app.js
import angular from 'angular';
import uiRouter from 'angular-ui-router';
import AppComponent from './app.component';
import Components from './components/components';
import Common from './common/common';

const root = angular
  .module('app', [
    Components,
    Common,
    uiRouter
  ])
  .component('app', AppComponent)
  .name;

export default root;
```

**[Back to top](#table-of-contents)**

### Component module

A Component module is the container reference for all reusable components. See above how we import `Components` and inject them into the Root module, this gives us a single place to import all components for the app. These modules we require are decoupled from all other modules and thus can be moved into any other application with ease.

```js
import angular from 'angular';
import Calendar from './calendar';
import Events from './events';

const components = angular
  .module('app.components', [
    Calendar,
    Events
  ])
  .name;

export default components;
```

**[Back to top](#table-of-contents)**

### Common module

The Common module is the container reference for all application specific components, that we don't want to use in another application. This can be things like layout, navigation and footers. See above how we import `Common` and inject them into the Root module, this gives us a single place to import all common components for the app.

```js
import angular from 'angular';
import Nav from './nav';
import Footer from './footer';

const common = angular
  .module('app.common', [
    Nav,
    Footer
  ])
  .name;

export default common;
```

**[Back to top](#table-of-contents)**

### Low-level modules

Low-level modules are individual component modules that contain the logic for each feature block. These will each define a module, to be imported to a higher-level module, such as a component or common module, an example below. Always remember to add the `.name` suffix to each `export` when creating a _new_ module, not when referencing one. You'll noticed routing definitions also exist here, we'll come onto this in later chapters in this guide.

```js
import angular from 'angular';
import uiRouter from 'angular-ui-router';
import CalendarComponent from './calendar.component';

const calendar = angular
  .module('calendar', [
    uiRouter
  ])
  .component('calendar', CalendarComponent)
  .config(($stateProvider, $urlRouterProvider) => {
    $stateProvider
      .state('calendar', {
        url: '/calendar',
        component: 'calendar'
      });
    $urlRouterProvider.otherwise('/');
  })
  .name;

export default calendar;
```

**[Back to top](#table-of-contents)**

### File naming conventions

Keep it simple and lowercase, use the component name, e.g. `calendar.*.js*`, `calendar-grid.*.js` - with the name of the type of file in the middle. Use `index.js` for the module definition file, so you can import the module by directory name.

```
index.js
calendar.controller.js
calendar.component.js
calendar.service.js
calendar.directive.js
calendar.filter.js
calendar.spec.js
```

**[Back to top](#table-of-contents)**

### Scalable file structure

File structure is extremely important, this describes a scalable and predictable structure. An example file structure to illustrate a modular component architecture.

```
├── app/
│   ├── components/
│   │  ├── calendar/
│   │  │  ├── index.js
│   │  │  ├── calendar.controller.js
│   │  │  ├── calendar.component.js
│   │  │  ├── calendar.service.js
│   │  │  ├── calendar.spec.js
│   │  │  └── calendar-grid/
│   │  │     ├── index.js
│   │  │     ├── calendar-grid.controller.js
│   │  │     ├── calendar-grid.component.js
│   │  │     ├── calendar-grid.directive.js
│   │  │     ├── calendar-grid.filter.js
│   │  │     └── calendar-grid.spec.js
│   │  └── events/
│   │     ├── index.js
│   │     ├── events.controller.js
│   │     ├── events.component.js
│   │     ├── events.directive.js
│   │     ├── events.service.js
│   │     ├── events.spec.js
│   │     └── events-signup/
│   │        ├── index.js
│   │        ├── events-signup.controller.js
│   │        ├── events-signup.component.js
│   │        ├── events-signup.service.js
│   │        └── events-signup.spec.js
│   ├── common/
│   │  ├── nav/
│   │  │     ├── index.js
│   │  │     ├── nav.controller.js
│   │  │     ├── nav.component.js
│   │  │     ├── nav.service.js
│   │  │     └── nav.spec.js
│   │  └── footer/
│   │        ├── index.js
│   │        ├── footer.controller.js
│   │        ├── footer.component.js
│   │        ├── footer.service.js
│   │        └── footer.spec.js
│   ├── app.js
│   └── app.component.js
└── index.html
```

The high level folder structure simply contains `index.html` and `app/`, a directory in which all our root, component, common and low-level modules live.

**[Back to top](#table-of-contents)**

# Components

### Component theory

Components are essentially templates with a controller. They are _not_ Directives, nor should you replace Directives with Components, unless you are upgrading "template Directives" with controllers, which are best suited as a component. Components also contain bindings that define inputs and outputs for data and events, lifecycle hooks and the ability to use one-way data flow and event Objects to get data back up to a parent component. These are the new defacto standard in Angular 1.5 and above. Everything template and controller driven that we create will likely be a component, which may be a stateful, stateless or routed component. You can think of a "component" as a complete piece of code, not just the `.component()` definition Object. Let's explore some best practices and advisories for components, then dive into how you should be structuring them via stateful, stateless and routed component concepts.

**[Back to top](#table-of-contents)**

### Supported properties

These are the supported properties for `.component()` that you can/should use:

| Property | Support |
|---|---|
| bindings | Yes, use `'@'`, `'<'`, `'&'` only |
| controller | Yes |
| controllerAs | Yes, default is `$ctrl` |
| require | Yes (new Object syntax) |
| template | Yes |
| templateUrl | Yes |
| transclude | Yes |

**[Back to top](#table-of-contents)**

### Controllers

Controllers should only be used alongside components, never anywhere else. If you feel you need a controller, what you really need is likely a stateless component to manage that particular piece of behaviour.

Here are some advisories for using `Class` for controllers:

* Always use the `constructor` for dependency injection purposes
* Don't export the `Class` directly, export its name to allow `$inject` annotations
* If you need to access the lexical scope, use arrow functions
* Alternatively to arrow functions, `let ctrl = this;` is also acceptable and may make more sense depending on the use case
* Bind all public functions directly to the `Class`
* Make use of the appropriate lifecycle hooks, `$onInit`, `$onChanges`, `$postLink` and `$onDestroy`
  * Note: `$onChanges` is called before `$onInit`, see [resources](#resources) section for articles detailing this in more depth
* Use `require` alongside `$onInit` to reference any inherited logic
* Do not override the default `$ctrl` alias for the `controllerAs` syntax, therefore do not use `controllerAs` anywhere

**[Back to top](#table-of-contents)**

### One-way dataflow and Events

One-way dataflow was introduced in Angular 1.5, and redefines component communication.

Here are some advisories for using one-way dataflow:

* In components that receive data, always use one-way databinding syntax `'<'`
* _Do not_ use `'='` two-way databinding syntax anymore, anywhere
* Components that have `bindings` should use `$onChanges` to clone the one-way binding data to break Objects passing by reference and updating the parent data
* Use `$event` as a function argument in the parent method (see stateful example below `$ctrl.addTodo($event)`)
* Pass an `$event: {}` Object back up from a stateless component (see stateless example below `this.onAddTodo`).
  * Bonus: Use an `EventEmitter` wrapper with `.value()` to mirror Angular 2, avoids manual `$event` Object creation
* Why? This mirrors Angular 2 and keeps consistency inside every component. It also makes state predictable.

**[Back to top](#table-of-contents)**

### Stateful components

Let's define what we'd call a "stateful component".

* Fetches state, essentially communicating to a backend API through a service
* Does not directly mutate state
* Renders child components that mutate state
* Also referred to as smart/container components

An example of a stateful component, complete with its low-level module definition (this is only for demonstration, so some code has been omitted for brevity):

```js
/* ----- todo/todo.component.js ----- */
import controller from './todo.controller';

const TodoComponent = {
  controller,
  template: `
    <div class="todo">
      <todo-form
        todo="$ctrl.newTodo"
        on-add-todo="$ctrl.addTodo($event);"></todo-form>
      <todo-list
        todos="$ctrl.todos"></todo-list>
    </div>
  `
};

export default TodoComponent;

/* ----- todo/todo.controller.js ----- */
class TodoController {
  constructor(TodoService) {
    this.todoService = TodoService;
  }
  $onInit() {
    this.newTodo = {
      title: '',
      selected: false
    };
    this.todos = [];
    this.todoService.getTodos().then(response => this.todos = response);
  }
  addTodo({ todo }) {
    if (!todo) return;
    this.todos.unshift(todo);
    this.newTodo = {
      title: '',
      selected: false
    };
  }
}

TodoController.$inject = ['TodoService'];

export default TodoController;

/* ----- todo/index.js ----- */
import angular from 'angular';
import TodoComponent from './todo.component';

const todo = angular
  .module('todo', [])
  .component('todo', TodoComponent)
  .name;

export default todo;
```

This example shows a stateful component, that fetches state inside the controller, through a service, and then passes it down into stateless child components. Notice how there are no Directives being used such as `ng-repeat` and friends inside the template. Instead, data and functions are delegated into `<todo-form>` and `<todo-list>` stateless components.

**[Back to top](#table-of-contents)**

### Stateless components

Let's define what we'd call a "stateless component".

* Has defined inputs and outputs using `bindings: {}`
* Data enters the component through attribute bindings (inputs)
* Data leaves the component through events (outputs)
* Mutates state, passes data back up on-demand (such as a click or submit event)
* Doesn't care where data comes from - it's stateless
* Are highly reusable components
* Also referred to as dumb/presentational components

An example of a stateless component (let's use `<todo-form>` as an example), complete with its low-level module definition (this is only for demonstration, so some code has been omitted for brevity):

```js
/* ----- todo/todo-form/todo-form.component.js ----- */
import controller from './todo-form.controller';

const TodoFormComponent = {
  bindings: {
    todo: '<',
    onAddTodo: '&'
  },
  controller,
  template: `
    <form name="todoForm" ng-submit="$ctrl.onSubmit();">
      <input type="text" ng-model="$ctrl.todo.title">
      <button type="submit">Submit</button>
    </form>
  `
};

export default TodoFormComponent;

/* ----- todo/todo-form/todo-form.controller.js ----- */
class TodoFormController {
  constructor(EventEmitter) {
      this.EventEmitter = EventEmitter;
  }
  $onChanges(changes) {
    if (changes.todo) {
      this.todo = Object.assign({}, this.todo);
    }
  }
  onSubmit() {
    if (!this.todo.title) return;
    // with EventEmitter wrapper
    this.onAddTodo(
      this.EventEmitter({
        todo: this.todo
      });
    );
    // without EventEmitter wrapper
    this.onAddTodo({
      $event: {
        todo: this.todo
      }
    });
  }
}

TodoFormController.$inject = ['EventEmitter'];

export default TodoFormController;

/* ----- todo/todo-form/index.js ----- */
import angular from 'angular';
import TodoFormComponent from './todo-form.component';

const todoForm = angular
  .module('todo.form', [])
  .component('todoForm', TodoFormComponent)
  .value('EventEmitter', payload => ({ $event: payload}))
  .name;

export default todoForm;
```

Note how the `<todo-form>` component fetches no state, it simply receives it, mutates an Object via the controller logic associated with it, and passes it back to the parent component through the property bindings. In this example, the `$onChanges` lifecycle hook makes a clone of the initial `this.todo` binding Object and reassigns it, which means the parent data is not affected until we submit the form, alongside one-way data flow new binding syntax `'<'`.

**[Back to top](#table-of-contents)**

### Routed components

Let's define what we'd call a "routed component".

* It's essentially a stateful component, with routing definitions
* No more `router.js` files
* We use Routed components to define their own routing logic
* Data "input" for the component is done via the route resolve (optional, still available in the controller with service calls)

For this example, we're going to take the existing `<todo>` component, refactor it to use a route definition and `bindings` on the component which receives data (the secret here with `ui-router` is the `resolve` properties we create, in this case `todoData` directly map across to `bindings` for us). We treat it as a routed component because it's essentially a "view":

```js
/* ----- todo/todo.component.js ----- */
import controller from './todo.controller';

const TodoComponent = {
  bindings: {
    todoData: '<'
  },
  controller,
  template: `
    <div class="todo">
      <todo-form
        todo="$ctrl.newTodo"
        on-add-todo="$ctrl.addTodo($event);"></todo-form>
      <todo-list
        todos="$ctrl.todos"></todo-list>
    </div>
  `
};

export default TodoComponent;

/* ----- todo/todo.controller.js ----- */
class TodoController {
  constructor() {}
  $onInit() {
    this.newTodo = {
      title: '',
      selected: false
    };
  }
  $onChanges(changes) {
    if (changes.todoData) {
      this.todos = Object.assign({}, this.todoData);
    }
  }
  addTodo({ todo }) {
    if (!todo) return;
    this.todos.unshift(todo);
    this.newTodo = {
      title: '',
      selected: false
    };
  }
}

export default TodoController;

/* ----- todo/todo.service.js ----- */
class TodoService {
  constructor($http) {
    this.$http = $http;
  }
  getTodos() {
    return this.$http.get('/api/todos').then(response => response.data);
  }
}

TodoService.$inject = ['$http'];

export default TodoService;

/* ----- todo/index.js ----- */
import angular from 'angular';
import uiRouter from 'angular-ui-router';
import TodoComponent from './todo.component';
import TodoService from './todo.service';

const todo = angular
  .module('todo', [
    uiRouter
  ])
  .component('todo', TodoComponent)
  .service('TodoService', TodoService)
  .config(($stateProvider, $urlRouterProvider) => {
    $stateProvider
      .state('todos', {
        url: '/todos',
        component: 'todo',
        resolve: {
          todoData: TodoService => TodoService.getTodos();
        }
      });
    $urlRouterProvider.otherwise('/');
  })
  .name;

export default todo;
```

**[Back to top](#table-of-contents)**

# Directives

### Directive theory

Directives gives us `template`, `scope` bindings, `bindToController`, `link` and many other things. The usage of these should be carefully considered now that `.component()` exists. Directives should not declare templates and controllers anymore, or receive data through bindings. Directives should be used solely for decorating the DOM. By this, it means extending existing HTML - created with `.component()`. In a simple sense, if you need custom DOM events/APIs and logic, use a Directive and bind it to a template inside a component. If you need a sensible amount of DOM manipulation, there is also the `$postLink` lifecycle hook to consider, however this is not a place to migrate all your DOM manipulation to, use a Directive if you can for non-Angular things.

Here are some advisories for using Directives:

* Never use templates, scope, bindToController or controllers
* Always `restrict: 'A'` with Directives
* Use compile and link where necessary
* Remember to destroy and unbind event handlers inside `$scope.$on('$destroy', fn);`

**[Back to top](#table-of-contents)**

### Recommended properties

Due to the fact directives support most of what `.component()` does (template directives were the original component), it's recommended to limit your directive Object definitions to only these properties, to avoid using directives incorrectly:

| Property | Use it? | Why |
|---|---|---|
| bindToController | No | Use `bindings` in components |
| compile | Yes | For pre-compile DOM manipulation/events |
| controller | No | Use a component |
| controllerAs | No | Use a component |
| link functions | Yes | For pre/post DOM manipulation/events |
| multiElement | Yes | [See docs](https://docs.angularjs.org/api/ng/service/$compile#-multielement-) |
| priority | Yes | [See docs](https://docs.angularjs.org/api/ng/service/$compile#-priority-) |
| require | No | Use a component |
| restrict | Yes | Defines directive usage, always use `'A'` |
| scope | No | Use a component |
| template | No | Use a component |
| templateNamespace | Yes (if you must) | [See docs](https://docs.angularjs.org/api/ng/service/$compile#-templatenamespace-) |
| templateUrl | No | Use a component |
| transclude | No | Use a component |

**[Back to top](#table-of-contents)**

### Constants or Classes

There are a few ways to approach using ES2015 and directives, either with an arrow function and easier assignment, or using an ES2015 `Class`. Choose what's best for you or your team, keep in mind Angular 2 uses `Class`.

Here's an example using a constant with an Arrow function an expression wrapper `() => ({})` returning an Object literal (note the usage differences inside `.directive()`):

```js
/* ----- todo/todo-autofocus.directive.js ----- */
import angular from 'angular';

const TodoAutoFocus = ($timeout) => ({
  restrict: 'A',
  link($scope, $element, $attrs) {
    $scope.$watch($attrs.todoAutofocus, (newValue, oldValue) => {
      if (!newValue) {
        return;
      }
      $timeout(() => $element[0].focus());
    });
  }
});

TodoAutoFocus.$inject = ['$timeout'];

export default TodoAutoFocus;

/* ----- todo/index.js ----- */
import angular from 'angular';
import TodoComponent from './todo.component';
import TodoAutofocus from './todo-autofocus.directive';

const todo = angular
  .module('todo', [])
  .component('todo', TodoComponent)
  .directive('todoAutofocus', TodoAutoFocus)
  .name;

export default todo;
```

Or using ES2015 `Class` (note manually calling `new TodoAutoFocus` when registering the directive) to create the Object:

```js
/* ----- todo/todo-autofocus.directive.js ----- */
import angular from 'angular';

class TodoAutoFocus {
  constructor($timeout) {
    this.restrict = 'A';
    this.$timeout = $timeout;
  }
  link($scope, $element, $attrs) {
    $scope.$watch($attrs.todoAutofocus, (newValue, oldValue) => {
      if (!newValue) {
        return;
      }
      this.$timeout(() => $element[0].focus());
    });
  }
}

TodoAutoFocus.$inject = ['$timeout'];

export default TodoAutoFocus;

/* ----- todo/index.js ----- */
import angular from 'angular';
import TodoComponent from './todo.component';
import TodoAutofocus from './todo-autofocus.directive';

const todo = angular
  .module('todo', [])
  .component('todo', TodoComponent)
  .directive('todoAutofocus', () => new TodoAutoFocus)
  .name;

export default todo;
```

**[Back to top](#table-of-contents)**

# Services

### Service theory

Services are essentially containers for business logic that our components shouldn't request directly. Services contain other built-in or external services such as `$http`, that we can then inject into component controllers elsewhere in our app. We have two ways of doing services, using `.service()` or `.factory()`. With ES2015 `Class`, we should only use `.service()`, complete with dependency injection annotation using `$inject`.

**[Back to top](#table-of-contents)**

### Classes for Service

Here's an example implementation for our `<todo>` app using ES2015 `Class`:

```js
/* ----- todo/todo.service.js ----- */
class TodoService {
  constructor($http) {
    this.$http = $http;
  }
  getTodos() {
    return this.$http.get('/api/todos').then(response => response.data);
  }
}

TodoService.$inject = ['$http'];

export default TodoService;

/* ----- todo/index.js ----- */
import angular from 'angular';
import TodoComponent from './todo.component';
import TodoService from './todo.service';

const todo = angular
  .module('todo', [])
  .component('todo', TodoComponent)
  .service('TodoService', TodoService)
  .name;

export default todo;
```

**[Back to top](#table-of-contents)**

# Linting

##### ESLint

* Use [ESLint](https://github.com/eslint/eslint) for linting your JavaScript and be sure to customize the options file and include it in your source control
```javascript
{
  "env": {
    "browser": true,
    "node": false,
    "es6": true
  },
  "globals": {
    "document": false,
    "navigator": false,
    "window": false
  },
  "parser": "babel-eslint",
  "rules": {
    "accessor-pairs": 2,
    "arrow-spacing": [
      2,
      {
        "before": true,
        "after": true
      }
    ],
    "brace-style": [
      2,
      "stroustrup",
      {
        "allowSingleLine": true
      }
    ],
    "comma-dangle": [
      2,
      "never"
    ],
    "comma-spacing": [
      2,
      {
        "before": false,
        "after": true
      }
    ],
    "comma-style": [
      2,
      "last"
    ],
    "constructor-super": 2,
    "curly": [
      2,
      "multi-line"
    ],
    "dot-location": [
      2,
      "property"
    ],
    "eol-last": 2,
    "eqeqeq": [
      2,
      "allow-null"
    ],
    "generator-star-spacing": [
      2,
      {
        "before": false,
        "after": true
      }
    ],
    "handle-callback-err": [
      2,
      "^(err|error)$"
    ],
    "indent": [
      2,
      2,
      {
        "SwitchCase": 1
      }
    ],
    "key-spacing": [
      2,
      {
        "beforeColon": false,
        "afterColon": true
      }
    ],
    "new-cap": [
      2,
      {
        "newIsCap": true,
        "capIsNew": false
      }
    ],
    "new-parens": 2,
    "no-array-constructor": 2,
    "no-caller": 2,
    "no-class-assign": 2,
    "no-cond-assign": 2,
    "no-const-assign": 2,
    "no-control-regex": 2,
    "no-debugger": 2,
    "no-delete-var": 2,
    "no-dupe-args": 2,
    "no-dupe-keys": 2,
    "no-duplicate-case": 2,
    "no-empty-character-class": 2,
    "no-eval": 2,
    "no-ex-assign": 2,
    "no-extend-native": 2,
    "no-extra-bind": 2,
    "no-extra-boolean-cast": 2,
    "no-extra-parens": [
      2,
      "functions"
    ],
    "no-fallthrough": 2,
    "no-floating-decimal": 2,
    "no-func-assign": 2,
    "no-implied-eval": 2,
    "no-inner-declarations": [
      2,
      "functions"
    ],
    "no-invalid-regexp": 2,
    "no-irregular-whitespace": 2,
    "no-iterator": 2,
    "no-label-var": 2,
    "no-labels": 2,
    "no-lone-blocks": 2,
    "no-mixed-spaces-and-tabs": 2,
    "no-multi-spaces": 2,
    "no-multi-str": 2,
    "no-multiple-empty-lines": [
      2,
      {
        "max": 1
      }
    ],
    "no-native-reassign": 2,
    "no-negated-in-lhs": 2,
    "no-new": 2,
    "no-new-func": 2,
    "no-new-object": 2,
    "no-new-require": 2,
    "no-new-wrappers": 2,
    "no-obj-calls": 2,
    "no-octal": 2,
    "no-octal-escape": 2,
    "no-proto": 2,
    "no-redeclare": 2,
    "no-regex-spaces": 2,
    "no-return-assign": 2,
    "no-self-compare": 2,
    "no-sequences": 2,
    "no-shadow-restricted-names": 2,
    "no-spaced-func": 2,
    "no-sparse-arrays": 2,
    "no-this-before-super": 2,
    "no-throw-literal": 2,
    "no-trailing-spaces": [
      2,
      {
        "skipBlankLines": true
      }
    ],
    "no-undef": 2,
    "no-undef-init": 2,
    "no-unexpected-multiline": 2,
    "no-unneeded-ternary": 2,
    "no-unreachable": 2,
    "no-unused-vars": [
      2,
      {
        "vars": "all",
        "args": "none"
      }
    ],
    "no-useless-call": 2,
    "no-with": 2,
    "one-var": [
      2,
      {
        "initialized": "never"
      }
    ],
    "operator-linebreak": [
      2,
      "after"
    ],
    "quotes": [
      2,
      "single",
      "avoid-escape"
    ],
    "radix": 2,
    "semi": [
      2,
      "always"
    ],
    "space-before-blocks": [
      2,
      "always"
    ],
    "space-in-parens": [
      2,
      "never"
    ],
    "keyword-spacing": 2,
    "space-infix-ops": 2,
    "space-unary-ops": [
      2,
      {
        "words": true,
        "nonwords": false
      }
    ],
    "spaced-comment": [
      2,
      "always",
      {
        "markers": [
          "global",
          "globals",
          "eslint",
          "eslint-disable",
          "*package",
          "!",
          ","
        ]
      }
    ],
    "use-isnan": 2,
    "valid-typeof": 2,
    "wrap-iife": [
      2,
      "any"
    ],
    "yoda": [
      2,
      "never"
    ],
    "angular/angularelement": 1,
    "angular/controller-as": 2,
    "angular/controller-as-route": 2,
    "angular/controller-as-vm": [
      2,
      "vm"
    ],
    "angular/controller-name": [
      2,
      "/[A-Z].*$/"
    ],
    "angular/deferred": 0,
    "angular/definedundefined": 2,
    "angular/di": 0,
    "angular/di-order": [
      0,
      true
    ],
    "angular/directive-name": 0,
    "angular/directive-restrict": [
      0,
      {
        "restrict": "AE",
        "explicit": "never"
      }
    ],
    "angular/component-limit": [
      0,
      1
    ],
    "angular/document-service": 2,
    "angular/empty-controller": 0,
    "angular/file-name": 0,
    "angular/filter-name": 0,
    "angular/foreach": 0,
    "angular/function-type": 0,
    "angular/interval-service": 2,
    "angular/json-functions": 2,
    "angular/log": 2,
    "angular/module-dependency-order": [
      0,
      {
        "grouped": true,
        "prefix": null
      }
    ],
    "angular/module-getter": 2,
    "angular/module-name": 0,
    "angular/module-setter": 2,
    "angular/no-angular-mock": 0,
    "angular/no-controller": 0,
    "angular/no-cookiestore": 2,
    "angular/no-http-callback": 2,
    "angular/no-inline-template": [
      0,
      {
        "allowSimple": true
      }
    ],
    "angular/no-jquery-angularelement": 2,
    "angular/no-private-call": 2,
    "angular/no-service-method": 2,
    "angular/no-services": [
      2,
      [
        "$http",
        "$resource",
        "Restangular"
      ]
    ],
    "angular/on-watch": 2,
    "angular/rest-service": 0,
    "angular/service-name": 2,
    "angular/timeout-service": 2,
    "angular/typecheck-array": 2,
    "angular/typecheck-date": 2,
    "angular/typecheck-function": 2,
    "angular/typecheck-number": 2,
    "angular/typecheck-object": 2,
    "angular/typecheck-string": 2,
    "angular/watchers-execution": [
      0,
      "$digest"
    ],
    "angular/window-service": 2
  },
  "extends": [
    "angular"
  ]
}
```

**[Back to top](#table-of-contents)**

# ES2015 and Tooling

##### ES2015

* Use [Babel](https://babeljs.io/) to compile your ES2015+ code and any polyfills
* Consider using [TypeScript](http://www.typescriptlang.org/) to make way for any Angular 2 upgrades

##### Tooling
* Use `ui-router` [latest alpha](https://github.com/angular-ui/ui-router) (see the Readme) if you want to support component-routing
  * Otherwise you're stuck with `template: '<component>'` and no `bindings`/resolve mapping
* Consider preloading templates into `$templateCache` with `angular-templates`
* Use [Webpack](https://webpack.github.io/) for compiling your ES2015 code and bundling your project
* Use [ngAnnotate](https://github.com/olov/ng-annotate) to automatically annotate `$inject` properties
* How to use [ngAnnotate with ES6](https://www.timroes.de/2015/07/29/using-ecmascript-6-es6-with-angularjs-1-x/#ng-annotate)

**[Back to top](#table-of-contents)**

# State management

Consider using Redux with Angular 1.5 for data management.

* [Angular Redux](https://github.com/angular-redux/ng-redux)

**[Back to top](#table-of-contents)**

# Resources

* [Understanding the .component() method](https://toddmotto.com/exploring-the-angular-1-5-component-method/)
* [Using "require" with $onInit](https://toddmotto.com/on-init-require-object-syntax-angular-component/)
* [Understanding all the lifecycle hooks, $onInit, $onChanges, $postLink, $onDestroy](https://toddmotto.com/angular-1-5-lifecycle-hooks)
* [Using "resolve" in routes](https://toddmotto.com/resolve-promises-in-angular-routes/)
* [Redux and Angular state management](http://blog.rangle.io/managing-state-redux-angular/)
* [Sample Application from Community](https://github.com/chihab/angular-styleguide-sample)

**[Back to top](#table-of-contents)**

# Documentation
For anything else, including API reference, check the [Angular documentation](//docs.angularjs.org/api).
