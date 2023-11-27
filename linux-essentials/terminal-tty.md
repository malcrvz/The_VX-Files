---
description: 'TL;DR: in a practical way they are almost the same!'
---

# Terminal/TTY

Terminal (or Terminal Emulator) is the software that gives us a graphical interface to interact with a TTY, the window that contains the prompt where we give commands to the computer that we are accustomed to use in our Kali Linux for example.

TTY (Teletype) is a text-based interface for interacting with the computer, what is underlying the terminal.&#x20;

Every time you open a terminal you are interacting with a TTY, just in a more user-friendly way.

The command `tty` shows in which TTY session you are currently. For example:&#x20;

```bash
$ tty
/dev/pts/0
```

Every terminal window you open will be assigned to a new one(try it!), so if for example you open 5 terminals the last one will be on:

```bash
$ tty
/dev/pts/4         #Remember 0 counts, so 5-1!
```

But instead of TTY we see _pts_?&#x20;

These _pts_ or "pseudo-terminals" will be inside your current TTY, they are like matryoshka dolls and they are given to your terminal software.

To know which TTY session you are currently on just enter the command `who` \
That will show what users are logged in and which TTY they are using, useful for remote connections.

The TTY where I have my graphic interface installed on this Kali virtual machine happens to be the tty7 (depends on the distro), so when I enter `tty` I get `/dev/pts/0` but you can change to other TTYs by pressing

`ctrl + alt + F1` to `F7`&#x20;

or entering the command:

`chvt n` \
"n" as the interface number

and you will have an only TTY screen, in a different session, and then when you enter `tty` you will see `/dev/tty1` (or whatever number you chose) because they are the whole TTY not a software pseudo-terminal.

This is very handy for troubleshooting or if you need a clean screen and all the resources you can for a specific task.

Its also used for remote administration and isolating sessions, you can have multiple accounts, remote connections, with different users in the same system, at the same time, with just a shortcut, all them protected by credentials, using only terminal emulator windows its all in the same login and can become pretty chaotic if there's more than one user.&#x20;

But most importantly for us, we can check the logs for unusual TTY sessions in a system, monitor&#x20;
