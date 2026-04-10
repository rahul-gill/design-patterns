# Creational Patterns: Factory & Prototype

> **Scope**: GoF official patterns (Factory Method, Abstract Factory, Prototype) + community patterns (Simple Factory, Factory Kit). Modern Java 21 throughout. Inheritance-based GoF formulations are noted where they differ from how you'd write this today.

---

## The Core Problem All These Patterns Solve

Client code that does `new ConcreteType()` is coupled to that concrete type. It can't be substituted, mocked, extended, or varied without changing the client. All factory-related patterns are different answers to the same question: **who decides what gets instantiated, and how do we keep that decision separate from the code that uses the result?**

---

## 1. Factory Method *(GoF official)*

**Also known as**: Virtual Constructor

### Intent

Define a method for creating an object, but let something else decide which concrete class to instantiate. The creator controls *when* and *how* the object is used; the factory method controls *what* gets created.

### GoF Formulation (Inheritance-Based)

```
Application (abstract)
  newDocument()            ← template method — calls createDocument(), then uses it
  createDocument()         ← the factory method, abstract, subclass must override

MyApplication extends Application
  createDocument()         ← returns new MyDocument()
```

`newDocument()` is a Template Method that calls `createDocument()` internally. The framework knows when to create a document; the subclass knows which kind. This is the GoF intent: **deferred instantiation via subclass override**.

### Modern Java 21 (Composition-Based)

Nobody forces framework consumers to subclass a class just to swap a creation strategy. The same intent — caller controls what gets created, framework controls when it's used — is expressed through injected `Supplier<T>`:

```java
public class DocumentProcessor {
    private final Supplier<Document> documentFactory;

    public DocumentProcessor(Supplier<Document> documentFactory) {
        this.documentFactory = documentFactory;
    }

    public void newDocument() {
        var doc = documentFactory.get();   // deferred creation, same intent as GoF
        doc.open();
        doc.render();
    }
}

// No subclassing. Caller decides what gets created.
var processor = new DocumentProcessor(ReportDocument::new);
var processor2 = new DocumentProcessor(SpreadsheetDocument::new);
```

The factory method is now a `Supplier<T>` field rather than an abstract method. **Intent preserved, inheritance eliminated.**

### Spring's `@Bean` — The Live Example

`FactoryBean<T>` (a Spring interface you implement to produce beans) is largely a historical artifact from the XML-config era. The modern equivalent is a `@Bean` method in a `@Configuration` class — which *is* a factory method in the GoF sense:

```java
@Configuration
public class DataSourceConfig {

    @Bean
    public DataSource dataSource() {           // ← factory method
        var ds = new HikariDataSource();
        ds.setJdbcUrl("jdbc:postgresql://...");
        ds.setMaxPoolSize(20);
        return ds;
    }

    @Bean
    public UserRepository userRepository(DataSource ds) {  // ← another factory method
        return new JdbcUserRepository(ds);
    }
}
```

The `@Configuration` class is the Creator. Spring calls the `@Bean` methods — it controls *when* objects are created. You control *what* gets created and *how* it's configured. This is exactly the GoF pattern, expressed via annotations instead of abstract class inheritance. If Spring were designed today from scratch, `FactoryBean` as a separate interface would almost certainly not exist.

### Parameterized Factory Methods

A factory method that accepts a type identifier and can create multiple product types from one method. GoF's motivating use case was deserializing objects from disk by reading a class identifier. The modern equivalent:

```java
public class WorkItemFactory {
    private static final Map<Class<?>, Supplier<?>> registry = Map.of(
        Document.class,     Document::new,
        Spreadsheet.class,  Spreadsheet::new,
        Presentation.class, Presentation::new
    );

    @SuppressWarnings("unchecked")
    public static <T extends WorkItem> T create(Class<T> type) {
        var supplier = (Supplier<T>) registry.get(type);
        if (supplier == null) throw new IllegalArgumentException("Unknown type: " + type);
        return supplier.get();
    }
}

WorkItem doc  = WorkItemFactory.create(Document.class);
WorkItem sheet = WorkItemFactory.create(Spreadsheet.class);
```

