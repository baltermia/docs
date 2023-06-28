# Strategy
**<big>Previous > [[State]]</big>**

---

Using the Strategy design pattern, a object uses a object that contracts and interface, but it doesn't know the concrete implementation. Unlike the [[State]], when using Strategy the object itself doesn't know what types of objects it uses and when these underlying objects change.

![[strategy.svg]]

You can compare the diagram to the Bridges diagram to compare the differences.

The Clients code will look similar to this:

```csharp
IStrategy str = new ConcreteStrategy();

Context c = new();

c.Strategy = str;

// then use the c object and execute methods
```

---

**<big>Next > [[Template Method]]</big>**