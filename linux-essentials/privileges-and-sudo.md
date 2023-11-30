# Privileges & sudo

## Privileges

Privileges are categorized in 3 kinds of users

* User(u): the user that created the item
* Group(g): users inside the group assigned to the item
* Others(o): rest of users. not owners of the item

They then grant 3 types of permission

* Read(r)
  * Files: allows user to see the contents of the file
  * Directories: allows the user to list the inside of the directory
* Write(w)
  * Files: allows user to modify the contents of the file
  * Directories: allows the user to create, delete or rename files inside the directory
* Execute(x)
  * Files: allows the use to execute the file
  * Directory: allows the user to access the directory (cd into it)

By default users have ownership of the item they create and the group assigned will be the main group of that user. User can change it to whatever groups is in.

If user is deleted file will still be property of the user, though it will show used UID and GUID of the late user.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>source: linuxcommand.org </p></figcaption></figure>

<pre class="language-bash" data-title="Syntax"><code class="lang-bash">#Numeric mode chart
7 == rwx
6 == rw-
5 == r-x
4 == r--
3 == -wx
2 == -w-
1 == --x
0 == ---

#Bits
r  w  x
2² 2¹ 2⁰ 
4  2  1

#Umask, to check default privileges when creating an item
umask


#Changing privileges with chmod
chmod 755 file1
chmod u+rwx,g+rx,o-w file1
chmod u=rwx,g=rx,o=r file1    #!Do not leave spaces between ","
chmod -R u+x directory/       #Changes the privileges recursively in the whole directory
<strong>chmod a-x file1               #Affects (a)ll, removes execution privileges, a = ugo
</strong><strong>
</strong><strong>#Changing ownership with chown
</strong><strong>sudo chown newOwner file1
</strong>sudo chown user:group file1         #You can change  user and group simultaneously 
sudo chown -R user:group directory/ #Changes the ownership recursively in the whole directory
chown :group file1                  #User can't change ownership of file without root but it can change group

#Changing group with chgrp
chgrp group file1
chgrp -R group directory/           #Changes the group recursively in the whole directory

#Stat, shows detailed info about file, included privileges
stat file1

</code></pre>

### **Special privileges**

When an executable(binarie) has special privileges "setuid" it means that whoever executes it, will do as if it was the owner of the binarie. This is used for example in `passwd` to let users change their own passwords and edit sensible files like `/etc/shadow` as root, but in a controlled way as they can only edit their password not others.

Is shown as `rws`, if it's capitalized (`rwS` ) means it has no executable privileges in the first place, making it useless. \


For groups we can use "setgid" and assign it to binaries and directories too, same as setuid, items assigned or created inside the folder (can also be recursive with -R) will be executed or created as the owner of the item.

Is shown as `s/S` same as setuid but in the group segment.

#### **Sticky Bit**

Its used to prevent users that are not the owner of the directory, owner of the file, or root, to delete a file from a designed directory.

Placed always on a directory, it will prohibit users -that are not the owner of a file- to delete anything inside the folder that's not theirs, meaning others could create or modify files in a more controlled and not so catastrophic way.

Useful for directories like `/tmp` where most users have access and deleting files from other users could be a problem.

Is shown at the end, in the "others" segment instead of "x", `rwt` if executable, `rwT` if not executable

{% hint style="warning" %}
Though useful, special permissions can easily become a vulnerability if not precisely configured, and a great vector for privilege scaling. Look for them when pentesting [;)](../baseline-pentesting/tools/6.-privilege-escalation-and-lateral-movement/possible-vectors.md)
{% endhint %}

{% code title="Syntax" %}
```bash
#Managing setuid
chmod u+s file1
chmod 4nnn file1
rwsr--r--

#Managing setgid
chmod g+s file1
chmod 2nnn file1
chmod -R g+s directory/
rwxrwsr--

#Managing sticky bit
chmod o+t directory/
chmod 1nnn directory/
chmod -R o+s directory/
rwxrwxrwt

```
{% endcode %}

