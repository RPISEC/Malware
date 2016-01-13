#### Malware Analysis - Fall 2015
#### Lab 02 Solution

##### Lab_02-1.malware


1. 
    a. Main is at 0x004011A0
    
    b. Main checks if there is internet connection, using http://reversing.rocks/ as a domain to check. If the test passes it runs a subroutine, otherwise it exits right away.
    
        i. It uses a call to an import in the import table. It also uses an if in the form of test/jz. Finally it calls the subroutine or exit.
        
        ii. "http://reversing.rocks/" Seems like an interesting string.
        
2. 

    a. InternetConnectA(hInternet, "reversing.rocks", 0x4D2, 0, 0, 3, 0, 0)

        i.  (HINTERNET) hInternet => Handle from the InternetOpen
            (LPCTSTR) lpszServerName => Server name = "reversing.rocks"
            (INTERNET_PORT) nServerPort => Port = 1234
            (LPCTSTR) lpszUsername => NULL
            (LPCTSTR) lpszPassword => NULL
            (DWORD) dwService => 3 => HTTP
            (DWORD) dwFlags => 0
            (DWORD_PTR) dwContext => NULL

    b. It opens a connection and goes to reversing.rocks and calls another subroutine. When that is done, it closes the connection.

        i. Makes several calls to import tables, and the subroutine. Also has an if to check that the connection was opened correctly.

3. 

    a. Many calls to imported functions. An if to check if the first file could be found, and a while that will go loop though all files.

    b. FindFirstFileA, HttpOpenRequestA, HttpSendRequestExA, InternetWriteFile, FindNextFileA, HttpEndRequestA, InternetCloseHandle, FindClose

    c. Sends files that match "\\*" through post

4. The malware attempts to connect to the creator's site and then exfiltrate files from the local drive to his server. It then closes the connection and quits.

##### Lab_02-2.malware

1. 

    a. AllocConsole, FindWindowA, ShowWindow, fopen, time, fputs, ctime, fclose

        i. AllocConsole creates a console for the process, FindWindow finds a window for the process and returns its handle, ShowWindow shows a window, the other functions are more normal c functions.

        ii. "\\WINDOWS\\lzwindowlz.av", "\nStarted logging:"
2. 

    a. GetAsyncKeyState, fopen, fseek, fread, fputc

    b. There is a large switch with several cases

3. The malware is a keylogger that then sends the log to the owner.

    a. One possible signature is looking for calls to GetAsyncKeyState

        i. This would be used by keyloggers to get the keypresses without needing to have an active window. Detecting this could help find keyloggers in general.

    b. The sample creates lzwindowlz.av which it fills with key presses that it records. Special keys are replaced with brackets and their name. This is then emailed every 100 characters to the address specified. The file is cleared at this point.
