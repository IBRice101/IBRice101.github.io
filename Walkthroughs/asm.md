# Assembly Basics

**NOTE**: With a few exceptions this guide assumes you have some basic knowledge of computing, such as binary, hexadecimal, memory, and so on.

## Background and Overview 

Assembly language is essentially the lowest level programming language that is at least somewhat reasonable for humans to use. "Low-level", in this instance, refers to the fact that the instructions the developer inputs and what is actually ran on the machine correspond extremely closely to one another, and hence you are "lower to the metal" of the computer itself.

The main thing that must be borne in mind for ASM, however, is that your mode of thinking has to be different. In order to read and understand ASM, you need to be able to logically execute instructions step by step. This is no easy task, and not something even I am very good at really, but I hope this guide will help you on the way to mastering this thing!

There are different assembly languages for different CPU architectures. The most common of which in the modern day, it can be argued, are x86 and ARM, with the former running on most of what would be considered to be traditional computers (Desktop PCs, Laptops, and so on), and ARM processors running on nearly everything else. There are plenty of exceptions to be sure but these are the general rules. 

Assembly, or asm, is generally converted into executable machine code by an assembler, such as nasm or masm.

Naturally, when working this close to the metal the allocation of very specific sizes of memory is of great importance, processors generally support the following data sizes:

- Word: 2-bytes
- Doubleword: 4-bytes
- Quadword: 8-bytes
- Paragraph: 16-bytes
- Kilobyte: 1024 Bytes
- Megabyte: 1048576-bytes

In each of these CPUs there is also what are known as "registers". These registers are small sets of data storage locations on the CPU itself. They are present to facilitate the extremely fast storage and transfer of data during the execution of a program.

In most processors there are 8 general purpose registers, four dedicated to storing data, two of which are pointers, and a final two which are indexes. Each of these sets of registers are divided into between two and four sizes depending on the number of bits within a processor and the type of register one is considering, the size of each of these registered is denoted by the prefix “r-“ for 64-bit processors (i.e., long values in C++), “e-“ for 32-bit (or ints), and no prefix for 16-bit (short values). Each register in the data group is further subdivided into 8-bit registers “-h” and “-l” (AH, AL, BH, BL, etc.), and the registers in the index and pointer groups have 8-bit register subdivisions that are only present on 64-bit devices (denoted by the “-l” suffix only)

## Syntax

### Instructions

With this in mind we can move on to syntax. Each assembly instruction can be generally separated into four sections, as can be seen below:

```
[label]    mnemonic    [operands]    [;comment]
```

What do these mean?

- Label: Symbols representing addresses in memory, specifically the current value of the active location counter, which keeps track of the next available memory location. There are two kinds of label.
  - Symbolic: these are semi permanent labels that can be thought of like variables. There are two further kinds of symbolic labels
    - Global: an identifier (name), followed by a colon (:), e.g., `label:`. These are found in the symbol table
    - Local: Same as global but with a full stop (.) prepended, e.g., `.label:`
  - Numeric: impermanent labels consisting of a number 0-9 followed by a colon, e.g, `8:`. These are used when quick references are needed purely locally and normally for one or two operations
- Mnemonic: The only strictly required section of an asm instruction, these can be converted 1:1 with so-called "opcodes", which are numerical (often displayed as hex) values that define or specify a specific function the computer is to perform, such as adding, subtracting, comparing, moving, returning, and so on and so on. A more in-depth exploration of mnemonics will be towards the end of this article
- Operands: The value(s) on which the instruction will operate. These can be registers, locations in memory, or simply numerical values. Operands are typically separated into `source` and `destination` operands, that is to day that if, for example, one were to add the values 1 and 2, where 2 was the source and 1 was the destination, the memory location of 1 would be overwritten with (1+2), or 3, and the memory location of 2 would be freed.
  - An interesting thing to consider here, however, is the order of the operands. There are two accepted syntaxes, Intel and AT&T: 
    - Intel: `add   eax, ebx    ; add contents of ebx to eax`
    - AT&T: `add   %ebx, %eax  // add contents of ebx to eax`
  - There are quite a few other differences here, I encourage you to look into them to see what you prefer!
  - I'll be using Intel syntax for this guide.
- Comments: Each line can be commented, as is to be expected in any decent programming language, comment syntax and common practise for intel is to separate the comment by at least one tab character and then begin it with a semicolon (;), as can be seen above. Frequent and in-depth comments are extremely welcome with asm, as it's very easy to get lost. 

