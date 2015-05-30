---
layout: post
title: What is unit testing and how it works.
description: article describes importance of testing, existing approaches to testing, what is unit testing and how it works. 
keywords: angular, angularjs, javascript, testing, unit testing, end-to-end testing, karma, jasmine, TDD, test-driven development 
---

## Building robust js apps is impossible without testing.
With the raise of javascript use we can watch how js apps become bigger and bigger.
For the last 5-10 years the complexity of javascript apps has increased enormously.
Building of such apps is impossible without well-tuned testing process.
So every javascript developer should know what testing actually is and how to use it.
I gonna tell you about most popular approaches to testing js apps.

## What kinds of testing are there?
There are 2 most popular approaches to testing js apps, that complement each other.
These are unit-testing and end-to-end testing.
What do they actually do, why and when should we use each one?
I gonna write about end-to-end testing in my next post.
And this post describes unit-testing.

## Unit-testing
### Why should we use it?
Unit-testing helps us to ensure that each part of our code works as expected under different conditions in different environments.

There are also other benefits unit testing brings us:

- understanding of what each code unit does and what it is supposed to do.
- it urges us to write cleaner and more robust code, breaking it into separate [loosely coupled](https://www.wikiwand.com/en/Loose_coupling) components.

### How it works?
Unit testing is all about testing individual functions and classes.
It utilizes [dynamic testing](https://www.wikiwand.com/en/Dynamic_testing),
i.e. it executes the function under test and checks its output, side effects and maybe exceptions handling.

It is important to note that when tested, the function under test is executed in isolation from the rest of the code and all dependencies are mocked.

Consider the following example:

```js
function loginUser(username, pass) {

  return server.login(username, pass)
    .then((result) => {
        notify.success('Congrats! New user has been created');
        return result;
    })
    .catch((error) => {
        notify.error('Some error occurred');
        throw error;
    });
}
```

This a simple function, that implements user login operation.
With unit-tests we can ensure that when calling that function

- a promise will be returned;
- `server.login` will be called with passed in parameters `username` and `pass`
- `notify.success` will be called if `server.login` succeeded;
- `notify.error` will be called if `server.login` rejected;

### How to implement it
To make and run unit tests javascript-developer needs to do the following:

1. Choose and setup unit testing framework, that determines how we write test specs.
2. Write test specs.
3. Install test-runner, that will execute our test specs against source code in different browsers.

There are a lot of test frameworks and test-runners.

In my angularjs projects I prefer to use [Jasmine](http://jasmine.github.io/) and [Karma](http://karma-runner.github.io/).


In my next post I'm going to write about end-to-end testing.