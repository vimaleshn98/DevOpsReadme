## Closing A Port
* Open CMD
```
netstat -a -o -n
```
* Find PID of the port you want to close
```
taskkill /F /PID 15509*
```