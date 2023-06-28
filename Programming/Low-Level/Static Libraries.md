# Static Libraries
Other than [[Dynamic Linked Libraries]], static libraries are loaded into the machine-code when building a program.

Static libraries don't have `main` entrypoint like the `DllMain` function in DLLs.

These libraries are mostly used with [[Header Files]], where the header files are [[Header Files#Includes|included]] in a source file and the static libraries hold the definitions (symbols) needed by the [[Build#Linker|linker]] when building. F.e. the `std` standard library is a static library, when you use any code of STL, this code is statically included into your program when building.