### **UMASK**

(User Mask) By default items are created with a mask, usually 022, you can check it with `umask` (it will show an extra 0, **0**002, it just means its in octal, some languages like Perl or C need it).  \
But the starting privileges are different on directories and files, having directories more "flexible" privileges and files not being able to get executed by default for security reasons (scripts are dangerous!). \
Directories start at 777 and files at 666.

Directories: 777 - 022 = 755 (rwx,rx,rx)

Files: 666 - 022 = 644 (rw-,r,r)

You can change the umask temporary for a session with `umask nnn`&#x20;

If you want it to be permanent you have to edit your shell profile, for example in bash you have to add `umask nnn` to your `~/.bashrc` file and in zsh(used on Kali) you have to edit `~/.zshrc`

{% hint style="info" %}
You can check the shell you are currently using with `echo $0` or the one you have assigned by default to your user with `echo $SHELL`&#x20;
{% endhint %}

To change the umask of all users in the system you have to add the line to `/etc/profile` and restart, though it will probably be over-passed by specific user profiles.

Some distros use `/etc/pam.d/common-session` or `/etc/login.defs`&#x20;

### FACL

(File Access control List) They a are more advanced mechanism that standard privileges, with FACL we can define specific and detailed permissions to individual files for individual users or groups in a finer way. Use in combination with standard privileges for flexible management.\
They are represented as a "+" sign at the end of the privileges string in `ls`.

<pre class="language-bash" data-title="Syntax"><code class="lang-bash">setfacl -m u:user1:rwx file1      #Gives "rwx" privileges to user1
getfacl file1                     #Shows privileges and ACLs on specified file
setfacl -R -m u:user1:rx dir1     #Gives "rx" privileges to specified user on specified directory and everything inside, recursively, but future items will not be included
setfacl -d                        #Makes the specified ACL the default on a directory and future new items inside
setfacl -k                        #Remove default specified ACLs from a directory
setfacl -R -m g:group1:rwX dir1   #Using "X" capitalized means it will give recursive "x" privileges only to directories inside dir1 and to dir1
<strong>setfacl -x u:user1 file1          #Removes specified user from the ACL list
</strong>setfacl -b file1                  #Removes all ACLs from specified file or directory
getfacl file1 | setfacl --set-file=- file2    #Copy ACL of one file to other  !Use regex for multiple files

</code></pre>

{% hint style="warning" %}
Useful, but they can be complex and conflict with standard privileges, so they amplify the _human factor_ hackers love. Look for them trying to scale privileges [;)](../baseline-pentesting/tools/6.-privilege-escalation-and-lateral-movement/possible-vectors.md)
{% endhint %}

***

## SUDO

Allows us to execute commands as another use, most commonly as the superuser, known as root.

Basically lets low privileged users execute critical commands without giving them access to root.

Normally login as root is disabled, so to be the administrator of a computer you have to be in the sudo group or have (ALL : ALL) ALL privileges in the /etc/sudoers file.

{% code title="Syntax" %}
```bash
sudo command                    #Execute a command as root
sudo -u UserX command           #Execute a command as UserX
sudo -g GroupX command          #Execute a command as GroupX
sudo -l                         #Lists what sudo privileges current user has
sudo su -                       #Executes "su" command as sudo, if we have that command privilege we become root  !This is why (ALL:ALL) ALL is dangerous!
sudo -k                         #For convenience sudo will keep credentials for a small period of time, but for security, if you finished already, you can stop the timer with -k
sudo !!                         #Very useful when you forget to use sudo, "!!" will repeat the last command, but we place sudo this time ;)

```
{% endcode %}

***

## sudoers file

Located on `/etc/sudoers`, Is the configuration file that determines which users or groups are allowed to execute which commands with elevated privileges or in other words with no restriction.

