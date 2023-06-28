# Pointers
Problem: `char const *` huh???, Why does `char* s = "MYSTRING";` not work

`"MYSTRING"` is a constant value that is created in the constant memory on the program startup.

So if you would like to have a pointer point to that constant value, you have to declare it with the `const` keyword:

```cpp
char* x // a char pointer is created, its value is not constant and it can therefore be changed

const char * y // a char pointer with a constant value is created, its value can never change
```

Consider reading [[Syntax#Usage of const in variable declaration]]

---

Pointers are mostly used to access objects stored on the Heap (see [[Stack & Heap]]).
These pointers are most of the time created on the Stack, they can also be stored on the Heap though in some cases.

### Dereferencing
As soon as we need to access the value a pointer points to or call any function of the underlying object, we need to use the dereference operators `*` and `->`.

```cpp
char * x = "test";

char val = *x; // value

x->member; // access member of the value directly (includes functions) (char in this example does not contain any member, see other types as example)
```

---

To create a pointer to a already existing value, we need to get the address to that value in the memory. We can use the `&` operator for this:

```cpp
char * pointer = &val;
```

---

A important use case of pointers are arrays (especially strings):
```cpp
const char * text = "MYSTRING"; // see at start of page why this char pointer needs to be constant
```

`"MYSTRING"` is a  `char[]`, `test` holds the address to the first value (the value is 'M' in this scenario)

It's the same for all kinds of arrays, also `int`s as an example:
```cpp
int* nums =  new int[2];
```

Important: These pointers point to a variable on the Heap, not the stack. See -> [[Stack & Heap]]

---

### Pointer shenanigans
A pointer that points to a pointer which points back to the pointer that points to itself (yes):
```cpp
int* x = new int(1);
int* ptr1 = x;
int **ptr2 = &ptr1;

*ptr1 = ptr2;

delete x;
```