# Behavioural Patterns

---

## Chain of Responsibility

- Pass request along a chain until handled / Request goes throught handlers

```mermaid
classDiagram

class Client {
    const orderHandler = new ValidationHandler();
    orderHandler
        .setNext(new DiscountHandler())
        .setNext(new PaymentHandler())
        .setNext(new ShippingHandler());

    orderHandler.handle(order);
}

class Order {
  +id: number
  +amount: number
}

class Handler {
  <<interface>>
  +setNext(handler: Handler): Handler
  +handle(order: Order): void
}

class AbstractHandler {
  -nextHandler: Handler
  +setNext(handler: Handler): Handler
  +handle(order: Order): void
}

class ValidationHandler {
  +handle(order: Order): void
}

class DiscountHandler {
  +handle(order: Order): void
}

class PaymentHandler {
  +handle(order: Order): void
}

class ShippingHandler {
  +handle(order: Order): void
}

%% Relationships
Handler <|.. AbstractHandler
AbstractHandler <|-- ValidationHandler
AbstractHandler <|-- DiscountHandler
AbstractHandler <|-- PaymentHandler
AbstractHandler <|-- ShippingHandler

AbstractHandler --> Handler : nextHandler
Handler --> Order : processes

%% Client relationships
Client --> Order : creates
Client --> ValidationHandler : builds chain
Client --> Handler : invokes handle()
```

---
<br />

## Command

- Give commands as objects / Encapsulates requests as objects
- Parameterize and queue actions

```mermaid
classDiagram
    class Bulb {
        +turnOnBulb(): void
        +turnOffBulb(): void
    }

    class Command {
        <<interface>>
        +pressSwitch(): void
    }

    class TurnOnCommand {
        -bulb: Bulb
        +pressSwitch(): void
    }

    class TurnOffCommand {
        -bulb: Bulb
        +pressSwitch(): void
    }

    class RemoteInvoker {
        -command: Command
        +setCommand(command: Command): void
        +pressButton(): void
    }

    class Client {
        +main(): void
    }

    Command <|.. TurnOnCommand
    Command <|.. TurnOffCommand

    RemoteInvoker --> Command

    Client --> Bulb
    Client --> RemoteInvoker
```

---
<br />

## Interpreter

- Define grammar and interpret sentences

```mermaid

```

---
<br />

## Iterator

- Sequentially access elements without exposing structure

```mermaid
classDiagram

%% Interfaces
class Iterator~T~ {
  <<interface>>
  +next() T | null
  +hasNext() boolean
}

%% Concrete Iterator
class NameIterator {
  -index: number
  -names: string[]
  +NameIterator(names: string[])
  +next() IteratorResult~string~
  +hasNext() boolean
}

%% Collection
class NameCollection {
  -names: string[]
  +add(name: string) void
  +getIterator() Iterator~string~
}

%% Client
class Client {
  +main() void
}

%% Relationships
Iterator <|.. NameIterator
NameCollection --> NameIterator : creates
Client --> NameCollection : uses
```

---
<br />

## Mediator

- Central object controls communication

```mermaid

```

---
<br />

## Memento

- Save and restore state
- Captures and restores object state

```mermaid
classDiagram

class Client {
  +main()
}

class History {
  -mementos: EditorMemento[]
  +push(memento: EditorMemento): void
  +pop(): EditorMemento | undefined
}

class Editor {
  -content: string
  +type(words: string): void
  +getContent(): string
  +save(): EditorMemento
  +restore(memento: EditorMemento): void
}

class EditorMemento {
  -content: string
  +EditorMemento(content: string)
  +getContent(): string
}

Client ..> Editor : creates & uses
Client ..> History : creates & uses

History *-- "0..*" EditorMemento : stores

Editor ..> EditorMemento : creates
Editor ..> EditorMemento : restores from
```

---
<br />

## Observer

- One-to-many dependency (notify on change)
- Notify observers on state change

