# Clean Architecture
Notes from the book *Clean Architecture - A Craftman's Guide to Software Structure and Design* by Robert C. Martin

## Chapter 1: What is Design and Architecture

There is **no** difference between architecture and design, despite preconceptions that architecture operates at a high level and design takes care of low-level details. Little, low-level details support all the high-level decisions.

The goal of software architecture is to minimize the human resources required to build and mantain the required system.

The bigger lie that developers buy into is the notion that writing messy code makes them go fast in the short term, and just slows them down in the long term. The fact is that making messes is always slower than staying clean, no matter which time scale you are using. *The only way to go fast is to go well.*

## Chapter 2: A Tale of Two Values

Every software system provides two different values to the stakeholders: behaviour and structure.
  * **Behaviour**: the software behaves in a way that aligns with stakeholders' goals
  * **Structure**: the software must be easy to change. The diffuclty in making a change should be proportional only to the scope of the change, and not to the *shape* of the change. The more the architecture prefers one shape over another, the more likely new features will be harder and harder to fit into the structure.

Which is more important?
* If you give me a program that works perfectly but is impossible to change, tne it won't work when the requirements change, and I won't be able to make it work. Therefore the program, will become useless.
* If you give me a program that does not work but is easy to change, then I can make it work, and keep it working as requirements change. Therefore the program will remain continually useful.

There are systems that are practically impossible to change, because the cost of change exceeds the benefit of change.

Eisenhower's Matrix
> I have two kinds of problems, the urgent and the important. The urgent are not important, and the important are never urgent.

Behaviour is urgent but not always particularly important. Architecture is important, but never particularly urgent.

It is the responsibility of software developers to assert the importance of architecture over the urgency of features. It's always a struggle, but remember, as a software developer, you are a stakeholder.
