---
layout: post
title: Literature on Event Sourcing, Event Stores and CQRS
categories: event-sourcing, event-store, cqrs
---

A collection of literature I recommend for reading on Event Sourcing/Event Stores and CQRS, to be extended over time.  Ranges from high-level beginner material to implementation details and code samples.

# Event Sourcing

* [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html): Entry in Martin Fowler's bliki
* [Event Sourcing Basics](https://github.com/eventstore/eventstore/wiki/Event-Sourcing-Basics): Introduction to Event Sourcing in the documentation of (Get)EventStore
* Aggregates and Event Sourcing: A+ES: Appendix A contributed by Rinat Abdullin in Vaughn Vernon's IDDD book (see below). The source code is availble on [Github](https://github.com/Lokad/lokad-iddd-sample), but I also recommend to read the book.
* [Introducing Event Sourcing](http://msdn.microsoft.com/en-us/library/jj591559.aspx): A chapter in the CQRS Journey e-book
* [Event Sourcing](http://cqrs.wikidot.com/doc:event-sourcing) in a CQRS wiki
* [Events as a Storage Mechanism](http://cqrs.wordpress.com/documents/events-as-storage-mechanism/)  by Greg Young
* [Event Sourcing a la Lokad](http://abdullin.com/post/event-sourcing-a-la-lokad/) by Rinat Abdullin
* [Event Sourcing as a strategic advantage](http://lostechies.com/jimmybogard/2011/10/11/event-sourcing-as-a-strategic-advantage/) by Jimmy Bogard
* [A better domain events pattern](http://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/) by Jimmy Bogard
* [IDDD Sample application](https://github.com/VaughnVernon/IDDD_Samples/tree/master/iddd_collaboration/src/main/java/com/saasovation/collaboration): The "Collaboration" Bounded Context of Vaughn Vernon's sample code for this IDDD book (see below) uses event sourcing, see, e.g., the [`Discussion`](https://github.com/VaughnVernon/IDDD_Samples/blob/master/iddd_collaboration/src/main/java/com/saasovation/collaboration/domain/model/forum/Discussion.java) AR derived from the [`EventSourcedRootEntity`](https://github.com/VaughnVernon/IDDD_Samples/blob/master/iddd_common/src/main/java/com/saasovation/common/domain/model/EventSourcedRootEntity.java).  This sample can get you started with Event Sourcing in Java very quickly.
* [NEventStore CommonDomain](https://github.com/NEventStore/CommonDomain): Originally by Jonathan Oliver and now maintained as part of the NEventStore project, the CommonDomain is a great foundation for C# applications that want to use Event Sourcing and can also serve as a reference for ports into other languages.
* [.NET Event Sourcing](https://github.com/elliotritchie/NES): a lightweight framework for C#, attempts to fill in the gaps between NServiceBus and NEventStore. 
* [AggregateSource](https://github.com/yreynhout/AggregateSource): A lightweight infrastructure for doing eventsourcing using aggregates in C# by Yves Reynhout; see also [AggregateSource Testing
](https://github.com/yreynhout/AggregateSource/blob/master/src/Testing/AggregateSource.Testing/README.md)

## Testing
* [Scenario-based Unit Tests for DDD with Event Sourcing](http://abdullin.com/post/scenario-based-unit-tests-for-ddd-with-event-sourcing/)
* [Axon framework - behavior driven testing](http://pkaczor.blogspot.de/2013/11/axon-framework-behaviour-driven-testing.html) - we are not using Axon, but the BDD Given--When--Then pattern presented is quite powerful
* [Trench Talk: `Assert.That(We.Understand());`](http://seabites.wordpress.com/2013/11/26/trenchtalk-assertthatweunderstand/) by Yves Reynhout on BDD testing with ES

# Event Store

* [Building an Event Storage](http://cqrs.wordpress.com/documents/building-event-storage/) by Greg Young
* [Your Coffee Shop Doesn't Use Two-Phase Commit](http://eaipatterns.com/docs/IEEE_Software_Design_2PC.pdf) by Gregor Hope
* [How I Avoid Two-Phase Commit](http://blog.jonathanoliver.com/how-i-avoid-two-phase-commit/) and [Removing 2PC (Two Phase Commit)](http://blog.jonathanoliver.com/removing-2pc-two-phase-commit/): Jonathan Oliver on how NEventStore avoids expensive 2PCs

# CQRS (including CQRS+ES)

* [DDDCQRS mailing list](http://dddcqrs.googlegroups.com): Worth reading the archive.
* [CQRS](http://martinfowler.com/bliki/CQRS.html): Entry in Martin Fowler's bliki
* [Eventually Consistent - Revisited](http://www.allthingsdistributed.com/2008/12/eventually_consistent.html) article by Werner Vogel, CTO of Amazon
* [CQRS Journey](http://msdn.microsoft.com/en-us/library/jj554200.aspx) long e-book on CQRS by a Microsoft team, a must-read
* [Command-Query Responsibility Segregation](http://www.infoq.com/presentations/Command-Query-Responsibility-Segregation): video/slides of a talk by Udi Dahan at QCon
* [CQRS Starter Kit and FAQ](http://www.cqrs.nu/)
* [Clarified CQRS](http://www.udidahan.com/2009/12/09/clarified-cqrs/) by Udi Dahan
* [Busting some CQRS myths](http://lostechies.com/jimmybogard/2012/08/22/busting-some-cqrs-myths/) by Jimmy Bogard --- definitely worth reading
]
* [Why you should be using CQRS almost everywhere...](http://www.udidahan.com/2011/10/02/why-you-should-be-using-cqrs-almost-everywhere%E2%80%A6/) by Udi Dahan
* [When to avoid CQRS](http://www.udidahan.com/2011/04/22/when-to-avoid-cqrs/) by Udi Dahan
* [CQRS and Event Sourcing](http://cqrs.wordpress.com/documents/cqrs-and-event-sourcing-synergy/) by Greg Young
* [Why I Still Love CQRS (and Messaging and Event Sourcing)](http://blog.jonathanoliver.com/why-i-still-love-cqrs-and-messaging-and-event-sourcing/) by Jonathan Oliver on benefits of CQRS+ES, many links included
* [CQRS: Out of Sequence Messages and Read Models](http://blog.jonathanoliver.com/cqrs-out-of-sequence-messages-and-read-models/) by Jonathan Oliver

* [CQRS and user experience](http://lostechies.com/jimmybogard/2012/08/23/cqrs-and-user-experience/) by Jimmy Bogard
* [CQRS -- Use Your Common Sense](http://eventuallyconsistent.net/2012/08/24/cqrs-use-your-common-sense/) by Steve Bate
* [CQRS Event Sourcing: Validate UserName uniqueness](http://stackoverflow.com/questions/9495985/cqrs-event-sourcing-validate-username-uniqueness): Stackoverflow question on a very common use-case
* [Greg Young's SimpleCQRS example](https://github.com/gregoryyoung/m-r/tree/master/SimpleCQRS): small sample project that let's one quickly understand the CQRS principle

* [Lokad.CQRS Sample Project](http://lokad.github.io/lokad-cqrs/) An advanced (and complex) CQRS sample project



# DDD

* [Domain Driven Design](http://www.amazon.com/dp/0321125215): *The* blue book by Eric J. Evans
* [Implementing Domain Driven Design](http://www.amazon.com/dp/0321834577):  A book by Vaughn Vernon, which I can recommend even more than Evan's original book;  if you want to read only one book on the topic, read this.
* [Don't Create Aggregate Roots](http://www.udidahan.com/2009/06/29/dont-create-aggregate-roots/) by Udi Dahan
* [FAQ on Aggregates](http://www.cqrs.nu/Faq/aggregates)
* [FAQ on Sagas](http://www.cqrs.nu/Faq/sagas)


# Messaging

* [The LMAX disruptor](http://martinfowler.com/articles/lmax.html): Entry in Martin Fowler's bliki.  The LMAX disruptor a high-performance, single-threaded messaging architecture for the JVM that for a financial application handles up to 6 million order per second.  Even if such scalability is way out of scope of most projects, you should be aware of it.
* [NServiceBus](https://github.com/Particular/NServiceBus): The most popular service bus for .NET.  If you are going to use CQRS+ES in a C# project, you should look at this.

