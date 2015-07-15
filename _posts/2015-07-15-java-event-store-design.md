---
layout: post
title: JEEventStore - Design Rationale and Feature Wishlist
categories: JEEventStore, event-store
---

Since no existing event store for Java [fits our needs]({% post_url 2014-09-02-event-store-for-java %}) and integrates well with Java EE, I decided to write our own implementation of an event store, which is straight-forward anyways. 

As for the architecture of an event store, Jonathan Oliver's [NEventStore](https://github.com/NEventStore/NEventStore) is extremely well designed.   In principle, it is exactly what we were looking for --- except that it is written in C#/.NET and not Java.  NEventStore nevertheless served as an architectural template for our own Java event store, which you'll surely notice when looking at the public API, see e.g. [IStoreEvents.cs](https://github.com/NEventStore/NEventStore/blob/master/src/NEventStore/IStoreEvents.cs) and [EventStore.java](https://github.com/JEEventStore/JEEventStore/blob/master/core/src/main/java/org/jeeventstore/EventStore.java).

At the same time, I did not want to reinvent the wheel.  In particular, although NEventStore advertises itself as a _persistence agnostic Event Store for .NET_, there already is a persistence API that is agnostic to the underlying database in Java EE --- JPA.  While JPA might mean some performance overhead, it is the standard persistence technology provided by Java EE and therefore widely supported on the containers and databases.  Additionally, authentication to the database server and other infrastructure concerns such as connection pooling can be configured directly in the container.  

Another (minor) problem regards asynchronicity in the Java EE context:  Java EE applications [must not](http://stackoverflow.com/questions/533783/why-spawning-threads-in-java-ee-container-is-discouraged) create or fiddle with their own threads as these interfere with the container's resources.  Anything that shall be run asynchronous must therefore be done with provided Java EE APIs.
On the positive side, the container guarantees thread-safety of EJBs and automatically manages a pool of stateless beans, which can be configured by the server operator.  In particular, when running multiple nodes in a cluster, the container can automatically route incoming requests between cluster nodes, i.e., we get horizontal scaling almost for free.

Finally, I wanted to keep the event store modular, such that one can easily exchange, say, the underlying persistence layer (e.g., from JPA to JDBC or some NoSQL store), change the serialization format or add caching layers.  Implemented properly, the injection of resources and other EJB beans into the services can be configured by the developers or the server operators as needed.  This opens the door for extreme modularity.  For example, suppose the team would like to add a caching layer to the serialization engine, since they found that too much time is spent in this part of the application.  Easy -- simply add a caching decorator of the serialization engine and inject this decorator into the event store.

# Feature List

This brings us to the feature wish list I had in mind when designing JEEventStore:

* Target platform: Java EE.
* No additional dependencies except, of course, for the serialization library of choice and/or the persistence engine.
* Events shall not be needed to implement an interface provided by the event store to be stored.
* Support multiple buckets in the event store (e.g., for multitenancy or multiple bounded context)
* Be able to query all events of a bucket or all events of a given event stream; order guarantee per stream
* Be able to query all events in a given event stream 
* Query specific versions of an event stream
* Append-only mode:  Store events without requiring to read the object from database (saves a database roundtrip)
* Event streams support change sets of multiple events with transaction semantics:  either store all events in a change set or none; each committed change set bumps the stream version by one
* Optimistic Locking 
* **Modularity**, i.e., all infrastructure details can be replaced transparent to the clients; support different serialization and persistence engines, to be configured in the deployment descriptor

# Modularity

Some of the modularity features I had in mind:

* Cache reads from the persistence layer by adding a decorator to the actual persistence engine
* Cache de-serialization of events by adding a decorator to the serialization engine
* Add encryption by adding a suitable decorator to the serialization engine
* Add other persistence engines beside JPA, e.g., direct JDBC access, MongoDB or other NoSQL engines with suitable atomicity semantics (Redis might be a candidate)
* Add other serialization engines beside GSON, e.g., XML, Protobuff, ...
* Allw event updasting by writing custom type converters
* Ease of use:  provide drop-in EJB-jars, which developers can simply include in the Maven configuration to add JEEventStore to their application
* be able to write own deployment descriptors for more complex setups with multiple decorators
* **Event Notification** interface that runs within the same transaction, to avoid polling of the database.  Support polling for persistence layers without transaction semends or when 2-Phase-Commits with the messaging infrastructure shall be avoided.

