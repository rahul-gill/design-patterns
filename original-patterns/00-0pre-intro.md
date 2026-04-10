[]

[]

Creational Patterns

Abstract Factory ( 87 ) Provide an interface for creating families of
related or dependent objects without specifying their concrete classes.

Builder ( 97 ) Separate the construction of a complex object from its
representation so that the same construction process can create
different representations.

Factory Method ( 107 ) Define an interface for creating an object, but
let subclasses decide which class to instantiate. Factory Method lets a
class defer instantiation to subclasses.

Prototype ( 117 ) Specify the kinds of objects to create using a
prototypical instance, and create new objects by copying this prototype.

Singleton ( 127 ) Ensure a class only has one instance, and provide a
global point of access to it.

Structural Patterns

Adapter ( 139 ) Convert the interface of a class into another interface
clients expect. Adapter lets classes work together that couldn’t
otherwise because of incompatible interfaces.

Bridge ( 151 ) Decouple an abstraction from its implementation so that
the two can vary independently.

Composite ( 163 ) Compose objects into tree structures to represent
part-whole hierarchies. Composite lets clients treat individual objects
and compositions of objects uniformly.

Decorator ( 175 ) Attach additional responsibilities to an object
dynamically. Decorators provide a flexible alternative to subclassing
for extending functionality.

Facade ( 185 ) Provide a unified interface to a set of interfaces in a
subsystem. Facade defines a higher-level interface that makes the
subsystem easier to use.

Flyweight ( 195 ) Use sharing to support large numbers of fine-grained
objects efficiently.

Proxy ( 207 ) Provide a surrogate or placeholder for another object to
control access to it.

Behavioral Patterns

Chain of Responsibility ( 223 ) Avoid coupling the sender of a request
to its receiver by giving more than one object a chance to handle the
request. Chain the receiving objects and pass the request along the
chain until an object handles it.

Command ( 233 ) Encapsulate a request as an object, thereby letting you
parameterize clients with different requests, queue or log requests, and
support undoable operations.

Interpreter ( 243 ) Given a language, define a represention for its
grammar along with an interpreter that uses the representation to
interpret sentences in the language.

Iterator ( 257 ) Provide a way to access the elements of an aggregate
object sequentially without exposing its underlying representation.

Mediator ( 273 ) Define an object that encapsulates how a set of objects
interact. Mediator promotes loose coupling by keeping objects from
referring to each other explicitly, and it lets you vary their
interaction independently.

Memento ( 283 ) Without violating encapsulation, capture and externalize
an object’s internal state so that the object can be restored to this
state later.

Observer ( 293 ) Define a one-to-many dependency between objects so that
when one object changes state, all its dependents are notified and
updated automatically.

State ( 305 ) Allow an object to alter its behavior when its internal
state changes. The object will appear to change its class.

Strategy ( 315 ) Define a family of algorithms, encapsulate each one,
and make them interchangeable. Strategy lets the algorithm vary
independently from clients that use it.

Template Method ( 325 ) Define the skeleton of an algorithm in an
operation, deferring some steps to subclasses. Template Method lets
subclasses redefine certain steps of an algorithm without changing the
algorithm’s structure.

Visitor ( 331 ) Represent an operation to be performed on the elements
of an object structure. Visitor lets you define a new operation without
changing the classes of the elements on which it operates.

Addison-Wesley Professional Computing Series

Brian W. Kernighan, Consulting Editor

Matthew H. Austern, Generic Programming and the STL: Using and Extending
the C++ Standard Template Library

David R. Butenhof, Programming with POSIX ^(®) Threads

Brent Callaghan, NFS Illustrated

Tom Cargill, C++ Programming Style

William R. Cheswick/Steven M. Bellovin/Aviel D. Rubin, Firewalls and
Internet Security, Second Edition: Repelling the Wily Hacker

David A. Curry, UNIX ^(®) System Security: A Guide for Users and System
Administrators

Stephen C. Dewhurst, C++ Gotchas: Avoiding Common Problems in Coding and
Design

Dan Farmer/Wietse Venema, Forensic Discovery

Erich Gamma/Richard Helm/Ralph Johnson/John Vlissides, Design Patterns:
Elements of Reusable Object-Oriented Software