### Sections

<!-- TODO: Continue the guide along these lines: https://www.tutorialspoint.com/assembly_programming/assembly_basic_syntax.htm
        specifically about data sections and stuff -->

## Code Example (Hello World)

The following is a Hello World code example using what we have learnt so far

```asm
section	.text
   global _start    ;must be declared for linker (ld)
	
_start:	            ;tells linker entry point
   mov	edx,len     ;message length
   mov	ecx,msg     ;message to write
   mov	ebx,1       ;file descriptor (stdout)
   mov	eax,4       ;system call number (sys_write)
   int	0x80        ;call kernel
	
   mov	eax,1       ;system call number (sys_exit)
   int	0x80        ;call kernel

section	.data
msg db 'Hello, world!', 0xa     ;string to be printed
len equ $ - msg                 ;length of the string
```

In Linux, this hello world program (`hello.asm`) can be ran on x86 architecture using the following commands:

```sh
nasm -f elf hello.asm
ld -m elf_i386 -s -o hello hello.o 
./hello
```

The first command uses the Netwide Assembler to assemble the code into an ELF (`-f` for format), a linux binary executable, and directs the output to an outfile (hello.o), after this the GNU linker takes over which uses `-m` to emulate an i386 architecture (x86_32), strips the executable of symbol information using `-s`, and then specifies the name of the output file using `-o [name]`. The final command simply runs the program.

## Mnemonic Table

Below is a non-exhaustive list of mnemonics that may be worth remembering, their syntax, what they do, and a couple of notes if they are so needed.

| Mnemonic | Description                   | Example           | Further Notes                                                                                   |
|----------|-------------------------------|-------------------|-------------------------------------------------------------------------------------------------|
| ADD      | Add                           | `ADD eax, ebx`    | dest = dest + src                                                                               |
| AND      | Logical and                   | `AND eax, 0Fh`    | consult logical and truth table to know what this does precisely                                |
| CALL     | Call subroutine               | `CALL func`       | transfers control of the program to another function                                            |
| DEC      | Decrement                     | `DEC ecx`         | synonymous with x-- or x-=1                                                                     |
| DIV      | Divide                        | `DIV ecx`         | always divides the 64 bits value across EDX:EAX by a value. <br/> the second operand is implied |
| IDIV     | Signed integer divide         | `IDIV ecx`        | same as DIV but signed                                                                          |
| IMUL     | Signed integer multiply       | `IMUL eax, ebx`   | dest = dest * src (the operands are signed)                                                     |
| INC      | Increment                     |
| INT      | Interrupt                     |
| JA       | Jump if above                 |
| JAE      | Jump if above or equal        |
| JB       | Jump if below                 |
| JBE      | Jump if below or equal        |
| JC       | Jump if carry                 |
| JCXZ     | Jump if cx zero               |
| JE       | Jump if equal                 |
| JECXZ    | Jump if ECX zero              |
| JG       | Jump if greater               |
| JGE      | Jump if greater or equal      |
| JL       | Jump if less                  |
| JLE      | Jump if less or equal         |
| JMP      | Jump                          |
| JNA      | Jump if not above             |
| JNAE     | Jump if not above or equal    |
| JNB      | Jump if not below             |
| JNBE     | Jump if not below or equal    |
| JNC      | Jump if no carry              |
| JNE      | Jump if not equal             |
| JNG      | Jump if not greater           |
| JNGE     | Jump if not greater or equal  |
| JNL      | Jump if not less              |
| JNLE     | Jump if not less or equal     |
| JNO      | Jump if no overflow           |
| JNS      | Jump if no sign (= positive)  |
| JNZ      | Jump if not zero              |
| JO       | Jump if overflow              |
| JS       | Jump if sign (= negative)     |
| JZ       | Jump if zero                  |
| MOV      | Move (copy)                   |
| MUL      | Multiply                      |
| NOP      | No operation (Ox90)           |
| NOT      | Invert each bit               |
| OR       | Logical or                    |
| POP      | Pop from stack                |
| PUSH     | Push onto stack               |
| RET      | Return from subroutine        |
| ROL      | Rotate left                   |
| ROR      | Rotate right                  |
| SAL      | Shift left                    |
| SAR      | Shift right                   |
| SHL      | Shift logical left            |
| SHR      | Shift logical right           |
| SUB      | Subtract                      |
| XCHG     | Exchange                      |
| XOR      | Logical exclusive or          |