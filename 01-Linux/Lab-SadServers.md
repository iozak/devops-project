URL
https://sadservers.com/

**"Saint John": what is writing to this log file?**
Description: A developer created a testing program that is continuously writing to a log file /var/log/bad.log and filling up disk. You can check for example with tail -f /var/log/bad.log.
This program is no longer needed. Find it and terminate it. Do not delete the log file.

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