Erich Gamma/Richard Helm/Ralph Johnson/John Vlissides, Design Patterns
CD: Elements of Reusable Object-Oriented Software

Peter Haggar, Practical Java™ Programming Language Guide

David R. Hanson, C Interfaces and Implementations: Techniques for
Creating Reusable Software

Mark Harrison/Michael McLennan, Effective Tcl/Tk Programming: Writing
Better Programs with Tcl and Tk

Michi Henning/Steve Vinoski, Advanced CORBA ^(®) Programming with C++

Brian W. Kernighan/Rob Pike, The Practice of Programming

S. Keshav, An Engineering Approach to Computer Networking: ATM Networks,
the Internet, and the Telephone Network

John Lakos, Large-Scale C++ Software Design

Scott Meyers, Effective C++ CD: 85 Specific Ways to Improve Your
Programs and Designs

Scott Meyers, Effective C++, Third Edition: 55 Specific Ways to Improve
Your Programs and Designs

Scott Meyers, More Effective C++: 35 New Ways to Improve Your Programs
and Designs

Scott Meyers, Effective STL: 50 Specific Ways to Improve Your Use of the
Standard Template Library

Robert B. Murray, C++ Strategies and Tactics

David R. Musser/Gillmer J. Derge/Atul Saini, STL Tutorial and Reference
Guide, Second Edition: C++ Programming with the Standard Template
Library

John K. Ousterhout, Tel and the Tk Toolkit

Craig Partridge, Gigabit Networking

Radia Perlman, Interconnections, Second Edition: Bridges, Routers,
Switches, and Internetworking Protocols

Stephen A. Rago, UNIX ^(®) System V Network Programming

Eric S. Raymond, The Art of UNIX Programming

Marc J. Rochkind, Advanced UNIX Programming, Second Edition

Curt Schimmel, UNIX ^(®) Systems for Modern Architectures: Symmetric
Multiprocessing and Caching for Kernel Programmers

W. Richard Stevens, TCP/IP Illustrated, Volume 1: The Protocols

W. Richard Stevens, TCP/IP Illustrated, Volume 3: TCP for Transactions,
HTTP, NNTP, and the UNIX ^(®) Domain Protocols

W. Richard Stevens/Bill Fenner/Andrew M. Rudoff, UNIX Network
Programming Volume 1, Third Edition: The Sockets Networking API

W. Richard Stevens/Stephen A. Rago, Advanced Programming in the UNIX
^(®) Environment, Second Edition

W. Richard Stevens/Gary R. Wright, TCP/IP Illustrated Volumes 1-3 Boxed
Set

John Viega/Gary McGraw, Building Secure Software: How to Avoid Security
Problems the Right Way

Gary R. Wright/W. Richard Stevens, TCP/IP Illustrated, Volume 2: The
Implementation

Ruixi Yuan/W. Timothy Strayer, Virtual Private Networks: Technologies
and Solutions

Visit www.awprofessional.com/series/professionalcomputing for more
information about these titles.

Design Patterns

Elements of Reusable Object-Oriented Software

Erich Gamma
Richard Helm
Ralph Johnson
John Vlissides

[]

Boston • San Francisco • New York • Toronto • Montreal
London • Munich • Paris • Madrid
Capetown • Sidney • Tokyo • Singapore • Mexico City

Material from A Pattern Language: Towns/Buildings/Construction by
Christopher Alexander, copyright © 1977 by Christopher Alexander is
reprinted by permission of Oxford University Press, Inc.

Many of the designations used by manufacturers and sellers to
distinguish their products are claimed as trademarks. Where those
designations appear in this book, and we were aware of a trademark
claim, the designations have been printed in initial capital letters or
in all capitals.

The author and publisher have taken care in the preparation of this
book, but make no expressed or implied warranty of any kind and assume
no responsibility for errors or omissions. No liability is assumed for
incidental or consequential damages in connection with or arising out of
the use of the information or programs contained herein.

The publisher offers discounts on this book when ordered in quantity for
special sales. For more information, please contact:

Pearson Education Corporate Sales Division
201 W. 103rd Street
Indianapolis, IN 46290
(800) 428-5331
corpsales@pearsoned.com

Visit AW on the Web: www.awprofessional.com