**The inherent trade-off**: the return type must be a common supertype. If the caller needs `Document`-specific methods, they must cast — losing static type safety. This is the fundamental tension in parameterized factories: flexibility vs. type safety. Generics (`<T>`) mitigate it but don't eliminate it.

**Note on serialization**: when persisting and reconstructing typed objects, prefer storing them in a standard format (JSON with a discriminator field) rather than an application-specific binary format. Use a `type` field to drive the factory:

```java
// JSON: { "type": "document", "title": "Q4 Report", ... }
// Registry maps type strings to deserializers/constructors — same factory pattern
```

### When to Use

- A framework or library knows *when* to create an object but not *which kind* — that's the caller's concern.
- You want to support multiple implementations of a product without the client knowing which is active.
- Spring `@Bean` methods are the everyday manifestation in Java applications.

---

## 2. Abstract Factory *(GoF official)*

**Also known as**: Kit

### Intent

An interface that declares a *family* of creation methods. Swap the factory object to swap the entire product family atomically. The client never sees concrete types.

### Structure

```java
// The abstract factory — declares the family
public interface UIFactory {
    Button     createButton();
    ScrollBar  createScrollBar();
    Dialog     createDialog();
}

// Concrete factories — one per product family
public class MacOSUIFactory implements UIFactory {
    public Button    createButton()    { return new MacOSButton(); }
    public ScrollBar createScrollBar() { return new MacOSScrollBar(); }
    public Dialog    createDialog()    { return new MacOSDialog(); }
}

public class WindowsUIFactory implements UIFactory {
    public Button    createButton()    { return new WindowsButton(); }
    public ScrollBar createScrollBar() { return new WindowsScrollBar(); }
    public Dialog    createDialog()    { return new WindowsDialog(); }
}

// Client — knows only UIFactory and the product interfaces
public class Application {
    private final UIFactory factory;

    public Application(UIFactory factory) {    // injected, not hard-coded
        this.factory = factory;
    }

    public void render() {
        var btn  = factory.createButton();     // never sees MacOSButton or WindowsButton
        var sb   = factory.createScrollBar();
        btn.draw(); sb.draw();
    }
}
```

The critical property: swapping `MacOSUIFactory` for `WindowsUIFactory` at construction time changes every product the application creates. Products within a family are guaranteed to be compatible — you can never accidentally mix a `MacOSButton` with a `WindowsScrollBar`.

### Abstract Factory Methods Are Implemented Using Factory Methods Internally

GoF's phrasing on this: each creation method on a concrete factory (`createButton()`, `createScrollBar()`) *is* a factory method. Abstract Factory is the architectural pattern (swap product families); Factory Method is the mechanism inside each concrete factory that actually produces each product. One is the blueprint, the other fills each slot.

### The Key Liability: Adding New Product Types Is Hard

Suppose you need to add `createTooltip()` to `UIFactory`. This forces changes to `UIFactory`, every concrete factory, and every class that depends on `UIFactory`. **Extending the family of product types requires touching all implementations.** Extending the number of families (adding a `LinuxUIFactory`) is trivial. This asymmetry is a core property of the pattern, not a bug to work around.

### Modern Usage

Database abstraction (swap a `MySQLRepositoryFactory` for a `PostgresRepositoryFactory` via DI), testing (swap a real infrastructure factory for a mock factory), multi-tenant systems where each tenant's configuration produces a different set of services. Concrete factories are typically singletons — one per family, constructed once at application startup and injected everywhere.

### When to Use

- Products *must* be used together as a family and mixing them would be wrong.
- You need to swap an entire product family at runtime or via configuration, not individual products.
- Client code should be fully isolated from concrete implementation classes.

---

## 3. Simple Factory *(Community Pattern — Not GoF)*

Not a GoF pattern. GoF mentions parameterized creation as an implementation technique but does not elevate it to a named pattern.

### The Classic Form and Its OCP Problem

