# Memory
## Debugging
Before we start with any real theory, we should know how we can view the memory when debugging. This is pretty easy in Visual Studio, just head to `Debug > Windows > Memory`. You should be greeted with a window full of HEX numbers. 

For better understanding, it is good practice to adjust the column width in the view, you can do that on the top-right under `Columns`. You should adjust it depending on your [[#Alignment]], by default 8 or 16 should be fine for readability.

## Read Memory
Here is a simple table that shows you how you can understand this view:

| Address    | Hex Bytes               | Encoded Bytes   |
| ---------- | ----------------------- | --------------- |
| 0x00F0FC38 | 2A 00 00 00 cc cc cc cc | B . . . Ì Ì Ì Ì |

What do these columns mean exactly?
- **Address**: What is the exact address of the first byte in the package. Depending on the CPU you run this code, it's either 4 bytes (32-bit) or 8 bytes (64-bit) long
- **Hex Bytes**: This is the exact data that is being stored. It is stored as binary (0s and 1s) but displayed as hex for better readability.
- **Encoded Bytes**: This is a representation of each byte being encoded to a text-readable format (f.e. hex `42` is a literal B). The `.` is just a placeholder for a non-readable character.

The Address and Encoded Bytes are not that interesting. But lets take a better look at the actual bytes:
```
2A 00 00 00 cc cc cc cc
```

The first 4 bytes are all set, the last 4 bytes are cleaned up memory (see [[MemoryManager#Clean cd cc Pattern]]). We can't know what datatype this memory is in the C++ code, we can only speculate. It could be a integer for example. A normal signed (and unsigned) integer in C++ takes up exactly 4 bytes. So if we read the bytes (keeping the [[#Endianness]] in mind), the value would be `42`.

You might ask yourself why this column is only 8 bytes long, this is because of the [[#Alignment]]

## Alignment

Memory Alignment defines how data is structured in program memory. It basically defines how long a group of data is in the memory. This group is called a package:

### Package
A memory package is a memory sequence the size of the defined memory alignment. The processor will read one package at a time. 

If a variable is stored in a single package, it is said to be aligned. If the memory of a variable is stored in two ore more packages it is unaligned, the processor needs to execute more than one read instruction and it is therefore more expensive to read/write memory.

32-bit systems use 4 byte and 64-bit system 8-byte alignment, most of the time. One could always change the alignment for a program itself, the alignment is defined per program, not system. That's also a difference between 32- and 64-bit programs.

### Padding

Padding is used so that memory can still be aligned even if different datatypes require different amounts of bytes in memory.

Lets assume we have a memory alignment of 4 bytes (a package is 4 bytes long).

Members in a `struct` are stored sequentially, so depending on how you define such a `struct` the data will be store differently in the memory and it will take up a different amount of needed space (packages).

```cpp
struct Data 
{
	int num1;
	bool state1;
	int num2;
	bool state2;
	char chr1;
	int num3;
};
```

In the memory it will look like this (the needed data is specified by the first character in the datatype, so `int` is "i"):

```
ii ii ii ii bb 00 00 00
ii ii ii ii bb cc 00 00
ii ii ii ii 00 00 00 00
```

In this scenario, the structure takes a total of 24 bytes.

You can see that this structure contains a lot of `00` memory, this is memory that will never be used by the struct (or anything else in the program). This memory is called **padding**. It's used to align the memory, as it would otherwise be unaligned. In this case, the `bool` which is 1 byte is followed by a 4 byte `int`. There are only 3 bytes left, so the `int` is moved to the next package in memory to make it aligned.

We could, of course, store this data sequentially, but that would make it unaligned and more expensive to read:

```
ii ii ii ii bb ii ii ii
ii bb cc ii ii ii ii --
```

If we define the `Data` `struct` in a different order, the memory will take up less space. We define variables that take up more space first:

```cpp
struct Data
{
	int num1;
	int num2;
	int num3;
	bool state1;
	bool state2;
	char chr1;
};
```

The stored memory will look like this:

```
ii ii ii ii ii ii ii ii
ii ii ii ii bb bb cc 00
``` 

The structure now only takes up 16 bytes, instead of 24, so a total package of memory could be saved.

---

But what use does this alignment bring exactly?

The underlying processor doesn't read 1 byte at a time. It will read one package (8 bytes in our case above) at a time. If you were to read 1 single byte at a time, memory access would be even more expensive than it already is.

This is why padding is. It is generally cheaper to store a variable on a new package, where it can be read in a single read operation than storing it on two different packages. The lost memory mostly are only a few bytes and the processing will be significantly faster.

## Endianness
The endianness basically defines in what order bytes should be read in the memory. There are only 2 specific types of endianness, little and big endian which are defined below:

### Big Endian
Bytes should be read from most to least significant bit in memory, which basically means left to right. This is used f.e. for C++ ints.

### Little Endian
This is the exact opposite of the big endian. Bytes should be read from least to most significant bit in memory, which basically means right to left.

---

Lets take a very simple example to apply these defined endians on a 4 byte integer:

```
2A 00 00 00
```

Using [[#Big Endian]], we read from left to right. The last 3 bytes are all `00` (NULL), so we don't need to consider them. So it's a basic manner of translating `2A` to decimal, which is 42.

With the [[#Little Endian]] though, we read from right to left, which means that we need to consider the `00` NULLs. We get a huge value when translating this `2A 00 00 00`, its decimal equivalent is 704'643'072.
