
.. Webhooks slides file, created by
   hieroglyph-quickstart on Tue May 20 12:19:10 2014.


Webhooks
========

by Daniel Greenfeld

* http://pydanny.com
* http://twoscoopspress.org (Two Scoops of Django)
* http://cartwheelweb.com
* Senior Python Engineer at `Eventbrite.com`_ (June 3rd)

.. _`Eventbrite.com`: http://eventbrite.com

Webhooks
====================

What are webhooks?


Definition of Webhooks
-----------------------

* https://en.wikipedia.org/wiki/Webhook
* A method of augmenting or altering the behavior of a web page, or web application, with custom callbacks. 
* The server pushes to the consumer, rather than the consumer pulling from the server.

Ugh
-----

* I don't want to try and reword the previous slide.
* I'm just going to give you an example.

Django Packages Example
-------------------------

* https://www.djangopackages.com
* Django Packages polls Github and BitBucket once every 24 hours for each package on the system.
* In a short period of time we make about 7000 API requests.
* Used to be hourly.

    * 3.5 years ago we were asked by GitHub to change from hourly to daily.
    * They've grown since then.

What if GitHub pushed to Django Packages?
-------------------------------------------

* I push code to pydanny/dj-stripe.
* GitHub tells Django Packages that an update has occured

    * ``commit.push``
    
* We could remove about 3K-5K GitHub API requests per day.
* Eventually remove more, but that's the first pass.

Definition of Webhooks
-----------------------

* Push (Webhook) vs Pull (Traditional API)
* Depends on where you stand.

Advantages of Webhooks
-------------------------

* Performance friendly to server and client.
* The client doesn't need to poll the server constantly.
* The server lets the client know when things are ready.

Django Packages using Github Webhook
-------------------------------------

* Added a receiver view: http://bit.ly/djangopackages-webhook-view
* You can add a webhook manually:
* Go to your app's settings:

1. Repo: http://bit.ly/1m6EfcB
2. URL: https://www.djangopackages.com/packages/github-webhook/

Django Packages Github Service Webhook
---------------------------------------

.. image:: http://cdn.shopify.com/s/files/1/0304/6901/files/github-service.png?305
   :name: Django Packages Github Service Webhook
   :align: center
   :target: http:/www.djangopackages.com


Webhooks are Great!
-----------------------

* Receiving them is easy.
* Just write a receiver view!

... but what about sending webhooks?

What about sending Webhooks?
----------------------------

* It's just python-requests? Right?
* Every time a user commits an action, python-requests just sends something out!

It's a bit more complicated...
---------------------------------

* How do you track push failures?
* How many repeats of push failures do you allow?
* How often between push attempts?
* How many push failures do you allow?

More complications...
-----------------------

* How can developer-users add a webhook?
* How can developer-users introspect adding a webhook?

More complications...
--------------------------

* How do you write tests?
* Do you write unit tests or functional tests?

... and more complications!
-----------------------------

* Performance

    * requests is fast
    * HTTP is slooooooow

* Querying database to send data takes time
* Logging the results takes time

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
* Make it fasterrerer!

Webhook Naming Problem
-------------------------

* Webhooks is a terrible name.
* Hook is for fishing
* Hooking is for ....

Enough Background
-------------------

Did I get it working?

Decorator-based API
---------------------------------

* Great for API design
* https://gist.github.com/pydanny/1098c194138bc666955e

Decorator-based API
---------------------------------

* Defined a base_hook function as a decorator

    * http://bit.ly/pydanny-webooks-L16-L49
    
* Extend hooks with Partials

Partials 'extend' the Decorators
--------------------------------

* Used partial to provide a good default
* Easy to create more
* Partial Reference: http://pydanny.com/python-partials-are-fun.html


My In-Progress Implementation
------------------------------

* https://github.com/pydanny/webhooks
* https://github.com/pydanny/webhooks#usage

Sender Construction
------------------------------

The sender_callable

* function: handy, but not easily extendable
* http://bit.ly/webhooks-simple

The senderable class

* Class: Not as handy, Easily extendable
* http://bit.ly/webhooks-senderable

Senderable Class
-----------------

* Serializes the data
* Makes all the attempts
* Records the response

Django Integration
------------------------------

* https://github.com/pydanny/dj-webhooks
* https://github.com/pydanny/dj-webhooks#quickstart

Example: dj-webhooks
------------------------------

The sender_callable

* function: copied, not extended
* http://bit.ly/webhooks-orm-L73-L127

The senderable object

* Class: extended the original
* http://bit.ly/djwebhooks-senderable-L48-L70

Senderable Class
-----------------

* Serializes the data
* Makes all the attempts
* Records the response (in the ORM)


Example in Action
-------------------

* Every time a project is updated:
* https://gist.github.com/pydanny/6345bb10e76a039e7172

The Problem of Time
----------------------

* What if calculating the payload takes forever?
* What if the payload is huge?
* What if the client's response takes too long?

How to Make it Fasterrerer?
----------------------------

* Asynchronous task/job queues
* Celery or RedisQ
* https://gist.github.com/pydanny/5540527049e4da55611a


Takeaways
===========

What came out of this...

Caching
-------

* ``django.utils.functional.cached_property``
* But outside of Django (or Flask, Bottle, et al)?
* https://github.com/pydanny/cached-property

**Now with theading support!**

JSON Encoding
--------------

* **webhooks** and **dj-webhooks** needed a better JSON encoder.
* Moar ECMA-262 and ECMA-404 compliance please!
* DateTime objects
* Decimals
* Testable code
* https://github.com/audreyr/standardjson

Functional vs OO Thoughts
---------------------------

* Functional code is awesome, but lean-and-mean OO is great.
* Both are wonderful until they get bloated.
* Don't try to stick to a paradigm if doing so makes ugly code.


Results!
-----------------------

* Clearly written and well tested code.
* Able to implement Webhooks in a working project quickly.
* Able to extend dj-webhooks into projects in a loosely coupled way.
* Not yet done documented it properly


Finis
======

Questions?