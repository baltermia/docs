# Dynamic Linked Libraries
DLLs can hold code which can be dynamically added while a process is running.

## Create DLL
The simplest way to create a DLL is to create it from the Visual Studio templates. Create a new project and search for the `Dynamic-Link Library (DLL)`. You can add your code in `dllmain.cpp`. By default, there is one method:

```cpp
#include "pch.h"

BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  ul_reason_for_call,
                       LPVOID lpReserved
                     )
{
    switch (ul_reason_for_call)
    {
    case DLL_PROCESS_ATTACH:
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}
```

The `pch.h` include adds some precompiled header stuff (used if you use header files).

The `DllMain` function is called when the DLL is loaded and freed from a process and/or thread. The `ul_reason_for_call` will state from which of these it is called. You might want to setup something in your library when it is loaded/unloaded. 

---

To export a method from your DLL you need some special keywords:

```cpp
extern "C" __declspec(dllexport) int sum(int a, int b)
{
    return a + b;
}
```

First you need to add `extern "C"` which indicates that we export a C function (there's no overloading etc.). Then we need to add another keyword `__declspec(dllexport)` which declares the function to be a DLL-export (`__declspec` is a Microsoft keyword, it has no real meaning).

## Load DLL
Then in another C++ application we can import this DLL. Of course you could simply add a reference to the VS Project, but we will look at the manual way to do it:

```cpp
// add the following include
#include <windows.h>

// first define the method signature in this file
typedef int (*sum)(int a, int b);

int main() 
{
	// we then get the module handle for the DLL
	HMODULE handle = LoadLibrary(TEXT("PATH_TO_DLL.dll"));
	
	// check if the DLL could be imported
	if (handle != null) 
	{
		// GetProcAddress gets the pointer to the function in the DLL so we can call it
		sum func = (sum)GetProcAddress(handle, "sum");
		
		// again, check if the function could be imported
		if (func != null) 
		{
			// call the function
			int result = func(1, 2);
		}
		
		// unload the DLL again
		FreeLibrary(handle);
	}
}
```

Note that we define the function signature again at line 5. Normally you would use [[Header Files]].

## Loading C++ DLLs into C#
You can also import C++ functions into C# using DLLs. .NET provides a very easy to use `DllImportAttribute`. Using the same defined DLL as above ([[#Create DLL]]), we can import the `sum` function and use it in our program:
```csharp
namespace Test;

public class Program
{
	private const string DLL_PATH = @"C:\path\to\your.dll";
	
	[DllImport(DLL_PATH, EntryPoint = "sum")] // if the method name is exactly the same as in the DLL, the EntryPoint argument is not needed
	public static extern int Sum(int a, int b); // imported methods from DLLs need to defined as static and extern
	
	public static void Main() 
	{
		// call the C++ DLL function from C#
		int result = Sum(1, 2);
	}
}
```

As always .NET takes care of almost all of our concerns. We only really need to define our method as `static extern` and add the path to the DLL (and optionally the function-name) in the attribute.

A lot of work happens in the background of .NET which makes this functionality possible. See [[Marshalling]] to learn more about how this process works..
