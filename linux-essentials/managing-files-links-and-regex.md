---
description: These are basic common-use, for more check the holy man!
---

# Managing files, links and regex

## File managing and text processing commands

{% tabs %}
{% tab title="echo" %}
```bash
echo string                     #Prints "string"
echo string > file1             #Overwrites or creates file1 with string 
echo string >> file1            #Creates file1 and/or appends string at the end of file1
echo $PATH                      #Prints contents of variable/environment variable
echo $(command)                 #Prints output of whatever command
echo $(date)                    #Print current date
echo *                          #Prints the name of all items in the current directory
echo */                         #Prints only the name of all directories in the current directory
echo *.txt                      #Prints all items in the current directory ending in .txt
echo \*                         #Escapes and prints literal "*"
echo a \\n b                    #Prints line feed between "a" and "b"
echo a \\c b                    #Produces no further output after \c, so "b" is not printed
echo a \\t b                    #Prints a TAB between "a" and "b"
echo a \\v b                    #Prints a vertical TAB between "a" and "b"
echo 12\\b3                     #Backspaces character just before \\b, will print 13
commandX && echo "Done"         #Will only print "Done" if "commandX" is succesful
commandX || echo "Error"        #Will print "Error" if "commandX" is unsuccesful
echo "\033[1;31mThis is red"    #Prints string after "m" in red color
echo "\033[5;31mFlashing red"   #Prints string in red that will blink constantly
#\033 is for Escape, some colors: [1;33mOrange, [1,34mBlue, [1;35mPurple


```
{% endtab %}

{% tab title="cat" %}
<pre class="language-bash"><code class="lang-bash">#Concatenation
# ">" overwrites | ">>" appends  | "-" standard input
cat file1                        #Prints content of file1
cat file1 file2                  #Prints content of file1 and file2 appended
cat file1 file2 > combined       #Combines the contents of file1 and file2 into a new file
cat file2 file3 >> file1         #Appends the contents of file2 and file3 on file1
cat >file.txt                    #Creates or overwrites "file.txt" and asks for input until we exit with ctrl + D, if it already exists the content is overwritten
cat >>file.txt                   #Creates "file.txt" if it doesnt exists or appends input into the existing file
cat file2 - >>file1.txt          #Creates "file1.txt" if it doesnt exists and appends contents of "file2.txt" and then the input on the file until we exit with ctrl + D
cat - file1 - file2 >file3       #Creates or overwrites "file3" with standard input until quitting and saving with ctrl +D, then file1, then standard input again, then file2
cat file1 - file2 &#x3C;  file3       #Prints file1, then file3 because its the standard input we are passing through the &#x3C;, and then file2
#Parameters
cat -n file1                     #"number" Show all line numbers
cat -s file1                     #"squeeze" Display multiple successive empty lines as one
cat -E file1                     #"ends" Show end of lines as $ character
cat -b file1                     #"blank" Shows line numbers only on non-blank lines
cat -T file1                     #"show-tabs" Show TAB characters as ^I
cat -v file1                     #"non-printing" Use ^ and M- notation for special characters

<strong>nl file1                         #Prints contents of file1 with numbered lines, same as cat -n
</strong><strong>
</strong><strong>
</strong></code></pre>
{% endtab %}

{% tab title="sort" %}
<pre class="language-bash"><code class="lang-bash">#Default order 1. Symbols 2. Digits 3. Uppercase 4. Lowercase 
<strong>sort file1                 #Prints contents of file1 in order by first character of the line and if repeated consecutive
</strong>sort -d file1              #Considers only alphanumeric characters and blanks
sort -f file1              #Ignores upper/lower distinction 
sort -g file1              #Sorts numbers by numeric value and places them last in the order list
sort -i file1              #Consider only printable characters
sort -M file1              #Sort by months, format "JAN, DEC, OCT" or full name, english only
sort -h file1              #Sort by human readable numbers, "200K &#x3C; 30M &#x3C; 1G"
sort -R file1              #Sort at random but keep identical values grouped
sort -r file1              #Reverse the result 
sort -V file1              #Sorts alphanumeric and natural numbers (4 > 20)
sort file1 -o output       #Do not print, write output into file
sort -k n file1            #Sort by n field, separated by space by default
sort -k n,m file1          #Sorts by range of fields in n-m, also can be same numer ex: 2,2 to considerate only that field
sort -t : -k n file1       #-t specifies what is the field separator, instead of default space
sort -u file1              #Unique, sorts and only prints 1 ocurrence of repeated lines


