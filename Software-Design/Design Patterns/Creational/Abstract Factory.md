# Abstract Factory
**<big>Previous > [[Factory Method]]</big>**

---

Abstract Factories are very different from [[Factory Method|Factory Methods]]. Abstract Factories are entire classes which create a set of different coherent types and they also have an additional abstraction layer.

Image we have some abstract clothing models:

```csharp
public abstract class Hoodie { /* abstract members */ }
public abstract class TShirt { /* abstract members */ }
public abstract class Pants { /* abstract members */ }
```

And then two different implementations for each of them:

```csharp
public class NikeHoodie : Hoodie { /* abstract members */ }
public class NikeTShirt : TShirt { /* abstract members */ }
public class NikePants : Pants { /* abstract members */ }
```

```csharp
public class AdidasHoodie : Hoodie { /* abstract members */ }
public class AdidasTShirt : TShirt { /* abstract members */ }
public class AdidasPants : Pants { /* abstract members */ }
```

Let's also define a factory interface for clothing creation:

```csharp
public interface IClothingFactory
{
	public Hoodie CreateHoodie();
	public TShirt CreateTShirt();
	public Pants CreatePants();
}
```

Of course we need to create the concrete Factories as well:

```csharp
public class NikeClothingFactory : IClothingFactory
{
	public NikeHoodie CreateHoodie() { /* object creation */ }
	public NikeTShirt CreateTShirt() { /* object creation */ }
	public NikePants CreatePants() { /* object creation */ }
}
```

```csharp
public class AdidasClothingFactory : IClothingFactory
{
	public AdidasHoodie CreateHoodie() { /* object creation */ }
	public AdidasTShirt CreateTShirt() { /* object creation */ }
	public AdidasPants CreatePants() { /* object creation */ }
}
```

Clients code could look like this:

```csharp
public class Client	
{
	private readonly IClothingFactory _factory;
	
	public Client(IClothingFactory factory)
	{
		_factory = factory;
	}
	
	public void DoWork()
	{
		// create objects using _factory
	}
}
```

You see the Client never actually knows the exact type of clothing that is created. This is the abstraction layer mentioned in the beginning, therefore adding the *Abstract* in the patterns name.

---

![[abstract factor.svg]]

---

**<big>Next > [[Builder]]</big>**