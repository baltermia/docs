# Builder
**<big>Previous > [[Abstract Factory]]</big>**

---

A Builder is a class whose only responsibility is the creation of a single object. The Builder is mostly used when a types constructor has too many arguments and/or a client wants to build different kinds of an abstract type.

Imagine the following model:

```csharp
public class House
{
	public readonly Color WallColor;
	public readonly Color RoofColor;
	public readonly FloorType Floor;
	public readonly bool HasGarage;
	public readonly bool HasGarden;
	// etc...
}
```

You see the `House` class has a lot of properties and many more coming. Now image you have to create the constructor for this class:

```csharp
public House(Color wallColor, Color roofColor, FloorType floor, bool hasGarage, bool hasGarden/* etc */)
{
	// set properties
}
```

You can see that this is already very unreadable code for the author of the House class but there's also some client that has to use this constructor. This is frustrating both for the `House`s author and the client who wants to create such a object. 

The solution for this problem is a builder class. Instead of a single Constructor (or [[Factory Method]]) the Builder is an entire own class that has multiple methods to create the object. Note the constructor above is removed again.

```csharp
public class HouseBuilder
{
	public House Result 
	{
		get 
		{
			House result = house;
			house = new();
			return result;
		}
	}
	
	private House house = new();
	
	public HouseBuilder BuildWalls(Color color) { /* do work */ return this; }
	public HouseBuilder BuildRoof(Color color) { /* do work */ return this; }
	public HouseBuilder AddGarage() { /* do work */ return this; }
	// etc...
}
```

The Builder has a method for almost every property, each method is doing work for that specific property. When a client thinks his object is ready, he can get the House using the `Result` property. 

You can see that the implemented code of that getter set the `private` `house` field to a new object. This is because a Builder should be able to create multiple objects, without the need of a new builder. Therefore after getting the result, the builder is ready to construct the next `House` object.

---

Most (depending on the author) Builder also implement a concept called *Fluent API Design*. You can see this all over the industry (C# especially, LINQ is a prefect example of Fluent API design). Take another look at the Methods in the `HouseBuilder` above. You can see that each of the `Build` and `Add` methods returns itself (type `HouseBuilder`) after it has done its work. That design allows for calls like these:

```csharp
HouseBuilder builder = new();

House first = builder
				.BuildWalls(new Color("Red"))
				.BuildRoof(new Color("Black"))
				.AddGarage()
				./*etc...*/()
				.Result;


House second = builder
				.BuildWalls(new Color("Beige"))
				.BuildRoof(new Color("Brown"))
				.Result;
```

As you can see, calls on the builder can be chained together, because each method returns the builder object again, so you can call the next method to build. The second house can also be created instantly after getting the result of the first.

Without the fluent design, it would look like this:
```csharp
HouseBuilder builder = new();

builder.BuildWalls(new Color("Red"));
builder.BuildRoof(new Color("Black"));
builder.AddGarage();

House house = builder.Result();

```

---

Additionally to adding fluent design to your builder, you can also similar to the [[Abstract Factory]] add another abstraction layer, so that the client doesn't know the concrete `House` he's building (maybe one builder builds a modern and another one more of an old traditional house).

---

**<big>Next > [[Prototype]]</big>**