</code></pre>
{% endtab %}

{% tab title="less" %}
```bash
#Navigation
q / ZZ                   #Exit the less pager
Space / Esc              #Move one windowforward
b                        #Move one window backwards
j / Enter / Down Arrow   #Move one line forward
k / Up Arrow             #Move one line backwards
10j / 15j / 300j         #Move N lines forward
10k / 17k / 400k         #Move N lines backward
d                        #Move half window forward
u                        #Move hald window backwards
G                        #Go to the end of the file
g / %                    #Go to the start of the file
50% / 50p                #Go to N% of the file
100P                     #Go to the line containing the N byte,

#Search
/word                    #Finds the next match of "word" and highlights the pattern
?word                    #Finds the previous match of "word" and highlights the pattern
n                        #Moves to next match forward
N                        #Moves to previous match backwards
-i                       #Will toggle on and off case sensivity for searches, active by default

#Others
F                        #"Follow", goes to the end of the file and keeps printing what's added in real time  !Useful for monitoring logs
m                        #Will ask for a letter and will mark the position with it
'                        #Will ask for a letter and wil go to the position that was marked with it
&example                 #Displays only lines that contain "example"
v                        #Opens the file with your default text editor
ctrl+G                   #Shows file location, lines, size and percentage statistics


```
{% endtab %}

{% tab title="split" %}
```bash
#Structure
split file1                    #Output file into pieces, default size 1000 lines and prefix "x"  $One file of 2000 lines would be divided into "xaa" "xab"
split file1 Prefix             #Output file into default pieces but prefix will be "Prefix" instead of "x"

#Suffix modification
split -d file1                 #Generates digit suffixes, starting 00
split -x file1                 #Generates hexadecimal suffixes, starting 00
split -a n file1               #Generates suffix of lenght n  $Suffix 4 would be "xaaaa" "xaaab"
split --additional-suffix=X    #Appends additional suffix, "X" to each file.

#Splitting methods
split -l n file1               #Splits by n number of lines
split -b nKB #K,M,G - KB,MB,GB #Splits by making each piece n bytes, can be Kibibytes, Mebibytes, Gigibytes(powers of 1024) or KyloBytes, MegaBytes... etc until Yottabyte(YB)
split -n n file1               #Splits file by n number of equal parts

#csplit, split by patterns inside the file
csplit file1 "/pattern/" "{n}"  #/pattern/ it's the separator, {n} the number of times we want to separate
"{*}"                           #Separate as many times as possible
csplit -f Prefix                #Uses prefix specified instead of "x"
csplit --suppress-matched       #Suppresses pattern used as separator 
csplit --elide-empty-files      #Remove empty output files
csplit -s                       #Do not print output with counts of file sizes
csplit -n n                     #Generate n number of suffix digits instead of 2

#Join
cat prefix* > joinedFile       #Join all pieces into a full file again


```
{% endtab %}

