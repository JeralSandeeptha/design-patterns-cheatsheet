# Structural Patterns

---

## Adapter

- Bridges incompatible interfaces
- Make incompatible interfaces work together

```mermaid
classDiagram

    %% Interface
    class IAdapter {
        <<interface>>
        +play(audioType: string, fileName: string) void
    }

    %% Adapter
    class LegacyAudioPlayerAdapter {
        -legacyAudioPlayer: LegacyAudioPlayer
        +LegacyAudioPlayerAdapter(legacyAudioPlayer: LegacyAudioPlayer)
        +play(audioType: string, fileName: string) void
    }

    %% Legacy class
    class LegacyAudioPlayer {
        +playMp3(fileName: string) void
    }

    %% Modern class
    class MediaPlayer {
        +play(audioType: string, fileName: string) void
    }

    %% Client code
    class ClientCode {
        +main() void
    }

    %% Relationships
    IAdapter <|.. LegacyAudioPlayerAdapter : is-a
    LegacyAudioPlayerAdapter --> LegacyAudioPlayer : adapts
    ClientCode --> MediaPlayer : uses
    ClientCode --> LegacyAudioPlayerAdapter : uses
```

---
<br />

## Bridge

- Separate abstractions from implementation and scale them independently

```mermaid

```

---
<br />

## Composite

- Tree structure for individual/group objects
- Treat individual and group objects the same

```mermaid
classDiagram

%% ===== Interfaces =====
class SystemComponent {
  <<interface>>
  +getName(): string
  +getSize(): number
}

class CompositeSystemComponent {
  <<interface>>
  +addFile(component: SystemComponent): void
  +removeFile(component: SystemComponent): void
  +getAllComponents(): SystemComponent[]
}

%% ===== Concrete Classes =====
class File {
  -name: string
  -size: number
  +getName(): string
  +getSize(): number
}

class Folder {
  -systemComponents: SystemComponent[]
  +addFile(component: SystemComponent): void
  +removeFile(component: SystemComponent): void
  +getAllComponents(): SystemComponent[]
  +getName(): string
  +getSize(): number
}

%% ===== Relationships =====
SystemComponent <|.. File
CompositeSystemComponent <|.. Folder
CompositeSystemComponent --> SystemComponent : has-a

%% ===== Client =====
class Client {
  +main(): void
}

Client --> SystemComponent
```

---
<br />

## Decorator

- Add responsibilities dynamically
- Add features dynamically without changing class / Add add-ons / Customization

```mermaid
classDiagram

%% Interface
class ICake {
  <<interface>>
  +getCost() number
  +getDescription() string
}

%% Concrete Component
class Cake {
  +getCost() number
  +getDescription() string
}

ICake <|.. Cake

%% Decorator Base
class CakeDecorator {
  <<abstract>>
  -cake: ICake
  +getCost() number
  +getDescription() string
}

ICake <|.. CakeDecorator


%% Concrete Decorators
class IcingDecorator {
  +getCost() number
  +getDescription() string
}

class StrawberryDecorator {
  +getCost() number
  +getDescription() string
}

class ChocoDecorator {
  +getCost() number
  +getDescription() string
}

CakeDecorator <|-- IcingDecorator
CakeDecorator <|-- StrawberryDecorator
CakeDecorator <|-- ChocoDecorator

%% Client
class Client {
  +main()
}

Client --> ICake : uses
CakeDecorator o--> ICake : has-a
```

---
<br />

## Facade

- Simplified interface to complex subsystems

```mermaid
classDiagram

class Client {
    +main()
}

class CoffeeMakerFacade {
    -grinder: Grinder
    -brewer: Brewer
    -boilder: Biolder
    +makeCoffee()
}

class Grinder {
    +grind()
}

class Brewer {
    +brew()
}

class Biolder {
    +boil()
}

%% Relationships
Client --> CoffeeMakerFacade : uses
CoffeeMakerFacade --> Grinder : delegates
CoffeeMakerFacade --> Brewer : delegates
CoffeeMakerFacade --> Biolder : delegates
```

---
<br />

## Flyweight

- Reuses shared objects to save memory
- High performance with many objects / Share common data to save memory

```mermaid
classDiagram
    class ITreeFlyweight {
        <<interface>>
        +render(xPosition: string, yPosition: string)
    }

    class TreeFlyweight {
        -color: string
        -name: string
        -texture: string
        +render(xPosition: string, yPosition: string)
    }

    class TreeFactory {
        -trees: Map
        +getTreeType(name, color, texture) ITreeFlyweight
        +getCount() number
    }

    ITreeFlyweight <|.. TreeFlyweight
    TreeFactory --> TreeFlyweight : creates / reuses

    class Client {
        const pine = TreeFactory.getTreeType("Pine", "Dark Green", "Scaly");
        oak.render("10", "20");
        oak.render("15", "25");
    }

    Client --> TreeFactory : requests tree types
    Client --> ITreeFlyweight : uses shared objects
```

---
<br />

## Proxy

- Placeholder that controls access to real object
- Controls access to real object (Controls access to real object)

```mermaid
classDiagram

%% Interface
class IUserImpl {
    <<interface>>
    +createUser() void
}

%% Concrete Implementation
class UserImpl {
    +createUser() void
}

%% Proxy Class
class UserImplProxy {
    -userImpl: IUserImpl
    +UserImplProxy(userImpl: IUserImpl)
    +createUser() void
}

%% Client
class Client {
    const userService = new UserImplProxy(new UserImpl());
    userService.createUser();
}

%% Relationships
IUserImpl <|.. UserImpl
IUserImpl <|.. UserImplProxy
Client --> IUserImpl : uses
UserImplProxy --> UserImpl : has-a (design)
```

---
<br />
