---
title: "Actor Oriented Model: Part 1"
seoTitle: "What is Actor Oriented Model?"
seoDescription: "Guide to Actor-Oriented Model, which enables the building of blockchain with high scalability and computation."
datePublished: Thu Jul 11 2024 12:30:19 GMT+0000 (Coordinated Universal Time)
cuid: clyh8ykas000d0aju3hlvbpkz
slug: actor-oriented-model-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720698316687/8a296ce7-05df-4893-a22d-e7ffc5a3b609.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1720698777007/be6b113b-388b-4225-ab50-64309ffb63e8.png
tags: blockchain, ao, actor-oriented

---

The Actor Oriented Model(AOM) is a conceptual model used in software engineering for concurrent computation.

Using AOM, developers can build highly scalable distributed networks that have the potential to grow and improve the efficiency of different technologies, like blockchain, as they scale.

**The AOM series** will help you to understand the core concepts of AOM.

*LFG (🧱.🚀)*

---

# Objectives

* Understand the principles of AOM
    

# What is an Actor?

Actor is the basic unit of computation of the AOM.

* It is **autonomous**,
    
* it **can execute concurrently** with other actors,
    
* and can only **communicate through message passing** with other actors.
    

Being autonomous enables the actor to have its own state management, which is not shared with other actors.

Since the state is not shared with other actors, no actor is required to wait for the execution of other actors, enabling the parallel execution of multiple actors simultaneously.

If the actors want to communicate with other actors, they need to use message-passing flow, which is the only method for actors to communicate with each other.

> ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1720696272686/dd705cae-fc60-45ed-ac6a-70276bde723e.png align="center")
> 
> You can imagine an actor as a piece of code with its own class and variables and is not dependent on the variables or classes of other code to get executed. If an actor *(actor2)* wants to read any variable, the source actor *(actor1)* will execute a separate function to send a message with the requested data.

# State Management

Every actor has its own state, which means its own memory that is not linked to other actors.

This enables actors to execute without any external requirement outside of their scope.

# Concurrency

As the actors are independent in both their computation limits and state management, multiple actors can be executed simultaneously.

This enables parallel computation, bringing high scalability to the applications.

# Message Passing

Complex applications require communications to share data between different computation units (or actors, in this case). Message passing among the actors makes this possible.

> Example:
> 
> An actor (actor1) wants to know the state of the variable `price` in another actor (actor2). To do this, actor1 will first send a message to actor2 with the request. Actor2 will compute the message request and then create another message with the state of the variable `price` to actor1.

As per the AOM,

* messages are **immutable,** meaning they cannot be changed once created.
    
* actors can process only one message at a time.
    

---

That's it for Part 1. *(I know it's really short, but I will increase the depth progressively.)*

*Thanks for reading!*

Any feedback is appreciated 🙌🏻

*Keep Building (🧱,🚀)*