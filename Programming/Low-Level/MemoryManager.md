# MemoryManager
> **Read [[Stack & Heap#Heap]] before proceeding.**

The MemoryManager (referred to as *MM* from now on) manages all the memory stored on the *Heap*. 

It is a common misconception that the MM also manages the Stack, but that memory is freed already at compile time.

## How does it work?
### Memory Patterns
#### Fence / `fd` Pattern
We'll assume the the memory manager already allocated the memory (we talk about this later under [[#Allocation]]) 

Say we create a new `int` with the value 42:
```cpp
int* p_num = new int(42);
```

We know (read [[Memory#Read Memory]]) that an `int` consists of 4 bytes, so the memory would look like this:
```
2A 00 00 00
```

But that is not the only memory the MM changes. This `int` of 4 bytes actually takes up an entire 12 bytes (or even 16 or more because of [[Memory#Alignment]]):
```
fd fd fd fd 2A 00 00 00
fd fd fd fd -- -- -- -- // we will not consider these last 4 bytes
```

You can see that there are each 4 bytes before and after our `int` where each byte is `fd`. This is because of Microsoft's implementation of the C++ compiler **MSVC**:

> [Wikipedia](https://en.wikipedia.org/wiki/Microsoft%20Visual%20C++)

These bytes are used as a "fence". When you free memory with the `delete` keyword, the MM will use this fence to check if the memory has been corrupted. We can write a very simple program to test this:

```cpp
int* p_arr = new int[2]; // we create a new array with a length of 2

// memory:
fd fd fd fd 00 00 00 00
00 00 00 00 fd fd fd fd

// lets fill our array
p_arr[0] = 10;
p_arr[1] = 10;

// memory:
fd fd fd fd 0A 00 00 00
0A 00 00 00 fd fd fd fd

// now we assign memory that is out of the scope of the array
p_arr[3] = 2;

//memory:
fd fd fd fd 0A 00 00 00
0A 00 00 00 02 00 00 00 // we've overwritten the fence

```

Now our `int[]` has a fence at its start, but no fence at the end. When we instruct to delete the array, the MM will always check if the memory has been corrupted and use the fence to safely delete our memory. Since there is no `fd` fence at the end of our array, a runtime exception will be thrown by the MM stating that our heap memory suffered under some sort of memory corruption.

This pattern shows just how expensive memory on the heap is. An `int` of 4 bytes would only take up space of 4 (maybe 8) bytes on the stack. but on the heap it takes thrice the amount.

#### Clean / `cd`/`cc` Pattern
The **MSVC** MM also has another memory pattern. Let's say we have a simple struct containing a few variables
```cpp
struct Foo 
{
	int num;
	char initial;
	bool state;
};
```
and we create a new object of this struct on the heap:
```cpp
Foo* my_foo = new Foo();
```

We created a new Foo, but we never initialized any of it's members. The memory will look like this:
```cpp
fd fd fd fd cd cd cd cd
cd cd cd cd cd cd cd cd
fd fd fd fd -- -- -- --
```

We see our fence again at the start and end, but the rest of the memory is filled with `cd`.  This is again a memory pattern by the **MSVC** and it is used to show the user that this memory is clean / cleared up.

An important thing to note is though, that this will only be the case, if you run the program in the debug mode. The only use of `cd` is for the user to see cleared up space in the memory, in a release build this would only cost additional processing time which is not really needed.

Also, if you would create the same struct on the stack instead of the heap, the compiler will also (only during debug) clean up the memory with `cc`.  But this is again not something that considers the MM, as this memory is managed at compile time.

---

A list of some other memory patterns:

```
* 0xABABABAB : Used by Microsoft's HeapAlloc() to mark "no man's land" guard bytes after allocated heap memory
* 0xABADCAFE : A startup to this value to initialize all free memory to catch errant pointers
* 0xBAADF00D : Used by Microsoft's LocalAlloc(LMEM_FIXED) to mark uninitialised allocated heap memory
* 0xBADCAB1E : Error Code returned to the Microsoft eVC debugger when connection is severed to the debugger
* 0xBEEFCACE : Used by Microsoft .NET as a magic number in resource files
* 0xCCCCCCCC : Used by Microsoft's C++ debugging runtime library to mark uninitialised stack memory
* 0xCDCDCDCD : Used by Microsoft's C++ debugging runtime library to mark uninitialised heap memory
* 0xDDDDDDDD : Used by Microsoft's C++ debugging heap to mark freed heap memory
* 0xDEADDEAD : A Microsoft Windows STOP Error code used when the user manually initiates the crash.
* 0xFDFDFDFD : Used by Microsoft's C++ debugging heap to mark "no man's land" guard bytes before and after allocated heap memory
* 0xFEEEFEEE : Used by Microsoft's HeapFree() to mark freed heap memory
```

Also see [Wikipedia / Magic Number > Debug Values](https://en.wikipedia.org/wiki/Magic_number_(programming)#Debug_values) for more information.

---

Other compilers may store memory a bit different.

### Allocation
A big question is how the MM knows where it stores memory and where it has free space. We already know that heap memory is not a continuous collection as the stack. The heap needs to know where to store memory a bit differently because it needs to be able to hold dynamic (not known during compile-time) memory.

![[MM-Heap-Allocation.svg]]

The MM will have some starting point somewhere in the memory. You can imagine the outter box in the visualization above as the heap size. At the start you can see a field. This memory is stored at the beginning of each object stored in the heap. It holds the following information (they are just pseudo-variables, this is not the concrete implementation):
- **Length**: How much memory is used until the next field comes up
- **IsUsed**: This specifies whether the memory is currently in use or not
- **Address**: The next known memory address.

You can imagine these fields to be sort of a linked list (each one holds the address to the next field).

When you create a new variable on the heap, the MM will then set the length of the current field to the used memory, set used to be true and it will then create a new field at the end of the allocated memory. The address of this newly created field will be stored in the field before our created variable. Each variable in the heap will have such a field before it's memory.

But what happens when we delete the variable? The MM will then free this memory, of course, but it also has to edit the fields. The used property will be set to false, so it can be used again later. It also checks if any of the fields that it points to have memory that is not in use. If the stored address directly points to a field that is not in use, the MM will merge these two fields together (basically it sums the length and sets the address to the latter). 

When the MM needs to allocate new memory, it starts at the start of the heap, at the first field, and it searches for a field that is not in use an whose length is enough for the needed space.