URL
https://sadservers.com/

**Scenario: "Saint John": what is writing to this log file?**
Description: A developer created a testing program that is continuously writing to a log file /var/log/bad.log and filling up disk. You can check for example with tail -f /var/log/bad.log.
This program is no longer needed. Find it and terminate it. Do not delete the log file.
Test: The log file size doesn't change (within a time interval bigger than the rate of change of the log file).

**Solution**
To see all the processes that are running
```
ps auxf
```
To further narrow it down to only open files and associated processes
```
lsof /var/log/bad.log
```
To kill the process
```
kill -9 597
```


**Scenario: "Saskatoon": counting IPs.**
Description: There's a web server access log file at /home/admin/access.log. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line (first column). 
Find what's the IP address that has the most requests in this file (there's no tie; the IP is unique). Write the solution into a file /home/admin/highestip.txt. For example, if your solution is "1.2.3.4", you can do echo "1.2.3.4" > /home/admin/highestip.txt
Test: The SHA1 checksum of the IP address sha1sum /home/admin/highestip.txt is 6ef426c40652babc0d081d438b9f353709008e93 (just a way to verify the solution without giving it away, we also accept the right IP with no ending newline in the file.) 

**Solution**
Read the log file. Print first field of every line, sort in reverse order, find duplicates, sort again, then grab first result. Redirect output to file as instructed.
```
awk '{print $1}' /home/admin/access.log | sort -nr | uniq -c | sort -nr | head -1 | awk '{print $2}' > /home/admin/highestip.txt
```

Then to verify with SHA1 checksum
```
sha1sum /home/admin/highestip.txt
```


**Scenario: "The Command Line Murders"**
Description: This is the Command Line Murders with a small twist as in the solution is different
Enter the name of the murderer in the file /home/admin/mysolution, for example echo "John Smith" > ~/mysolution
Test: md5sum ~/mysolution returns 9bba101c7369f49ca890ea96aa242dd5

**Solution**
Used various commands such as cat, grep, ls, grep, sed to navigate and process through all the clues and information at hand.

![[LinuxSadServerLab3.png|697]]

Answer - Joe Germuska