# Decorator
**<big>Previous > [[Composite]]</big>**

---

The decorator pattern defines a design which might look similar to the [[Composite]] pattern but has an entirely different meaning/usage. The pattern lets you define additional behaviors to already existing types. A decorator often composites a object of a type itself inherits. 

Let's make a quick example. You have a program that currently has a `SqlStorer` class (implementing a very basic `IDataStorer` interface) which saves your data on a SQL server. Now you would also like to store the data in a JSON and XML file (for whatever reason). Decorators can come very handy for this kind of problem. Instead of calling three classes all by yourself, decorators call the methods in their own overriding methods of the previous class and then execute some additional code. You can also think of decorators kind of as additions.

![[decorator.svg]]

You see the similarities to the composite. 

So we have our main `SqlStorer` and using the `StorerDecorator` when can then also store the data in JSON and XML. We can simply use the decorators like this:

```csharp
IDataStorer sql = new SqlStorer();
IDataStorer json = new JsonStorerDecorator(sql);
IDataStorer xml = new XmlStorerDecorator(json);

Client c = new (xml);
```

As always, we used abstractions so the client doesn't know what concrete implementation it's given :)

---

Let's also quickly look at one of the decorators concrete implementation to draw a better picture on how the pattern works in actual code (not all classes are included, see the diagram for reference).

```csharp
public interface IDataStorer 
{
	public void StoreData();
}

public abstract class StorerDecorator : IDataStorer
{
	protected readonly IDataStorer _storer;
	
	public StorerDecorator(IDataStorer storer)
	{
		_storer = storer;
	}
	
	public virtual void StoreData(); // might also have '=> _storer.StoreData();' as default impl.
}

public class JsonStorerDecorator : StorerDecorator
{
	public override void StoreData()
	{
		// do something before the previous storer
		
		_storer.StoreData();
		
		// do something after the previous storer
	}
}
```

You override the `StoreData` method and in that method you class the same method from the `_storer` (basically previous decorator) object. You can then execute code before/after that method call. In this case, you might wait until the previous storer saved the data, then save the data as JSON.

---

**<big>Next > [[Facade]]</big>**