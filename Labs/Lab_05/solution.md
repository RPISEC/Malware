#### Malware Analysis - Fall 2015
#### Lab 05 Solution

##### Lab_05-1.malware

1. It drops a file from its resource section (RC_DATA "DROP") into "C:\Program Files\Google\Update\GoogleUpdate.exe"

2. By replacincg "C:\Program Files\Google\Update\GoogleUpdate.exe" the malware is run every time Google Updater is triggered.
   This is a great host-based signature because we can check the validity of this file.
   
3. It uses the mutex 'WODUDE'

4. It hides the console window

   It replaces a "trusted" file/program

5. SetWindowsHookExW - Enables a callback function on keypresses

   SetWinEventHook - Enables a callback function on window focus change

6. WH_KEYBOARD_LL, EVENT_SYSTEM_FOREGROUND, WINEVENT_SKIPOWNPROCESS|WINEVENT_OUTOFCONTEXT

7. It writes keylogged data to a file in the current directory, in this case "C:\Program Files\Google\Update\\\<hostname\>"

##### Lab_05-2.malware

1. This malware downloads the file at "http://malcode.rpis.ec/update_defender" and uses it to replace the file at 
   "C:\Program Files\Mozilla Maintenance Service\maintenanceservice.exe".  If that fails, it will replace that file 
   with the DROP resource

2. Similar to Lab_05-1.malware, this overwrites an update service, this time for Firefox. We can verify this file to
   confirm presence of the malware
   
3. A second mutex is required so that only one enumeration of child windows is done at a time. The first enumeration to 
   run will grab the mutex, and the next enumerations will have to wait for this mutex to be released
   
4. Sends an Event/Message to a window. This can be used for updates or triggers, e.g. mouse, keyboard

5.  0xD2 - EM_GETPASSWORDCHAR - gets the character that an edit control message shows when a user is typing a password 
                                instead of showing the password
                                
    0xCC - EM_SETPASSWORDCHAR - sets the character that an edit control message shows when a user is typing a password 
                                instead of showing the password.  In this case, the malware sends a parameter of 0 which
                                means the control message will show the password plainly
                                
    0xC4 - EM_GETLINE - gets the line of text specified in an edit control message
    
6. This sample looks for password boxes in foreground windows. Once it finds one it will remove the password mask
   using EM_SETPASSWORDCHAR, steal the password with EM_GETLINE, and then reset the password mask. This differs from the
   last sample which hooked keyboard events to log all keystrokes. This sample specifically targets password fields
   
7. The malware writes all the data it collects into a file in the current directory,
   so it will be in "C:\Program Files\Mozilla Maintenance Service\\\<hostname\>"
