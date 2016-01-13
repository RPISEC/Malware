#### Malware Analysis - Fall 2015
#### Lab 01 Solution

##### Lab_01-1.malware


1. 2009-05-14 10:12:41

2. 
    a. ShellExecuteExA - Can be used to run applications

    b. Socket APIs - Make network connections
    
    c. File API - read/modify files

3. 
    a. 60.248.52.95 - Potential network signature

    b. http://www.ueopen.com/test.html - Potential network signature
    
    c. cmd.exe - The malware could be trying to run shell commands
    
    d. *(SY)# - Potential network signature, possible used for a remote shell prompt

4. Connects to 60.248.52.95, offers up a remote shell, then deletes itself

5. Process name. Ensures procmon data involves the sample

6. Nothing particular, except for the command it runs to delete itself

    `cmd.exe /c del $PATH > null`

7. 
    a. Connects to port 443 on 60.248.52.95

    b. *(SY)# - Remote shell prompt
    
8. The file's self deletion was a nuisance. This can be overcome by keeping a separate 
   copy, or by NOP'ing the delete call

9. To act as a backdoor by offering a remote shell to the attacker

##### Lab_01-2.malware

1. 02658bc9801f98dfdf167accf57f6a36

2. 
    a. CreateProcessA - Execute applications

    b. WriteFile - Write to files
    
    c. HttpOpenRequestA - Access websites

3. 
    a. wuauclt.exe - Windows update program, potential trojan or disguise

    b. cmd /c - run shell commands
    
    c. 69.25.50.10 - Potential network signature

4. Nothing appears on screen. In the background it is attempting to connect to 
   69.25.50.10, but fails. If it succeeds it offers a remote shell.

5. Process name. Ensures procmon data involves the sample

6. Runs wuauclt.exe

7. Connects to 69.25.50.10. Remote pseudo-shell commands (putf, getf, /tasks/, exit)

8. No, though more information could have been made available if 69.25.50.10 was up

9. Acts as a backdoor, allowing remote file access and program execution.

##### Lab_01-3.malware

1. Yes, very few strings and imports. VirtualSize >> Size of Raw Data. Possibly UPX packed.

2. No, UPX reports an error, "file is modified/hacked/protected; take care!!!"

3. 
    a. Mozilla/4.0 - Possible user agent

    b. http://%s/%s/ - Format string for making URLs
    
    c. www.practicalmalwareanalysis.com - Potential network signature

4. Connects to website "http://\<url from resources\>/\<base64 local hostname\>/"

5. No

6. The URL and user agent

7. The packing, I'm not sure what else the malware is doing besides connecting out.
   This program will have to be unpacked manually.

8. Besides reporting the hostname to the attacker, there's no way to tell without further
   analysis.

