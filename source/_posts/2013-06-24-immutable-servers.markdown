---
layout: post
title: "Immutable Servers"
date: 2013-06-24 08:40
comments: true
categories: DevOps
---
Functional programming has become increasingly popular over the past few years. One of the key ideas of functional programming is the preference for immutability. Another important devlopment has been the rise of DevOps and the idea of infrastrucuture as code. If we value immutability in our code and we treat our infrastructure as code, can we gain benefit by having immutable servers?

What do I mean by an immutable server? Clearly, when we peer into the details, much of any server is mutable, but in terms of our interaction with them as developers and maintainers of the system; installing packages, updating configuration, deploying new versions of an application, we can keep to the simple rule that once the server has initialised we don't attempt to make any changes. This can mean creating machine images with everything ready to start, but may also include some initialisation scripts, perhaps using a tool like Chef solo, or the direct application of Puppet, but definitely doesn't include allowing a central Puppet or Chef server to effect changes.

It will, naturally, be necessary to make changes to the service running on the server, but it's important to make the distinction that it's the service that we're ultimately interested in updating. The server is an implementation detail. Using deployment methods based on bringing up parallel, or growing and shrinking auto-scaling groups fit perfectly with this idiom.

Why might it be desirable to have immutable servers? The key reason for favouring immutability, whether in software or the servers they run on is to minimise the possible states of the system. This makes it much easier to reason about what state the system is currently in. There's no need to worry about whether or not a particular change was applied to the servers. We know what state they started in, so that's the state they're in now.

Of course, no choice is without it's caveats: virtualisation and fast boot times are necessary to make this approach practical, it's much easier to apply this approach to stateless applications than datastores, but having used this approach for the past nine months, I'm convinced it's a valuable way of working.
