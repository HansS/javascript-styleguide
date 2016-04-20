##Landdox Javascript Styleguide

**A very opinionted styleguide by developers who work with Javascript.**

Landdox heavily takes advantage of Javascript for the back-end and front-end and thus arose a need for our own styleguide that solves and explains edge-cases not covered by others.

###Our stack

This styleguide covers the usage of Javascript under these circumstances:

1. AngularJS
2. Browserify
3. ES2015 front-end and back-end
4. NodeJS back-end 4.x+

With the ability to use ReactJS when necessary.

###Inspiration

Other styleguides that we drew inspiration from:

- [John Papa's Angular Guide](https://github.com/johnpapa/angular-styleguide)

###Table of Contents

1. Javascript semantics 
2. Modularity

##Content

**Note** I will be using the words "component" and "module" interchangeably.

###Javascript Semantics

This section details the codestyle for basic Javascript no matter where it's used. It should be assumed that ES2015 is always ready and available.

**1. Whitespace and semicolons**

Whitespace should be used sparingly and only when necessary. Code should be indented by 2 spaces and semicolons should always be used. One newline at a time is recommended and there should be no instances of multiple newlines.

```js
//wrong way
var test1 = 0
var testObj = {
  key: val
}
var test2 = 0



var sayHello = function() {
  console.log('Hello')
}
```
Instead, use one newline between logical blocks of code. Functions and other blocks of code (if/else, switch, multiline objects) should always have one new line below declarations and often above as well; however, variables and imperative code. There should also be a newline above a final `return` statement. Early returns do not need to abide by that rule.

```
//the right way
var isFlagged = false;
var isNumber = false;
var config = {
  
}; //no newline above

//newline above and below
function pickNumber() {
  var numbers = [];
  
  return function() {
    var num = Math.rand();
    numbers.push(Math.rand());
    
    return num;
  }; //semicolon
} //no semicolon

```

**2. Variable declarations**

Variables should be declared one-per-line except when using deconstruction. Each variable

```js
//bad way
var test = 1, test2 = 2, test3 = 3;

//no semicolon
var test = 1
```

###Modularity

1. Bootstrap component
2. Bootstrap bootstrap component
3. One component per file
4. Always export module

**1. Bootstrap component**

To avoid getting into a dependency mess, use a bootstrap module to hold your entire app together and bring in the rest of the dependencies. The dependencies should only be the top-most layer of the application and "globals" (such as helper directives, etc.).

```js
import { name as myComponent } from './component/my-component';
import { name as myPage } from './component/my-component'; // but not mySubPage

angular
  .module('app', [
    myComponent,
    myPage
  ])
;
```

**2. Bootstrap bootstrap component**

Various areas of an application can be grouped together. For instance, if you have a `Dashboard` module which holds the top-most logic for all Dasboard sections, it should also list its child-components dependencies. These dependencies should not be listed in the `bootstrap` module. Example:

```js
//the wrong way
import { name as Dashboard } from './pages/dashboard';
import { name as DashboardMetrics } from './pages/dashboard/metrics';

angular.
  .module('app', [
    Dashboard,
    DashboardMetrics
  ])
;
```

```js
//recommended

//app.js bootstrap file
import { name as Dashboard } from './pages/dashboard';

angular
  .module('app', [Dashboard])
;

// ./pages/dashboard.js file

import { name as DashboardMetrics } from './dashboard/metrics';

var dashboard = angular
  .module('dashboard', [DashboardMetrics])
;

export default dashboard;
```

This perserves the dependency tree like so:

```
- App
  - Dashboard
     - Metrics
```

rather than:

```
- App
  - Dashboard
  - Metrics
```

This can be done to group various helper directives and global modules as well.

**3. One Component Per File**

There should only ever be one component per file:

```js
//bad!
angular
  .module('dashboard', []);

angular
  .module('dashboard.metrics', []);
```

This makes it a nightmare to properly export.

```js
//good
import { name as DashboardMetrics } from './dashboard/metrics';

angular
  .module('dashboard', [DashboardMetrics])
;
```

**4. Always export module**

When using ES6, use the following syntax:

```js
import angular from 'angular';

var dashboard = angular
  .module('dashboard', [])
;

export default dashboard;

//rest of the logic goes here
```


