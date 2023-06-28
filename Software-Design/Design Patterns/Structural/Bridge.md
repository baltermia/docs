# Bridge
**<big>Previous > [[Adapter]]</big>**

---

Bridge is a very common design pattern that you likely have used before without even knowing. 


The Bridge is very similar to the [[Strategy]], the only difference is that the Bridge is 2-Dimensional (compare class diagrams in page footers).

Let's make a very quick example. You have some sort of geometry drawing application in which you have different shapes. These shapes can have different colors. Say you have *circles* & *squares* and the colors *red* & *blue*. Now you might create a bunch of *RedCircle*, *RedSquare* *...* etc. classes, that all inherit a single *Shape* class. 

This approach brings a very big problem though, as soon as you extend your application. When you add more shapes and more colors, the amount of classes you have to create rises exponentially.


![[bridge.svg]]


You can already see the obvious solution for the problem. Instead of inheritance, you should be using composition (or aggregation in this case). Now you don't need to create a new class for every shape when adding a color, you can just simply create the new color class (inheriting from *Color*) and then have that class in the derived *Shape*.

Always **F**avor **c**omposition **o**ver **i**nheritance ([FCOI](https://en.wikipedia.org/wiki/Composition_over_inheritance)).

---

**<big>Next > [[Composite]]</big>**