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

# find

Your ally once exploited a system, will help you find sensible data and escalate privileges, getting comfortable with it will save you tons of time! \
"find" is not based on indexes, instead scans the system sequentially so it may be slower but with enough privileges will let you find anything with very specific or flexible options.

{% hint style="info" %}
Send the output of `find` to `less` or `grep` for easy reading and parsing.

`find x | less | grep x`
{% endhint %}

{% hint style="info" %}
When searching, you may encounter a spam of errors from inaccesible directories, if you don't want to see or they are too spammy, send the STDERR (standard error) to the black hole `/dev/null` with `find x 2> /dev/null`
{% endhint %}

{% hint style="info" %}
If searching through a symbolic link of a directory remember to place an extra "/" at the end, for example `find /bin/` referencing in some systems like Kali `/bin -> /usr/bin`
{% endhint %}

{% code title="Syntax" %}
```bash
#Basic structure
find /dir_to_search -options target
find /multiple /dirs /possible -options target
find /dir -options -options -options... target
find /dir ! -negatedOption target
find /dir -globalOptions -options target
find /dir -not -option reversedTarget

#Types
find / -type f                         #Prints all files in the specified directory
find / -type d                         #Prints all directories in the specified directory
find / -type l                         #Prints all symbolic links in the specified directory

#Names
find . -name file.txt                  #Prints files with matching name "file.txt" in current directory
find / -name *.pdf                     #Prints files with matching name using regex
find / -iname FiLe                     #Prints files ignoring case
find / \( -name "a" -o -name "b" \)    #Prints files either named "a" OR "b"
find / -not -name abc                  #Prints all files NOT named "abc"

#Links
find / -links +1                       #Prints all items with more than 1 hard link
find / -inum n                         #Prints items with exact inode number, can also be +n or -n for greater or lower than

#Size
find / -size +4k                       #Print files larger than 4 Kibibytes (KiB)
find / -size -100M                     #Print files smaller than 100 Mebibytes (MiB)
find / -size 1G                        #Prints files exactly 1 Gibibytes (GiB)
find / -size +100c -size -1M           #Prints files between 100 Bytes and 1MiB
#Each size unit rounds up, narrow down the unit when looking for precise results
#Or just always search by Bytes so you never miss files:
find / -size -1048576c                 #Prints files smaller than 1 MiB
find / -size -1M                       #Prints empty files
find / -size -2M                       #Prints all files smaller than 2 MiB, including KiB and Bytes
find / -empty                          #Prints only empty items

#User
find / -user user1                     #Prints items owned by specified user
find / ! -user root                    #Prints not owned by specified user
find / -group group1                   #Prints items owned by specified group
find / -uid 1000                       #Prints items owned by specified userID
find / -gid 1000                       #Prints items owned by specified groupID

#Privileges
#If you don't add "-type f" or "d" you will get links too, making it messy
find / -perm 777                       #Prints files/dirs with EXACT specified privileges
find / -perm u=r                       #Prints items with EXACT privileges, 400 in the example
find / -perm -u=rx                     #Prints items which (u)ser can read AND e(x)ecute, ignores the other privileges
find / -perm /u=rx                     #Prints items which (u)ser can read OR e(x)ecute, ignores the other privileges             
find / -perm -a=rw                     #Prints items which (a)ll can read AND (w)rite, ignores the other privileges
find / -perm /u=s                      #Prints items with setuid privileges
find / -perm /g=s                      #Prints items with setgid privileges

#Depth
find / -maxdepth n                     #Searches in n maximum depth of subdirectories  ?"-maxdepth 1" will search on current and 1 level of subdirs only
find / -mindepth n                     #Searches after n depth of subdirectories  ?"-mindepth 1" will ignore current directory and search on lower levels
find / -mindepth n -maxdepth n         #Searches between range specified, starts at minimum n depth until max n depth

#Date and time
#+n = more than n ago | -n = less than n ago | n = exact, rounded in days, minutes...
#Two time options: time = days || min = minutes
#(a)ccessed, (m)odified, (c)hanged status/metadata(privileges)
find / -atime 0                        #Prints items (a)ccessed between now and 24 hours ago  ?"-atime -1" prints the same
find / -atime +0                       #Prints items accessed more than 24h ago
find / -atime 1                        #Prints items accesed between 24h and 48h ago
find / -amin +1                        #Prints items accesed more than 1 minutes ago
find / -amin -60                       #Prints items accesed last 60 minutes
find / -atime +50 -atime -100          #Prints items accessed between 50 and 100 days ago
find / -mtime n                        #Prints items (m)odified n ago
find / -ctime n                        #Prints items with its status/metadata(privileges) (c)hanged n ago

find / -newer file1                    #Prints items modified more recently than file1
find / -newerat 1990-01-01             #Prints items accessed after the 1 of january of 1990, "t" specifies we will pass a date
find / -newermt 1990-01-01             #Prints items modified after the 1 of january of 1990
find / -newerct 1990-01-01             #Prints items which inode status changed after the 1 of january of 1990

#Find and execute commands
find / -exec command {} \;             #Executes x command on each file found
#Think of {} like a for loop, one by one each file will pass through {} and get the command done
#Example: -exec cp {} /newDir \;
#"\;" to escape the ";" that indicates the end of the command

find / -exec sh -c "command {} ; command {}" \; #Executes multiple commands on each file found
#Example uses "sh" but you can use whatever shell you want, bash, zsh...

find / -exec command {} + | command | command
#If you are going to pipeline the output, after -exec place "+" instead of "\;"
```
{% endcode %}

{% hint style="danger" %}
Be very careful, `find` can be a weapon of mass destruction! \
Never use on a production system unless you REALLY know what you are doing.
{% endhint %}

{% code title="Useful examples" %}
```bash
#Finds the top 10 largest files on a system
sudo find / -type f -exec du -h {} + | sort -rh | head -n 10 2> /dev/null

#Finds all .txt files of a user in the whole system
find / -user user1 -iname *.txt

#Finds all files with 777 privileges on the system
find / -type f -perm 777

#Finds all files not owned by root in which (o)thers can read and execute
find / -type f ! -user root -perm -o=rx

#Finds and deletes all .log files from the system
sudo find / -type f -name *.log -exec rm -f {} \;

#Searches all system, excluding specified dir for files with full privileges on others
find / -path /proc -prune -o -type f -perm -o=rwx

#Searches all system, excluding specified dirs for files with others=rwx
find / \(-path /proc -o -path /sys \) -prune -o -type f -perm -o=rwx

#Searches for files with modified status or privileges in the last 7 days and ls them
find / -type f -ctime -7 -exec ls -l {} \;

#Finds all files with .bak extension and counts how many there are
find / -type f -name *.bak | wc -l

```
{% endcode %}

### Other searching commands

<pre class="language-bash" data-title="Syntax"><code class="lang-bash"><strong>which command       #Finds location of binaries in the $PATH
</strong>whereis command     #Finds location of binaries, its man page and source code in the whole system
<strong>locate item         #Searches matching string through a pregenerated database on a periodic cronjob, simple but quick
</strong><strong>
</strong></code></pre>
