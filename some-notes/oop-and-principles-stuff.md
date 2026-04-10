## Object Oriented Programming
- A programming paradigm based on the concept of objects which can contain data in form of fields(attributes or properties) and behavior in form of procedures(methods or member functions)
- four related concepts
	- **abstraction**: Abstraction is the idea of hiding unnecessary(to the consumer of this code) details. When you call a function like `sendEmail()`, you don't need to know how it connects to a mail server — you just use it. The internal complexity is abstracted away.
	- **Encapsulation**: bundle data and procedures that operate on them(classes and their methods basically) and allows to provide consistent external interface independent of internal workings. Refers to these related concepts
		- cohesion: keeping related data and procedures(methods) that operate on them together. Basically code locality, when you want to look at code doing a certain thing, you shouldn't have to search too wide.
		- decoupling: Reducing the hard dependencies between different components so they can evolve independently
		- data hiding: keep internal workings of object hidden. Consuming code only works with the public api provided by object(class).
	- **Inheritance**: Allows classes to inherit fields and behaviors from other classes(parent class - child class relation)
	- **Polymorphism**: it is the use of single symbol to represent multiple different things(e.g `draw()` function which does different things based on which object: circle,triangle etc. it is called on)

- Abstraction is a general concept. The hierarchies of `transistors<->logic gates<->circuits<->cpu<->operating system<->applications` and `network access layer <-> internet layer <-> transport layer <-> application layer` are examples of abstraction hierarchies where each layer hides details from the upper layer and upper layer only deals with what interface the lower layer provides. It could be said that encapsulation is just a type of abstraction.
- Polymorphism is a general concept, even inheritance is a kind of polymorphism. Common forms of polymorphism:
	- Ad hoc polymorphism: function overloading and operator overloading. Also implicit type conversion is also defined as form of polymorphism called coercion polymorphism
	- Parametric polymorphism: it allows a function or data-type to handle values uniformly without depending on their type(usually implemented with generics/templates)
	- Subtyping: allows a function to take not only an object of type `T` but also objects that belong to a type `S` which is subtype of `T`(basically inheritance)
		- Inheritance can be implemented with classes or prototypes(like in JavaScript). Prototype way only allow single inheritance(can only inherit from a single parent).
		- In some languages, parent need not to explicitly implement an interface. If it implement the interface methods then its subtype of that interface. Like in python(called duck typing, happens at runtime) and go(they call it structural typing, happens at compile time)
		- In Java, C++ like dynamic dispatch, virtual function calls are bound simply by looking through virtual table(or vtable) provided by the implicit first argument(`this`), so runtime types of other arguments if irrelevant. This is called single dispatch, but some languages support multiple dispatch too, in which the method to be called is decided by multiple arguments of the methods.
		- Predicate dispatch is generalization of multiple dispatch that allow the method to be selected at runtime based on arbitrary logical predicates
	- Polymorphism can also be distinguished by how it's implemented: statically(compile time) or dynamically(runtime), also known as static dispatch and dynamic dispatch  or early binding and late binding.