```mermaid
classDiagram
    direction LR

    class IObserver {
        <<interface>>
        +update(data: any): void
    }

    class ISubject {
        <<interface>>
        +subscribe(observer: any): void
        +unsubscribe(observer: any): void
        +notify(data: any): void
        +getState(): number
        +setState(state: number): void
    }

    class Observer {
        -name: string
        +Observer(name: string)
        +update(data: any): void
    }

    class Subject {
        -observers: any[]
        -state: number
        +Subject()
        +subscribe(observer: any): void
        +unsubscribe(observer: any): void
        +notify(data: any): void
        +getState(): number
        +setState(state: number): void
    }

    class Client {
        +create Subject
        +create Observer("Jeral")
        +create Observer("Silmi")
        +subscribe observers
        +setState(8)
    }

    IObserver <|.. Observer
    ISubject <|.. Subject

    Subject "1" o-- "*" Observer : maintains

    Client --> Subject : uses

    Subject ..> Observer : notify()
```

---
<br />

## State

- Object behaviour change based on internal state

```mermaid
classDiagram

    class ITool {
        <<interface>>
        +onMouseUp()
        +onMouseDown()
    }

    class BrushTool {
        +onMouseUp()
        +onMouseDown()
    }

    class SelectionTool {
        +onMouseUp()
        +onMouseDown()
    }

    class EraserTool {
        +onMouseUp()
        +onMouseDown()
    }

    class Canvas {
        -tool: ITool
        +setTool(tool: ITool)
        +onMouseDown()
        +onMouseUp()
    }

    ITool <|.. BrushTool
    ITool <|.. SelectionTool
    ITool <|.. EraserTool

    Canvas --> ITool : has-a

    %% Client Code Flow
    class Client {
        const canvas = new Canvas(new BrushTool())
        canvas.onMouseUp()
        canvas.onMouseDown()
    }

    Client --> Canvas
```

---
<br />

## Strategy

- Swap algorithms at runtime

```mermaid
classDiagram

    class PlatformStrategy {
        <<interface>>
        +login(): void
        +logout(): void
        +createTask(): void
        +getTasks(): void
        +getAllTasks(): void
        +deleteTask(): void
        +editTask(): void
    }

    class JiraStrategy {
        +login(): void
        +logout(): void
        +createTask(): void
        +getTasks(): void
        +getAllTasks(): void
        +deleteTask(): void
        +editTask(): void
    }

    class WrikeStrategy {
        +login(): void
        +logout(): void
        +createTask(): void
        +getTasks(): void
        +getAllTasks(): void
        +deleteTask(): void
        +editTask(): void
    }

    class TaskManagerStategy {
        -strategy: PlatformStrategy
        +TaskManagerStategy(strategy: PlatformStrategy)
        +setStrategy(strategy: PlatformStrategy): void
        +login(): void
        +logout(): void
        +createTask(): void
        +getTasks(): void
        +getAllTasks(): void
        +deleteTask(): void
        +editTask(): void
    }

    class ClientCode {
        +main(): void
    }

    PlatformStrategy <|.. JiraStrategy
    PlatformStrategy <|.. WrikeStrategy
```

---
<br />

## Template

- Define skeleton, subclasses fill steps / follow exact pattern

```mermaid
classDiagram

%% Abstract Template
class DatabaseTemplate {
    <<abstract>>
    +connect() void
    +loadDriver() void*
    +validateConfigurations() void*
    +establishDBConnection() void*
    +postProcess() void*
}

%% Concrete Implementations
class MySQLDatabase {
    +loadDriver() void
    +validateConfigurations() void
    +establishDBConnection() void
    +postProcess() void
}

class MongoDBDatabase {
    +loadDriver() void
    +validateConfigurations() void
    +establishDBConnection() void
    +postProcess() void
}

%% Client
class Client {
    const sqlDatabase = new MySQLDatabase();
    sqlDatabase.connect();
}

%% Relationships
DatabaseTemplate <|-- MySQLDatabase : extends
DatabaseTemplate <|-- MongoDBDatabase : extends

Client --> DatabaseTemplate : uses
```

---
<br />

## Visitor

- Add new operations to structure

```mermaid

```

---
<br />
