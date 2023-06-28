# Marshalling
We've already seen that we can import C++ functions into our C# code using [[Dynamic Linked Libraries]]. .NET does a lot of background work to make this step as easy as possible. 

We will look at some crucial steps in detail, one of them is the Marshal.

> In computer science, marshalling or marshaling (US spelling) is the process of transforming the memory representation of an object into a data format suitable for storage or transmission. It is typically used when data must be moved between different parts of a computer program or from one program to another. 
>
> Marshalling can be somewhat similar or synonymous to serialization. Marshalling is describing an intent or process to transfer some object from a client to server, intent is to have the same object that is present in one running program, to be present in another running program, i.e. object on a client to be transferred to and present on the server. Serialization does not necessarily have this intent since it is only concerned about transforming data into a, for example, stream of bytes. One could say that marshalling might be done in some other way from serialization, but some form of serialization is usually used.It simplifies complex communications, because it uses composite objects in order to communicate instead of primitive objects. The inverse process of marshalling is called unmarshalling (or demarshalling, similar to deserialization). An unmarshalling interface takes a serialized object and transforms it into an internal data structure, which can be referred to as executable.
>
> The accurate definition of marshalling differs across programming languages such as Python, Java, and .NET, and in some contexts, is used interchangeably with serialization.
>
> [Wikipedia](https://en.wikipedia.org/wiki/Marshalling%20(computer%20science))

In our case marshalling is being used to convert C# objects (and datatypes) to C++/C datatypes and vice-versa. This can mean a lot of things. Take for instance some number datatype in C++ (no specific implementation) that takes up 4 bytes and is stored as BE (big endian, see [[Memory#Endianness]]). The same datatype is stored as LE (little endian) in C# and therefore the marshal basically converts these bytes so the same datatype can be used in both languages.

## String Problem
A very simple problem will occur if your try to call a C++ function with string which returns a string back to C#.

Take the following C++ code as a DLL example (read [[Dynamic Linked Libraries]] on how to create such files/functions):
```cpp
extern "C" __declspec(dllexport) size_t GetStringLength(char* text)
{
    return strlen(text);
}

extern "C" __declspec(dllexport) char* CallString(char* text)
{
    size_t length = GetStringLength(text) + 1;

    char* cpy = new char[length];

    std::copy(text, text + length, cpy);

    return cpy;
}
```

and the following C# code
```csharp
namespace Test;

public class Program 
{
	[DllImport("PATH_TO.DLL")]
	public static extern string CallString(string text);
	
	public static void Main() 
	{
		string str = CallString("Hello World!");	
	}	
}
```

You see the `CallString` just simply copies the string onto the heap and returns the address of it's first character.

But with this code you will very quickly run into a lot of problems. There will be a lot of stuff going on in the background and on the memory manager. To sum it up, the problem is, that there are two different memory managers running at the same time, and the C# marshal will try to access memory from the wrong memory manager.

To fix this problem, you can use the following code to create a copy of the array on the C# memory managers heap:
```cpp
char* cpy = static_cast<char*>(CoTaskMemAlloc(length));
```

This is obviously a very ugly way, there are definitely other ways to achieve the same. What this code does is, it allocates memory of the given size using `CoTaskMemAlloc` using the memory manager that loaded the DLL file. In this example it uses the C# memory manager from the program that is being run. The `static_cast` casts the created memory to the given type, static means that it checks whether this cast is possible already during compile time.