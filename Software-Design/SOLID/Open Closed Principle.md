# Open Closed Principle
**<big>Previous > [[Single Responsibility Principle]]</big>**

---

This means that

> A class should be **open** for extensions but closed for **modification**

Say some developer writes class `A`. Then some other developer wants to add some functionality to that class. But instead of modifying class `A`, he should create a new class `B` which extends class `A`.

```csharp
public class A
{
	public void SomeFunctionality()
	{
		// anything
	}
}

public class B : A
{
	public void AdditionalFunctionality()
	{
		// anything additional
	}
}
```

This would be a correct usage of the open/closed principle. Instead of adding the `AdditionalFunctionality` method to class `A` we extend it and add it to the child class `B`.

---

**<big>Next > [[Liskov's Substitution Principle]]</big>**