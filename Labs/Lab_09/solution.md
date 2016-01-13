#### Malware Analysis - Fall 2015
#### Lab 09 Solution

##### Lab_09-1.malware

1. PEiD detects it as UPX, but it appears to be a custom packer.

2. 0x00401000 - Beginning of NOP slide to OEP

3. Using the "Find OEP by Section Hop" option in OllyDump will take you to OEP.

##### Lab_09-2.malware

1. There are very few imports, there is a non-standard section named .petite

2. This sample is packed with PEtite

3. 0x004012C0

4. You can use the PUSHAD/POPAD breakpoint technique to find OEP

##### Lab_09-3.malware

1. Search for anti-analysis tricks, GetTickCount is called very close to the end of the packing phase.

   If you run the program until the message box appears LordPE can dump it well.
   
2. The malware NULLs the PE header as an anti-dump mechanism. You can use LordPE to reconstruct the header
   manually, copy the header from the unpacked version and fix it up in LordPE, or if you dumped the program
   with LordPE the header will be fine.
   
3. Run the original .exe until the message box appears, at this point all of the imports are loaded.
   Attach ImpREC, enter OEP, clirk IAT AutoSearch and GetImports, then fix your dump.
