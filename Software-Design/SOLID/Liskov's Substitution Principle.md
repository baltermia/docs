# Liskov's Substitution Principle
**<big>Previous > [[Open Closed Principle]]</big>**

---

The name itself doesn't really tell much. That's because the principle is named after the woman that defined it.

This principle says, that any code that takes a class object should function without the need to know whether it's a derived class object or base. The method should have the same functionality no matter the inheritance.

We take two classes `Square` and `Rectangle` as an example. In geometry a square **is** a rectangle, not vice-versa. Take the following code into consideration:

```csharp
public class Rectangle
{
	protected int m_height;
	protected int m_width;
	
	public virtual int Height 
	{ 
		get => m_height; 
		set => m_height = value;
	}
	
	public virtual int Width 
	{ 
		get => m_width; 
		set => m_width = value;
	}
}

public class Square : Rectangle
{	
	public override int Width 
	{ 
		get => throw new Exception("Square can't have height and width, they are equal.");
		set => throw new Exception("Square can't have height and width, they are equal.");
	}
}
```

This directly violates the Liskov's substitution, because a method that takes a `Rectangle` object, expects it to always act like one, so both the `Height` and `Width` properties should be implemented to work. But in the `Square` implementation, because a square only has one size, we throw an exception when trying to access `Width`.

So following the Liskov's substitution, a Rectangle would be a Square, the exact opposite of what it is in geometry. This is because the rectangle inherits the properties of the square, in geometry that is not really comparable.

---

**<big>Next > [[Interface Segregation Principle]]</big>**