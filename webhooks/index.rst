
.. Webhooks slides file, created by
   hieroglyph-quickstart on Tue May 20 12:19:10 2014.


Webhooks
========

by Daniel Greenfeld

http://bit.ly/webhook-slides


Me
===

* http://pydanny.com
* http://twoscoopspress.org (Two Scoops of Django)
* http://cartwheelweb.com
* Senior Python Engineer at `Eventbrite`_ (June 3rd)

.. _`Eventbrite`: http://eventbrite.com

What are Webhooks?
====================


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
    
* We removed about 3K-5K GitHub API requests per day.
* Eventually we'll remove more, but that's the first pass.

Definition of Webhooks
-----------------------

* Push (Webhook) vs Pull (Traditional API)
* Depends on where you stand.

Advantages of Webhooks
-------------------------

* Performance friendly to server and client.

    * **66x according to http://resthooks.org!!!**
    * Don't know if that's true. but it's dramatic.

* The client doesn't need to poll the server constantly.
* The server lets the client know when things are ready.

Django Packages / Github Webhook
-------------------------------------

Added a receiver view

.. code-block:: python

    @csrf_view_exempt
    def github_webhook(request):
        if request.method == "POST":
            data = json.loads(request.POST['payload'])

            # Webhook Test
            if "zen" in data:
                return HttpResponse(data['hook_id'])
        # moar code after this
    
http://bit.ly/djangopackages-webhook-view

Django Packages / Github Webhook
-------------------------------------

* Add a webhook manually:
* Go to your app's settings:

1. Repo: http://bit.ly/1m6EfcB
2. URL: https://www.djangopackages.com/packages/github-webhook/

Django Packages / Github Service
---------------------------------------

* Just a webhook written in Ruby hosted by Github

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

* It's just python-requests, right?
* Every time a user commits an action, python-requests just sends something out!
* Easy!!!

.. rst-class:: build

* Well... actually...

It's a bit more complicated...
---------------------------------

**Planning for failure:**

* How do you track push failures?
* How many repeats of push failures do you allow?
* How often between push attempts?
* How many push failures do you allow?

More complications...
-----------------------

**Planning for developers using your system:**

* How can developer-users add a webhook?
* How can developer-users introspect what a webhook is doing?

More complications...
--------------------------

**Making your stuff work all the time:**

* How do you write tests?
* Do you write unit tests or functional tests?

... and more complications!
-----------------------------

**Time is money:**

* Sending

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

Great for API design!

.. code-block:: python

    from webhooks import webhook
    from webhooks.senders import targeted
 
    @webhook(sender_callable=targeted.sender)
    def basic(url, wife, husband):
        return {"husband": husband, "wife": wife}
 
    r = basic(url="http://httpbin.org/post", husband="Danny", wife="Audrey")
    
Results
---------

.. code-block:: python

    >>> import pprint
    >>> pprint.pprint(r)
    {'attempt': 1,
    'hash': '29788eb987104b8a87d201292fa459d9',
    'husband': 'Danny',
    'response': b'{snipped}',
    'status_code': 200,
    'url': 'http://httpbin.org/post',
    'wife': 'Audrey'}

Decorator-based API
---------------------------------

Defined a base_hook function as a decorator

.. code-block:: python
    :emphasize-lines: 2

    def base_hook(sender_callable, hash_function, **dkwargs):
        @wrapt.decorator
        def wrapper(wrapped, instance, args, kwargs):
            if not callable(sender_callable):
                raise SenderNotCallable(sender_callable)
            hash_value = None
            if hash_function is not None:
                hash_value = hash_function()
            return sender_callable(wrapped, dkwargs, hash_value, *args, **kwargs)
        return wrapper
        
http://bit.ly/pydanny-webooks-L16-L49

Partials 'extend' the Decorator
--------------------------------

Used partial to provide a good default

.. code-block:: python
    
    from functools import partial

    hook = partial(base_hook, hash_function=basic_hash_function)
    
.. rst-class:: build

* Partials allow you to create new functions that are old functions with defaults.
* Easy to create more hooks
* Partial Reference: http://pydanny.com/python-partials-are-fun.html

dj-webhooks partials example
----------------------------

.. code-block:: python
    :emphasize-lines: 1, 5, 11

    from functools import partial
    from .senders import orm_callable, redislog_hook

    # The pure ORM callable.
    hook = partial(
        base_hook,
        sender_callable=orm_callable,
        hash_function=basic_hash_function
    )
    # The ORM/redislog callable.
    hook = partial(
        base_hook,
        sender_callable=redislog_hook,
        hash_function=basic_hash_function
    )

My In-Progress Implementation
------------------------------

* https://github.com/pydanny/webhooks
* https://github.com/pydanny/webhooks#usage

sender_callable
------------------------------

