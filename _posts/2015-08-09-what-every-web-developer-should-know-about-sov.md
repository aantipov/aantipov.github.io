---
layout: post
title: What every web developer should know about same-origin policy
description: article describes what same-origin policy is, what it is for and how to use it to build robust web applications.
keywords: web, security, SOP, same-origin policy, origin, CORS, XMLHttpRequest, WebSocket
---

Every web-developer has ever met with the subject called Same Origin Policy (SOP) but even many web-professionals do not know what it exactly is, who is responsible for implementing it and how to use it to build modern secure web applications. Moreover, it is difficult to find good resources on this topic that describe it in simple words. At the same time, it is very important for every web-developer to grasp the concept 'cause it is the cornerstone of web application security model and many web resources rely upon it.

SOP is a big topic and I aren't gonna cope all the aspects and corner cases of it. What I'm actually gonna do is to help you to get your head around it, understand the basics and grasp the ideas behind it.

## What SOP is
Unfortunately there is no single and concise definition of the concept, that would cope all the aspects of its application.
To start explaining the topic I think it would be helpful to check the ideas behind the concept first.

## Ideas behind SOP
User can visit several websites with a single browser simultaneously by opening new tabs or windows.
Lets consider an example of logging into internet-bank account and having opened other website in a different tab. I'm sure that as a user you would not want to grant other websites access to your bank account - to read any info from it. So I think you'd like browsers to impose restrictions on what scripts from the document opened at one tab can do with the document, opened in other tab. And browsers actually do this thing and such sort of restrictions is called Same-Origin Policy.

Of course SOP scope is much bigger the case of two opened tab and covers a lot of other things.
But the mentioned case helps to understand the main idea behind SOP - restrict reading (stealing) confidential information.

The SOP was originally implemented in Netscape Navigator 2.0 and since then it was incorporated into other browsers.  

Nowadays, SOP's scope goes beyond inter-tab (windows) communications and is incorporated into many other schemes of communications.

## Who is responsible for imposing restrictions defined by SOP?
As it is became clear from the above it is browsers's responsibility to impose restrictions defined by SOP. It is your browser that restricts scripts from other tabs from reading information at tab with opened internet-bank account page.

The SOP topic is quite broad and there are some differences in implementation of SOP across browsers. But in general most things defined by SOP work in the same way across main browsers.

## Origin notion.
As it is clear from its name, SOP is built around a concept of origin.


Lets find out what it is.

Each document or resource in Web is identified by url.
URL consists from several parts and in terms of SOP a combination of scheme, host, and port defines an origin of a web resource.

For example, an origin of a resource found at the url [http://www.w3.org/Security/wiki/Same_Origin_Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy) consists of `http` scheme, `www.w3.org` host and 80 port (default port).
Two resources have the same origin if, and only if, their urls have the same scheme, host and port. Origins of documents `http://example.com/doc1.html` and `https://example.com/doc2.html` aren't the same, 'cause they have different schemes `http` and `https`.

## SOP and Origin.
In general, browsers implement SOP by isolating resources from different origins and permitting controlled communication between them.

There are quite a lot of different types of possible communication and SOP application varies depending on the type of communication.

## Communication between 2 different opened documents (web pages).
Access to documents loaded by browser is represented by [Document Object Model](https://www.wikiwand.com/en/Document_Object_Model).
Documents can be loaded in separate tabs, windows or iframes. No matter the way documents were loaded they obey a simple principle: _document have access to other document's DOM if and only if both documents have the same origin_.

Browsers provide the following JavaScript API, that allows documents to directly reference each other: `iframe.contentWindow`, `window.parent`, `window.open` and `window.opener`.
If documents do not have the same origin, browsers provide very limited access to [Window](https://developer.mozilla.org/en-US/docs/Web/API/Window) and [Location](https://developer.mozilla.org/en-US/docs/Web/API/Location) objects only.

## Instantiation of network requests and interpretation of their responses.
During document loading and its lifetime it can instantiate network requests of different types. For example, requests for stylesheets, scripts, images (static assets), ajax requests and others.
In terms of SOP application it is convenient to group these types of requests into 4 categories: new document requests, form submissions, embedded objects requests and scripting requests.
Let's consider how browsers impose SOP on each of these types requests.

### New documents requests.
Browsers permit web pages to open other web pages of any origin. Opened document may replace original one or it may be opened in a different tab or window or iframe.
Opening new documents can be achieved by user clicking hyperlinks or by invoking appropriate DOM methods (e.g. `window.open`).

So browsers do not impose SOP restrictions on opening new documents. But the do impose SOP restrictions on the access to the contents of opened documents, as we have already know from the above. And this behaviour will become obvious if you remember the main idea behind SOP - _restrict access to reading information from other origin resources, not to loading or executing it_.

### Form submission.
Browsers permit web pages to instantiate cross-origin post requests via form submission. And we can think about it as a case of a new document request, 'cause form submission leads to opening a new document, which contents are not available for reading in cross-origin case.

### Embedded objects requests.
HTML has a handful of tags, instantiating their own requests and interpreting the responses of those requests somehow.
For example, `script` tags instantiate network http requests for receiving and executing scripts, `img` tags instantiate network http requests for receiving and displaying images.
The general rule here is that browser doesn't not forbid such cross-origin requests. But the response of this requests may only be interpreted and executed and can't be read.
For example, we can't get contents of script file, that was received through script tag request. But the contents of this file are interpreted and executed no matter of the script file origin.

### Scripting requests.
Nowadays, two most favorite technologies used for doing network requests from scripts are [XMLHttpRequest](https://www.wikiwand.com/en/XMLHttpRequest) and [WebSocket](https://www.wikiwand.com/en/WebSocket).

XMLHttpRequest object provides API for doing HTTP requests. This API is quite sophisticated and provides a lot of configuration options for customizing requests - HTTP method, headers, etc.
Browsers do impose SOP restrictions on Ajax requests and a while ago they completely forbade doing cross-origin ajax requests, 'cause the response of these requests is always fully available to the script.
But currently browsers support doing cross-origin requests using [CORS](https://www.wikiwand.com/en/Cross-origin_resource_sharing), which allows resource owners to determine which origins their resource will be available for.
For example, `http://foo.example.com` page can instantiate request to the `http://bar.example.com` page and get and read the response of the request if the owner of the `http://bar.example.com` site permits to do so.

WebSocket is a different protocol for network communication and browsers do not forbid cross-origin communication using this protocol and defer it to WebSocket server owner to define which origins have access to it.

## Summary
Same-origin policy is a cornerstone of web application security model. It instructs browser to restrict communication between resources with different origins and thus preventing malicious web pages from stealing your confidential information.

Most common applications of the policy discussed above should get your head around of the concept and help you to build secure web applications.

## References
[http://www.w3.org/Security/wiki/Same_Origin_Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy)  
[http://www.w3.org/html/wg/drafts/html/master/browsers.html#origin](http://www.w3.org/html/wg/drafts/html/master/browsers.html#origin)  
[http://www.w3.org/TR/cors/](http://www.w3.org/TR/cors/)  
[http://tools.ietf.org/html/rfc6454](http://tools.ietf.org/html/rfc6454)  
[https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)  
[https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)  
[https://code.google.com/p/browsersec/wiki/Part2#Same-origin_policy](https://code.google.com/p/browsersec/wiki/Part2#Same-origin_policy)  
[http://blogs.msdn.com/b/ieinternals/archive/2009/08/28/explaining-same-origin-policy-part-1-deny-read.aspx](http://blogs.msdn.com/b/ieinternals/archive/2009/08/28/explaining-same-origin-policy-part-1-deny-read.aspx)   
