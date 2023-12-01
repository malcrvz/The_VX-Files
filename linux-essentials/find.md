# find

Your ally once exploited a system, will help you find sensible data and escalate privileges, getting comfortable with it will save you tons of time! \
"find" is not based on indexes, instead scans the system sequentially so it may be slower but with enough privileges will let you find anything with very specific or flexible options.

{% hint style="info" %}
When searching, you may encounter a spam of errors from inaccesible directories, if you don't want to see or they are too spammy, send the STDERR (standard error) to the black hole `/dev/null` with `find x 2> /dev/null`
{% endhint %}

{% hint style="info" %}
Send the output of `find` to `less` or `grep` for easy reading and parsing.

`find x | less | grep x`
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

```
{% endcode %}



{% code title="Useful examples" %}
```bash
#Find the top 10 largest files on a system
sudo find / -type f -exec du -h {} + | sort -rh | head -n 10 2> /dev/null

```
{% endcode %}
