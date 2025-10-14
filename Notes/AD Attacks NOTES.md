![image](https://github.com/user-attachments/assets/188f3428-eafc-479d-8608-003f2f109e94)

In this module, we will cover enumerating and leveraging four specific ACEs to highlight the power of ACL attacks:

 **ForceChangePassword** gives us the right to reset a user's password without first knowing their password (should be used cautiously and typically best to consult our client before resetting passwords).

**GenericWrite** - gives us the right to write to any non-protected attribute on an object. If we have this access over a user, we could assign them an SPN and perform a Kerberoasting attack (which relies on the target account having a weak password set). Over a group means we could add ourselves or another security principal to a given group. Finally, if we have this access over a computer object, we could perform a resource-based constrained delegation attack which is outside the scope of this module.

**AddSelf** - shows security groups that a user can add themselves to.

**GenericAll** - this grants us full control over a target object. Again, depending on if this is granted over a user or group, we could modify group membership, force change a password, or perform a targeted Kerberoasting attack. If we have this access over a computer object and the Local Administrator Password Solution (LAPS) is in use in the environment, we can read the LAPS password and gain local admin access to the machine which may aid us in lateral movement or privilege escalation in the domain if we can obtain privileged controls or gain some sort of privileged access.

*** Get-ADObject -Filter 'isDeleted -eq $true' -IncludeDeletedObjects *** - check deleted items in ad 

# Create PS Credential Object
PS C:\htb> $SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
PS C:\htb> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)

SyncMaster757
https://github.com/NetSPI/PowerUpSQL/wiki/PowerUpSQL-Cheat-Sheet
https://github.com/dirkjanm/adidnsdump

# Domain Trust

# Rev Shell
ReverseShell wraping using iconv and base64:
<img width="1280" height="811" alt="image" src="https://github.com/user-attachments/assets/4e9f697c-205b-41ed-a966-7d9e00eccd97" />

https://github.com/antonioCoco/RunasCs/blob/master/compile_commands.txt 
<img width="1016" height="391" alt="image" src="https://github.com/user-attachments/assets/2a74594d-78b8-43eb-8f3d-cd25031b9026" />

## SQL enumeration cherry
From GHOST

```
select * from openquery([PRIMARY], 'select host_name()')
select * from openquery([PRIMARY], 'exec xp_cmdshell "whoami"') 
### select raw permissions with ids instead of usernames
select * from openquery([PRIMARY], 'select * from sys.server_permissions')
### Select user permissions with usernames replaced id
select * from openquery([PRIMARY], 'select grantee.name, perm.permission_name from sys.server_permissions perm JOIN sys.server_principals AS grantee on grantee.principal_id = perm.grantee_principal_id') 
```
