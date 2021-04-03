## Shell Scripting


* Scripts are run by interpreters
* bash or zsh

```
#! Shebang or Hashbang
```
specifies which interpreter should run the code

as zsh is stored in diff folder str in ubuntu machine and mac

#!/usr/bin/env zsh (use env command)

#!/usr/bin/env bash



```
ls
head shipments.csv (displays Output)
nano create_report.sh
```
#### File Contents
```
#!/bin/bash

# Author Harvey

mkdir reports
grep H6 shipments.csv > report1/H6.csv
```

#### Permissions
#### LINK : https://tldp.org/LDP/intro-linux/html/sect_03_04.html
```

Code	Meaning
0 or -	The access right that is supposed to be on this place is not granted.
4 or r	read access is granted to the user category defined in this place
2 or w	write permission is granted to the user category defined in this place
1 or x	execute permission is granted to the user category defined in this place

```


```

Code	Meaning
u	user permissions
g	group permissions
o	permissions for others

```
```

Command	Meaning
chmod 400 file	To protect a file against accidental overwriting.
chmod 500 directory	To protect yourself from accidentally removing, renaming or moving files from this directory.
chmod 600 file	A private file only changeable by the user who entered this command.
chmod 644 file	A publicly readable file that can only be changed by the issuing user.
chmod 660 file	Users belonging to your group can change this file, others don't have any access to it at all.
chmod 700 file	Protects a file against any access from other users, while the issuing user still has full access.
chmod 755 directory	For files that should be readable and executable by others, but only changeable by the issuing user.
chmod 775 file	Standard file sharing mode for a group.
chmod 777 file	Everybody can do everything to this file.

```
```

chmod a+x filename (Executable for everyone)
chmod a-x filename (Remove executable permission)

```
#### Copy a file
```
ls
a.txt
cp a.txt b.txt
ls
a.txt b.txt

ls 
a.txt b.txt new
ls new 
(empty)
cp a.txt b.txt new
ls new
a.txt b.txt

```

#### Move Contents
```
ls
a.txt b.txt c.txt d.txt
mv a.txt geek.txt
ls
b.txt c.txt d.txt geek.txt


ls
b.txt c.txt d.txt geek.txt
cat geek.txt
India
cat b.txt
geeksforgeeks
mv geek.txt b.txt
ls
b.txt c.txt d.txt
cat b.txt
India
```
#### DEMO
```
$ ls -l create_report.sh (check file permissions)
$ chmod u+x create_report.sh (exec permissions for user group)
$ ls -l create_report.sh
if the script is not on your PATH: include the directory
$ ./create_report.sh (was in pwd)
(or else /home/reindert/myscript)
$ ls reports
mkdir -p reports (-p : Not to fail if target folder exists)
```


* Shell variables dont have any data types.
* They just store strings


* To retrieve the value , prefix with$
* and to print it echo $x

```
greeting="hello"
echo $greeting
```
```
filename="somefile.txt"
touch $filename
ls -l $filename
rm $filename
```
* For values containing spaces use quotes
```
files="file1 file2"
touch $files (op depends on shell bash/zsh)
```
* two files will be created for bash shell as each work will be treated as separate arguments
* one file with aspace in name will be created in zshell

#### Some variables are predefined (all are in uppercase)
```
echo $greeting, $USER
```
(Note : Variables name are case sensitive)

$user will give an empty string as value as it is not defined but no error

usergreeting=$greeting, $USER (will treat ubuntu as an command . So ERROR)

usergreeting="$greeting, $USER"

x = 5 (ERROR)

sees x as command and = as an argument

So for assignment to work

x=5 (dont put spaces around =)

```
rmfiles="rm file1 file2"
```
$rmfiles (using without echo will destroy both files)

* So always use echo $rmfiles if u want to inspect values

#### File Content
```
#!/bin/bash

directory=reports

mkdir -p $directory
grep H6 shipments.csv > $directory/H6.csv
echo Report created.
```
#### RUN : ./create_report.sh


### Scripts reacting to user input
#### File Content
```
#!/bin/bash

directory=reports

mkdir -p $directory
grep $1 shipments.csv > $directory/$1.csv
echo wrote report $directory/$1.csv

```
#### RUN : ./create_report.sh M8

* $1-first arg
* $2-second arg