Library of Congress Cataloging-in-Publication Data
Design Patterns : elements of reusable object-oriented software / Erich
Gamma ... [et al.].
               p. cm.—(Addison-Wesley professional computing series)
       Includes bibliographical references and index.
       ISBN 0-201-63361-2
       1. Object-oriented programming (Computer science)  2. Computer
software—Reusability.
     I. Gamma, Erich.  II. Series.
    QA76.64.D47 1994
    005.1'2—dc20
                                                                                                                        94-34264
                                                                                                                                 CIP

Copyright © 1995 by Addison-Wesley

All rights reserved. No part of this publication may be reproduced,
stored in a retrieval system, or transmitted, in any form, or by any
means, electronic, mechanical, photocopying, recording, or otherwise,
without the prior consent of the publisher. Printed in the United States
of America. Published simultaneously in Canada.

Cover art © M.C. Escher/Cordon Art - Baarn - Holland. All rights
reserved.

  ISBN 0-201-63361-2
  Text printed in the United States on recycled paper at Courier
  Westford in Westford, Massachusetts.
  37th Printing        March 2009

To Karin

—E.G.

To Sylvie

—R.H.

To Faith

—R.J.

To Dru Ann and Matthew Joshua 24:15b

—J.V.

Praise for Design Patterns: Elements of Reusable Object-Oriented
Software

  “This is one of the best written and wonderfully insightful books that
  I have read in a great long while...this book establishes the
  legitimacy of patterns in the best way: not by argument but by
  example.”

    —Stan Lippman, C++ Report

  “...this new book by Gamma, Helm, Johnson, and Vlissides promises to
  have an important and lasting impact on the discipline of software
  design. Because Design Patterns bills itself as being concerned with
  object-oriented software alone, I fear that software developers
  outside the object community may ignore it. This would be a shame.
  This book has something for everyone who designs software. All
  software designers use patterns; understanding better the reusable
  abstractions of our work can only make us better at it.”

    —Tom DeMarco, IEEE Software

  “Overall, I think this book represents an extremely valuable and
  unique contribution to the field because it captures a wealth of
  object-oriented design experience in a compact and reusable form. This
  book is certainly one that I shall turn to often in search of powerful
  object-oriented design ideas; after all, that’s what reuse is all
  about, isn’t it?”

    —Sanjiv Gossain, Journal of Object-Oriented Programming

  “This much-anticipated book lives up to its full year of advance buzz.
  The metaphor is of an architect’s pattern book filled with
  time-tested, usable designs. The authors have chosen 23 patterns from
  decades of object-oriented experience. The brilliance of the book lies
  in the discipline represented by that number. Give a copy of Design
  Patterns to every good programmer you know who wants to be better.”

    —Larry O’Brien, Software Development

  “The simple fact of the matter is that patterns have the potential to
  permanently alter the software engineering field, catapulting it into
  the realm of true elegant design. Of the books to date on this
  subject, Design Patterns is far and away the best. It is a book to be
  read, studied, internalized, and loved. The book will forever change
  the way you view software.”

    —Steve Bilow, Journal of Object-Oriented Programming

  “Design Patterns is a powerful book. After a modest investment of time
  with it, most C++ programmers will be able to start applying its
  “patterns” to produce better software. This book delivers intellectual
  leverage: concrete tools that help us think and express ourselves more
  effectively. It may fundamentally change the way you think about
  programming.

    —Tom Cargill, C++ Report

Contents

   Preface

   Foreword

   Guide to Readers

   1 Introduction

   1.1 What Is a Design Pattern?

   1.2 Design Patterns in Smalltalk MVC

   1.3 Describing Design Patterns

   1.4 The Catalog of Design Patterns

   1.5 Organizing the Catalog

   1.6 How Design Patterns Solve Design Problems

   1.7 How to Select a Design Pattern

   1.8 How to Use a Design Pattern

   2 A Case Study: Designing a Document Editor

   2.1 Design Problems

   2.2 Document Structure

   2.3 Formatting

   2.4 Embellishing the User Interface

   2.5 Supporting Multiple Look-and-Feel Standards

   2.6 Supporting Multiple Window Systems

   2.7 User Operations

   2.8 Spelling Checking and Hyphenation

   2.9 Summary

