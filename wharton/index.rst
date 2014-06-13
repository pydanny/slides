===============================================================================
From NASA to Startups to Big Commerce: Building Maintainable, Scalable Projects
===============================================================================

by Daniel Greenfeld

http://pydanny

http://www.eventbrite.com/

Description
============

The decisions made in the beginning of a project are the ones you are stuck with for years. A early bad decision for a project can cost organizations time, money, and engineering resources throughout it's lifetime. In this session we’ll attempt to answer the following questions: How do you take scale into consideration during the early stages of a project? How do you do this without using up the critical ramp-up time of a project or organization? Finally, how do you fix critical problems when they arise without bringing everything down?


Caveat: The Constant of Worst Code
===================================

* There is a phenomena where anytime any software engineer comes into an existing project, they complain about their predecessors.

* It's always the worst code we've ever seen.

* We gripe about every decision, from the choice of data store to programming languages to deployment tool.

* It's so very easy for us with our hindsight to complain the mistakes made, the needless complications and over-simplifications.

* The ranting begins, often shared with our peers on IRC and Twitter. Here are some of my favorites, indulged by countless coders, including myself:

	* Why did they choose a relational database?
	* Why did they choose a non-relational database?
	* Why did they choose these programming languages?
	* Why did they choose this deployment tool?
	* Why did they choose this front-end CSS/JavaScript framework?
	* Why did they follow this design pattern?
	* Why did they use this or that framework?

Hindsight is 20/20
-------------------

* Coming into a new project and mocking about the mistakes of our predecessors is like making fun of how a well-to-do person has done on the stock markets.

* Projects grow organically, often as requirements come flying in at the last minute.

* If a project has the ability to hire you, it means they've been successful at turning a profit.

* Whatever faults the project might have, they've been successful. The code isn't academic, it's doing enough to make enough to hire you.

Be Understanding
-----------------

* Don't be a jerk.

* Try to understand how things happened the way they did

* Forgive your predecessor

	* They can provide useful information, even if not on the project

	* You never know when circumstances will be reversed.


Technical Debt
==============

* It's a loan against the future

	* Get it done fast!
	* Switch the requirements!
	* Test later!
	* Pivot!


Major Decisions: Political
==========================

Witness:

* ColdFusion instead of options
* Zope instead of Django
* Refusing naming standards "just be consistent"
* Allowing low bus factor
* Allowing irresponsible behavior


Major Decision: Technical
==========================

Witness:

* BigTable instead of Relational
* Documents instead of Records
* EAVs instead of Documents
* Refusing tests

Stay Modest
==================================

* You aren't the smartest
* You always have to learn
* Anyone who thinks they are the answer to the problems is a problem


Principals
============

* Simplicity

	* Your project's will be enough to complicate things

* Have tests

	* Even if the test coverage is low, have it working
	* Unit tests are best
	* Functional tests are better than nothing
	* Integration tests are better than nothing

* Have standards

	* Use the language standards
	* Document the standards
	* 
