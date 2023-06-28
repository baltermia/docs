# Chain of Responsibility
**<big>Previous > [[Behavioral]]</big>**

---
<sub>Chain of Responsibility is abbreviated to **COR**</sub>

COR might seem like the [[Decorator]] pattern in the beginning. It passes a request along a chain of handlers. The difference to the Decorator is, that it not just simply adds additional functionality but it can also decide whether to stop the chain. It also has similarities to the [[Observer]] as handlers are used.

The structure looks a lot like the Decorator as a Component always has a member (see [[Strategy]]) that links to another. So COR is technically also a linked list.

Refactoring Guru has a great [exmaple](https://refactoring.guru/design-patterns/chain-of-responsibility) online. Check it out for a more detailed explanation.


---

**<big>Next > [[Command]]</big>**