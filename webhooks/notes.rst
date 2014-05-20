===================
Webhooks
===================

What are Webhooks?
====================

* https://en.wikipedia.org/wiki/Webhook
* A method of augmenting or altering the behavior of a web page, or web application, with custom callbacks. 
* The server pushes to the consumer, rather than the consumer pulls from the server.
* Push (Webhook) vs Pull (Traditional API)
* Great for SOA and SaaS

Advantages of Webhooks
========================

* Performance friendly to server and client
* The client doesn't need to poll the server constantly
* The server lets the client know when things are ready.

Django Packages Example
=========================

* Django Packages polls Github and BitBucket once every 24 hours for each package on the system.
* Historical Note: 3.5 years ago we were asked by GitHub to change from hourly to daily.
* In a short period of time we make about 7000 API requests. 

Django Packages with Webhook
==============================

* Added a receiver view: http://bit.ly/djangopackages-webhook-view
* You can add a webhook manually:
* https://www.djangopackages.com/packages/github-webhook/
* Or add as a service (Demonstrate)


Webhooks are Great!
====================

* Receiving them is easy.
* Writing an extendable library to send them? 
* Let's take a look!

Building a Webhook Library
===========================

Design considerations

* Pleasant developer experience
* Keep code abstraction to a minimum
* Make introspectable
* Make extending it very easy (functional vs OO)
* Make new senders easy to write
* Make tests easy to write

Webhook Naming Problem
=======================

* Webhooks is a terrible name.
* Hook is for fishing
* Hooking....


My Implementation
==================

Still in alpha/beta state, however:

* https://github.com/pydanny/webhooks
* https://github.com/pydanny/webhooks#usage
* http://pypi.python.org/pypi/webhooks

Django Integration
====================

* https://github.com/pydanny/dj-webhooks
* https://github.com/pydanny/dj-webhooks#quickstart
* http://pypi.python.org/pypi/dj-webhooks

Decorator Construction
=======================

* Defined a base_hook function as a decorator
* https://github.com/pydanny/webhooks/blob/master/webhooks/decorators.py#L16-L49
* Used partial to provide a good default
* https://github.com/pydanny/webhooks/blob/master/webhooks/decorators.py#L53
* Partial Reference: http://pydanny.com/python-partials-are-fun.html

Sender Construction
==============================

The sender_callable (function - handy, but not easily extendable)

* https://github.com/pydanny/webhooks/blob/master/webhooks/senders/simple.py

The senderable object (object - Easily extendable)

* https://github.com/pydanny/webhooks/blob/master/webhooks/senders/base.py#L16-L111

Case in Point: dj-webhooks ORM sender
=====================================

The sender_callable (function - copied, not extended)

* https://github.com/pydanny/dj-webhooks/blob/master/djwebhooks/senders/orm.py#L73-L127

The senderable object (object - Easily extended)

* Senderablehttps://github.com/pydanny/dj-webhooks/blob/master/djwebhooks/senders/orm.py#L48-L70

Functional vs OO
==================

* Functional code is awesome, but lean OO is great too
* Both are great until they get bloated.
* Don't try to stick to a paradigm if doing so makes ugly code.

Caching and Encoding
======================

* webhooks needed something like ``django.utils.functional.cached_property``
* https://pypi.python.org/pypi/cached-property (with threading support!)
* webhooks and dj-webhooks needed a better JSON encoder (tests!)
* https://pypi.python.org/pypi/json262
