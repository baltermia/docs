# Singleton
**<big>Previous > [[Prototype]]</big>**

---

The Singleton pattern ensures that a class only has one single instance, while giving global access to that instance. 

A class following the singleton pattern has no public constructors, but has a static property `Instance` (name is not defined in the pattern, you can choose whatever you want) that returns you the single only instance of that class.

```csharp
public sealed class SomeClass	
{
	public readonly static SomeClass Instance;
	
	static SomeClass()
	{
		Instance = new ();
	}
	
	private SomeClass() { }
}
```

In C# you can use static constructors to constructor the `Instance` as soon as the class is called from somewhere. In this case the static constructor is not really necessary, but when working with more complex types that define something in the class' constructor you might want to have some additional code for construction.	 

It's important that you don't define any `public` constructors. You can also define your class as `sealed` so no derived types for the class can be created (which could provide a constructor).

---

**<big>Next > [[Structural]]</big>**