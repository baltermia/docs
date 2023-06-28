# Null Object
**<big>Previous > [[Memento]]</big>**

---

The Null Object pattern defines a placeholder object for a class, so that existing code doesn't throw errors (ArgumentNullExceptions) while also not executing any code.

What if you want to use a class that has a dependency which you don't really need? You could just not initialize any object and send `null` to the class as the dependency argument. But most likely the class will be throwing `ArgumentNullException`s all over you.

```csharp
public interface IDependency
{
	public void Execute();
}

public class Context
{
	private readonly IDependency _dependency;
	
	public Context(IDependency dependency)
	{
		_dependency = dependency ?? throw new ArgumentNullException(nameof(dependency));
	}
	
	public void DoSomeWork()
	{
		_dependency.Execute(); // this would also throw an exception if _dependency was null
	}
}
```

Using Context without an `IDependency` implementation.

```csharp
Context c = new(null); // throws and ArgumentNullException
```

We want to use `Context`, but we don't have any `IDependency` implementations and also don't really want the `Context` class to do anything associated with the Dependency (if we knew what kind of dependency it is and what its intent is).

So we define a Null Object.

```csharp
public class NullObjectDependency : IDependency
{
	public void Execute() { /* do absolutely nothing (yes, not a single thing, don't you date) */ }
}
```

And now we can use that implementation to create and use our `Context`.

```csharp
IDependency nullObject = new NullObjectDependency();

Context c = new (nullObject); // no exceptions

c.DoWork(); // no exceptions (and also doesn't really do anything)
```

---

**<big>Next > [[Observer]]</big>**