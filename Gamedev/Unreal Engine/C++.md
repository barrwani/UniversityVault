# C++
#gamedev #cs

## C++ and BP

- There is a thing as too much C++.
- Do not ever hardcode an asset path into C++
	- These often lead to broken references and packaging errors. 
	- Total ban on `ConstructorHelpers` and similar functionality.   
- Configuration is also better done using the engine’s functionality instead of piles of `#define`'s or `constexpr` values in headers.

- It’s very common to build a C++ base class and a BP subclass together that form a single logical unit, especially with actor classes. 
- C++ is great at core architecture, performance, and being source controlled
- BP is great at asset references, visuals, ease of programming for non-technical people, and fast iteration.

## Build files

- Build files are in `Build.cs` and `Target.cs`
- `Target.cs` governs building entire project
- `Build.cs` govern an individual module (.dll or static lib)
- An `Example` module lives in an Example folder under Source with an `Example.Build.cs` file in it
	- Exported classes marked with `EXAMPLE_API`
- All `.cs` files get compiled together, so you can define an intermediate base class or extension methods to apply common build settings.


## Type System

- `UCLASS` and `USTRUCT`
- `UCLASS` always passed by pointer, heap only
- `USTRUCT` semantically passed by value, even if reference
	- BP will object slice
- Macros (`UFUNCTION, UENUM, DECLARE_DYNAMIC, etc..`) not `#define`'d, parsed by UHT at runtime


- It is automatically enforced to `#include` the `.generated.h` as the last item and use `GENERATED_BODY()` within these types to inject extra members that the engine needs
- `UFUNCTION` can only pass `UOBJECT` by pointer or reference-by-pointer, struct pointers are disallowed. 
- Templates are whitelisted to built-in containers such as `TArray,TMap`,etc..
  
- `UFUNCTION`s must have a unique parameter list
	- Overloads need to have a different name
	- Epic naming convention is enforced for classes participating in this type system (`USomething`, `FSomething`, `ASomething`, etc..)
	- These names are incompatible with namespaces
	- Common workaround is to use namespaces for raw types and a short (3-6 character) prefix based on project or company to avoid name collisions with the engine and third-party plugins

### `UObject`

- `UObject`s live exclusively on the heap with a garbage collector taking care of them
	- Usually passed around by pointers or references to pointers
	- Create them with `NewObject<T>` or `CreateDefaultSubobject<T>`
- Most important subclass is `AActor`
	- Has a complex lifecycle beyond garbage collection and reachability analysis
	- Comes with its own create function `UWorld::SpawnActor`
- Garbage collector only sees members marked with `UPROPERTY`
	- Anything not reachable through them is considered unreachable and may be deleted
	- `USTRUCT` can be held by value as a `UPROPERTY` and in turn hold `UObject* UPROPERTY`s which would be considered reachable
	- Circular references are fine


To check validity of raw `UObject*` use `IsValid()` or `GetValid()`:
```cpp
if (auto* Ptr = GetValid(Something->GetSomeUObject()))
{
    // Ptr may be used here
}
```

`IsValid()` returns bool, `GetValid()` is a shortcut for `IsValid(Input) ? Input : nullptr` (if valid return input otherwise return `nullptr`)


There are smart pointers that let you reference `UObject`s from a regular C++ object that cannot have `UPROPERTY`s. These all have `Object` in their names such as `TStrongObjectPtr<T>`, `TWeakObjectPtr<T>`. 

`TObjectPtr` is not a smart pointer, it is equivalent to a `T*` raw pointer. It is used for `UPROPERTY`s only (required to write `UPROPERTY`), function parameters and return values are still `UObject*`.


Classes with only `Ptr` are reinventions of STL counterparts (unique ptr, shared ptr, etc..) and work with common C++ objects. `Ref`s are the same but disallow being nullptr

 Object and non-object pointers may not be mixed in either direction, `TSharedPtr<UObject>` and `TWeakObjectPtr<FVector>` are both invalid.


Casting is done using `Cast<T>` for `UObjects`, with `CastChecked<T>` behaving like a static cast.





