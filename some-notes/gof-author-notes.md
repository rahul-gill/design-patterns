# talk by gof author
- ref  Twenty years of Design Patterns - Ralph E. Johnson (GoF author) - SugarLoafPLoP 2014 : https://www.youtube.com/watch?v=ALxQdnOdYXQ
- ref  Root Cause Analysis of some Faults in Design Patterns • Ralph E. Johnson • GOTO 2014 : https://www.youtube.com/watch?v=ImUcJctyUQ0
- ref  JDD2015 - Twenty-one Years of Design Patterns (Ralph Johnson) : https://www.youtube.com/watch?v=BfHJo_aYj3g
- ref  14 - Ralph Johnson - 22 Years Of Design Patterns: https://www.youtube.com/watch?v=Lf0uHSBTKEw
--- 

- The original book divided the 23 patterns into Creational, Structural, and Behavioral categories simply because a list of 23 was too long. Ralph Johnson admits that Structural and Behavioral are not good categories and suggests that dividing patterns into "core" and "peripheral" would be much more useful for teaching
- A major weakness of the original book is that it lacked time to properly explain how patterns combine and relate to one another: "we tried to avoid doing was saying this pattern is made up of this other pattern"
- Johnson highlights a strong need for the software community to document patterns specifically for functional programming

- core pattern according to him
    - composite
    - stargetgy
    - decorator
    - state
    - iterator
    - observer
    - value object
    - mediator
    - facade
    - proxy
    - command
    - template methods
    - adapter
    - null object
- peripheral
    - memento
    - chain of responsibility
    - bridge
    - visitor
    - type object
    - extension object
    - generation gap
- compound 
    - flyweight
    - interpreter
- creational

--- 

- A major example of misinterpretation is the Singleton pattern, which was intended to manage unavoidable global state, but developers wrongly interpreted it as a way to "bless" or encourage the use of global state
- Modern Dependency Injection frameworks have largely eliminated the need for many of the book's creational patterns
- Reactive extensions modernize the Observer pattern by treating subjects as continuous event streams, The authors entirely missed "Data Binding", a highly pluggable pattern where property values automatically update their observers without explicit change messages, despite it existing in some 1990s Smalltalk systems
- Early reviewers criticized the book for focusing entirely on GUI patterns while missing distributed and real-time programming patterns.
- For undo/redo functionality, the book's example using the Memento pattern is overly expensive; the Command pattern is far superior because it effectively saves the exact state needed to reverse computatation

# my notes from talk
- composite, chain of responsibility, interpreter related
- singleton and flyweight related in the sense that if we want a single object for combination of parameters we use flyweight, but if there's jst one combination, it's singleton. But, usually, for