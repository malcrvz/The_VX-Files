# CMD File management



{% tabs %}
{% tab title="fsutil" %}
```powershell
#File
fsutil file createNew file1 n    #Creates new file1 with n number of bytes  ?To create the pre-defined size it uses invisible characters, you can delete them
fsutil file layout file1         #Gives detailed info about the file


```
{% endtab %}

{% tab title="xcopy" %}
```powershell
copy file1 C:\destiny_directory    #Copies the file into the specified directory
xcopy directory1 new_directory     #Copies directory and only the files inside (no subdirectories)
xcopy /S directory1 new_directory  #Copies recursively, files and subdirectories and its files
xcopy /E directory1 new_directory  #Same as /S but includes empty directories too
xcopy /I                           #If specified destiny doesn't exist, it supposes its a directory and creates it
xcopy /W                           #Asks user for confirmation pressing a key before copying
xcopy /P                           #Asks confirmation (Y/N) for each file is going to copy
xcopy /L                           #Prints what would be copied if executed, but doesn't copy, needs to be removed before

#Copy multiple files in different folders
for %i in ("dir1\file1.txt" "a\b\c\file.pdf" "dir2\file3.csv") do copy %i Destiny\
```
{% endtab %}

{% tab title="mkdir" %}
```powershell
#mkdir/md
mkdir directory1        #Creates directory1 in current directory
mkdir a\b\c             #Creates recusirsive directories, c inside b inside a

```
{% endtab %}

{% tab title="dir" %}
```powershell
dir             #Prints files and folders in current directory
dir /A          #Prints all types of files and folders in current directory
dir /A:D        #Prints only folders
dir /A:H        #Prints hidden files and folders
dir /A:-D       #Prints only files ("-" minus directories)
dir /O          #Prints files and folders sorted alphabetically
dir /O:S        #Prints files and folders sorted by size smallest to largest
dir /O:-S       #Prints files and folders sorted by size largest to smallest
dir /B          #Prints only the names of files and folders

#Pipeline examples
dir /B | sort   #Sorts in alphabetical order the input from dir /B
dir /B | sort /R /O file.txt    #Sorts the input in reverse order and saves the output into a file
notepad file.txt     #Check file just created

```
{% endtab %}

{% tab title="move" %}
```powershell
move c:\folder1\file1.txt c:\folder2  #Move file1 to folder

```
{% endtab %}

{% tab title="tree" %}
```powershell
tree         #Prints the directory structure of current folder
tree /F      #Prints the directory structure and the files inside each

```
{% endtab %}

{% tab title="rm" %}
```powershell
rm file1                 #Removes file

```
{% endtab %}
{% endtabs %}
