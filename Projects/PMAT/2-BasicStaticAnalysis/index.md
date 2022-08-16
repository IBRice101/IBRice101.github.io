# Basic Static Analysis

## Hashing Malware Samples

- "static" means "not running the malware"
- very early in analysis
- will not be able to learn much, tbh
  - but its a start

README contents as follows:

```
Analyst,

We do not have the file hashes for this sample yet. Please pull the hashes and submit.

-RE Team
```

- Collect 2 file hashes
  - SHA256:
    - `sha256sum.exe Malware.Unknown.exe.malz`
    - SHA256 sum gets outputted, cannot copy and paste and its too long for me to type, trust me that it's there tho lol
    - Normally you should copy paste that out onto notes, but I can't right now because this bloody hypervisor is giving me grief
  - MD5:
    - `md5sum.exe Malware.Unknown..exe.malz`
    - See above
- Place them into notes, txt file for example.

## Malware Repositories: VirusTotal

- on host machine visit [VirusTotal](https://virustotal.com/gui/home/upload)
  - You can submit hashes here to find information regarding whether or not a sample has been seen before
- in this case "no matches found"
  - no VirusTotal hits on majority of code in the repo, but probably in future

## Strings & FLOSS: Static String Analysis

- String is an array of characters
- ends with a null byte so the computer knows a string has ended
- Certain things (such as URLs) need to be encoded into the malware as strings
- Strings may let you know what the binary is doing
- Don't have to run the binary to know what the strings are doing
  - Can use either `strings` (basic option) or `floss` (chad option)
  - In this case it extracts many interesting things, such as
    - Directory path for malware as developed
    - various DLL function names
    - a cmd command

## Analysing the Import Table Address Table

- Go to `C:\Users\Isaac\Desktop\FLARE\Utilities\peview.exe`
- load in `Malware.Unknown.exe.malz`
- A PE is just a massive array of bytes, peview shows this in a hex editor and various headers on the left
- PEs are structured like so:
  - Starts with `MZ` always, this is a dead giveaway that it's a PE
  - Followed by `!This program cannot be run in DOS mode`
  - And so on...
- They follow a specific format
- peview can extract this information and show it to you
- Go to `IMAGE_FILE_HEADER`
  - Look at the Time Date Stamp
    - This may be reliable or may not
    - Borland Delphi malware will always have a date of 1992
    - In this way we know that, while the timestamp may not be reliable, it can still give us information
- Go to `IMAGE_SECTION_HEADER .text`
  - Look at virtual size and size of raw data
    - Compare the two values using programming calculator
    - If the two values are similar we know the virtual and raw values are roughly the same, therefore likely not much is hidden
    - If virtual size > raw size we can ascertain that this is a "packed binary"
- Go to `SECTION .rdata > IMPORT Address Table`
  - One of the most important parts of peview
  - We need to consider the Windows API...

## Introduction to the Windows API

- API == Application Programming Interface
- Created by Windows Dev so people writing in C/C++ don't have to learn Assembly or mess anything up at Kernel level lol
- Many different functions, each function is easier to work with than the ASM
- If you see something that looks like an API function, just google it lol
  - Ex: `ShellExecuteW`

### Update: MalAPI.io

- Use [MalAPI.io](https://malapi.io/) to find specifics on certain possibly malicious API functions
- Categorised into:
  - Enum
  - Injection
  - Evasion
  - Spying
  - Internet
  - Anti-Debugging
  - Ransomware
  - Helper
- Also has associated attacks etc.
- And documentation
- And Library information
- And a description
- And a malware sample
- There is also a mapping mode
  - Select all the API functions your heart desires
  - Take a screenshot of the resulting exported table and use that as part of a report
- Incredibly useful

## To Pack Or Not tO pACK: Packed malware Analysis

- Pack malware to make it look different than its original source
  - Like a zip file, for example
- Compression of an already existing piece of malware
- Example:
  - Take malware `malz.exe`
  - get packing software (in this example `UPX`)
  - UPX will put a packer stub (aka compression or coder stub) inside the malware
  - Take all of the code that is below that point in the malware and crunch it down
  - Will have the PE header, packer stub, then tiny piece of code
  - **At runtime** the stub will look at the code below and unpack it
- Can evade antiviruses, packing will make the hash different
- Comparing packed and unpacked malware in peview:
  - Unpacked:
    - Plenty of human readable strings
    - Lots of PE file information
    - Large body of information in the hex viewer
  - Unpacked
    - Very few if any human readable strings
    - Reduced set of PE file information
    - Smaller (but still kinda large) body of information in the hex viewer

## Combining Analysis Methods: PEStudio

- Unfortunately I do not have PE Studio in my installation of FlareVM, but I have played around in it for quite some time now and know what it does, it's very cool
- Makes early stage static analysis very straightforward
- What does PE Studio have?
  - Hash values
  - First bytes (both hex and text)
  - Total size in byres
  - architecture
- Many many other sections, but has an additional layer of analysis
  - Indicators - Information on strings or patterns it sees in a PE and if they tend to be malicious or not
  - Libraries - All of the libraries the malware imports, with special field for "blacklisted" items, libraries that are frequently imported by malware
  - Strings analysis, with blacklist, hints (what it thinks the string is doing or where it has come from), can sort by size, alphabet, group, and so on
- Moving forward I probably only need FLOSS and PE Studio lol