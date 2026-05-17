# Creational Patterns

---

## Singleton

- Ensures only one instance of a class exists
- Centralized access and control of shared resources

```mermaid
classDiagram
class Singleton {
    -static instance : Singleton
    -Singleton()
    +static getInstance() : Singleton
    +someOperation()
}

class Client {
    DBSingleton.getInstance()
}

Singleton --> Singleton : creates / holds instance
Client --> Singleton : uses
```

---
<br />

## Factory

- Subclasses decide which class to instantiate
- Delegates object creation

```mermaid
classDiagram

%% Interface
class Shape {
  <<interface>>
  +area() number
  +perimeter() number
}

%% Concrete Classes
class Circle {
  -radius: number
  +area() number
  +perimeter() number
}

class Triangle {
  -base: number
  -height: number
  -sideA: number
  -sideB: number
  -sideC: number
  +area() number
  +perimeter() number
}

%% Factory
class ShapeFactory {
  +createShape(type: EShape) Shape
}

%% Client
class Client {
  const circle = ShapeFactory.createShape(EShape.Circle)
}

%% Relationships
Shape <|.. Circle
Shape <|.. Triangle


Client --> ShapeFactory : uses
Client --> EShape : selects type
Client --> Shape : interacts with
```

---
<br />

## Abstract Factory

- Creates families of related objects
- Ensures consistency

```mermaid

```

---
<br />

## Builder

- Constructs complex objects step by step
- Avoids constructor pollution

```mermaid

```

---
<br />

## Prototype

- Clone new objects out of existing objects instead of creating new objects
- Avoids costly object instantiation

```mermaid
classDiagram

class IPrototype {
  <<interface>>
  +clone() IPrototype
}

class User {
  +name: string
  +age: number
  +clone() IPrototype
}

class Admin {
  +id: number
  +email: string
  +password: string
  +clone() IPrototype
}

class Client {
  const user1 = new User("Alice", 30);
  const user2 = user1.clone();
}

IPrototype <|.. User : implements
User <|-- Admin : extends

Client --> IPrototype : uses
```
