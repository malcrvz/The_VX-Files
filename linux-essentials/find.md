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
#Structure
find /dir_to_search -options item
find /multiple /directories /possible -options item
find /dir_to_search -options -options -options... item
find /dir_to_search ! -negatedOption objective
find /dir_to_search -globalOptions -options objective

#Types
find / -type f                         #Prints all files in the specified directory
find / -type d                         #Prints all directories in the specified directory
find / -type l                         #Prints all symbolic links in the specified directory

#Names
find . -name file.txt                  #Prints files with matching name "file.txt" in current directory
find / -name *.pdf                     #Prints files with matching name using regex
find / -iname FiLe                     #Prints files ignoring case

#Links
find / -links +1                       #Prints all items with more than 1 hard link

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

#User
find / -user user1                     #Prints items owned by specified user
find / ! -user root                    #Prints not owned by specified user
find / -group group1                   #Prints items owned by specified group

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


```
{% endcode %}



{% code title="Useful examples" %}
```bash
#Find the top 10 largest files on a system
sudo find / -type f -exec du -h {} + | sort -rh | head -n 10 2> /dev/null

#Find all .txt files of a user in the whole system
find / -user user1 -iname *.txt

#Find all files with 777 privileges on the system
find / -type f -perm 777

#Find all files not owned by root in which (o)thers can read and execute
find / -type f ! -user root -perm -o=rx



```
{% endcode %}
