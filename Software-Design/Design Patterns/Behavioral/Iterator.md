# Iterator
**<big>Previous > [[Command]]</big>**

---

The Iterator pattern is another pattern that has direct interfaces defined in .NET. When coding in C#, you have very likely worked with `IEnumerator`. Almost every collection implements this interface as this is the basic interface for iteration. When you want to use a `foreach`, .NET searches for specific fields a class implements: `Current` and `MoveNext()`. The `IEnumerator` interfaces also defines another method, `Reset()`, but it is not necessary for `foreach` loops to work. 

If you have any class and implement the two methods mentioned above, your class can be used in `foreach` loops. You don't actually have to implement the interface, it's just a nice help for you.

In other languages, where iteration is not so directly implemented into the language, you will have to define interfaces like the `IEnumerator` yourself, the context is basically the same though.

If you've never coded a manual `foreach` loop before (basically the .NET implementation in the backend) using a `IEnumerator` object:

```csharp
IEnumerator collection = // some implementation
	
while (collection.MoveNext()) // sets 'Current' to the next element or returns false if default
{
	object current = collection.Current;
	
	// you can use current the exact same way as in a foreach loop
}
```

---

**<big>Next > [[Mediator]]</big>**