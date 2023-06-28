# Factory Method
**<big>Previous > [[Creational]]</big>**

---

**Factory Methods** are often used when simple constructors do not provide the needed functionality.
Constructors allow overloading, but you can never give a constructor a different name.
 
 Imagine the following problem: You have a class which holds coordinates *X* & *Y* like this:
 
 ```csharp
public class Point
{
	public double X { get; private set; }
	public double Y { get; private set; }
}
```

Then you want the client to be able to create such Points both with Polar (Rho & Theta) and Cartesian (X & Y) coordinates, so your create two constructors.

```csharp
public Point(double x, double y) 
{
	// set X & Y
}

public Point(double rho, double theta)
{
	// calculate and set X & Y
}
```

You can see the problem here. There are two constructors, both take two double values as parameters. Method/Constructor overloading by different parameter/variable-names is not possible in most languages. Therefore we need some kind of other solution for this issue. 

---

The example above shows just one of many use cases where the Factory Method can be very useful. Instead of having different constructors, we define static methods, which will create the wanted objects for us. Most of the time, the types constructors are also set to `private` or `protected`, so public access is not possible and object creation is only possible via Factory Methods.

```csharp
public class Point
{
	public double X { get; private set; }
	public double Y { get; private set; }
	
	private Point(double x, double y)
	{
		X = x;
		Y = y;
	}
	
	public static Point CreateFromPolar(double rho, double theta) =>
		new Point(/*provide calculated x & y coordinates*/);
	
	public static Point CreateFromCartesian(double x, double y) =>
		new Point(x, y);
}
```

Objects of type `Point` can no be created using the static Factory Methods `CreateFromPolar`/`Cartesian`. The method name defines how the provided data is used to create a new object.

--- 

Different from the [[Builder]] and [[Abstract Factory]], Factory Methods are almost always in the same class of it's creating type and they are static methods.

---

**<big>Next > [[Abstract Factory]]</big>**