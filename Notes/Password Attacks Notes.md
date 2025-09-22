Kerberoasting:
![image](https://github.com/user-attachments/assets/f6ac63b1-714b-4352-baf3-b1517cb22ab4)

![image](https://github.com/user-attachments/assets/7be6c3a7-cecf-4e99-9a39-d80d0a5f5513)

## **Attaacking SAM LSASS and NTDS**:

Concenpt:
### **SAM**
Local interactive logon is handled through the coordination of several components: the logon process (WinLogon), the logon user interface process (LogonUI), credential providers, the Local Security Authority Subsystem Service (LSASS), one or more authentication packages, and either the Security Accounts Manager (SAM) or Active Directory. Authentication packages, in this context, are Dynamic-Link Libraries (DLLs) responsible for performing authentication checks. For example, for non-domain-joined and interactive logins, the Msv1_0.dll authentication package is typically used.


### **LSASS**
The Local Security Authority Subsystem Service (LSASS) is comprised of multiple modules and governs all authentication processes. Located at %SystemRoot%\System32\Lsass.exein the file system, it is responsible for enforcing the local security policy, authenticating users, and forwarding security audit logs to the Event Log. In essence, LSASS serves as the gatekeeper in Windows-based operating systems. A more detailed illustration of the LSASS architecture can be found here.

### Authentication Packages	Description

| DLL| Funciton |
|----------|----------|
| Lsasrv.dll |	The LSA Server service both enforces security policies and acts as the security package manager for the LSA. The LSA contains the Negotiate function, which selects either the NTLM or  Kerberos protocol after determining which protocol is to be successful. |
| Msv1_0.dll |	Authentication package for local machine logons that don't require custom authentication. |
| Samsrv.dll |	The Security Accounts Manager (SAM) stores local security accounts, enforces locally stored policies, and supports APIs. |
| Kerberos.dll |	Security package loaded by the LSA for Kerberos-based authentication on a machine. |
| Netlogon.dll |	Network-based logon service. |
| Ntdsa.dll |	This library is used to create new records and folders in the Windows registry |


### **Credential manager**

- %UserProfile%\AppData\Local\Microsoft\Vault\
- %UserProfile%\AppData\Local\Microsoft\Credentials\
- %UserProfile%\AppData\Roaming\Microsoft\Vault\
- %ProgramData%\Microsoft\Vault\
- %SystemRoot%\System32\config\systemprofile\AppData\Roaming\Microsoft\Vault\
Each vault folder contains a Policy.vpol file with AES keys (AES-128 or AES-256) that is protected by DPAPI. These AES keys are used to encrypt the credentials. Newer versions of Windows make use of Credential Guard to further protect the DPAPI master keys by storing them in secured memory enclaves (Virtualization-based Security).

It is possible to export Windows Vaults to .crd files either via Control Panel or with the following command. Backups created this way are encrypted with a password supplied by the user, and can be imported on other Windows systems.

<img width="775" height="98" alt="image" src="https://github.com/user-attachments/assets/3b5275b2-a19d-4e5a-8dab-fb5b6ba4467b" />

**C:\Users\sadams>rundll32 keymgr.dll,KRShowKeyMgr**

## Linux 

Linux-based distributions support various authentication mechanisms. One of the most commonly used is Pluggable Authentication Modules (PAM). The modules responsible for this functionality, such as pam_unix.so or pam_unix2.so, are typically located in /usr/lib/x86_64-linux-gnu/security/ on Debian-based systems. These modules manage user information, authentication, sessions, and password changes. For example, when a user changes their password using the passwd command, PAM is invoked, which takes the appropriate precautions to handle and store the information accordingly.

The pam_unix.so module uses standardized API calls from system libraries to update account information. The primary files it reads from and writes to are /etc/passwd and /etc/shadow. PAM also includes many other service modules, such as those for LDAP, mount operations, and Kerberos authentication.

#### Cracking Linux Credentials
Once we have root access on a Linux machine, we can gather user password hashes and attempt to crack them using various methods to recover the plaintext passwords. To do this, we can use a tool called unshadow, which is included with John the Ripper (JtR). It works by combining the passwd and shadow files into a single file suitable for cracking.
