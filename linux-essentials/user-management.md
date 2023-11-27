---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# User management

## Syntax

<pre class="language-sh"><code class="lang-sh">useradd -m -s /bin/bash UserX           #Creates user, -m gives home directory and -s gives default shell
passwd UserX                            #Gives password to a user  ?To not give it by insecure plaintext, we first create the user, then use the passwd command on the new user
chage -M n UserX                        #Makes the password expire in n days for UserX
chage -l UserX                          #Shows password caducity configuration
chage -d 0 UserX                        #Forces to change password in next login
chage -m n UserX                        #Configures n days before user can change password again
<strong>adduser UserX                           #Creates user but step by step, easier
</strong>id UserX                                #Prints user ID and GID (Group ID)
su UserY                                #Changes user
deluser --remove-home UserX             #Removes user and files in home directory
userdel -r UserX                        #Removes user and files in home directory
cat /etc/group                          #File with all groups and its users in the system
getent group GroupX                     #Shows users in a group
getent group sudo                       #Check users with sudo privileges
usermod -aG GroupX UserX                #Adds user to a group
sudo usermod -aG sudo UserX             #Give sudo privileges to a user
gpasswd -d UserX GroupX                 #Removes user from group
usermod -G GroupY,GroupZ UserX          #Overwrites user to specified groups and removes it from others not listed  ?For example if he was on GroupX he would be removed and added to GroupY and GroupZ
</code></pre>



## Configure password policies&#x20;

{% hint style="warning" %}
Using `sudo`will ignore all policies
{% endhint %}

```bash
#libpam-pwquality configuration file
sudo nano /etc/security/pwquality.conf         #Configuration file for pass polcies
minlen = n                                     #Minimum lenght for new passwords
ucredit = -n                                   #Minimum upper characters  ?ucredit = -2 would mean at least 2
dcreit = -n                                    #Minimum digits 
ocredit = -n                                   #Minimum others/symbols
badwords = P@ssword Passw0rd iloveyou          #Prohibits specified passwords
remember = n                                   #Prohibits using N last passwords used


```
