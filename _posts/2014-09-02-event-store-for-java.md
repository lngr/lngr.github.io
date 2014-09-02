---
layout: post
title: Building an Event Store for Java
categories: JEEventStore
---

A common legal requirement in the area of medical software is that the change history of medical health records must be kept available.  With traditional relational databases, we used a one-revision--one-row approach, storing each revision of a record in a single row and having an extra field in the table point to the previous revision. This was working fine but put a significant amount of burden onto the development team.

When I learned about the concept of [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html), I figured that it fits naturally with these requirements, since reliable change history is given for free; in particular, obtaining the state of the whole system at a given time in the past is almost trivial. (Only much later we learned to love the additional benefits of event sourcing.) For a new project, we therefore decided to go the event sourcing path.  For that, we were looking for an [Event Store](https://github.com/eventstore/eventstore/wiki/Event-Sourcing-Basics#what-is-an-event-store) that we can integrate with our technology stack, which is based around Java EE (development done on Mac OS X or Linux and the production servers running Linux).


# Existing Event Stores

## .NET

The .NET-community has quickly adopted Event Sourcing (most often combined with
[CQRS](http://martinfowler.com/bliki/CQRS.html), i.e., CQRS+ES), and it is therefore not surprising that there are at least two mature open-source event stores for .NET.  The two most popular ones are:

* [NEventStore](https://github.com/NEventStore/NEventStore), originally developed by [Jonathan Oliver](https://github.com/joliver), now maintained by [Andrea Balducci](https://github.com/andreabalducci), [Damian Hickey](https://github.com/damianh), and [Jonathan Matheus](https://github.com/kblooie).
* [EventStore](http://geteventstore.com) by [Greg Young](https://github.com/gregoryyoung) and his team

If our technology stack was Windows with .NET, we would most definitely use one of these two, in particular since you can buy SLAs for EventStore. However,  .NET is simply not an option for us, since we do not want the vendor lock-in that comes with .NET. (Sorry, Mono, we're [not going to](http://techrights.org/2008/12/23/second-class-novellsoft-ide/) [use you](https://news.ycombinator.com/item?id=5042192) [in production](http://blog.jonathanoliver.com/why-i-left-dot-net/#mono).)  Furthermore, due to Germany's strict data protection laws, we cannot use a cloud solution.  For medical software, local deployments are a must.

## Java / JVM

I therefore evaluated the event stores that are available for the JVM.  In 2013 (the situation hasn't change much since), I was aware of the following projects that provide an event store, in some form or another:

* [Axon Framework](http://www.axonframework.org/)
* [Qi4j](http://qi4j.org/)
* [JDON](http://en.jdon.com/)
* [Eligo Eventsourced](https://github.com/eligosource/eventsourced), now superseded by [Akka Persistence](http://doc.akka.io/docs/akka/snapshot/scala/persistence.html)
* [EventStore2](https://github.com/ks-no/eventstore2)

### EventStore2

EventStore2 is a standalone event store, but it's built upon Akka, whose threading model conflicts with JEE containers. Furthermore, its interface depends on its `Event` class, meaning you'd introduce a technical dependency on the event store library into your domain model, which is a no-go for a [clean architecture](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html). We might have been able to fork the project and refactor this, but the dependency on Akka remains.

### Axon, JDON, Qi4j, Akka (Persistence)

None of Axon, JDON, Qi4J nor Akka is a standalone event store in the style of the .NET projects EventStore and NEventStore.  Rather, these are frameworks that each solve a different problem:

* Axon and JDON are CQRS+ES frameworks, where the event store is necessary infrastructure.
* Akka Persistence is a library that provides event sourcing for applications using the Actor programming model of Akka.
* Qi4j is an implementation of the [DCI](http://en.wikipedia.org/wiki/Data,_context_and_interaction) paradigm for Java.  An event sourcing mixin is provided, which, as of today, [lacks documentation](http://qi4j.org/2.0/library-eventsourcing.html). However, adding mixins to Java is arguably magic, at the least non-standard and most probably completely unportable --- good luck migrating your application later or introducing new team members.

Consequently, because of their broader focus, Axon, JDON, and Qi4j require you to annotate your domain model with framework annotations, which, as above, means introducing a strong technical dependency into your domain model.  In particular, let me stress that your domain model is not even testable without these annotations, i.e., without using the framework.  Again, this violates the principles of a clean architecture, where your domain model is supposed to have absolutely no dependencies to "outer" layers (particularly not on infrastructure concerns such as persistence).

Furthermore, Axon and Akka apparently require you to derive your domain classes from certain base classes provided by the framework.  To quote a very recent blog post by Dariusz Pasciak in the 8th Light blog, entitled [_'Convenient' Does Not Necessarily Mean 'Right'_](http://blog.8thlight.com/dariusz-pasciak/2014/08/27/convenient-does-not-necessarily-mean-right.html):

> If you are using a framework in which you are required to derive from some base class and then call methods on that base class in order to accomplish something that you are trying to accomplish, and the framework designers have provided inheritance as the only way of accomplishing this thing that you are trying to accomplish, and you are comfortable with forming such a strong relationship [footnote: Inheritance is one of the strongest forms of coupling in object-oriented code.] with that framework—then deriving your class from some other class that has methods on it that you would like to use may be right.
> <cite>Dariusz Pasciak</cite>


## Building an Event Store for Java EE

Due to the lack of suitable event stores for Java that can easily be used in a Java EE environment, we decided to build our own, called [JEEventStore](https://github.com/JEEventStore/JEEventStore).  In a series of upcoming posts, I'm going to write up the design rationale for our event store, wish-list and feature set, describe its architecture and will tell you how to use it in your own project.