```java
// OCP violation — adding a new animal requires modifying this class
public class AnimalFactory {
    public static Animal create(String type) {
        return switch (type) {
            case "dog"  -> new Dog();
            case "cat"  -> new Cat();
            default     -> throw new IllegalArgumentException("Unknown: " + type);
        };
    }
}
```

Every new type requires modifying `AnimalFactory`. This is an Open/Closed Principle violation: the class is not closed for modification.

### Mitigation 1: Enum-Encoded Dispatch (Fixed Type Space)

When the set of types is intentionally closed, move the dispatch table into the enum itself:

```java
@RequiredArgsConstructor
@Getter
public enum CoinType {
    COPPER(CopperCoin::new),
    GOLD(GoldCoin::new);

    private final Supplier<Coin> constructor;
}

public class CoinFactory {
    public static Coin getCoin(CoinType type) {
        return type.getConstructor().get();   // factory knows nothing about concrete types
    }
}
```

`CoinFactory` itself never changes. Adding a new coin type modifies the enum — which is the intentional, bounded extension point. Combine with a `sealed interface Coin` and the compiler enforces exhaustiveness. **OCP is respected at the factory level; the enum is deliberately closed.**

### Mitigation 2: Registry-Based Factory (Open Type Space)

When the set of types is open for extension — such as a plugin system — a registry is OCP-compliant:

```java
public class AnimalFactory {
    private static final Map<String, Supplier<Animal>> registry = new ConcurrentHashMap<>();

    public static void register(String type, Supplier<Animal> supplier) {
        registry.put(type, supplier);
    }

    public static Animal create(String type) {
        var supplier = registry.get(type);
        if (supplier == null) throw new IllegalArgumentException("Unknown: " + type);
        return supplier.get();
    }
}

// Each module registers itself — AnimalFactory never changes
AnimalFactory.register("dog",  Dog::new);
AnimalFactory.register("cat",  Cat::new);
AnimalFactory.register("bird", Bird::new);   // new type, zero changes to factory
```

This is structurally equivalent to Factory Kit (see next section) in a static form.

### When to Use

- Simple, low-stakes creation logic where full Factory Method or Abstract Factory is overkill.
- Paired with sealed types or enums when the type space is intentionally closed.
- Registry form when the type space needs to be open for extension without modifying the factory.

---

## 4. Factory Kit *(Community Pattern — Not GoF)*

A modern, composition-first pattern that separates **factory assembly** from **factory usage**. No inheritance, no subclassing, factory configured at call-site.

```java
public interface Weapon {}
public enum WeaponType { SWORD, AXE, BOW, SPEAR }

public interface Builder {
    void add(WeaponType name, Supplier<Weapon> supplier);
}

public interface WeaponFactory {
    Weapon create(WeaponType name);

    static WeaponFactory factory(Consumer<Builder> consumer) {
        var map = new HashMap<WeaponType, Supplier<Weapon>>();
        consumer.accept(map::put);                        // builder IS the map's put method
        return name -> map.get(name).get();               // factory IS a lambda over the map
    }
}
```

Usage:

```java
var factory = WeaponFactory.factory(builder -> {
    builder.add(WeaponType.SWORD, Sword::new);
    builder.add(WeaponType.AXE,   Axe::new);
    builder.add(WeaponType.SPEAR, Spear::new);
    builder.add(WeaponType.BOW,   Bow::new);
});

var weapons = List.of(
    factory.create(WeaponType.AXE),
    factory.create(WeaponType.SPEAR),
    factory.create(WeaponType.SWORD)
);
```

### What Makes This Pattern Distinct

The factory's *author* provides only the assembly mechanism (`static factory(Consumer<Builder>)`). The factory's *consumer* decides what goes into it at call-site. No class must be subclassed; no registry mutated globally.

The `WeaponFactory` interface itself is implemented by a lambda — `name -> map.get(name).get()`. The factory object is closed (no modification possible) once assembled. The `Consumer<Builder>` callback is the extension point.

**Structural breakdown:**

