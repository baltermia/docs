# Facade
**<big>Previous > [[Decorator]]</big>**

---

Facade is (IMHO) probably the easiest pattern to design and use. A Facade simply provides a simpler API for clients. It can use multiple different types, but doesn't add/modify any functionality. Facades are used when a client only needs some simple functionality of complex components and doesn't need to have access to all that responsibility.

![[facade.svg]]

The client doesn't need to use all three types but can simply use `MultiFunctionDevice` which provides all the functionality it needs.

When using a Facade, always make sure that you're not accidentally using [[Adapter|Adapters]] or [[Proxy|Proxies]].

---

**<big>Next > [[Flyweight]]</big>**