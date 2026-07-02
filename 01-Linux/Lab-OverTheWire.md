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
6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
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
PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
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
7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
```
```
cd inhere
```
```
cat ...Hiding-From-You
```

**Level 4**
The password for the next level is stored in the only human-readable file in the **inhere** directory. 
```
ssh -p 2220 bandit4@bandit.labs.overthewire.org
```
```
xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
```
```
cd \inhere
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
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
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
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
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
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
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
VR1ljMayciFxbnUokuQmJFw6QC9VKtub
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
EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
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
B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
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
pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
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
GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```
```
mkdir /tmp/hextmp
```
```
cp ~/data.txt .
```
```
mv data.txt hexdump
```
```
cat hexdump | head
```
```
Keep mv and uncompressing depending on the file signatures. gzip, tar, xxd. https://en.wikipedia.org/wiki/List_of_file_signatures
```

**Level 13**
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.
If you need help with this level: a hint file can be found in the home directory.
Make sure to read the error messages as they are informative.
```
ssh -p 2220 bandit13@bandit.labs.overthewire.org
```
```
qQYQiHOBPR8zR61qxYqX45quvihF2uzk
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
First to get the password for bandit14
```
cat /etc/bandit_pass/bandit14
```
```
nc localhost 30000
```
```
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

**Level 15**
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
```
ssh -p 2220 bandit15@bandit.labs.overthewire.org
```
```
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```
```
openssl s_client -connect localhost:30001
```
```
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

**Level 16**
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
```
ssh -p 2220 bandit16@bandit.labs.overthewire.org
```
```
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```
```
nmap -sV localhost -p 31000-32000
```
```
openssl s_client -connect localhost:31790
```
```
openssl s_client -connect localhost:31790 -ign_eof
```
```
Copy private key to local and chmod
```
```
ssh -i bandit16.ssh bandit17@bandit.labs.overthewire.org -p 2220
```

**Level 17**
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
```
ssh -i bandit16.ssh bandit17@bandit.labs.overthewire.org -p 2220
```
```
diff passwords.old passwords.new
```
```
The text after > will be the password
```

**Level 18**
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
```
ssh -p 2220 bandit18@bandit.labs.overthewire.org
```
```
OQxXZjELndr90zuhOTDYBEomI0SZITXI
```
```
We can try using a different shell
```
```
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "/bin/sh"
```
```
cat readme
```

**Level 19**
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

```
ssh -p 2220 bandit19@bandit.labs.overthewire.org
```
```
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
```
```
./bandit20-do
```
Run a command as another user.
```
./bandit20-do id
```
```
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Level 20**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20).

```
ssh -p 2220 bandit20@bandit.labs.overthewire.org
```
```
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```
```
ls -l - should see a suconnect file
```
At this point had to look into netcat usage and figured this would require 2 terminals. One to listen, one to connect.
On terminal 1
```
nc -lvp 9999
```
On terminal 2
```
./suconnect 9999
```
On terminal 1 - enter password
```
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```
Then we get the next password on terminal 1
```
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```