Design Pattern Catalog

   3 Creational Patterns

   Abstract Factory

   Builder

   Factory Method

   Prototype

   Singleton

   Discussion of Creational Patterns

   4 Structural Patterns

   Adapter

   Bridge

   Composite

   Decorator

   Facade

   Flyweight

   Proxy

   Discussion of Structural Patterns

   5 Behavioral Patterns

   Chain of Responsibility

   Command

   Interpreter

   Iterator

   Mediator

   Memento

   Observer

   State

   Strategy

   Template Method

   Visitor

   Discussion of Behavioral Patterns

   6 Conclusion

   6.1 What to Expect from Design Patterns

   6.2 A Brief History

   6.3 The Pattern Community

   6.4 An Invitation

   6.5 A Parting Thought

A Glossary

B Guide to Notation

   B.1 Class Diagram

   B.2 Object Diagram

   B.3 Interaction Diagram

C Foundation Classes

   C.1 List

   C.2 Iterator

   C.3 ListIterator

   C.4 Point

   C.5 Rect

Bibliography

   Index

Preface

This book isn’t an introduction to object-oriented technology or design.
Many books already do a good job of that. This book assumes you are
reasonably proficient in at least one object-oriented programming
language, and you should have some experience in object-oriented design
as well. You definitely shouldn’t have to rush to the nearest dictionary
the moment we mention “types” and “polymorphism,” or “interface” as
opposed to “implementation” inheritance.

On the other hand, this isn’t an advanced technical treatise either.
It’s a book of design patterns that describes simple and elegant
solutions to specific problems in object-oriented software design.
Design patterns capture solutions that have developed and evolved over
time. Hence they aren’t the designs people tend to generate initially.
They reflect untold redesign and recoding as developers have struggled
for greater reuse and flexibility in their software. Design patterns
capture these solutions in a succinct and easily applied form.

The design patterns require neither unusual language features nor
amazing programming tricks with which to astound your friends and
managers. All can be implemented in standard object-oriented languages,
though they might take a little more work than ad hoc solutions. But the
extra effort invariably pays dividends in increased flexibility and
reusability.

Once you understand the design patterns and have had an “Aha!” (and not
just a “Huh?”) experience with them, you won’t ever think about
object-oriented design in the same way. You’ll have insights that can
make your own designs more flexible, modular, reusable, and
understandable—which is why you’re interested in object-oriented
technology in the first place, right?

A word of warning and encouragement: Don’t worry if you don’t understand
this book completely on the first reading. We didn’t understand it all
on the first writing! Remember that this isn’t a book to read once and
put on a shelf. We hope you’ll find yourself referring to it again and
again for design insights and for inspiration.

This book has had a long gestation. It has seen four countries, three of
its authors’ marriages, and the birth of two (unrelated) offspring. Many
people have had a part in its development. Special thanks are due Bruce
Anderson, Kent Beck, and André Weinand for their inspiration and advice.
We also thank those who reviewed drafts of the manuscript: Roger
Bielefeld, Grady Booch, Tom Cargill, Marshall Cline, Ralph Hyre, Brian
Kernighan, Thomas Laliberty, Mark Lorenz, Arthur Riel, Doug Schmidt,
Clovis Tondo, Steve Vinoski, and Rebecca Wirfs-Brock. We are also
grateful to the team at Addison-Wesley for their help and patience: Kate
Habib, Tiffany Moore, Lisa Raffaele, Pradeepa Siva, and John Wait.
Special thanks to Carl Kessler, Danny Sabbah, and Mark Wegman at IBM
Research for their unflagging support of this work.

Last but certainly not least, we thank everyone on the Internet and
points beyond who commented on versions of the patterns, offered
encouraging words, and told us that what we were doing was worthwhile.
These people include but are not limited to Jon Avotins, Steve Berczuk,
Julian Berdych, Matthias Bohlen, John Brant, Allan Clarke, Paul
Chisholm, Jens Coldewey, Dave Collins, Jim Coplien, Don Dwiggins,
Gabriele Elia, Doug Felt, Brian Foote, Denis Fortin, Ward Harold,
Hermann Hueni, Nayeem Islam, Bikramjit Kalra, Paul Keefer, Thomas
Kofler, Doug Lea, Dan LaLiberte, James Long, Ann Louise Luu, Pundi
Madhavan, Brian Marick, Robert Martin, Dave McComb, Carl McConnell,
Christine Mingins, Hanspeter Mössenböck, Eric Newton, Marianne Ozkan,
Roxsan Payette, Larry Podmolik, George Radin, Sita Ramakrishnan, Russ
Ramirez, Alexander Ran, Dirk Riehle, Bryan Rosenburg, Aamod Sane, Duri
Schmidt, Robert Seidl, Xin Shu, and Bill Walker.

