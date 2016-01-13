#### Malware Analysis - Fall 2015
#### Lab 08 Solution

##### Lab_08-1.malware

1. It has a few unconditional jumps that would mess up a linear disassembler due to rogue bytes. It also uses SEH to jump to the desired location by causing a crash. It also does indirect jumps, jumping to a different location before calls.
	
2. It uses IsDebuggerPresent first to tell if there is a debugger. If so it returns and exits. It also uses rdtsc to tell how many cycles passed. If more than 0xFFFFF cycles has passed between the two points, it will jump back into previous code, causing a crash.
	
3. The sample first gets the username, then copies it into "C:\Users/{username}/AppData/Roaming/Microsoft/Windows/Start Menu/Programs/Startup/kingkai.bat". It creates this file and then writes 
   ```cmd
   @echo off
   shutdown /s/ f /t 0
   ```
   into the file. This would cause your computer to shutdown every time you start it up.
