---
layout: post
title: What is end-to-end testing and how it works.
description: article describes what is end-to-end testing, how it works and how to automate it. 
keywords: angular, angularjs, javascript, testing, unit testing, end-to-end testing, karma, jasmine, Protractor, selenium, webdriver, webdriverjs, TDD, test-driven development 
---

My [previous post]({% post_url 2015-05-30-what-is-unit-testing-and-how-it-works %}) was about unit-testing. I described what it is for and how to use it.
But unit-tests only test separate units of code in isolation, they don't test how those units work together.
So we also need another type of testing, that can help us to ensure the app as a whole works correctly.
In this post I'm going to talk about end-to-end (E2E) testing.

## What end-to-end testing is for
End-to-end testing is all about checking a work of your app as a whole, not a work of individual units of code.

During E2E testing we consider our app as a black-box only dealing with UI exposed to the user.
Such testing is fulfilled from user's point of view to ensure that separate app features work as expected.
For example, it may include checking login/logout process, registration process, etc.

For small projects with a single developer it may be sufficient to do manual E2E testing: fill the form => click a button => see results.
But when an app is quite large and there are multiple developers it becomes hard to manually check each feature.
And there ways to automate end-to-end testing.

## How it works
As we can understand now E2E testing is kind of dynamic testing, i.e. it requires code execution.
The mechanism of automatic E2E testing is quite simple.

1. To test a feature developer writes a E2E spec, that defines:
    * what actions needs to be done (load a page, fill a form, click buttons, etc.)
    * assertions about desired result (new page loading at certain url, loading of modal window, showing an warning message, etc.)
1. Developer runs a program that connects to a browser and uses the spec to do all the defined actions.
1. The program uses the spec to compare the actual results with the desired results, defined in the spec.
1. The program notifies a developer about results.

Here is an example of how an E2E spec may look like:

```js
driver.get('http://www.example.com', function() {
 driver.findElement(By.name('username'), function(q) {
   q.sendKeys('Alexey', function() {
     driver.findElement(By.name('submit-button'), function(sbmtBtn) {
       sbmtBtn.click(function() {
         driver.getTitle(function(title) {
           assertEquals("Search results for 'Alexei'", title);
         });
       });
     });
   });
 });
});
```

`driver` here is a program, that communicates with a browser.

## Which tools are used in automatic E2E testing

1. To run E2E specs you need [selenium-webdriver](https://code.google.com/p/selenium/wiki/WebDriverJs) npm package installed.
This program is able run specs against your application in all major browsers - Chrome, Firefox, IE.
2. To write specs you need a framework, like [Jasmine](http://jasmine.github.io/).

AngularJS developers have a bonus - there is a node.js program [Protractor](https://github.com/angular/protractor), 
built on top of selenium-webdriver, that greatly simplifies writing specs for angular.js applications.

## Links and references
WebDriverJS: [https://code.google.com/p/selenium/wiki/WebDriverJs](https://code.google.com/p/selenium/wiki/WebDriverJs) 

Jasmine: [http://jasmine.github.io/](http://jasmine.github.io/)

Protractor: [https://github.com/angular/protractor](https://github.com/angular/protractor)