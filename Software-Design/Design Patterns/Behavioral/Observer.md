# Observer
**<big>Previous > [[Null Object]]</big>**

---

The Observer pattern is another pattern that the .NET Framework already has implementations for (like the [[Prototype]] e.g.). There are two types of objects in the Observer pattern, a *Observable* and the *Observer*. The Observer waits for the Observable to call some method when an event it subscribed to occurred.

You can also image the Observer to be a Subscriber, while the Observable is a Publisher. The .NET interfaces (which you can implement in your classes to use the pattern) are `IObservable<T>` and `IObserver<T>`. There is also `Subject<T>`, which kind of combines the two other interfaces together into a class. Read the Microsoft Docs if you want to use them ;)

You really don't need the pattern in .NET if you know how to use events. You would only really need it when you want to do some more work when Observers un-/subscribe to an Observable object.

---

**<big>Next > [[State]]</big>**