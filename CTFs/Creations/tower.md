# Tower CTF Challenge

*By izbr*

**You must climb the tower to claim what you seek**

**Concept**: A simple application that makes use of stack strings to hide the flag inside of a binary.
**Category**: Reverse Engineering

## Ideas

- Don't make the stack string the flag, instead make the stack string a password for a flag
  - A tool like FLOSS could easily discover the flag in this case
  - Only a beginner CTF, though
- Maybe use multiple stack strings that each output different sections of the flag?
  - Ensure the results are prepended with a number so players don't get confused
- Should probably be console application, would make implementation easier

## Research

- [ ] How to create a stack string
- [ ] How to retrieve a stack string (based on user input especially)
- [ ] Figure out how to obfuscate the genuine flag
  
## Implementation

Done in C for Linux systems.

<!-- More info here -->

## Walkthrough