{% tab title="cut" %}
```bash
#Cutting by fields
cut -fn file1                  #Prints the n Field of a file separated by TABs
cut -f1,3 file1                #Prints the 1 and the 3 field of a file separated by TABs
cut -fn-                       #Prints from n field to the end of the file, "-" indicates until the end
cut -d ""                      #Specifies which character cut will use as a Delimiter
cut -d " " -f1 file            #Prints the 1 field of a file separated by espaces
cut -d ":" -f1,7 /etc/passwd   #Prints username & shell of all users in system
cut -d ":" -f1,100 /etc/passwd #Because field 100 does not exist will print only field 1
cut -d "," -f5- file.csv       #Prints from field 5 onwards on a "Comma-Separated Values" file
cut -s                         #Do not print lines not containing delimeters
cut -s -d "," -f1-5  file.csv  #Prints only the first 5 fields from a .csv, ignores the rest of the file

#Cutting by characters
cut -c n file1                 #Prints only n characters of each line
cut -c 1 file1                 #Prints only the first character of each line
cut -c 1-10 file1              #Prints only the first 10 characters of each line

#Cutting by bytes
cut -b n file1                 #Prints only the byte n from the file
cut -b n-m file1               #Prints only the bytes from n to m in the file

```
{% endtab %}

{% tab title="tr" %}
```bash
#"a-z" "A-Z" "0-9" "symbols" [[:lower:]] [[:alpha::]] [[:alnum:]] [[:digit:]] [[:space:]] [[:punct:]] [[:xdigit:]]
#Squeeze
tr -s "x"                                 #Replace repeated secuences of x character with one ocurrence 
echo "a  b   c" | tr -s " "               #Replace each repeated ocurrence of space " " with one space
cat file1 | tr -s " " > new_file          #Remove extra spaces in a file and saves in new file  !Do not use > on the same file you are editing, it will get erased, for using the same name check example below
cat file1 | tr -s " " > new_file && mv new_file file1 #Removes extra spaces from file1
echo "aabbcc" | tr -s "a-z"               #Replace repeated secuences of a range of characters with one ocurrence

#Delete
echo "142-526-23-2322" | tr -d "-"        #Removes all ocurrences of the specified character
cat file1 | tr -d "\n" > new_file         #Removes all line feeds from file
cat file1 | tr -d "\t" > new_file         #Removes all TABS from file 
cat file1 | tr -cd [:print:] > new_file   #Removes all non-printable characters
cat file1 | tr -cd [:alnum:] > new_file   #Removes all non-alphanumeric characters

#Translate/Replace
echo "1_2_3_4" | tr "_" " "               #Replaces first character with second specified
echo "1_2_3_4" | tr "0-9" "x"             #Replaces first range of characters with second character specified
echo "ABC" | tr [:upper:] [:lower:]       #Replace uppercase with lowercase

#Truncate
echo "12345" | tr -t "12345" "abc"        #Truncates array1 to lengh of array2, prints "abc45"


```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="grep" %}
<pre class="language-bash"><code class="lang-bash">#grep will look for literal | egrep or "grep -E" will use regular expressions
grep abc file1             #Prints all the lines with literal "abc" in them
grep ^abc file1            #Prints all the lines that start with "abc"
grep abc$ file1            #Prints all the lines that end with "abc"
grep -i a file1            #Ignores case sensitive, will print "a" or "A" lines
grep -v                    #inVert the result
grep -v ^abc file1         #Prints all the lines that do not start with "abc
<strong>grep -c a file1            #Counts number of lines with "a"
</strong><strong>grep -cv a file1           #Count number of lines that doesn't contain "a"
</strong><strong>grep -o abc file1          #Only prints every iteration of "abc", ignores rest of the line
</strong><strong>grep -n a file1            #Shows the line numbers of the matches
</strong><strong>grep ^$                    #Prints all the empty lines
</strong><strong>grep -v ^$                 #Prints all the non-empty lines
</strong>grep ^#                    #Prints only commented lines
grep -w of file1           #Prints only lines with full word "of", will ignore "offer"
grep [a1?] file1           #Prints all inside [] independently, example will look for lines matching "a", "1" and "?"
grep ^[abc] file1          #Prints lines starting with either "a" "b" or "c"
grep [n-m] file1           #Prints the range between n and m $[0-9] will print all digits
grep -A n abc file1        #Prints n lines after matching string
grep -B n abc file1        #Prints n lines before matching string
grep -C n abc file1        #Prints n lines around matching string
grep .                     #Prints any single character, AKA everything
grep .a.                   #Prints any line which has "a" with one character after and before  !Spaces count as characters
grep a *                   #Prints all lines in the current directory that contain "a" and tags the name of the file
grep -r a *                #Prints all the lines in the current directory and others inside, recursive, that contain "a" aand tags the name of the file
grep -h a *                #Prints all lines in the current directory that contain "a" but ignores the name tag of the file
grep -l a *                #Prints the name of the files in the current directory that contain "a"
grep [[:upper:]] file1     #Prints all characters in uppercase
#[[:lower:]] [[:alpha::]] [[:alnum:]] [[:digit:]] [[:space:]] [[:punct:]] [[:xdigit:]] 