Its strongly recommended to edit it with the command `visudo` as it creates a temporal copy, sudoers.tmp, and checks for syntax errors before merging it with the original file.

{% code title="Syntax" %}
```bash
#Structure
user    host=(user:group) commands   

#Parameters
%group                               #Specifies group 
NOPASSWD:                            #Doesnt need password to execute command
PASSWD:                              #Asks for password, default, but can be used in combination with NOPASSWD
NOEXEC:                              #For security, it lets a user use a command, but restrains the command from executing another commands
ALL                                  #Wildcard for everything, same as *
!command                             #Prohibits that specific command
/directory/                          #Will be able to execute everything inside specified directory

#Defaults
root    ALL=(ALL:ALL) ALL        #The user root can do anything
%sudo   ALL=(ALL:ALL) ALL        #The users in the group sudo can do anything, this is where we place our main user in a personal computer, or sysadmins in a company

#Examples
UserX    ALL=(ALL:ALL) ALL                           #UserX can execute all commands, in all hosts, as all users, as all groups  !This would be the sysadmin if you dont use %sudo
%GroupX  ALL=(ALL:ALL) ALL                           #GroupX, so all its members, can execute all commands, in all hosts, as all users, as all groups
UserX    ALL=(ALL:ALL) ALL, !/bin/                   #UserX can execute all commands, in all hosts, as all users, as all groups, EXCEPT the commands inside /bin/
%GroupX  ALL=(ALL:ALL) /bin/ !/bin/rm                #GroupX, so all its members, can execute, in all hosts, as all users, as all groups, the commands inside /bin/ except "rm"
UserX    server1=(root) /usr/bin/apt                 #UserX can execute "apt" on host "server1" with root privileges
UserX    (server1, server2)=(Bob:Devs) /usr/bin/find #UserX can execute "find" on "server1" and "server2" with the privileges that Bob and the group Devs have on "find"
UserX    ALL=(ALL) NOEXEC: /usr/bin/less             #UserX can execute "less" as privileged, but will restrict "less" from executing another commands, preventing the exploit of executing commands from the "less" interface with "less" privileges 
%GroupX  ALL=(ALL:ALL) NOPASSWD: ALL                 #GroupX can execute all commands, on all hosts, as all users, as all groups, and it will not be asked for a password  !Extremely insecure!
UserX    ALL=(ALL:ALL) NOPASSWD:ALL, PASSWD:/bin/rm  #UserX can execute everything without password, except for "rm"
UserX    ALL=(ALL:ALL) /usr/bin/cat                  #UserX can execute "cat" command in every host, as all users, as all groups
UserX    server1=(operator) /bin/ls, (root) /bin/kill, /usr/bin/cat #UserX, on server1, can execute "ls" as operator, and "kill" and "cat" as root

```
{% endcode %}

```bash
#Alias with examples
*_Alias    ALIAS=(ALIAS:ALIAS) ALIAS                 #Syntax

User_Alias    GROUP1 = Lin, Amelia, Zack             #Places users under the alias GROUP1
GROUP1    ALL=(ALL:ALL) /usr/bin/apt                 #Now all 3 users can use apt privileged

Cmd_Alias    POWER = /sbin/shutdown, /sbin/reboot    #Places "shutdown" and "reboot" commands under the alias POWER
GROUP1       ALL=(ALL:ALL) POWER                     #All users inside GROUP1 can use all the commands inside POWER with privileges

Runas_Alias  WEB = www-data, apache, BobWebDeveloper #When we place WEB in the "run as" syntax, the user will have the privileges of all this group of users   
GROUP1       ALL= (WEB) ALL                          #All users inside alias GROUP1 will have the privileges of all the users in alias WEB

Host_Alias    SERVERS= server1, dhcp, DNS4           #Places hosts under the alias SERVERS
Zack          SERVERS=(ALL:ALL) POWER                #Now the user Zack can use the commands inside POWER as privileged used in all the hosts inside the alias SERVERS

```
