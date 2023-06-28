# Proxy
**<big>Previous > [[Flyweight]]</big>**

---

A proxy is a placeholder for another object. It can control access to the original object, allowing you to perform something before/after the default request goes to the original object. It looks very similar to the [[Facade]] and [[Adapter]] patterns, but its use has more similarities to the [[Decorator]] pattern.

![[proxy.svg]]

The Proxy basically adds an additional layer over an object without the Client actually knowing (the client should obviously talk to the `IServiceInterface` - "Depend on abstractions, not on concretions").

---

**<big>Next > [[Behavioral]]</big>**