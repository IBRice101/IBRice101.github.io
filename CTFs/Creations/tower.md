# Tower CTF Challenge

*By izbr*

[**You must climb the tower to claim what you seek**](media/tower.out)

[SPOILERS AHEAD]

**Concept**: A simple application that makes use of stack strings to hide the flag inside of a binary.
**Category**: Reverse Engineering

## Ideas

- Don't make the stack string the flag, instead make the stack string a password for a flag
  - A tool like FLOSS could easily discover the flag in this case
  - Only a beginner CTF, though
  - Issue here is that when the player decompiles the program they may discover the flag first
- Above problem can be fixed with a simple XOR cipher (as per [this stack overflow answer](https://stackoverflow.com/a/72168280/12207605))
  - Can be reversed by applying the same key
  - Maybe an alternate way of solving this is by manually XOR-ing everything lol
- Maybe use multiple stack strings that each output different sections of the flag?
  - Ensure the results are prepended with a number so players don't get confused
- Should probably be console application, would make implementation easier

## Research

- [X] How to create a stack string
  - With help from [This video](https://youtube.com/watch?v=DV4DKq7zTfE) (Which also gave me the idea to obfuscate the string)
- [X] How to retrieve a stack string (based on user input especially)
- [X] Figure out how to obfuscate the genuine flag
  - Will be done via XOR (as mentioned above)
  
## Implementation

Done in C for Linux systems.

User input is a command prompt-like concept with `input> ` as the prompt itself. Aim is to input the correct string of characters (password) in order to unlock the flag.

The flag itself is stored as a stack string in the binary. In the `stackString()` function is a regular c string with the contents "nothing", that eventually gets converted into "Babylon" on the stack (the genuine password). When the user enters this, the `encryptedPassword` gets decrypted. This password is encrypted with some basic XOR encryption because just having the flag on the stack like that was a little too trivial in my opinion.  

Once the code was written it was compiled with GCC and the -O0 flag (no optimisations), and its symbol table was removed, ensuring no meaningful variables or function names could slip though. 

### Iteration

Thanks to Praeceps on Discord for the suggestion, removed the obfuscation function that gets optimised away by GCC and replaced it with a second XOR (like the flag) for the password

May also hash/checksum the flag decrypt output then check that

## Walkthrough

[COMING SOON!]

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/A0A1D0FSN)