#Advanced Regex
#You can escape meta characters with \ or "quote" all the search
grep -E "a{n}" file1            #Prints all the lines that contain exactly n "a", a{3} will print only "aaa"
grep -E "a{n,m}" file1          #Prints lines that contain "a" repeated in the range n to m times
grep -E "a{n,}" file1           #Prints lines that contain "a" repeated n or more times 
grep -E "a{,n}" file1           #Prints lines that contain "a" repeated maximum n times
grep -E "a|b" file1             #Prints lines that contain "a" or "b"
grep -E "Ja(s|cks)on" file1     #Prints lines that contain "Jason" or "Jackson" 
</code></pre>
{% endtab %}

{% tab title="ls" %}
```bash
#Listing
ls                           #Prints the items in the current directory
ls -l                        #Detailed list
ls -a                        #Do not ignore hidden files (starting with .)
ls -R                        #Prints subdirectories recursively 
ls -l --author               #Also print author of each file
ls -l --block-size=n(K,M,G)  #Prints block size column by n size specified
ls -B                        #Ignore backups, AKA files ending in "~"
ls -F                        #Append a classificator (*/=>@|) for different file types
ls -lh                       #Prints human readable sizes
ls --ignore=X                #Ignore entries specified  $ls --ignore=*.txt
ls -n                        #Like -l but list user and group IDs
ls -s                        #Prints the allocated size of each item
ls -d                        #Prints only current directory, not the contents
ls -d */                     #Prints only directories in current directory
ls */                        #Prints only directories and items inside
ls -p                        #Adds "/" at the end of each directory
ls -p | grep -v /            #Prints only files 
ls -l *.format               #Prints only files with specified format

#Sorting
ls -lc                       #Sort by name
ls --group-directories-first #Groups directories first
ls -r                        #Reverse sorting
ls -S                        #Sort by size, largest first
ls -lSh                      #Sort by size in human readable format
ls -t                        #Sort by time, newest first
ls -U                        #Do not sort, list entries by directory order
ls -v                        #Sort numbers by natural order
ls -X                        #Sort alphabetically by entry extension 

#Extra
ls -i                        #Inode, print the index number of each file 
ls -m                        #Prints entries separated by commas ", "
ls -Q                        #Quote all items
ls -x                        #Prints by lines instead of columns
ls -1                        #Prints one item per line
ls -A                        #Do not list implied "." and ".." directories

```
{% endtab %}

{% tab title="uniq" %}
<pre class="language-bash"><code class="lang-bash">uniq file1                 #Filters duplicated consecutive lines and merges them into one
uniq -c file1              #Counts how many duplicated ocurrences and prefixes the number
uniq -d                    #Prints only duplicated lines, filtered into one
uniq -D                    #Prints only duplicated lines, all of them
uniq -u                    #Prints only unique lines
<strong>uniq -i                    #Ignore case sensitive when comparing, prints lowercase over uppercase
</strong>uniq -w n file1            #Compare no more than n first characters in lines
uniq -s n file1            #Ignore the first n characters
uniq -f n file1            #Avoid comparing the first n fields

#Examples
sort file.txt | uniq       #Delete duplicated lines from a sorted file, so separated duplicates do not get ignored
cut -d"," -f1 file1 | uniq #Delete duplicated first fields in a .csv
</code></pre>
{% endtab %}

