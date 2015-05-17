---
layout: post
title: Dependencies handling in angular with ES6 classes
---

ES6 gives us javascript devs a lot of new things and enhancements.
It changes how we write our apps.
As a developer of angularjs apps I like to write controllers with the help of ES6 classes.

{% highlight js %}
class MyController {
  constructor(service1, service2) {
    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
  //...
  }
}
{% endhighlight %}

Properties defined on `this` automatically become available on scope using `controllerAs` syntax.
The same applies to the methods, defined in class.
Cool!

But to have injected services available from inside the class methods you should define them on `this`, and so to make them public, because ES6 classes don't have private properties yet.
There are some workarounds (WeakMap, Symbol), but I don't like them, because they bring complexity to our code.
The best we can do now is to prefix our private properties by `_`, so we can let other developers know that these properties can't be used in views.
So our controller code become:

{% highlight js %}
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
{% endhighlight %}

Such a code works fine till you try to minify it.
Minified versions of this code won't work because controller injections can't be resolved
In ES5 you can use ng-annotate to handle such minification problems.
But as for now it doesn't work good yet with ES6 (INSERT A LINK).
So we should use explicit injection annotation.
Our controller now become to look like this:

{% highlight js %}
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
{% endhighlight %}

It works.
But we can improve it - we can remove the need to syncronize our injections lists in `$inject` and constructor definition and we can remove the need to publish each of our services separately.
Here is how we can do it:

{% highlight js %}
const deps = ['service1', 'service2'];

class MyController {
  constructor(...args) {
    // Publish each service on 'this' with '_' prefix.
    deps.map((val) => '_' + val).forEach((val, key)=> this[val] = args[key]);

    this.someProperty = 'Foo';
    //...
  }

  someMethod() {
    //...
  }
}

MyController.$inject = deps;
{% endhighlight %}

Now we handle all controller's dependencies in a single place -`deps` constant, and they are all automatically published on `this` with `_` prefix.
Good!

You can also apply such technique of handling of injections to services definitions.

Useful links:

- http://stackoverflow.com/questions/22156326/private-properties-in-javascript-es6-classes
- https://github.com/zenparsing/es-private-fields
