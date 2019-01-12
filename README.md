# Wando Team 2oe Linux Checklist

## Checklist
1. Read the readme

      Take notes on neccessary services, users, and any other important information.
      
1. Do the Forensics Questions

      Forensics questions can point you towards other vulnerabilities. Keep this in mind. (ex: find a media file, find a hidden message, find a backdoor, etc)
      
1. Account Configuration
      1. Lock the root account
      
      `$ sudo passwd -l root`
      1. Disable the guest account in `/etc/lightdm/lightdm.conf`
      
      ```
      allow-guest=false
      greeter-hide-users=true
      greeter-show-manual-login=true
      autologin-user=none
      ```
      1. Compare `/etc/passwd` and `/etc/group` to the readme
      
      Look out for uid 0 and hidden users!
      1. Delete unauthorized users:
      
      `$ sudo userdel -r $user`
      
      `$ sudo groupdel $user`
      1. Add users:
      
      `$ sudo useradd -G $group1,$group2 $user'
      
      `$ sudo passwd $user`
      1. Remove unauthorized users from adm and sudo groups:
      
      `$ sudo gpasswd -d $user $group`
      1. Add authorized users to groups:
      
      `$sudo gpasswd -a $user $group`
      1. Check `/etc/sudoers` and `/etc/sudoers.d` for unauthorized users and groups.
            1. Remove any instances of `nopasswd`
            1. Any commands listed can be run without a password (ex: /bin/chmod)
            1. Group lines are preceded by `%`
1. Password Policy
      1. Change password expiration requirements in `/etc/login.defs`:
      
      ```
      PASS_MAX_DAYS 30
      PASS_MIN_DAYS 7
      PASS_WARN_AGE 12
      ```
      1. Add password history, minimum password length, and password complexity requirements in `/etc/pam.d/common-password`
      
      **INSTALL CRACKLIB PRIOR TO CHANGING COMMON-PASSWORD**
      
      `$ sudo apt-get install libpam-cracklib`
      
      ```
      password  required  pam_unix.so  obscure sha512 remember=12 use_authtok
      password  required  pam_cracklib.so  retry=3 minlen=13 difok=4 dcredit=-1 ucredit=-1 ocredit=-1 lcredit=-1 maxrepeat=3
      ```
      1. Enforce account lockout policy in `/etc/pam.d/common-auth`:
      
      **MUST COME FIRST**
      
      `auth   required    pam_tally2.so deny=5 audit unlock_time=1800 onerr=fail even_deny_root`
      1. Change account expiry defaults in `/etc/default/useradd`:
      
      ```
      EXPIRE=30
      INACTIVE=30
      ```
      1. Check minimum and maximum password ages in `/etc/shadow`
      
      Use `chage` to change password expiration.
      
      `$ chage -m $MIN -M $MAX $user`
      1. **CHANGE PASSWORDS---YOU WILL BE LOCKED OUT IF YOU DON'T!**
      
      Be sure to record new user passwords!
      
      `passwd $user`
1. Enable automatic updates

Update Manager -> Settings - > Updates
1. Find and delete media files

`$ sudo find / -iname "*.$extension"`