- **Composition**: Objects can contain other objects, representing  "has-a" relationship(like `Student` has a `EnrollmentInfo` object
- **Delegation**: this refers to evaluating a member(property or method) of an object(receiver) in the context of another object(sender). So for example this
	```kotlin
	interface Auditable : Serializable {
	var whoCreated: String
	var whoModified: String
	var whenCreated: LocalDateTime
	var whenModified: LocalDateTime
	var timeZone: ZoneId
	
	@PrePersist
	fun createAuditInfo()
	@PreUpdate
	fun updateAuditInfo()
	}
	
	class Audit : Auditable {
	//impl.
	}
	
	@Entity
	@NamedQuery(name = Book.FIND_ALL, query = "SELECT b FROM Book b")
	class Book : Manuscript(),
	Identifiable by Identifier() 
	Auditable by Audit(),
	Orderable by Ordered() {
	
	var author: String = ""
	
	@get:OneToMany(mappedBy = "book", cascade = arrayOf(CascadeType.PERSIST))
	var chapters: MutableList<Chapter> = ArrayList()
	}
	```
## OOP Problems
- **encapsulation**
	- OK when used in high level code to an extent(coarse-grained encapsulation—specifically at the module, package, service, or subsystem level, rather than the individual class or struct level)
	- At fine grained levels of code, headache of associating data types with behaviors(functions/methods)
	- for example: designing sender and receiver: Should sender have send method and receiver receive method or what, which class should call which, should there be a third class channel and so on headache
	- So we should not strive for encapsulation with classes(or structs) but do it when we see it appearing in the context
- **inheritance**:
	- breaking locality big time
		- when we want to see implementation of a method which is overridden multiple times, we have to look at multiple super-classes for complete implementation
		- a single field might be mutated in multiple sub-classes(mutation only allowed from methods of super class can be done but it's not enforced always)
		- big example of this: [AppCompatActivity](https://developer.android.com/reference/androidx/appcompat/app/AppCompatActivity) class of Android. Just to see what `onCreate` does, you'll have to see several classes in this inheritance hierarchy and try to make senses of it.
	- **so prefer object composition over class inhertitance**
- So in OOP, we have to think about inheritance hierarchy, composition graph(who calls who), call graph, so many things to think about. In procedural code, only call graph.


## solid principles
- Single responsibility principle: a module should be responsible for one and only one actor(group that can make a change in the module). Gather together things that change for the same reason, seperate those that change for different reason.
	- example: a module that compiled and prints and report. This module can change for two reason: content of report can change, format of report can change and different actors make those changes so these two should be seperate
- Open closed principle: module open to exntension but closed to modification(entity can allow its behaviour to be extended without modifying its source code)
	- so module should expose sane intefaces and those intefaces should be well-defined and stable
- Liskov Substitution Principle
	- particular definition of a subtyping relation, called strong behavioral subtyping
	- behavioural subtyping is undecidable in general so programmer will take care upto some degree
		- requirements
			- producers should be covariant(`producer of string` subtype of `producer of object`)
				```java
				interface ObjectSource {
					Object next();
				}
				//this should be ok
				ObjectSource src = () -> "Some string"; // Not allowed in Java though
				//becuase this is ok
				Object obj = src.next();
				//to let java know, you do something like this(TODO not sure) 
				interface ObjectSource<? extends Object> {
					Object next();
				}
				```
			- consumers should be contravariant(`consumer of number` subtype of `consumer of double`)
				```kotlin
				interface Comparable<in T> {
					operator fun compareTo(other: T): Int
				}
				//this is ok
				Comparable<Double> doubleComparable = { number: Number -> return /*impl*/;  }
				//because this is ok
				val numberComparable = { number: Number -> return /*impl*/;  }
				numberComparable(1.0)
				```
			- entity which is both producer and consumer, should invariant
			- new exceptions can't be thrown by methods of subtype, except those which are subtypes of exceptions thrown by supertype method
			- Preconditions cannot be strengthened in the subtype: e.g. if super class accepts int parameters both negative and positive, then so should subclass. subclass shouldn't throw exception on negative numbers
			- Postconditions cannot be weakened in the subtype: say superclass specify return int to be always positive then so should subclass; if supertype defines that the db connection used should be closed in the end of the method, then it should be closed in the subclass method too
			- Invariant cannot be weakened in the subtype: in subclassing, if a field of superclass is always positive then it should be always positive in subclass also, an array field is non empty in superclass then it should be non empty in subclass also
			- History constraint: Objects are regarded as being modifiable only through their methods (encapsulation). The history constraint prohibits direct modification of superclass fields by new methods introduces in subclass
			- some of these rules are enforced by compiler
- Interface Segregation Principle: clients shouldn’t be forced to depend on methods they do not us
- Dependency inversion principle

# Other principles
- composition over inheritance
