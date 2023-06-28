# State
**<big>Previous > [[Observer]]</big>**

---

Let's talk about the State pattern. You will see many similarities with the [[Strategy]] and [[Bridge]] pattern, the differences will be explained at the end. The State design pattern lets an object change its behavior internally. It can appear as if the objects itself changed its class. 

What this actually means is, that the class *has* (composition/aggregation) a state in itself (so a reference to another object) which can change during runtime and will change the behavior of this class. UML diagrams look almost identical to Strategies or Bridges, the key differences should be noted though.
- The class itself or the client know the different States that the object is in (basically no real abstraction)
- It is only 1-Dimensional, not like the Bridge
- The State does not have the same intent as the Bridge, which is more similar to the Strategy

For graphical illustration to understand the differences even better, I suggest you visit [Refactoring Guru - State](https://refactoring.guru/design-patterns/state).

---

**<big>Next > [[Strategy]]</big>**