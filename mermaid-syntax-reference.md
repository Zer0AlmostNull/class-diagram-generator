# Mermaid Class Diagram Syntax Reference

## Basic Structure

```mermaid
classDiagram
    class ClassName {
        +Type property
        -Type privateProp
        #Type protectedProp
        ~Type internalProp
        +method() ReturnType
        -privateMethod() void
    }
```

## Visibility Modifiers

| Symbol | Meaning |
|--------|---------|
| `+` | Public |
| `-` | Private |
| `#` | Protected |
| `~` | Package/Internal |

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Class names | **PascalCase**, singular | `UserService`, `OrderItem` |
| Class IDs | alphanumeric + `_`/`-` only | `UserService`, `order-item` |
| Attributes | **camelCase** with type prefix | `+String email`, `-int count` |
| Methods | **camelCase** with parens | `+login()`, `+findById(id String)` |
| Relationship labels | **lowercase verb phrases** | `: places`, `: manages` |
| Annotations | `<<PascalCase>>` | `<<Interface>>`, `<<Abstract>>` |
| Generics | **tilde** not angle brackets | `+list~String~ items` (NOT `<String>`) |

## Data Types

```mermaid
classDiagram
    class Example {
        +String name
        +int count
        +bool isActive
        +float price
        +List~String~ items
        +Map~String, int~ scores
        +Optional~User~ user
    }
```

## Methods with Parameters

```mermaid
classDiagram
    class Service {
        +fetchData(String url, int timeout) Response
        +processItems(List~Item~ items) void
        +validate(User user, bool strict) bool
    }
```

## Relationships

### All 8 Relationship Types

| Syntax | UML Type | Use When |
|--------|----------|----------|
| `<|--` | **Inheritance** (Generalization) | `Car` is-a `Vehicle` |
| `*--` | **Composition** | `Order` owns `OrderItem` (part dies with whole) |
| `o--` | **Aggregation** | `Team` has `Player` (player exists independently) |
| `-->` | **Association** | `User` uses `Service` (structural, long-lived) |
| `--` | **Link (solid)** | Generic relationship when type is ambiguous |
| `..>` | **Dependency** | `Controller` depends-on `Repository` (transient/weak) |
| `..|>` | **Realization** | `ServiceImpl` implements `<<interface>> Service` |
| `..` | **Link (dashed)** | Generic weak relationship |

### Inheritance (Generalization)
```mermaid
classDiagram
    Animal <|-- Dog : inherits
    Animal <|-- Cat : inherits
```

### Composition (Strong ownership)
```mermaid
classDiagram
    Car *-- Engine : has
    Car *-- Wheel : has
```

### Aggregation (Weak ownership)
```mermaid
classDiagram
    Department o-- Employee : has
```

### Association (Relationship)
```mermaid
classDiagram
    Teacher --> Student : teaches
```

### Dependency (Uses)
```mermaid
classDiagram
    Order ..> Payment : uses
    Order ..> Shipping : uses
```

### Realization (Interface implementation)
```mermaid
classDiagram
    class Serializable {
        <<interface>>
        +serialize() String
    }
    User implements Serializable
```

## Cardinality

```mermaid
classDiagram
    Customer "1" --> "0..*" Order : places
    Order "1" --> "1..*" OrderItem : contains
    OrderItem "0..*" --> "1" Product : references
```

Cardinality labels: `"1"`, `"*"`, `"0..1"`, `"1..*"`, `"0..*"`

## Annotations

```mermaid
classDiagram
    class Service {
        <<interface>>
        +process() void
    }
    class AbstractBase {
        <<abstract>>
        +templateMethod() void
    }
    class Enum {
        <<enumeration>>
        VALUE_A
        VALUE_B
    }
```

## Namespaces (Modules)

```mermaid
classDiagram
    namespace ENGINE {
        class CoreEngine {
            +start() void
            +stop() void
        }
        class EventBus {
            +emit(String event) void
            +on(String event, Handler h) void
        }
    }
    namespace UI {
        class MainView {
            +render() void
        }
        class Dialog {
            +open() void
            +close() void
        }
    }
    namespace SERVICES {
        class ApiService {
            +fetch() Response
            +post(data) void
        }
    }
    MainView --> CoreEngine : uses
    Dialog --> ApiService : calls
```

Hierarchical namespaces with dot notation: `namespace A.B.C`

## Notes

```mermaid
classDiagram
    class User {
        +String id
    }
    note "This is a user"
    note for User "Represents system user"
    note right of User "User entity"
```

## Diagram Size Management

### When to Split
- **>20-30 classes**: Mermaid's dagre layout struggles (overlapping, poor spacing)
- Split by bounded context, module, or domain area

### Split Strategy
```
docs/diagrams/
├── domain-users.mmd
├── domain-orders.mmd
├── domain-payments.mmd
└── infrastructure.mmd
```

### Hiding Members
When full detail isn't needed, hide empty members box in config.

## Complete Example

```mermaid
classDiagram
    namespace ENGINE {
        class GameEngine {
            <<abstract>>
            +float fps
            -bool running
            +start() void
            +stop() void
            +update(float dt) void
            #onTick() void
        }
        class PhysicsEngine {
            +gravity: float
            +simulate(List~Body~ bodies) void
        }
        class RenderEngine {
            +resolution: String
            +render(Scene scene) void
        }
    }
    namespace UI {
        class HUD {
            +update() void
            +showMessage(String msg) void
        }
        class Menu {
            +open() void
            +close() void
        }
    }
    namespace DATA {
        class Player {
            +String name
            +int score
            +save() void
        }
        class Scoreboard {
            +List~Player~ players
            +addScore(Player p, int score) void
            +getTop(int n) List~Player~
        }
    }
    GameEngine <|-- PhysicsEngine : extends
    GameEngine <|-- RenderEngine : extends
    PhysicsEngine --> Player : simulates
    RenderEngine --> HUD : updates
    HUD --> Scoreboard : displays
    Menu --> Player : manages
```

## Syntax Rules

1. **Semicolons required** - Every property/method line ends with `;`
2. **Relationships after classes** - Define all classes first, then relationships
3. **No duplicate names** - Each class name must be unique
4. **Escape special chars** - Use `\"` for quotes in notes
5. **Generic syntax** - Use `~` for generics: `List~String~`
6. **Namespace blocks** - Use `namespace Name { }` with proper braces
7. **Quote special characters** - Always quote labels with special characters: `["Label with (parens)"]`
8. **Avoid reserved words** - Don't use `end`, `default`, `style` as node IDs (or quote them)