We don’t consider this collection of design patterns complete and
static; it’s more a recording of our current thoughts on design. We
welcome comments on it, whether criticisms of our examples, references
and known uses we’ve missed, or design patterns we should have included.
You can write us care of Addison-Wesley, or send electronic mail to
  design-patterns@cs.uiuc.edu   . You can also obtain softcopy for the
code in the Sample Code sections by sending the message “send design
pattern source” to   design-patterns-source@cs.uiuc.edu   . And now
there’s a Web page at
  http://st-www.cs.uiuc.edu/users/patterns/DPBook/DPBook.html   for
late-breaking information and updates.

Mountain View, California

            E.G.

Montreal, Quebec

            R.H.

Urbana, Illinois

            R.J.

Hawthorne, New York

            J.V.

August 1994

Foreword

All well-structured object-oriented architectures are full of patterns.
Indeed, one of the ways that I measure the quality of an object-oriented
system is to judge whether or not its developers have paid careful
attention to the common collaborations among its objects. Focusing on
such mechanisms during a system’s development can yield an architecture
that is smaller, simpler, and far more understandable than if these
patterns are ignored.

The importance of patterns in crafting complex systems has been long
recognized in other disciplines. In particular, Christopher Alexander
and his colleagues were perhaps the first to propose the idea of using a
pattern language to architect buildings and cities. His ideas and the
contributions of others have now taken root in the object-oriented
software community. In short, the concept of the design pattern in
software provides a key to helping developers leverage the expertise of
other skilled architects.

In this book, Erich Gamma, Richard Helm, Ralph Johnson, and John
Vlissides introduce the principles of design patterns and then offer a
catalog of such patterns. Thus, this book makes two important
contributions. First, it shows the role that patterns can play in
architecting complex systems. Second, it provides a very pragmatic
reference to a set of well-engineered patterns that the practicing
developer can apply to crafting his or her own specific applications.

I’m honored to have had the opportunity to work directly with some of
the authors of this book in architectural design efforts. I have learned
much from them, and I suspect that in reading this book, you will also.

Grady Booch
Chief Scientist, Rational Software Corporation

Guide to Readers

This book has two main parts. The first part ( Chapters 1 and 2 )
describes what design patterns are and how they help you design
object-oriented software. It includes a design case study that
demonstrates how design patterns apply in practice. The second part of
the book ( Chapters 3 , 4 , and 5 ) is a catalog of the actual design
patterns.

The catalog makes up the majority of the book. Its chapters divide the
design patterns into three types: creational, structural, and
behavioral. You can use the catalog in several ways. You can read the
catalog from start to finish, or you can just browse from pattern to
pattern. Another approach is to study one of the chapters. That will
help you see how closely related patterns distinguish themselves.

You can use the references between the patterns as a logical route
through the catalog. This approach will give you insight into how
patterns relate to each other, how they can be combined with other
patterns, and which patterns work well together. Figure 1.1 (page 12 )
depicts these references graphically.

Yet another way to read the catalog is to use a more problem-directed
approach. Skip to Section 1.6 (page 24 ) to read about some common
problems in designing reusable object-oriented software; then read the
patterns that address these problems. Some people read the catalog
through first and then use a problem-directed approach to apply the
patterns to their projects.

If you aren’t an experienced object-oriented designer, then start with
the simplest and most common patterns:

    • Abstract Factory (page 87 )

    • Adapter ( 139 )

    • Composite ( 163 )

    • Decorator ( 175 )

    • Factory Method ( 107 )

    • Observer ( 293 )

    • Strategy ( 315 )

    • Template Method ( 325 )

It’s hard to find an object-oriented system that doesn’t use at least a
couple of these patterns, and large systems use nearly all of them. This
subset will help you understand design patterns in particular and good
object-oriented design in general.