---
layout: post
title: Dependencies handling in angular with ES6 classes
---

ES6 gives us javascript devs a lot of new things and enhancements.
It changes how we write our apps.
As a developer of angularjs apps I like to write controllers with the help of ES6 classes.

```js
class MyController {
  constructor(service1, service2) {
    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
  //...
  }
}
```

Properties defined on `this` automatically become available on scope using `controllerAs` syntax.
The same applies to the methods, defined in class.
Cool!

But to have injected services available from inside the class methods you should define them on `this`, and so to make them public,
because ES6 classes don't have private properties and methods yet.
There are some workarounds (WeakMap, Symbol), but I don't like them, because they bring complexity to our code.
The best we can do now is to prefix our private properties by `_`, so we can let other developers know that these properties can't be used in views.
So our controller code become:

```js
class MyController {
  constructor(service1, service2) {
    this._service1 = service1;
    this._service2 = service2;

    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
    //...
  }
}
```

Such a code works fine till you try to minify it.
Minified versions of this code won't work because controller injections can't be resolved.
In ES5 you can use [ng-annotate](https://github.com/olov/ng-annotate) to handle such minification problems.
But as for now [it doesn't work good](https://github.com/olov/ng-annotate/issues/64) yet with ES6.
So we should use explicit dependency annotation.
Our controller now become to look like this:

```js
class MyController {
  constructor(service1, service2) {
    this._service1 = service1;
    this._service2 = service2;

    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
    //...
  }
}

MyController.$inject = ['service1', 'service2'];
```

It works.
But we can improve it - we can remove the need to keep `$inject` array in sync with the parameters in the constructor definition 
and we can also remove the need to publish each of our dependencies separately.
Here is how we can do it:

```js
const deps = ['service1', 'service2'];

class MyController {
  constructor(...args) {
    // Publish each service on 'this' with '_' prefix.
    deps.map((val) => '_' + val).forEach((val, key) => {
      this[val] = args[key])
    };

    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
    //...
  }
}

MyController.$inject = deps;
```

Now we handle all controller's dependencies in a single place -`deps` constant, and they all are automatically published on `this` with `_` prefix.
Good!

You can also apply such a technique of dependencies handling to other application components.

Useful links:

- [Private properties in javascript es6 classes](http://stackoverflow.com/questions/22156326/private-properties-in-javascript-es6-classes)
- [A Private Fields Proposal for ECMAScript](https://github.com/zenparsing/es-private-fields)
