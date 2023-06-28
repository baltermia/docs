# Header Files
Header files are used to [[#Declaration|declares]] entities, that are used in external code that the [[Build#Linker|build linker]] later uses to link object files together to form a binary.

First let's take a look at declaration and definition:

## Declaration
The declaration of an entity identifies it and specifies properties. A function declaration can be the following:

```cpp
int add(int a, int b);
```

The declaration does not define any body for the function. It only tells use what input it takes and what output it generates (and other properties like [[Assembly#Calling Conventions]] etc.).

## Definition
A definition can also be a declaration. Lets take our method from above and define it:

```cpp
int add(int a, int b) 
{
	return a + b;
}
```

---

Header files draw a distinct line between declaration and definition. The declaration always happens in the `.h` files, so that they can be included in external libraries, while the definition happens in a `.cpp` file, having the same name as the header file and `#include`-ing this header file. The C++ file should implement all entities declared in the header file, as otherwise the [[Build#Linker]] would have no symbol to load.

## Includes
An include statement is a directive that causes the contents of the specifies file to be inserted into the original file. This step happens during compilation, see [[Build#Preprocessor]].

Includes are used to include declarations from header files. These declarations can then be used to call functions, create objects etc. while their actual implementation will only be inserted during [[Build#Linker|build-linkage]]. Includes should most of the time only include declarations (because they are copied into the source file) and not definitions.

## ODR ->  `#pragma once`
The C++ STL defines a **One Definition Rule** (ODR). This rule implies, that a entity can be declared multiply times, but only defined once. The use of this rule is, that entity definitions are only copied from an [[#Includes|include]] once during the compilers [[Build#Preprocessor|preprocessing]] to minimize duplicate code.
`#pragma once` is a preprocessor directive which is not in any actual C/C++ standard, that is designed to cause a source file to be included only once. It's basically used to make header files follow the one definition rule.

But as said above, this directive is not actually in any standard, and also not the C++ STL. Therefore it is heavily recommended to deal with the ODR using the following code:

```cpp
#ifndef HEADER_H
#define HEADER_H
... contents of header.h
#endif 
```

The actual name (`HEADER_H` in this case) can be any identifier, it does not have to follow the example above (recommended though). 

What happens here is, that a preprocessing directive checks whether the name is already defined. If not, the name is defined and the code is loaded. If the same file i loaded again, the name will be defined already, so the code is not copied into the source file.

Pay good attention to the defined identifier though. In a bigger project, if using the same identifier twice could lead to entity definitions not being loaded. You will receive a linker error message. Therefore following some standard at least (having the file name in the identifier) prevents this problem.