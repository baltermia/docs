# Adapter
**<big>Previous > [[Structural]]</big>**

---

The Adapter pattern is used when some component can't use another component without some extra code in between (e.g. conversion, translation etc.). 

You can imagine a real-life example of the use of an adapter with a power-plug. You're on vacation in the US, but only brought your European plugs. You need an adapter to plug your European plugs into the sockets in the US.

In software, there's also a pretty straight-forward example.

Imagine a component that has a dependency on some XML data-storer:

```csharp
public interface IXmlDataStorer
{
	public string ReadXml();
}

public class Client 
{
	private readonly IXmlDataStorer _storer;
	
	public Client(IXmlDataStorer storer)
	{
		_storer = storer;
	}
}
```

The problem is, you currently only have a data-storer that uses JSON.
```csharp
public class JsonDataStorer
{
	public string ReadJson();
}
```

You can easily provide the `Client` class its needed `IXmlDataStorer` dependency by defining an adapter:
```csharp
public class JsonToXmlDataStorerAdapter : IXmlDataStorer
{
	private readonly JsonDataStorer _storer;
	
	public JsonToXmlDataStorerAdapter(JsonDataStorer storer)
	{
		_storer = storer;
	}
	
	public string ReadXml() =>
		ConvertJsonToXml(_storer.ReadJson());
		
	private string ConvertJsonToXml(string json)
	{
		// return convertet xml string
	}
}
```

You can provide the `Client` its `IXmlDataStorer` dependency using the created adapter.

---

![[adapter.svg]]

---

**<big>Next > [[Bridge]]</big>**