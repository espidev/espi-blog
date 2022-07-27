---
title: "Creating an API in Java"
date: 2019-07-01T08:58:29-04:00
draft: false
---

**Warning: This page is incomplete!**

So I have recently finished making a beta of a SpigotMC plugin (ProtectionStones), and so I am going to share my thoughts on Java API development, from the ecosystem, to simply the structure of your program.

### Do I need to develop an API for my project?

Think about the use case of your project, and how large the codebase will be. Will you be maintaining this project far into the future? Developing your project in a way that can accommodate an API then would well be worth the work. Over time, you may choose to split your project into multiple projects, which would then require an interface in between them. If the project had been well-designed, and used the API internally, then decoupling different sections of the project would not be much work.

### Choosing a Build System

The two largest build systems in Java's ecosystems are Gradle and Maven. Maven is older, but more widely used. It uses XML tags to structure the build, and has a very mature ecosystem of plugins, allowing automation of many different tasks with a simple command. The main drawback however, is how long the build information file (pom.xml) will be with larger and more complex projects, since XML tags do lengthen simple tasks. Gradle on the other hand, is newer, and instead uses Groovy (the programming language) for its build scripts. It was designed with larger projects in mind, shortening build scripts because of its more fluent syntax, and improving build speeds. It is most well known for being used as the default build system for Android. In my experience, choosing maven may be easier for beginners, since tutorials are more widely available, and the XML syntax is relatively easy to understand. There is also a larger and more diverse choice of plugins for Maven, so the inexperienced will find it to be easier to setup a full build toolchain. However, with larger projects, I would definitely recommend gradle, since the syntax is much cleaner and shorter. Complex Maven build files can end up being thousands of lines long, mostly taken up by beginning and ending XML tags. They both can share the same package formats and pull from Maven Central (the largest package repository in the Java ecosystem), so the choice of packages is not a problem.

### Designing an API

There are many standards and views on designing APIs in Java. In my experience, the simplest way to structure projects for an API is to make extensive use of abstracted utility classes, as well as reuse of the API internally. I am assuming that you have knowledge of object oriented programming when reading this section.

### Objects

Choosing what to make an object can be easy for the obvious, but can be much more difficult when dealing with more abstract concepts. For example, it may be obvious to create a "Command" interface to represent a single command argument, but it can be hard to choose whether or not to create a common interface for similar implementations of a command. It's usually better to think about the following questions when encountering this situation. Are you trying to make an object for the sake of making an object? Can you think of two or more use cases for an object like this? Would it be too confusing for people to use in an API? Of course, there is the choice of making some more abstract objects (ex. a bunch of executor types) internal use only, to avoid confusing people that use the API.

### Utility Classes

You will find that you will have a lot of static methods that provide a single, unique purpose, and don't fit in objects. In this case, you can group them into utility classes. For these utility classes, it is good to name the global ones after the project (ex. toml API would have a TOML class), with static methods returning sub-utility classes (ex. TOML.getConfigManager()).

### Visibility

It is standard practice to only expose and keep public methods and classes that are relevant to the API, and not helper functions. If you really need helper functions to be public since they are used throughout your implementations, consider moving and grouping them in utility classes to avoid confusing developers that use your API.

### Implementation Style

While Javadocs and documentation is extremely helpful for developers, they might sometimes want to be check source code for the way that implementations work. As a general rule, keep API methods short and simple, and strive to move as much implementation and logic into utility classes instead. This keeps the code easy to read and navigate, so that developers do not have to scan through logic that may be irrelevant to what they are searching for.

### The Billion Dollar Mistake (null)

NullPointerExceptions are perhaps the most common (and annoying) sort of errors to occur in a Java program. Use Google's preconditions. @NotNull, @Nullable

### Concurrency

warn when using synchronized blocks

### Deploying the API
