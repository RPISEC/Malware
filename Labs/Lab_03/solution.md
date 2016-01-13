#### Malware Analysis - Fall 2015
#### Lab 03 Solution

##### Lab_03-1.malware

1. Yes, there is a dll, it can be extracted using FileAlyzer or Resource Hacker.

2. 
   a. LoadResource - Load the dll info from the resources

   c. WriteFile – Allows malware to log to file or write more malware to a file
   
   d. IsDebuggerPresent – Malware could act differently if it detects a debugger is being used 
to analyze it

3. 
   a. "http://rpis.ec/" - Potential network signature
   
   b. "regsvr32 /s C:\Windows\atidrv.dll" - Potential persistence / hiding place
   
   c. "C:\Users\IEUser\Downloads\BHOinCPP_src\BHOinCPP\Release\launch.pdb" - BHOinCPP is a project from CodeProject
   
4. It unpacks and creates a dll, and then registers that dll as with regsvr
   ```
   CLSID\\{3543619C-D563-43f7-95EA-4DA7E1CC396A}\\InProcServer3
   Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Browser Helper Objects\\{3543619C-D563-43f7-95EA-4DA7E1CC396A}
   CodeProject Example BHO
   ```
   
5. {3543619C-D563-43f7-95EA-4DA7E1CC396A}

6. IWebBrowser2

7. 0xa4 = put_Visible - Show the window 

   0x2c = Navigate - Go to page in browser
   
   This combination displays one of the RPISEC URLs found in the adware

##### Lab_03-2.malware

1. MD5 is bf4f5b4ff7ed9c7275496c07f9836028. VirusTotal reports that it created and opened a file in the C drive, then copied it to the user's directory as java.exe.
    It also says it made a DNS request to us.t28.net

2. 
    a. GetLogicalDrives – Gets bitmask representing all available drives. Could be used for environmental keying or host identification
    
    b. gethostbyname - Could be used to resolve an attackers host for communication
    
    c. GetOEMCP - Could be checking for VM

3. 
    a. 'SOFTWARE\Microsoft\Windows\CurrentVersion\Run' -  Registry key that auto runs when the user logs in, possible persistence mechanism
    
    b. 'configserver)/r(ndr29(xhhoxxx2)00xAAAAAA....' - Could be an encrypted configuration file
    
    c. '\java.exe' - The file it might make for persistence.

4. It sets the key in 'SOFTWARE\Microsoft\Windows\CurrentVersion\Run' to 'C:\DOCUME~1\User\java.exe', which is a copy of itself that it made. Some host-based signatures are that its in documents and settings for the user and copies under 'java.exe'.

5. 
   Lists processes: 0x0402310
    
   Remote Shell: 0x0402490 and 0x0402660 to use
    
   Upload File: 00402210

6. 
   List processes: The command id is 0x7
    
   Remote Shell: The command id is 0x9 and 0x10
    
   Upload File: The command id is 0x6

7. 
   List processes: It sends the process name (xored with 0x55) and process id back to the control server
    
   Remote Shell: 0x9 opens cmd.exe, 0x10 sends a command to it (xored with 0x55) and then reads from the named pipe and sends the result back (xored with 0x55)
    
   Upload File: It maps the file into memory, xors it with 0x55, and sends it to the control server

8. 
   Lists processes: CreateToolhelp32Snapshot, Process32First, Process32Next
    
   Remote Shell: CreateProcessA, PeekNamedPipe, WriteFile
    
   Upload File: CreateFileA, CreateFileMappingA, MapViewOfFile

9. 
   0x2 - List contents of directory

   0x5 - Download a file to infected computer

   0x8 - Terminate process by PID
