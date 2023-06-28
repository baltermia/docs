# Build
A common misconception is that the compiler generates machine-code directly out of source code and no other steps are needed when building a C++ project. The build consists of multiple steps, shown in the diagram below:

![[Build.svg]]

The Build consists of a compiler, linker and a builder:

## MSBuild / Builder
The builder is a tool that combines a compiler and linker together so that a binary end-product can be produced, an executable. MSBuild is a build tool from Microsoft, which uses the MSVC [[#Compiler]] and generates a binary output for example for a Visual Studio C++ project. Such a project always contains a main `.vcxproj` file ("project file", XML), that consists of configuration settings for compilation and linkage, e.g. directories and files that the [[#Linker]] needs.

## Compiler
The compiler does not directly create a [[#Binary]], it creates [[#Object Files]] from source files.

### Preprocessor
Before the compiler generates any output, the preprocessor is run. What the preprocessor does is, it basically edits text. 

It takes `#include`s and replaces them with the code that stands behind these files. More about this process under [[Header Files#Includes]].

It will also include/exclude code in `#ifndef` blocks.

### Object Files
An object file is the product of a compiled C++ file. It's machine code, that still is unlinked: 

## Linker
The linker links [[#Object Files]] together. Compiled obj files may contain e.g. functions that are defined in another obj file. See the illustration [[Build#Build|above]].

The library project contains a [[Header Files|header file]]. It [[Header Files#Declaration|declares]] a function, which is then used in the main project that contains a main method (a starting point). The library C++ file [[Header Files#Definition|defines]] this function then. Both these C++ files are then compiled into different obj files. They are not linked together. The main obj fail contains a [[#Symbols|symbol]] of a function defined in the library obj file.

What the linker does, is that it takes unresolved symbols in the machine code and takes the functions from linked sources, the library obj file in this example. The linker doesn't only links object files together, it can also link functions/classes etc. from the STL library or `.lib` ([[Static Libraries]]) files, when you use `std::string` from std for example.

### Symbols
Symbols (also called program symbols or symbolic cross-references) are identifiers for functions, classes, structs etc. that come from a external source from a program. They define the exact behavior of external resources and they are mostly used for the linker to search/know what references he needs to link to existing machine-code.

Symbols are used to refer to the same entity throughout a program or [[#Translation Unit]].

#### Name Mangling
Name mangling is the technique used to translate a declaration into a symbol and vice-versa.

### Binary
The Linker will create the final binary file that can be execute, the executable. Compiled object files, when containing external symbols, will not run execute. The linker adds all remaining machine-code that is necessary to form an executable. Without a linker, compiled code / object files, could not be executed.

## Translation Unit
A translation unit is all input to a compiler from which an [[#Object Files|object file]] is generated. It consists of a source file, that ran through the [[#Preprocessor]], meaning that `#include`s are literally included (the code is copied) into the code and code sections from `#if`s are pasted/removed.