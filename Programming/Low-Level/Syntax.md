# Syntax
## Usage of `const` in variable declaration
> Always read the variable declaration from right to left!

```cpp
int i; 					// a variable i as integer

const int i; 			// a variable i as integer with a constant VALUE

int const i; 			// same as above

const int * i; 			// a pointer i which points to a constant VALUE. Note: not the pointer itself, but the value it points to is constant!

int * const i; 			// reading backwards is important: a pointer whose address is constant is declared, the value it points to is not constant though

const int * const i; 	// a constant pointer which points to a constant value. nothing can be changed here
```
