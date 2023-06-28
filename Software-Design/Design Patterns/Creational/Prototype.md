# Prototype
**<big>Previous > [[Builder]]</big>**

---

Prototype is a very simple pattern adding an interface to clone objects. .NET has a `ICloneable<T>` interface, which completely covers the Prototype design pattern.

```csharp
public class Point : ICloneable<Point>
{
	public readonly double X;
	public readonly double Y;
	
	public Point Clone() =>
		new Point
		{
			X = this.X,
			Y = this.Y
		};
}
```

The class `Point` implements the `ICloneable<Point>` interface by adding the `Clone` method. You now implemented the Prototype pattern.

---

**<big>Next > [[Singleton]]</big>**