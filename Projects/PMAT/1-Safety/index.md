# Safety Always! Setting Up A Lab & Malware Handling

Tools:
- VirtualBox (I will be using VMWare because I already have it and I know how to use it)
- [Windows 10 VM](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) (64-Bit)
- [REMnux VM](https://remnux.org/)
- [FLARE-VM](https://github.com/mandiant/flare-vm)
- Analysis Network
- INetSim

- This section is primarily concerning installation and stuff, notes are unnecessary here

## Initial Detonation

- Grabbed [PMAT-labs](https://github.com/HuskyHacks/PMAT-labs) repo and placed it in the corresponding subdirectory within this repository (at least locally)
- First sample to detonate is WannaCry (Exciting!!!)
  - Sample is unarmed, can be armed by appending `.exe` (or in this case, removing `.malz`)
  - Nothing seemed to have happened initially, although a file `@WannaDecryptor@.exe` appeared almost immediately
  - Once clicked, the famous desktop background and exe file appeared. Fully encrypted drive, etc.
  - Restored to `pre-detonation` snapshot.

## Basic Malware Handling

- **SAFETY ~~FIRST~~ ALWAYS**
- Most vulnerable time is when malware is in transit
- Add an extension (ex. `malz`) after the `.exe`
- Create a naming convention and stuck to it.
  - Examples:
  - Malware.Calc.exe
  - MAL.Calc.exe
  - Trojan.Calc.exe
  - And so on...
- Standard convention is to password protect malware
  - Not to keep anyone out, as a sort of check to make sure you *intended* on doing this

## Safe Malware Sourcing

- How to find malware responsibly
- Three main Malware repositories:
  - [TheZoo](https://github.com/ytisf/theZoo)
    - Live binaries *and* source code
  - [vx-underground](https://github.com/vxunderground/MalwareSourceCode)
    - Likes to source malware from various locations
    - Kiely does not recommend going to main website (although I have and its fine)
    - Very up to date (sus perhaps but w/e)
  - [Zeltser Resources](https://zeltser.com/malware-sample-sources/)
    - Zeltser is a titan of industry, very reputable
    - Put together a bunch of resources for Malware researchers and analysts
- Only required samples in this course is in `PMAT-labs`