.. code-block:: python
    :emphasize-lines: 6, 8-11, 13

    #webhooks.senders.targeted
    from .base import Senderable, value_in

    ATTEMPTS = [0, 1, 2, 3]


    def sender(wrapped, dkwargs, hash_value=None, *args, **kwargs):
        senderobj = Senderable(
            wrapped, dkwargs, hash_value, ATTEMPTS, *args, **kwargs
        )

        senderobj.url = value_in("url", dkwargs, kwargs)
        return senderobj.send()
        
Senderable Class
------------------------------

.. code-block:: python
    :emphasize-lines: 8

    #webhooks.senders.base
    class Senderable(object):
        #cached properties
        url
        payload
        jsonified_payload
        
        # action methods designed to be easily overwritten
        get_url()
        get_payload()
        get_jsonified_payload()
        notify()
        send() # makes the attempts and uses notify()
        
Senderable Class (What it does)
--------------------------------

* Serializes the data
* Makes all the attempts
* Records the response

Sender Construction
------------------------------

The sender_callable

* function: handy, but not easily extendable
* http://bit.ly/webhooks-simple

The senderable class

* Class: Not as handy, Easily extendable
* http://bit.ly/webhooks-senderable


Django Integration
------------------------------

* https://github.com/pydanny/dj-webhooks
* https://github.com/pydanny/dj-webhooks#quickstart

dj-webhooks sender_callable I 
------------------------------

* Trying to avoid function argument mess. Slow refactor.

.. code-block:: python

    def orm_callable(wrapped, dkwargs, hash_value=None, *args, **kwargs):

        if "event" not in dkwargs:
            msg = "djwebhooks.decorators.hook requires an 'event' argument in the decorator."
            raise TypeError(msg)
        event = dkwargs['event']
        
        # Check for two more arguments. Truncated for space.
        
        senderobj = DjangoSenderable(
                wrapped, dkwargs, hash_value, WEBHOOK_ATTEMPTS, *args, **kwargs
        )

        
dj-webhooks sender_callable II 
------------------------------

.. code-block:: python

    try:
        senderobj.webhook_target = WebhookTarget.objects.get(
            event=event,
            owner=owner,
            identifier=identifier
        )
    except WebhookTarget.DoesNotExist:
        return {"error": "WebhookTarget not found"}
    senderobj.url = senderobj.webhook_target.target_url
    senderobj.payload = senderobj.get_payload()
    senderobj.payload['owner'] = getattr(kwargs['owner'], WEBHOOK_OWNER_FIELD)
    senderobj.payload['event'] = dkwargs['event']

    return senderobj.send()
    
dj-webhooks Senderable
-----------------------

.. code-block:: python
    :emphasize-lines: 3

    class DjangoSenderable(Senderable):

        def notify(self, message):
            if self.success:
                Delivery.objects.create(
                    webhook_target=self.webhook_target,
                    payload=self.payload,
                    # truncated for space
                )
            else:
                Delivery.objects.create(
                    webhook_target=self.webhook_target,
                    payload=self.payload,
                    # truncated for space
                )

Senderable Class
-----------------

* Serializes the data
* Makes all the attempts
* Records the response (in the ORM)


Example: dj-webhooks
------------------------------

The sender_callable

* function: copied, not extended
* http://bit.ly/webhooks-orm-L73-L127

The senderable object

* Class: extended the original
* http://bit.ly/djwebhooks-senderable-L48-L70


Example in Action
-------------------

Every time a project is updated:


.. code-block:: python
    :emphasize-lines: 3, 9-12

    # This assumes the project update was committed by user 'audreyr'
 
    @djwebhooks.decorators.hook(event="project.update") 
    def send_project_update(project, owner, identifier):
        """ :event: i.e. GitHub commit.push. Not unique! 
            :owner: Who created a webhook. I.E. pydanny
            :identifier: A owner or system defined key."""
        # Add MOAR logic here as needed
        return {
                'title': project.title,
                'description': project.description,
                # Truncated for space

The Problem of Time
----------------------

* What if calculating the payload takes forever?
* What if the payload is huge?
* What if the client's response takes too long?

How to Make it Fasterrerer?
----------------------------

* Asynchronous task/job queues
* Celery or RedisQ

Example of Fasterrererer
---------------------------

.. code-block:: python
    :emphasize-lines: 3,13

    from django_rq import job

    @job
    @djwebhooks.decorators.ook(event="project.update") 
    def send_project_update(project, owner, identifier):
        """ :event: i.e. GitHub commit.push. Not unique! 
            :owner: Who created a webhook. I.E. pydanny
            :identifier: A owner or system defined key.
        """
        # Add MOAR logic here as needed
        return {
                # Truncated for space
    send_project_update.delay()
    
Testing (Unit vs Functional)
================================

* Easier to test against http://httpbin.org than not
* Trying to stay in units, but not losing sleep over it


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