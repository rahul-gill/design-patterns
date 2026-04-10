Chapter 3. Creational Patterns

Creational design patterns abstract the instantiation process. They help
make a system independent of how its objects are created, composed, and
represented. A class creational pattern uses inheritance to vary the
class that’s instantiated, whereas an object creational pattern will
delegate instantiation to another object.

Creational patterns become important as systems evolve to depend more on
object composition than class inheritance. As that happens, emphasis
shifts away from hard-coding a fixed set of behaviors toward defining a
smaller set of fundamental behaviors that can be composed into any
number of more complex ones. Thus creating objects with particular
behaviors requires more than simply instantiating a class.

There are two recurring themes in these patterns. First, they all
encapsulate knowledge about which concrete classes the system uses.
Second, they hide how instances of these classes are created and put
together. All the system at large knows about the objects is their
interfaces as defined by abstract classes. Consequently, the creational
patterns give you a lot of flexibility in what gets created, who creates
it, how it gets created, and when. They let you configure a system with
“product” objects that vary widely in structure and functionality.
Configuration can be static (that is, specified at compile-time) or
dynamic (at run-time).

Sometimes creational patterns are competitors. For example, there are
cases when either Prototype ( 117 ) or Abstract Factory ( 87 ) could be
used profitably. At other times they are complementary: Builder ( 97 )
can use one of the other patterns to implement which components get
built. Prototype ( 117 ) can use Singleton ( 127 ) in its
implementation.

Because the creational patterns are closely related, we’ll study all
five of them together to highlight their similarities and differences.
We’ll also use a common example—building a maze for a computer game—to
illustrate their implementations. The maze and the game will vary
slightly from pattern to pattern. Sometimes the game will be simply to
find your way out of a maze; in that case the player will probably only
have a local view of the maze. Sometimes mazes contain problems to solve
and dangers to overcome, and these games may provide a map of the part
of the maze that has been explored.

We’ll ignore many details of what can be in a maze and whether a maze
game has a single or multiple players. Instead, we’ll just focus on how
mazes get created. We define a maze as a set of rooms. A room knows its
neighbors; possible neighbors are another room, a wall, or a door to
another room.

The classes Room , Door , and Wall define the components of the maze
used in all our examples. We define only the parts of these classes that
are important for creating a maze. We’ll ignore players, operations for
displaying and wandering around in a maze, and other important
functionality that isn’t relevant to building the maze.

The following diagram shows the relationships between these classes:

[]

Each room has four sides. We use an enumeration Direction in C++
implementations to specify the north, south, east, and west sides of a
room:

  enum Direction {North, South, East, West};

The Smalltalk implementations use corresponding symbols to represent
these directions.

The class MapSite is the common abstract class for all the components of
a maze. To simplify the example, MapSite defines only one operation,
Enter . Its meaning depends on what you’re entering. If you enter a
room, then your location changes. If you try to enter a door, then one
of two things happen: If the door is open, you go into the next room. If
the door is closed, then you hurt your nose.

  []

Enter provides a simple basis for more sophisticated game operations.
For example, if you are in a room and say “Go East,” the game can simply
determine which MapSite is immediately to the east and then call Enter
on it. The subclass-specific Enter operation will figure out whether
your location changed or your nose got hurt. In a real game, Enter could
take the player object that’s moving about as an argument.

Room is the concrete subclass of MapSite that defines the key
relationships between components in the maze. It maintains references to
other MapSite objects and stores a room number. The number will identify
rooms in the maze.

  []

The following classes represent the wall or door that occurs on each
side of a room.

  []

We need to know about more than just the parts of a maze. We’ll also
define a Maze class to represent a collection of rooms. Maze can also
find a particular room given a room number using its RoomNo operation.

  []

RoomNo could do a look-up using a linear search, a hash table, or even a
simple array. But we won’t worry about such details here. Instead, we’ll
focus on how to specify the components of a maze object.

Another class we define is MazeGame , which creates the maze. One
straightforward way to create a maze is with a series of operations that
add components to a maze and then interconnect them. For example, the
following member function will create a maze consisting of two rooms
with a door between them:

  []

This function is pretty complicated, considering that all it does is
create a maze with two rooms. There are obvious ways to make it simpler.
For example, the Room constructor could initialize the sides with walls
ahead of time. But that just moves the code somewhere else. The real
problem with this member function isn’t its size but its inflexibility.
It hard-codes the maze layout. Changing the layout means changing this
member function, either by overriding it—which means reimplementing the
whole thing—or by changing parts of it—which is error-prone and doesn’t
promote reuse.

The creational patterns show how to make this design more flexible, not
necessarily smaller. In particular, they will make it easy to change the
classes that define the components of a maze.

Suppose you wanted to reuse an existing maze layout for a new game
containing (of all things) enchanted mazes. The enchanted maze game has
new kinds of components, like DoorNeedingSpell , a door that can be
locked and opened subsequently only with a spell; and EnchantedRoom , a
room that can have unconventional items in it, like magic keys or
spells. How can you change CreateMaze easily so that it creates mazes
with these new classes of objects?

In this case, the biggest barrier to change lies in hard-coding the
classes that get instantiated. The creational patterns provide different
ways to remove explicit references to concrete classes from code that
needs to instantiate them:

  • If CreateMaze calls virtual functions instead of constructor calls
  to create the rooms, walls, and doors it requires, then you can change
  the classes that get instantiated by making a subclass of MazeGame and
  redefining those virtual functions. This approach is an example of the
  Factory Method ( 107 ) pattern.

  • If CreateMaze is passed an object as a parameter to use to create
  rooms, walls, and doors, then you can change the classes of rooms,
  walls, and doors by passing a different parameter. This is an example
  of the Abstract Factory ( 87 ) pattern.

  • If CreateMaze is passed an object that can create a new maze in its
  entirety using operations for adding rooms, doors, and walls to the
  maze it builds, then you can use inheritance to change parts of the
  maze or the way the maze is built. This is an example of the Builder (
  97 ) pattern.

  • If CreateMaze is parameterized by various prototypical room, door,
  and wall objects, which it then copies and adds to the maze, then you
  can change the maze’s composition by replacing these prototypical
  objects with different ones. This is an example of the Prototype ( 117
  ) pattern.

The remaining creational pattern, Singleton ( 127 ), can ensure there’s
only one maze per game and that all game objects have ready access to
it—without resorting to global variables or functions. Singleton also
makes it easy to extend or replace the maze without touching existing
code.