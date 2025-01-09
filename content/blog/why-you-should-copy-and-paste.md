---
title: "Why you should copy and paste"
date: 2013-04-11
categories: design
---
DON'T REPEAT YOURSELF!
----------------------

It's a good rule of thumb, but I worry about the number of developers for whom it's a comforting mantra. 

Why is it we should avoid repition in our code? Sustaining the pace of delivery and minimising defects are goals most of us can agree on. Repitition leads to updating the same thing in many places, which can be time consuming and error prone. Therefore we should never repeat ourselves, right? I don't believe it's so simple.

What often seems to be ignored are the costs of avoiding repitition. Large companies appear to have huge benefits from being able to spread shared costs, such as HR, yet some start-ups without these apparent efficiencies, but with the ability to change rapidly can overhaul them. The same advantages can be seen in small independent software systems and we should accept that repitition may be a cost worth paying to achieve it.

Where the instances of the repition occur in close proximity, e.g. within the same function or class, these can be trivial, but as we move out to separate codebases maintained by separate teams, the costs can become prohibitive. Developers who haven't maintained a library that is a dependency of other systems tend to underestimate the overhead of doing so and presume that it will automatically lead to those systems moving seamlessly in step. The reality is that downstream systems will often overlook an updated library and the assumed benefit in everyone receiving a change never materialises.

An even more fundamental problem that can occur is that what at first appears as repitition can upon further development be revealed as a number of subtly, but importantly different use cases.  In these cases attempting to cram both into using the same code can unnecessarily complicate design, impeding delivery speed and increasing the chance of defects.

So what are the critical factors when examining reptition for deciding whether copy and paste is appropriate:

 * Locality - sharing code locally has a much lower overhead than sharing between teams
 * Frequency - dealing with code copied and pasted once is a lot more palatable than doing it five times
 * Scope - the overhead of managing the library or other abstraction to avoid repitition is more likely to be repaid when the scope of repitition is larger
 * Clarity of abstraction - collections of utilities without a single defining theme attract many small changes making them more complex for their consumers to keep up with
 * Implementation maturity - fresh code and especially not yet written code can often appear to involve repitition, but upon closer inspection have different needs

In many cases there are small amounts of code that are useful to two or three different projects, but aren't on the critical path of those systems. Copying and pasting such code is the responsible thing to do.
