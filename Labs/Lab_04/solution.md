#### Malware Analysis - Fall 2015
#### Lab 04 Solution

##### Lab_04-1.malware

This sample was first statically analyzed with IDA to determine what calls to look at. It was then run in a VM with no ASLR with break points at the interesting calls, and ran with these break points to see what happened.
    
1. Its calling KERNEL32!GetProcAddress for VirtualAlloc
2. Its calling VirtualAlloc to allocate 0xB000 bytes at 0x0C000000 as PAGE_EXECUTE_READWRITE
3. 0x401360 calls KERNEL32!GetProcAddress, 0x40137e also calls KERNEL32!GetProcAddress, but with advapi32.dll, and 0x401388 uses user32.dll.
4. GetModuleFileNameA,ExitProcess,CopyFileA,GetWindowsDirectoryA,LoadLibraryA,RegCreateKeyA,RegSetKeyValueA,RegCloseKey,MessageBoxA,
5. I set break points on the functions calling getProcAddress and looked at the arguments that were being passed.
6. Copies itself to C:\\WINDOWS\\virus.exe and then sets a registry key to auto run itself:
   ```C
   RegCreateKeyA("Software\\Microsoft\\Windows\\CurrentVersion\\Run");
   RegSetKeyValueA("viri","C:\\WINDOWS\\virus.exe");
   ```
   
   It then creates a message box saying "Infected!". After that it exits.

##### Lab_04-2.malware

This sample was unpacked with UPX when there was no aslr enabled, otherwise it failed to run after unpacked. Once unpacked, I statically analyzed it and recognized the structure of a few loops preforming xor and comparison operations, as well as a nibble swap loop. To reverse this I wrote a small python script.

1. 0x004011BC For the win function
2. For each character that you enter it flips the nibbles. So 0x41 becomes 0x14 and so on.
3. The encrypted data is at 0x0040303C, and the string xored with it to decrypt it is at 0x00403018.
4. flag{Pra1se_th3_Sun!}

   Script:
   ```Python
   f = "{ga1F_1auTca_eht_t0n_s!_s1hT}galf"
   data = "1DA17747F15A16776663359418E35B816A23D67C88000000"
   data = data.decode('hex')
   flag = ""

   for i,c in enumerate(f):
       nc=ord(data[i%21])^ord(c)
       flag+=chr(((nc&0xf)<<4)+(nc>>4))

   print flag
   ```
