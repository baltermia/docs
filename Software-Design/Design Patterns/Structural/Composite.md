# Composite
**<big>Previous > [[Bridge]]</big>**

---

The composite pattern is used to design tree-pattern style structures. The easiest example for composition is the file/folder structure (e.g. in the windows explorer). Your first thought when designing such a structure might have two classes *Folder* and *File*, while a Folder has two list, one subfolders and another files.

![[composite.svg]]

You can already see two design flaws with the first approach. A Folder and File both have very similar properties, which already hints using a super type (interface preferably). The solution is then pretty simple. The Folder doesn't contain two individual lists, but one single list, holding a super type which it itself implements. That composition then creates the tree-like folder structure.

---

**<big>Next > [[Decorator]]</big>**