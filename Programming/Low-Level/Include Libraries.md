# Include Libraries
You've already learned [[Static Libraries]] and [[Dynamic Linked Libraries]]. After playing around a bit with C++ and 3rd-Party libraries, you will probably encounter Include Libraries, in some way or another, probably unaware until now.

Include Libraries are [[Static Libraries]], which you can add to your linker to compile/link a project. The Include Libraries in itself use [[Dynamic Linked Libraries]].

---

You can imagine the Include Libraries sort of as an importer/includer (as the name suggests). What you would normally do when using 3rd-party libraries is, that you import the libraries [[Header Files]] and then load the definitions for those headings with [[Dynamic Linked Libraries#Load DLL]].

That is a very tedious process though, and it is basically the same for all headings. Therefore most libraries that need to be included through DLLs, provide you a include Library, which is a Static `.lib` Library, which the linker can use at compile-time. These static libraries have all the Load DLL calls, therefore taking a lot of work for you. But eventually, the functions which you use from the 3rd-party library are still defined in the DLL.