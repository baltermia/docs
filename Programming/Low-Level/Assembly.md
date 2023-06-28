# Assembly
C++ is compiled into assembly code which can then be translated to machine-code to be run by a computer. We can learn a lot about how a computer works by looking at and understanding the assembly language.

## Registers
To understand the concepts we talk about later below we first need to learn what processor registers are. Basically a register is memory that lives directly on the processor that can be accessed very quickly. The speed advantage compared to normal RAM is immense. This internal processor memory is very expensive though, so there is not a lot a single program can access. 

The name of a register depends on its use and the architecture it underlies. You will see specific implementations later. When using 32-bit, a registers name looks like this: `EXX` where "XX" is a placeholder for the specific identifier. In 16-bit architecture the "E" prefix is dropped, so the same register would only be called `XX`.
### Pointer Registers
#### Instruction Pointer
The Instruction Pointer `EIP` (also called program counter) indicates where a computer is in its current program. This basically means that the pointer holds the address of code that should currently be executed. The address is incremented as soon as it executed a sequence of instructions.

So the instruction pointer tells us where in the program code is executed currently. This pointer can be changed by the program itself too. This is used for example for `jmp` statements (also known as `goto`s) and mostly for [[#Function calls]] (where the `call` instruction is used).
#### Stack Pointer
Another crucial register is the stack pointer `ESP`. This pointer holds the address of the most recent entry that was pushed (see [[Stack & Heap#Stack]] to learn about how the stack works) onto the stack. This address is increment and decremented as soon as you `push` and `pop` data to/from the stack.

### Data Registers
The process registers also include data registers. These can be used to store data in the current program. Generally there are 4 of these registers; `EAX`, `EBX`, `ECX` and `EDX`.
Each of these can be simply used to store temporary memory, but they are also used for arithmetical operations (copied from [here](https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm)):

> **AX is the primary accumulator**; it is used in input/output and most arithmetic instructions. For example, in multiplication operation, one operand is stored in EAX or AX or AL register according to the size of the operand.
>
> **BX is known as the base register**, as it could be used in indexed addressing.
>
> **CX is known as the count register**, as the ECX, CX registers store the loop count in iterative operations.
>
> **DX is known as the data register**. It is also used in input/output operations. It is also used with AX register along with DX for multiply and divide operations involving large values.

---

These are by far not all registers, but just the most basic notable ones. Visit [this online tutorial](https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm) for a definition of all registers.

## Function calls
Most of today's developers don't know how a function is actually called. Basically, a function can be called using a function pointer, which is just an address that leads to the first processing-line in a function.

![[ASM-Function-Calls.svg]]
<sub>(Pseudo code, won't compile)</sub>

So to call a function, we need to `call` it using it's address. What actually happens is that the Instruction Pointer is set to that address, you already read about this above under [[#Instruction Pointer]].

Now we know how we can call a simple function. But what about functions with parameters? This leads us to calling conventions:

## Calling Conventions
Calling conventions define how internal and external functions are called. They define how parameters for a function are created/managed, where they are stored and who (the caller or callee) is responsible for the memory cleanup. The same applies for the return type.

Calling conventions define the following:
- Where are _parameters_ stored?
- Who cleans these _parameters_ up?
- Where is the _return value_ stored?

In a assembly program for example, you need to create the variables somewhere before you call the `call` instruction (the call instruction directly redirects the instruction pointer). This memory can for example be stored in the [[#Data Registers]], but if there are more than say 4 parameters, they might also me stored on the stack. This exact behavior is defined by the calling conventions, because the function needs to know where the parameters are stored.

After a function returns, these parameters need to be cleaned up again. Either the function itself, or the caller can do this, the definition, again, lays in the calling convention.

And same as the parameters, there might also be a return value. Again, this might be stored in the [[#Data Registers]], but depending on it's size it could also be stored on the stack. 

With a return type it might make sense to clean up the parameters in the function, because you want the return value to be the last entry in the stack, you don't want your parameters laying before it. This is just a simple example showing the importance of calling conventions and why there need to be multiple different ways to call a function.

### C++ Calling Conventions Table
Microsoft defines the following calling conventions in their C++ compiler:

| Keyword        | Stack Cleanup | Parameter passing                                                          |
| -------------- | ------------- | -------------------------------------------------------------------------- |
| `__cdecl`      | Caller        | Pushes parameters on the stack, in reverse order (right to left)           |
| `__clrcall`    | n/a           | Load parameters onto CLR expression stack in order (left to right).        |
| `__stdcall`    | Callee        | Pushes parameters on the stack, in reverse order (right to left)           |
| `__fastcall`   | Callee        | Stored in registers, then pushed on stack                                  |
| `__thiscall`   | Callee        | Pushed on stack; **`this`** pointer stored in ECX                          |
| `__vectorcall` | Callee        | Stored in registers, then pushed on stack in reverse order (right to left) | 

## Debugging
### Disassembly
You can see the compiled assembly code from a C++ project in the disassembly. It also allows you to debug the assembly code directly. You can set breakpoints and skip/step over code like in a normal debug manner. To access the disassembly start your program to debug, then head to `Debug > Windows > Disassembly`.

### Registers Window
Visual Studio allows you to see the values of processor registers when debugging a program.

First, put a breakpoint somewhere in your program and start it. As soon as the breakpoint is hit, you can open the `Registers` window under `Debug > Windows > Registers`.