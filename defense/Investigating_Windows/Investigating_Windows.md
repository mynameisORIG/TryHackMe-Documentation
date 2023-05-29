# task 1 - Investigating Windows

1. Whats the version and year of the windows machine?

Windows Server 2016

**Explanation**: Used command prompt and types `systeminfo`

2. Which user logged in last?

Administrator

**Explanation**: Went into event viewer than selected Windows Logs -> Security. There I searched for the last user. Could also do `net user`, and then type `net user <user>` where the second user is a username you found.

3. When did John log onto the system last?

03/02/2019 5:48:32 PM

**Explanation**: `net user John`

4. What IP does the system connect to when it first starts?

10.34.2.3

**Explanation**: Open up regedit. Then go to HKEY_LOCAL_MACHINE -> Software -> Microsoft -> Windows -> CurrentVersion -> Run

5. What two accounts had administrative privileges (other than the Administrator)

Jenny, Guest

**Explanation**:Do `net user`, and then type `net user <user>` where the second user is a username you found.

6. Whats the name of the scheduled task that is malicious.

clean file system

**Explanation** Open *task scheduler* and click on *Task Scheduler Library*. There you can find scheduled tasks.

7. What file was the task trying to run daily?

nc.ps1

**explanation**:After following explanation in question 6, go to *Actions* subpannel.

8. What port did this file listen locally for?

03/02/2019

**explanation**:After following explanation in question 6, go to *Actions* subpannel.

9. When did Jenny last logon?

Never

**Explanation**:Do `net user`, and then type `net user <user>` where the second user is a username you found.

10. At what date did the compromise take place?

03/02/2019

11. During the compromise, at what time did Windows first assign special privileges to a new logon?

03/02/2019 4:04:49 PM

12. What tool was used to get Windows passwords?

Mimikatz

**explanation**: This was found through task scheduler

13. What was the attackers external control and command servers IP?

76.32.97.132

**explanation**:
```
type C:\Windows\System32\drivers\etc\hosts

76.32.97.132 google.com                                                                       76.32.97.132 www.google.com
```

14. What was the extension name of the shell uploaded via the servers website?

.jsp

**explanation**: `dir C:\inetpub\wwwroot`

15. What was the last port the attacker opened?

1337

16. Check for DNS poisoning, what site was targeted?

google.com

**explanation**:
```
type C:\Windows\System32\drivers\etc\hosts

76.32.97.132 google.com                                                                       76.32.97.132 www.google.com
```

