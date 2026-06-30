URL
https://overthewire.org/wargames/bandit/bandit0.html

**Level 0**
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **bandit.labs.overthewire.org**, on port 2220. The username is **bandit0** and the password is **bandit0**.
```
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```
The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.
```
ls
```
```
cat readme
```

**Level 1**
The password for the next level is stored in a file called **-** located in the home directory.
```
ssh -p 2220 bandit1@bandit.labs.overthewire.org
```
```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```
```
ls
```
```
cat ./-
```

**Level 2**
The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.
```
ssh -p 2220 bandit2@bandit.labs.overthewire.org
```
```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```
```
ls
```
```
cat ./*spaces*
```

**Level 3**
The password for the next level is stored in a hidden file in the **inhere** directory.
```
ssh -p 2220 bandit3@bandit.labs.overthewire.org
```
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```
```
cd inhere
```
```
cat ...Hiding-From-You
```

**Level 4**
The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.
```
ssh -p 2220 bandit4@bandit.labs.overthewire.org
```
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```
```
ls -la \inhere
```
```
file ./*
```
```
cat ./-file07
```

**Level 5**
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties: -human-readable -1033 bytes in size -not executable
```
ssh -p 2220 bandit5@bandit.labs.overthewire.org
```
```
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```
```
cd inhere
```
```
find -type f -size 1033c
```
```
cat ./maybehere07/.file2
```

**Level 6**
The password for the next level is stored somewhere on the server and has all of the following properties:
owned by user bandit7
owned by group bandit6
33 bytes in size
```
ssh -p 2220 bandit6@bandit.labs.overthewire.org
```
```
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```
```
find / -type f -user bandit7 -group bandit6 -size 33c 2> /dev/null
```
```
cat /var/lib/dpkg/info/bandit7.password
```

**Level 7**
The password for the next level is stored in the file data.txt next to the word millionth.
```
ssh -p 2220 bandit7@bandit.labs.overthewire.org
```
```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```
```
vim data.txt
```
```
/millionth
```
Or
```
grep millionth data.txt
```

**Level 8**
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once
```
ssh -p 2220 bandit8@bandit.labs.overthewire.org
```
```
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```
```
cat data.txt | sort | uniq -u
```

**Level 9**
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
```
ssh -p 2220 bandit9@bandit.labs.overthewire.org
```
```
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```
```
strings data.txt | grep ===
```

**Level 10**
The password for the next level is stored in the file data.txt, which contains base64 encoded data.
```
ssh -p 2220 bandit10@bandit.labs.overthewire.org
```
```
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```
```
base64 -d data.txt
```

**Level 11**
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.
```
ssh -p 2220 bandit11@bandit.labs.overthewire.org
```
```
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Level 12**
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)
```
ssh -p 2220 bandit12@bandit.labs.overthewire.org
```
```
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
```
cat hexdump_data | head
```
```
https://en.wikipedia.org/wiki/List_of_file_signatures
```
```
Keep mv and uncompressing depending on the file signatures. gzip, tar, xxd.
```

**Level 13**
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.
If you need help with this level: a hint file can be found in the home directory.
Make sure to read the error messages as they are informative.
```
ssh -p 2220 bandit13@bandit.labs.overthewire.org
```
```
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```
```
ls command showed me the ssh key so I exited back to local terminal and ran next command
```
```
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .   
```
```
exit again and connect using ssh key on next command. Issue with permissions on ssh key so did next chmod command first then connect again
```
```
chmod 700 sshkey.private
```
```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

**Level 14**
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
