![image](https://github.com/user-attachments/assets/55b7ea1b-ab2b-4343-afb9-f2a187c4661c)
SeImpersonate and SeAssignPrimaryToken - JuicyPotato Escalation
PrintSpoofer and RoguePotato
JuicyPotato doesn't work on Windows Server 2019 and Windows 10 build 1809 onwards. However, PrintSpoofer and RoguePotato can be used to leverage the same privileges and gain NT AUTHORITY\SYSTEM level access. This blog post goes in-depth on the PrintSpoofer tool, which can be used to abuse impersonation privileges on Windows 10 and Server 2019 hosts where JuicyPotato no longer works.


https://github.com/geeksniper/windows-privilege-escalation?tab=readme-ov-file#eop---impersonation-privileges

It is also worth searching for weak service ACLs in the Windows Registry. We can do this using accesschk.
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
Modifiable Registry Autorun Binary
Check Startup Programs
We can use WMIC to see what programs run at system startup. Suppose we have write permissions to the registry for a given binary or can overwrite a binary listed. In that case, we may be able to escalate privileges to another user the next time that the user logs in.
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User |fl

general line of tought: 
1. what are the user privileges?
2. what services are running ? (how to enumerate services with usefull info)
   dll injfection
   unqoted service path
   overwrite service binary
   
4. is it install elevated
5. what are the schudled tasks.?
   writable executable files ?
   dependency writable ?
7. sensitive files readable ? 
