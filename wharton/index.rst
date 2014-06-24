===============================================================================
From NASA to Startups to Big Commerce: Building Maintainable, Scalable Projects
===============================================================================

by Daniel Greenfeld

http://pydanny

http://www.eventbrite.com/

Description
============

The decisions made in the beginning of a project are the ones you are stuck with for years. A early bad decision for a project can cost organizations time, money, and engineering resources throughout it's lifetime. In this session we’ll attempt to answer the following questions: How do you take scale into consideration during the early stages of a project? How do you do this without using up the critical ramp-up time of a project or organization? Finally, how do you fix critical problems when they arise without bringing everything down?

Intro
========

About an hour's drive from here, over in New Jersey is where my maternal grandparents lived. I grew up spending my vacations there, in their house. This house, you see, was magical. Not just because it was where my grandparents lived until they passed away. Because my grandpa designed it himself. Built in the 1950s by a family of recent immigrants to the USA from scratch, way before pre-built frames were acceptable for the middle class.

One of my favorite stories of the construction of the house was how a single son of the family insisted on doing the chimney himself. I've been told it took the guy six months to assemble it, and it was clearly worth the effort. To me it was perfect, without a blemish.

In contrast I grew up in houses assembled from pre-made components. Factories build frames, which are then nailed together by carpenters, plugged in by plumbers and electritians. Even as a child I could tell that the house I lived in didn't compare to my grandparent's carefully designed, custom crafted home.

Yet, my grandparent's house wasn't perfect. The last year or so it belong in my family I discovered the blemishes. The most notable was the balcony I never knew existed. It was off the master bedroom, and my grandparents had allowed a pine tree to overwhelm it. Why? Well, it wasn't well built. They worried it might break, and didn't want to risk anyone, especially curious grandchildren on it. Fixing it was impossible, as it was assembled in a non-standard way that meant carpenters wanted unreasonable amounts of money to do the work of repair.

Where am I going with this?

Software projects are like houses.

The parallels of foundation, construction, management, team, and maintenance are very closely related.

We'll fix it in the next Version 2!
====================================

* Dangerous (Digg, Oraqnge, etc)
* You are doing construction on a property people are living in
* https://twitter.com/AcademicsSay/status/461290570082947074/photo/1



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

``Hindsight`` is 20/20
--------------------------

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














Choosing Tech
==============

Infant mortality amongst (web)frameworks and languages is ridiculously high, choosing wrong can get expensive very rapidly.
(https://twitter.com/jmattheij/status/468677218823323648)

Managers - Who do you listen to?
==================================

* The engineer who clocks 16 hours a day, who tells you everything wonderful that they do?
* The engineer who clocks 6-10 hours a day, who makes their deadlines like clockwork?
* The hard-to-reach engineer who holds the mission critical pieces?

