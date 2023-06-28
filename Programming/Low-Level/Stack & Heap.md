# Stack
Normally newly created variables are saved on the stack

When leaving the current scope 
```cpp
{
	int i = 2;
} // here
```
the memory of those variables is not freed by the memory manager but by the compiler already at compile time. Thus, storing data on the stack is way less expensive than storing it on the heap.

Arrays with a fixed length (length known at compile-time) can also be saved on the stack
```cpp
int arr[2];
```
These arrays will also be freed when leaving the scope they're created in.
# Heap
As soon as the `new` keyword is used, memory is stored on the Heap by the MemoryManager. We need to know where the memory is stored on the Heap though, therefore the MemoryManager also creates a pointer that points to the created address on the heap.

```cpp
int* i = new int(2); // memory of int stored on heap, int ponter 'i' is stored on the stack
```

The big difference to the Stack is that this memory is not freed when leaving the scope.

We need to explicitly call `delete` resp. `delete[]` (when deleting an array) on the pointers to free the memory. This can happen at any time, but it should happen, otherwise the memory is lost and the [[MemoryManager]] can not allocate it anymore.

```cpp
delete i; // memory is freed
```

## Arrays

When creating an array with a dynamic length (length only known at runtime), that array *must* (see [[#Variable Length Array]] below) also be stored on the Heap:

```cpp
int size = 3; // length may come from say a function paramter
int* arr = new int[size]; // array with dynamic length can only be created with the 'new' keyword, therefore it is stored on the heap
```

When deleting the array, it is **very** important to use the right `delete` operator:
```cpp
delete[] arr;
```

If you accidentally only use `delete` instead of `delete[]`, only the first element in the array (the item the `int *` points to) will be deleted. Any other data in the array stays allocated, so the MemoryManager cannot use it

### Variable Length Array

VLA is a `C`-lang feature that was added after the creation of `C++`. C++ does not support this feature (for good).

What is this feature?

Variable Length Array means, that we can create an array with a dynamic length on the stack (we showed above that this is not possible in C++).

> In computer programming, a variable-length array (VLA), also called variable-sized or runtime-sized, is an array data structure whose length is determined at run time (instead of at compile time).
> [Wikipedia](https://en.wikipedia.org/wiki/Variable-length%20array)

# How is the Memory stored?
You might know the C++ datatype `std::stack`. You can imagine the stack to hold the data the same way, but don't mix the two together in your head, one is a datatype and the other is a form of storing memory.

When you create a scoped variable, the compiler will **push** that variable to the stack in the background. As soon as you leave the current scope, it is being **pop**ped out of the stack again (you know this concept from the stack datatype). The C++ compiler does not let any variables in the stack get accessible out of the scope.

Heap Memory is stored in a different way. Unlike the Stack, memory is not stored in a continuous memory-lane, but rather anywhere in the available heap where there is enough space for the needed memory. This data is not directly managed by the compilation, it's managed by the [[MemoryManager]].

---

A very interesting thing to note is that variables on the stack will always be stored with the same offset from the stack-start on each program run. This is because the memory is "managed" by the compiler at compile-time, not run-time. Heap memory does not behave the same, it's dynamic memory which is managed at run-time.

Therefore it is possible to access memory using the address of the stack and it's offset from outside the program. This allows for example for game-cheats to be run but it is also used in malware to inject malicious code.