### Variable Naming
* Use letters numbers underscores
* First character can be a letter or underscore
* Case Sensitive..use smallcase (advisable)
* Dont use UpperCasing to avoid accidentally overwritting predef variables


### Bugs
without any args

./create_reports.csv

(does nothing just hangs)

#!/bin/bash -v

(print every line before it runs)

#!/bin/bash -x


### Scripts reacting to user input (two inputs)
#### File Content
```
#!/bin/bash

container=$1
directory=$2

mkdir -p $directory
grep $container shipments.csv > $directory/$container.csv

echo wrote report $directory/$container.csv
```
#### RUN : ./create_report.sh P5 works
#### RUN : ./create_report.sh P5 "my docs"
(creates two folders and give a ambiguous redirect warning)

* So its a good practice to surround variables by a " " as it
protects the special meaning and whitespaces

#### File Content
```
#!/bin/bash

container="$1"
directory="$2"

mkdir -p "$directory"
grep "$container" shipments.csv > "$directory/$container.csv"

echo "wrote report $directory/$container.csv"
```
#### RUN : ./create_report.sh P5 "my docs"
(Runs Just Fine Now)

#### sudo snap install shellcheck

#### shellcheck create_reports.csv 
(checks if you missed quotes or something)

op : Double quote to prevent globbing and word splitting.

#### www.shellcheck.net

zsh doesnt does word splitting by default (one of the nice things)
so if u dont surround the variables with " " then also works just fine

But quoting your variables is a best practice

### New Bugs
-c commands maybe interpreted as options and the script may freeze
```
./create_report.sh -c works
```
#### we use -- to say its end of option
#### everything that comes after this is normal argument and not option

but wont work with echo
```
echo -- "$USER" just prints -- ubuntu
```
### So We May Use
```
printf "%s,\n" $USER
printf "I am %s and my shell is %s\n" $USER $SHELL
(powerful but complex)
```
```
grep "$container" shipments.csv > "$directory/$container_report.csv"
```
* .csv file will be created (takes it as empty string arg as no variable as container_report is found

* so use { }_report
```
grep "$container" shipments.csv > "$directory/${container}_report.csv"
```
* Using single quote will miss the $ sign and you will end up with dollar sign in output i guess

```
echo "${foo}bar
prints value of foo
followed by string "bar"

echo "$foobar"
prints value of foobar
```

### EXPORT
```
grep "$container" "$input_file" > "$directory/$container.csv"
```
$ input_file=shipments.csv

$ export input_file

$ ./create_report.sh A5 reports

### With Some Caution Change This File If Required
```
vim .bashrc
export input_file="$HOME/shipments.csv"
```


### Conditionals
```
$ if mkdir a; then echo "ok"; else echo "error"; fi
```
fi marks the end of if else statement.

(first time o/p : ok and next time : error)

#### File Content
```
#!/bin/bash

if [[ ! $1 ]]; then
	echo "Error: missing parameter;container name"
	exit 1
fi
container="$1"

if [[ ! $2 ]]; then
	directory="$2"
else
	directory="$HOME/reports"
fi


mkdir -p "$directory"
if grep "$container" shipments.csv > "$directory/$container.csv"
then
	echo "Wrote Report"
else
	echo "Container Not Found"
fi

echo "wrote report $directory/$container.csv"
```



#### RUN : if ./create_report.sh; then echo "Success"; else echo "Error"; fi

### The Template
```
if testcode; then
	"xyz"
else
	"zyx"
fi 

if testcode
then
fi
```
* if, then, else, fi have to be first on aline OR following a semicolon
* bash will know you are starting next part of if statement

### Return Values
returned by program upon exit

0-255

0 is success

and other values errors

good habit to write exit 0 at the end

always call exit with a value


### Conditional Expression
```
[[ $str ]]
[[ $str="something" ]] #Wrong: always true
[[ $str = "something ]] # String equals "something"

[[ ! $1 ]] # Argument Empty
[[ $a || $b ]] "or"
```
```
$ man test  
(manual for tests)
```
-e FILE :  
	File Exists

*  [ ] dont use this syntax as its a exec command 

* [[ ]] is a bash extension
* no quote reqd around variables
* special syntax not a command



### HAPPY SCRIPTING . . !

