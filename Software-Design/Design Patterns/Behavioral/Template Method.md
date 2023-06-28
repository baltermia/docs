# Template Method
**<big>Previous > [[Strategy]]</big>**

---

The Template Method pattern is a very interesting pattern. The pattern defines the skeleton of some sort of function in a superclass, while the subclasses have to provide specific implementations.

What this means exactly is, that an abstract class defines some `virtual` or `abstract` methods (the so called Template Methods), that it itself (the superclass) uses in a method defined only in the superclass. Therefore, the class defines the skeleton using the virtual methods, while combining them together in a method available to the client.

```csharp
public abstract class DatabaseReceiver
{
	public bool GetData(string username, string password, string query, out string data)
	{
		// do something in beginning
		
		if (!Login(username, password)) 
		{
			data = null;
			return false;
		}
		
		// login success, do something else
		
		data = ExecuteQuery(query);
		
		// data received, maybe do some additional work
		
		return data != null;
	}
	
	protected abstract bool Login(string username, string password);	// <--- Template Method
	
	protected abstract string ExecuteQuery(string query);				// <--- Template Method
}
```

We defined the superclass above. To use it though, we also need a subclass, which `override`s the two `protected` methods.

```csharp
public class MySqlReceiver : DatabaseReceiver
{
	protected override bool Login(string username, string password)
	{
		// do some work, perhaps use a library and return a bool
	}
	
	protected override string ExecuteQuery(string query)
	{
		// again, do whatever
	}
}
```

You can use the Template Method pattern when you know some steps of an algorithm, but the concrete code of those steps might have different implementations.

---

**<big>Next > [[Visitor]]</big>**