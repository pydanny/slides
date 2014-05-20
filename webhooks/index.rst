
.. Webhooks slides file, created by
   hieroglyph-quickstart on Tue May 20 12:19:10 2014.


Webhooks
========

by Daniel Greenfeld

* http://pydanny.com
* http://twoscoopspress.org
* Senior Python Engineer at Eventbrite.com

Webhooks?
====================


What are Webhooks?
-------------------

* https://en.wikipedia.org/wiki/Webhook
* A method of augmenting or altering the behavior of a web page, or web application, with custom callbacks. 
* The server pushes to the consumer, rather than the consumer pulls from the server.
* Push (Webhook) vs Pull (Traditional API)
* Great for SOA and SaaS

Advantages of Webhooks
-------------------------

* Performance friendly to server and client
* The client doesn't need to poll the server constantly
* The server lets the client know when things are ready.

Django Packages Example
-------------------------

* https://www.djangopackages.com
* Django Packages polls Github and BitBucket once every 24 hours for each package on the system.
* Historical Note: 3.5 years ago we were asked by GitHub to change from hourly to daily.
* In a short period of time we make about 7000 API requests. 

Django Packages with Webhook
------------------------------

* Added a receiver view: http://bit.ly/djangopackages-webhook-view
* You can add a webhook manually:
* https://www.djangopackages.com/packages/github-webhook/
* Or add as a service (Demonstrate)
* Many less requests to GitHub. Hooray!

Webhooks are Great!
-----------------------

.. rst-class:: build

* Receiving them is easy.
* But writing an extendable library to send them? 
* Let's take a look!

Building a Webhook Library
===========================

Design considerations
------------------------

* Pleasant developer experience
* Keep code abstraction to a minimum
* Make introspectable
* Make extending it very easy (functional vs OO)
* Make new senders easy to write
* Make tests easy to write

Webhook Naming Problem
-------------------------

.. rst-class:: build

* Webhooks is a terrible name.
* Hook is for fishing
* Hooking is for ....


My In-Progress Implementation
------------------------------

* https://github.com/pydanny/webhooks
* https://github.com/pydanny/webhooks#usage
* http://pypi.python.org/pypi/webhooks

Django Integration
------------------------------

* https://github.com/pydanny/dj-webhooks
* https://github.com/pydanny/dj-webhooks#quickstart
* http://pypi.python.org/pypi/dj-webhooks

Decorator Construction
------------------------------

* Defined a base_hook function as a decorator
* http://bit.ly/pydanny-webooks-L16-L49
* Used partial to provide a good default
* http://bit.ly/webhooks-decorators-L53
* Partial Reference: http://pydanny.com/python-partials-are-fun.html

Sender Construction
------------------------------

The sender_callable

* function: handy, but not easily extendable
* http://bit.ly/webhooks-simple

The senderable class

* Class: Not as handy, Easily extentable
* http://bit.ly/webhooks-senderable

Example: dj-webhooks
------------------------------

The sender_callable

* function: copied, not extended
* http://bit.ly/webhooks-orm-L73-L127

The senderable object

* Class: extended the original
* http://bit.ly/djwebhooks-senderable-L48-L70

Takeaways
===========

What came out of this...

Caching and Encoding
---------------------------

.. rst-class:: build

* **webhooks** needed something like ``django.utils.functional.cached_property``
* https://pypi.python.org/pypi/cached-property (with threading support!)
* **webhooks** and **dj-webhooks** needed a better JSON encoder (tests!)
* https://pypi.python.org/pypi/json262

Functional vs OO Thoughts
---------------------------

* Functional code is awesome, but lean-and-mean OO is great.
* Both are wonderful until they get bloated.
* Don't try to stick to a paradigm if doing so makes ugly code.


Implementation!
-----------------------

* Able to implement Webhooks in a working project quickly.
* Able to extend dj-webhooks into projects in a loosely coupled way.
* Show internal project code if there is time.


Finis
======

Questions?