{% tab title="wc" %}
```bash
wc file1            #Prints in order: number of lines, words and bytes of a file. Lines as every \n, words delimited by spaces and bytes just the size of the file in bytes
wc -l               #Counts only number of lines in the file
wc -m               #Counts only how many characters in the file
wc -w               #Counts only number of words, delimited by space
wc -c               #Counts only number of bytes in the file
wc -L               #Prints the maximum display width of the biggest line in the terminal  ?The longest number of characters in a line
wc file1 file2      #Prints all the default for each file and appends a total of each

#Pipelines examples
grep -o "abc" file1 | wc -l  #grep -o will find only the lines with "abc" and wc -l will count them, giving us the count of times that "abc" appears in the file

```
{% endtab %}

{% tab title="od" %}
```bash
od file1             #Converts to octal system the value of all characters in the file passed
od -c                #Converts to ASCII
od -x                #Converts to hexadecimal

```
{% endtab %}

{% tab title="pr" %}
<pre class="language-bash"><code class="lang-bash"><strong>pr file1              #Paginates content of file to printing format
</strong><strong>pr -d                 #Sets double space
</strong><strong>pr --columns n        #Sets n number of columns
</strong><strong>pr -w n               #Sets n character width for each page
</strong><strong>pr -n                 #Prints line numbers 
</strong><strong>
</strong></code></pre>
{% endtab %}

{% tab title="diff" %}
```bash
diff file1 file2        #Prints the lines that are different between 2 files
diff -y                 #Compares in two columns and marks with "|" the different lines
diff -q                 #Only tells if files differ, not what does
diff -s                 #Only tells if files are equal, not what does
diff -sq                #Only tells if files are equal or different, not what does
diff -c                 #Also prints n lines up and above the different ones, by default 3
diff -u                 #Compares lines and n lines up and above and prints unified as if they where 1 file
diff -l                 #Paginates output with "pr" as in ready to send to printer
diff -r dir1 dir2       #Recursively compares file names inside directories specified
diff -i                 #Ignore case differences
diff -E                 #Ignore differences due to TAB expansion
diff -Z                 #Ignore white space at line end
diff -a                 #Threat all files as text

diff file1 file2 | grep "^[><] > diff.txt #Saves only the lines that are different into a new file

```
{% endtab %}
{% endtabs %}

***

## Links

Links are special files that point to other files, we distinguish 2 types:

* **Hard Link:** points to the data in the disk itself, to the exact inode, if you create a hardlink and check with `ls -i` you will see that they both point to the same inode index. It's like opening another door to the same place. So if you delete the original file, it will still be there, until all hardlinks are removed the file isn't removed from the disk.\
  Also they can't be pointing to different filesystems(partitions), as inodes would collide since every partition has it's own index. You can only do hardlinks in the same partition.\
  Neither they can point to directories because they could cause errors in correlation.\
  \
  Take in mind that hardlinks share also metadata, so privileges will be the same everywhere in every link, but you can create a hardlink on a public directory pointing to a file on a private directory (ex: your home where no one can access) and they could have access to it while not being able to even see the directory where it comes from.\

* **Symbolic Link (AKA Soft Link):** points to the path to the data, not the data itself. Unlike hardlinks, when you create a symlink it will use a different inode, they are not the same file per se. So if you delete the original file, the symlink will remain broken, unless you place a file with the same exact name and path again and if you delete the symlink, just create another, no drama. \
  Unlike hard links, they can be used for directories.\
  They are like shortcuts on Windows, a convenient "within reach" pathway to another file/directory. \
  A big positive is that as they only point to a route, you can use them between different filesystems(partitions) or even external storage devices. \
  \
  They usually have `lrwxrwxrwx` privileges, but its meaningless as they share the privileges of the original file and the directory they are in. \
  Also on a sticky bit directory, only the owner of the link may use it



```bash
ln /route/sourceFile /route/newLink        #Creates a Hard link of sourceFile
ln -s /route/sourceFile /route/newLink     #Creates a Symbolic link of sourceFile

```
