#### Malware Analysis - Fall 2015
#### Lab 06 Solution

##### Lab_06-1.malware


1. 0x00403420

    a. 
    
       GetCommandLineA: Gets the command string for the current process.
       
       GetStartupInfoA: Populates a STARUPINFO struct for the current process. 

       GetModuleHandleA: When passed NULL (like it is here) returns a handle to the file that created the process.

2. 

    a. Yes

       It looks like there is a decoding function at 0x4012EC
        
       It is decoding a bunch of null terminated strings in the .data section.
    
    b. The very large basic block is doing a bunch of decoding to be used later, since there isn't any logic, just a lot of decoding, it makes the block large.

3. 

    a. All the GetProcAddress calls are dynamically loading library imports after it has decoded them with the decode function at 0x4012EC.

    b. It loads all the functions it decoded, then if they all loaded successfully, it returns 1, otherwise it frees the library and returns 0.

4. 
 
	The first thing it does after loading all the functions it wants to use is to calculate the expected size of the exe due to headers, then read extra bytes from the end of the file that were not loaded in with the binary. 

	It then takes this string and decodes it using the same function as before. Then it uses RtlDecompressBuffer on it, but for some reason this does not work at all, so an empty buffer is used. 

	It would have done a bunch of operations based on this decompressed value, but we will have to skip those and look at what it would have done after this. Next it gets the executable path and appends an buffer to it (maybe as arguments) but that buffer appears to empty at this moment. 

	It then creates a new process of this file with CreateProcess. It then seems like it would have read memory from that process and preform some sanity checks on it. It then also does a bunch of writing to this process' memory, possibly some sort of dynamic code modification. 

	It is very difficult to only use static analysis of these operations, but based on the unused but still decoded strings, I would make a guess that it does some checks to see if it is in a sandbox/vm ('sandbox' and 'vmware' strings). 

	It may also do other redpill tests using GetUserNameA and reading registry values, since these functions were also decoded. Finally, it may try to swap the mouse buttons, due to the 'Control Panel //Mouse' and 'SwapMouseButtons' strings which were also unused. 

	Looking at the data that was at the end of the file, there may some sort of botnet kind of thing connected though IRC, due to the IRC commands there, however, there is not a lot of code in the binary, so that may be unlikely.


