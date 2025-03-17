# Game Programming Patterns
#gamedev 

## Design Patterns

Design patterns are reusable solutions to common problems in software design. They provide a shared vocabulary and best practices for structuring code.

The **Game Programming Patterns** are a collection of patterns found in games that **make code cleaner, easier to understand, and faster**.

---

### Command
- **Definition**: Encapsulates a request as an object, allowing parameterization of clients with queues, requests, and operations.
  
- **C++ Example**:
  ```cpp
  class Command {
  public:
      virtual void execute() = 0;
  };

  class JumpCommand : public Command {
  public:
      void execute() override {
          // Perform jump action
      }
  };
  ```

- **Unreal Engine Example**: Input actions in Unreal can be mapped to command objects for handling player input.


### Flyweight
- **Definition**: Shares data among multiple objects to reduce memory usage.
  
- **C++ Example**:
  ```cpp
  class Flyweight {
      std::string sharedState;
  public:
      Flyweight(const std::string& state) : sharedState(state) {}
      void operation(const std::string& uniqueState) {
          // Use shared and unique state
      }
  };
  ```

- **Unreal Engine Example**: Shared material instances in Unreal reduce memory overhead for similar objects.



### Observer
- **Definition**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.
  
- **C++ Example**:
  ```cpp
  class Observer {
  public:
      virtual void update() = 0;
  };

  class Subject {
      std::vector<Observer*> observers;
  public:
      void attach(Observer* observer) {
          observers.push_back(observer);
      }
      void notify() {
          for (auto observer : observers) {
              observer->update();
          }
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `Delegates` and `Event Dispatchers` implement the Observer pattern.



### Prototype
- **Definition**: Creates new objects by copying an existing object, known as the prototype.
  
- **C++ Example**:
  ```cpp
  class Prototype {
  public:
      virtual Prototype* clone() const = 0;
  };

  class ConcretePrototype : public Prototype {
  public:
      Prototype* clone() const override {
          return new ConcretePrototype(*this);
      }
  };
  ```

- **Unreal Engine Example**: Blueprint actors can be cloned at runtime using the Prototype pattern.



### Singleton
- **Definition**: Ensures a class has only one instance and provides a global point of access to it.
  
- **C++ Example**:
  ```cpp
  class Singleton {
  private:
      static Singleton* instance;
      Singleton() {}
  public:
      static Singleton* getInstance() {
          if (!instance) {
              instance = new Singleton();
          }
          return instance;
      }
  };
  ```

- **Unreal Engine Example**: `GameInstance` in Unreal acts as a Singleton for global game state.



### State
- **Definition**: Allows an object to alter its behavior when its internal state changes.
  
- **C++ Example**:
  ```cpp
  class State {
  public:
      virtual void handle() = 0;
  };

  class ConcreteStateA : public State {
  public:
      void handle() override {
          // State-specific behavior
      }
  };
  ```

- **Unreal Engine Example**: State machines in Unreal (e.g., AI behavior trees) use the State pattern.

---

## Sequencing Patterns

### Double Buffer
- **Definition**: Uses two buffers to prevent rendering artifacts by separating rendering and update logic.
  
- **C++ Example**:
  ```cpp
  class DoubleBuffer {
      std::vector<int> buffers[2];
      int currentBuffer = 0;
  public:
      void swap() {
          currentBuffer = 1 - currentBuffer;
      }
      std::vector<int>& getBuffer() {
          return buffers[currentBuffer];
      }
  };
  ```

- **Unreal Engine Example**: Unreal's rendering system uses double buffering to avoid tearing.



### Game Loop
- **Definition**: Continuously processes user input, updates the game state, and renders the game.
  
- **C++ Example**:
  ```cpp
  while (gameIsRunning) {
      processInput();
      update();
      render();
  }
  ```

- **Unreal Engine Example**: Unreal's `Tick` function implements the Game Loop pattern.



### Update Method
- **Definition**: Updates game objects once per frame.
  
- **C++ Example**:
  
  ```cpp
  class GameObject {
  public:
      virtual void update() = 0;
  };
  ```

- **Unreal Engine Example**: Unreal's `AActor::Tick` method updates actors every frame.

---

## Behavioral Patterns

### Bytecode
- **Definition**: Uses a virtual machine to interpret bytecode instructions for game logic.
  
- **C++ Example**:
  ```cpp
  class VM {
      std::vector<uint8_t> bytecode;
  public:
      void interpret() {
          // Interpret bytecode
      }
  };
  ```

- **Unreal Engine Example**: Unreal's Blueprint VM interprets compiled Blueprint bytecode.



### Subclass Sandbox
- **Definition**: Provides a base class with common functionality, allowing subclasses to override specific behavior.
  
- **C++ Example**:
  ```cpp
  class Sandbox {
  public:
      void update() {
          // Common behavior
      }
  };

  class Player : public Sandbox {
  public:
      void update() override {
          // Player-specific behavior
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `APawn` class provides a sandbox for player-controlled actors.


### Type Object
- **Definition**: Creates a flexible type system by defining types as data.
  
- **C++ Example**:
  ```cpp
  class TypeObject {
  public:
      std::string name;
      int health;
  };

  class GameObject {
      TypeObject* type;
  public:
      GameObject(TypeObject* type) : type(type) {}
  };
  ```
- **Unreal Engine Example**: Unreal's `UClass` system allows runtime type information.

---

## Decoupling Patterns

### Component
- **Definition**: Encapsulates behavior into reusable components.
  
- **C++ Example**:
  ```cpp
  class Component {
  public:
      virtual void update() = 0;
  };

  class HealthComponent : public Component {
  public:
      void update() override {
          // Update health
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `UActorComponent` system enables modular actor design.


### Event Queue
- **Definition**: Stores and processes events asynchronously.
  
- **C++ Example**:
  ```cpp
  class EventQueue {
      std::queue<Event> events;
  public:
      void push(const Event& event) {
          events.push(event);
      }
      void process() {
          while (!events.empty()) {
              events.front().handle();
              events.pop();
          }
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `TQueue` and `Task Graph` systems implement event queues.

---

### Service Locator
- **Definition**: Provides a global point of access to services.
  
- **C++ Example**:
  ```cpp
  class ServiceLocator {
      static AudioService* service;
  public:
      static void provide(AudioService* service) {
          ServiceLocator::service = service;
      }
      static AudioService* getAudio() {
          return service;
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `GameInstance` acts as a service locator for global services.

---

## Optimization Patterns

### Data Locality
- **Definition**: Organizes data to maximize cache efficiency.
  
- **C++ Example**:
  ```cpp
  struct Transform {
      float x, y, z;
  };

  std::vector<Transform> transforms; // Contiguous memory for cache efficiency
  ```

- **Unreal Engine Example**: Unreal's `TArray` ensures contiguous memory for game objects.



### Dirty Flag
- **Definition**: Tracks whether data has changed to avoid redundant computations.
  
- **C++ Example**:
  ```cpp
  class GameObject {
      bool isDirty = true;
  public:
      void markDirty() { isDirty = true; }
      void update() {
          if (isDirty) {
              // Recompute
              isDirty = false;
          }
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `UPROPERTY` system uses dirty flags to track changes.



### Object Pool
- **Definition**: Reuses objects to avoid costly allocations.
  
- **C++ Example**:
  ```cpp
  class ObjectPool {
      std::vector<GameObject*> pool;
  public:
      GameObject* acquire() {
          if (pool.empty()) {
              return new GameObject();
          }
          auto obj = pool.back();
          pool.pop_back();
          return obj;
      }
      void release(GameObject* obj) {
          pool.push_back(obj);
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `Object Pooling` plugin reuses actors for performance.


### Spatial Partition
- **Definition**: Divides space into regions to optimize queries.
  
- **C++ Example**:
  ```cpp
  class SpatialPartition {
      std::vector<GameObject*> grid[10][10];
  public:
      void add(GameObject* obj, int x, int y) {
          grid[x][y].push_back(obj);
      }
  };
  ```

- **Unreal Engine Example**: Unreal's `NavMesh` system uses spatial partitioning for AI navigation.