- `Consumer<Builder>` — configuration DSL; the caller's opportunity to register mappings
- `Builder` (the `add` interface) — the registration contract
- `HashMap<WeaponType, Supplier<Weapon>>` — the prototype registry (see Prototype section)
- The returned lambda — the factory object itself, holding the map in its closure

### OCP Status

`WeaponFactory` never changes. New weapon types are added by passing a different `Consumer<Builder>`. The factory mechanism is closed for modification, open for extension through configuration.

### Relationship to GoF Patterns

Achieves the *intent* of Abstract Factory (client isolated from concrete classes, product sets are swappable) without any inheritance. The "product family" is defined at runtime by the `Consumer`, not at compile time by subclasses. Uses a Supplier-as-prototype registry (see Prototype, Implementation section below).

---

## 5. Prototype *(GoF official)*

### Intent

Specify the kinds of objects to create using a prototypical instance, and create new objects by copying that prototype. Useful when the class to instantiate is specified at runtime, or when subclassing an object factory would produce a class hierarchy that parallels the product hierarchy.

### The `clone()` Problem in Java

GoF's original formulation relies on `clone()`. In Java, `Object.clone()` is broken: it requires implementing `Cloneable` (a marker interface with no methods), performs a shallow copy by default, and is not enforced by the type system. **Do not use `Object.clone()` in modern Java.** The idiomatic replacements are:

```java
// Option 1: copy constructor
public class Circle implements Shape {
    private final double radius;
    private final Color color;

    public Circle(Circle other) {         // copy constructor — explicit, readable
        this.radius = other.radius;
        this.color  = other.color;
    }
}

// Option 2: factory method returning a copy
public Shape copy() {
    return new Circle(this);
}
```

### Classic Prototype Registry

```java
public class ShapeRegistry {
    private final Map<String, Shape> prototypes = new HashMap<>();

    public void register(String key, Shape prototype) {
        prototypes.put(key, prototype);
    }

    public Shape create(String key) {
        var proto = prototypes.get(key);
        if (proto == null) throw new IllegalArgumentException("Unknown shape: " + key);
        return proto.copy();              // copy, not clone()
    }
}

// Registration
var registry = new ShapeRegistry();
registry.register("unit-circle",    new Circle(1.0, Color.RED));
registry.register("large-square",   new Square(10.0, Color.BLUE));

// Creation — no constructor call, no knowledge of concrete types
var c1 = registry.create("unit-circle");
var c2 = registry.create("unit-circle");   // independent copy
```

### Prototype vs. Constructor — When Does It Matter?

Prototype makes sense when:

- Object construction is expensive (deep graph, external resources) and a configured instance can be cheaply copied.
- The exact class is unknown at compile time — you have an instance but not the type.
- Many related configurations exist and you want to stamp out variants from a pre-configured base.

In most plain Java code, `new SomeClass(params)` is cheap and Prototype adds complexity without benefit. The pattern earns its place in plugin systems, configuration-driven object graphs, and frameworks.

### Supplier-as-Prototype (Modern Idiom)

In modern Java, constructor references as `Supplier<T>` act as "prototypes" in the spirit of the pattern — they encode the recipe for creating an instance without the caller knowing the concrete type:

```java
// The Supplier<T> IS the prototype — the factory can "clone" it by calling .get()
Map<String, Supplier<Shape>> registry = Map.of(
    "circle", Circle::new,
    "square", Square::new
);

Shape s = registry.get("circle").get();   // same intent as proto.copy(), no clone() needed
```

This is exactly the registry in the Factory Kit pattern. Factory Kit is, structurally, a Prototype-based Abstract Factory expressed through Java's functional interfaces.

### Prototype as an Alternative to Subclassing in Abstract Factory

GoF explicitly notes: when many product families differ only slightly, creating a new concrete factory subclass per family causes a class explosion. The Prototype-based alternative stores configured instances (or `Supplier`s) rather than subclassing:

```java
// Subclassing approach — N families × M products = N concrete factory classes
class MacOSUIFactory  implements UIFactory { ... }
class WindowsUIFactory implements UIFactory { ... }
class LinuxUIFactory   implements UIFactory { ... }   // and so on

// Prototype approach — one factory class, configured with instances
public class PrototypeUIFactory implements UIFactory {
    private final Map<String, Supplier<UIComponent>> prototypes;

    public PrototypeUIFactory(Map<String, Supplier<UIComponent>> prototypes) {
        this.prototypes = prototypes;
    }

    public UIComponent create(String key) {
        return prototypes.get(key).get();
    }
}

// Configuring different families without new classes
var macFactory = new PrototypeUIFactory(Map.of(
    "button",    MacOSButton::new,
    "scrollbar", MacOSScrollBar::new
));

var winFactory = new PrototypeUIFactory(Map.of(
    "button",    WindowsButton::new,
    "scrollbar", WindowsScrollBar::new
));
```

No new subclass for each family. Extension is configuration, not inheritance.

### Serialization Note

A related use of Prototype thinking is persisting and reconstructing object graphs. The standard practice is to serialize to a well-known format (JSON, with a `type` discriminator field) rather than an application-specific binary format. On deserialization, the `type` field drives a factory registry:

```json
{ "type": "circle", "radius": 5.0, "color": "RED" }
```

```java
// Deserializer uses a registry — the same Prototype/factory pattern
Map<String, Function<JsonNode, Shape>> deserializers = Map.of(
    "circle", node -> new Circle(node.get("radius").asDouble(), ...),
    "square", node -> new Square(node.get("side").asDouble(), ...)
);
```

This keeps serialized data readable, portable, and decoupled from Java class names or binary layout — which change across refactors.

---

## Pattern Comparison

| Pattern | GoF | Mechanism | Extends via | Creates |
|---|---|---|---|---|
| Factory Method | ✓ | Inheritance → Composition | Subclass / injected `Supplier` | One product type |
| Abstract Factory | ✓ | Object composition | New factory implementation | A family of products |
| Prototype | ✓ | Copy / `Supplier` | New registered prototype | Copy of a known instance |
| Simple Factory | ✗ | Static dispatch | Enum / registry | One product type |
| Factory Kit | ✗ | Lambda + registry | `Consumer<Builder>` config | One product per key |

---

## Mental Models

**Factory Method** — *"I know when to create something. You tell me what to create."*
The framework holds the when; the caller provides the what via override or injection.

**Abstract Factory** — *"Give me a factory object and I'll use its entire product family."*
Swap the factory, swap every product. Families stay coherent; no mixing.

**Prototype** — *"I have a configured instance. Give me a copy."*
Creation by duplication rather than construction. The instance encodes the configuration.

**Simple Factory** — *"Call this method and pass a type token."*
Pragmatic, low-overhead. OCP-fragile unless paired with sealed types or a registry.

**Factory Kit** — *"You configure the factory; I'll use it."*
The most modern idiom. Composition-only, OCP-compliant, assembly separated from usage.

---

## Key Ideas to Carry Forward

**Composition over inheritance applies here directly.** GoF's Factory Method and Abstract Factory were designed for an inheritance-heavy world. The *intents* remain valid; the *inheritance mechanisms* are largely obsolete in application code. `Supplier<T>`, `Function<T,R>`, and `Consumer<T>` replace abstract method overrides for most purposes.

**The registry is the universal primitive.** Every factory pattern beyond Simple Factory converges on a `Map<Key, Supplier<T>>`. Factory Kit makes this explicit. Prototype uses it with instance copying. Parameterized factory methods use it with type tokens. Once you see this, the patterns feel like variations on one idea: **keep a table of creation recipes, look up by key, invoke.**

**Sealed types make Simple Factory respectable.** A `switch` over a `sealed interface` is exhaustive by compiler enforcement. The OCP violation is intentional — the type space is closed by design. This is not a workaround; it is the right tool when the set of variants is genuinely finite and controlled.

**Abstract Factory's hard trade-off is real.** Adding new product *types* to an existing Abstract Factory requires touching the interface and every implementation. This is not a fixable design flaw — it is the inherent cost of the pattern's guarantee that families are coherent. Know this before choosing the pattern.