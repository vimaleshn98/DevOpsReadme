## 1.Closing A Port
* Open CMD
```
netstat -a -o -n
```
* Find PID of the port you want to close
```
taskkill /F /PID 15509*
```

### Alternative
```
netstat -ano | findstr :8080
```
```
taskkill/PID 14260 /F
```

## 2.Preview In VSCode

#### To Preview the current .md extension files
#### Use :
```
Ctrl+Shift+V
```

## 3. Will Come Soon