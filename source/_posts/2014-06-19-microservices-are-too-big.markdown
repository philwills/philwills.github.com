---
layout: post
title: "Microservices are too (conceptually) big"
date: 2014-06-19 22:00
comments: true
categories: architecture microservices
---
The idea of microservices has attracted a lot of attention, particularly following [James Lewis and Martin Fowler's article](http://martinfowler.com/articles/microservices.html). There's lots of good advice and sensible thoughts in that piece, but despite some [noble attempts](http://www.brunton-spall.co.uk/post/2014/05/21/what-is-a-microservice-and-why-does-it-matter/) to try and pin them down, microservices have suffered from a great deal of [semantic diffusion](http://martinfowler.com/bliki/SemanticDiffusion.html) and questions along the lines of *'Is anyone actually doing microservices properly?'*. 

I believe that much of this is down to two valuable architectural patterns frequently becoming conflated when discussing microservices: independent services and single purpose applications.

###Independent services

Independent services allow teams to align themselves with some capability in the business and focus on that without having to constantly consider the implementation detail of other services. In order to achieve this, they communicate over stable, well-defined interfaces to other services they interact with.

###Single purpose applications

Single purpose applications do one thing. 

If there's more than one property which defines whether the app is healthy or not, it's probably trying to do more than one thing. 

If you think there's more than one metric you'd like to scale the application on, it's probably trying to do more than one thing. 

The aim of this approach is to give the design of an app focus and provide simple answers to key operability questions. Often such apps are small enough that, if things change, you can rewrite them without having to negotiate budgetary approval to do so.

###Why does the distinction matter?

While two independent services accessing the same data store is an anti-pattern, this isn't necessarily the case for single purpose applications. As long as they form part of the same service, having different apps responsible for writing to and querying from the same store can be a beneficial design. 

There are significant benefits in making apps independently deployable, but routinely deploying apps that form part of the same service together as an optimisation (e.g. to maximise cache consistency) can be sensible. Similarly, it might make sesnse to make sure you deploy such apps in a specific order sometimes to ease a migration. However, if you find yourself regularly deploying more than one service at time, or frequently worrying about the sequence of their releases, they're probably not very independent.

As ever, it's better to focus on the qualities that help us achieve the goals we are striving for; teams able to evolve independently, resiliency, simplicity, rather than striving for some platonic ideal of doing microservices 'properly'. 
