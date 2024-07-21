---
title: "Actor Oriented Model: Part 2"
seoTitle: "Lifecycle of an Actor and Communication between Actors"
seoDescription: "A guide to understanding the lifecycle of an actor and communication between actors, in the actor-oriented model."
datePublished: Mon Jul 15 2024 13:00:35 GMT+0000 (Coordinated Universal Time)
cuid: clymzsvyx00190ajv72dvbla0
slug: actor-oriented-model-part-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721047321234/bb456ede-276e-4bda-9aba-2f20a7e49cfe.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1721047325051/cff4d303-f805-4bea-bcb1-712e90899496.png

---

gm again!

This is Part 2 of the AOM series. In the first part, we discuss the fundamentals of The actor-oriented model.

%[https://megabyte0x.xyz/blogs/actor-oriented-model-part-1] 

In this part, we will discuss the **Lifecycle of Actors and Communication b/w Actors.**

---

# Lifecycle of Actors

In the AOM, **Actors can create other actors**. This brings in the parent-child hierarchy and contributes to the scalable model.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721040878299/1d295bca-1795-46a2-85ae-48ba3407e662.png align="center")

## Child Actor Properties

As explained before, every actor is isolated, i.e., they have their own memory, state management, and message queue. This also applies to child actors. So, when a parent actor creates a child actor, the child actor will have a separate state, memory, and message queue from the parent actor.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721041221282/1b64be51-6209-4c77-ac6b-21461d4e0722.png align="center")

Having separate state management, memory, and message queues helps create highly efficient and scalable network models. For example, a parent actor can create a separate child actor for each function, and since the child actor has the same properties as other actors, all the child actors (and their respective functions) can be executed simultaneously, creating high-speed computation.

## Parent-Child Actor Relationship

In a parent-child relationship, parents have some supervision over their child, and the same is implemented in the parent-child actors. Parent actors can supervise child actors using different strategies for different conditions.

Parent actors can manage the lifecycle of child actors, including

* creation,
    
* monitoring,
    
* restarting,
    
* and termination.
    

For example, If a child actor gets into a deadlock, the parent actor can restart or terminate that child actor.

# Communication b/w actors

The only way for an actor to communicate with another actor is by using the messages.

For Example:

If **actor2** wants to get a value of a variable `count` from **actor1,** they will need to go through the following process of message passing:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721033020733/4e22d038-a34c-42a3-a405-15ca7184c9a7.png align="center")

1. The **actor2** will send a message to **actor1** to get the `count` value.
    
2. The actor1 will receive the message, process the message and send a message, including the `count` value, back to **actor2.**
    

The following are the key characteristics of the message passing:

* Asynchronous Communication
    
* Message Queue
    
* Message Immutability
    
* Message Processing
    
* Message Handling and Context Switching
    
* Fault Tolerance and Error Handling
    

Let's understand each of them.

## Asynchronous Communication

When an actor(sender) sends a message to another actor(receiver), they can execute the remaining code without waiting for a reply. This removes the sender's dependency on the receiver; now, both can work simultaneously.

## Message Queue

When a message is sent, it is wrapped in an envelope and placed in the receiver's message queue.

Each actor has a dedicated message queue where incoming messages are stored until the actor processes them.

## Message Immutability

After sending a message, the message and its content can't be changed. This ensures the consistency of the system and prevents any side effects that might occur due to this.

## Message Processing Loop

Every Actor has an event loop that keeps looking for the new message received in the message queue.

When a new message is found, the following happens:

1. The message gets dequeued from the message queue.
    
2. The corresponding handler function is then called for the message in the actor.
    

This ensures that the actor processes only one message at a time and maintains a single-threaded execution model, i.e., a message starts processing after the previous message is being processed completely.

## Message Handling

When a message is dequeued from the message queue, the actor's message handlers are invoked to handle it.

The handler's invocation may update the actor's current state (like changing the variable values), send new messages (a response back to the sender), or create child actors (new actors to distribute the computation).

## Fault Tolerance and Error Handling

If an actor encounters an error while processing a message, the actor system or the parent actor can manage the actor according to the defined supervision strategy.

For example:

If an actor encounters an error while processing a message, the actor system will detect the error. Depending on the supervision strategy, the actor system or the parent actor can restart or terminate the actor or report this error to a higher level in the hierarchy.

---

Thanks for reading! If you have any queries, feel free to DM 🫡

Feedback is always appreciated 🙌🏻

*Keep Building (🧱, 🚀)*