# **Palowan Linux guru Cheatsheet - Z shell 2**

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [User and Group management](#user-and-group-management)
2. [Useful utility](#useful-utility)
3. [Shell Scripting](#shell-scripting)
    - [Conditional Statements](#conditional-statements)
    - [Loop Statements](#loop-statements)
4.  [Fetching data from internet](#fetching-data-from-internet)

5. [References](#references) 

---

> **NOTES**
> 
> 1. For the sake of consistency, here is the structure of commands in this cheat sheet: 
>     - `command [OPTIONS] PARAMETER`
>       - Commands are indicated in lowercase.
>       - Parameters are indicated in uppercase.
>       - Parameters inside square brackets "[ ]" are optional.
>
> 2. You may know more about the command by typing `command --help` or `man command`
>     - for example:
>       - `useradd --help` or `man useradd` displays the user manual of command `useradd`
>       - `man echo` for the user manual of command echo
>
> 3. Most of the ZSH command accepts compound options
>     - for example: `ls -alt` combines three options which are `-a (list all including hidden entries )`, `-l (view in long listing format)` and `-t (sort by time)`.


---

## **TL;DR**

### **_User and Group management_**

- List all users: `getent passwd`
- List all groups: `getent group`
- Create user with home directory: `sudo useradd -m USER_NAME`
- Change password: `sudo passwd USER_NAME`
- Remove user including home directory: `sudo userdel -r USER_NAME`
- Create group: `groupadd GROUP_NAME`
- Remove group: `groupdel GROUP_NAME`
- Add user to group: `sudo usermod -aG GROUP USER_NAME`

### **_Useful utility_**

- Load profile to current session: `source PATH`

### **_Shell scripting_**

- Conditionals: 
```sh
if CONDITIONAL_EXPRESSION; then
    ACTIONS 
elif CONDITIONAL_EXPRESSION; then
    ACTIONS
else
    ACTIONS
fi
```
```sh
case SUBJECT in

CONDITION_1)

    ACTIONS

;;

CONDITION_2)

    ACTIONS

;;

esac
```

- Loop:
```sh
for ELEMENT in ITERABLE
do
    ACTIONS
done
```

### **_Fetching data from internet_**

- Display fetched data: `curl URL`
- Download data: 
    - `curl -O URL`
    - `curl -o FILE_NAME URL`
    - `wget --progress=bar URL`
    - `wget --progress=bar -O FILE_NAME URL`

---

## **User and Group management**

User configuration file: `/etc/passwd`  
Password file (All passwords are encrypted): `/etc/shadow`  
Group info file: `/etc/group`

`getent passwd` = List all users

`getent group` = List all groups

`sudo useradd [OPTIONS] USER_NAME` = Create user
- `-d PATH` = Specify home directory for the new user
- `-m` = Create home directory for the new user if it does not exist
- `-s PATH` = Set user's shell (Default shell is /bin/bash)

`sudo passwd USER_NAME` = Change user's password

`sudo usermod [OPTIONS] USER_NAME` = Modify user

- `-aG GROUP` = Add user to group
- `-l NEW_USER_NAME` = Rename user
- `-d NEW_PATH ` = Modify home directory

`su USERNAME` = Switch user (By using `sudo`, you may login without password.)

`sudo userdel [OPTIONS] USER_NAME` = Remove user
- `-r` = Remove user including mail and home directory

`groups` = List groups that the user is currently in

`groupadd [OPTIONS] GROUP_NAME` = Create group

- `-f` = Create group regardless error
- `-g` = With GID (group ID)

`groupmod GROUP_NAME` = Modify group

`groupdel GROUP_NAME` = Remove group

---

## **Useful utility**

`;` = Indicates the end of command

`$` = Articulate variable

`|` = Pipeline connects two commands, pass the output of the former as the input of the latter
> For example:
> - `ls -al ~ | grep .git`
> - `ls -alr ~ | more`

<!-- `awk` TODO -->

`grep [OPTIONS] "PATTERN" FILE` = Search the file for lines containing particular pattern and print it on terminal (or you may use I/O redirection symbol: ">" and ">>" to write the results to file)

- `-c` = Count number of the matched lines
- `-i` = Case insensitive
- `-n` = Print the matched lines with line number
- `-v` = Display lines that do not match the pattern

> **Commands that are often used together with `grep` command:**  
> `more` = Stop displaying once the result exceed the max-number of lines of screen, and prompt the MORE option  
> `sort [OPTIONS]` = Display lines in alphabetical or numeric order
>
> - `-f` = case insensitive
> - `-n` = sort in numeric order
> - `-r` = reverse sorting order

> For example:
>
> - `ls -alR ~| grep .json | sort -r | more`

<!-- `xargs` TODO -->

`eval $COMMAND_VARIABLES` = Run shell command stored in variable(s)
> For example:
> 
> 1. `command="ls -alR | grep .json | more"` \
> 2. run `eval $command` (equivalent to run `ls -alR | grep .json | more"`)

`export ENV_VARIABLE=VALUE` = Assign value to a environment variable which can be accessed from all the shells and processes under the same original shell.

`set VARIABLE=VALUE` = Assign value to a local variable.

> Know more about the difference between `export` and `set`, see [article](https://www.theunixschool.com/2010/04/what-is-difference-between-export-set.html)

`source PATH` = Load a profile to current session

---

## **Shell Scripting**

> Create a shell script
>
> 1.  Create a file with .sh extension. e.g. hello.sh
> 2.  Type `#!/bin/zsh` at the very beginning of the file telling the loader executes this script program using ZSH (it would be executed using bash if you type `#!/bin/bash` instead.)
> 3.  Type `echo "Hello World"`(or any other commands you want)
> 4.  Save the file
> 5.  Run `chmod +x hello.sh` in order to give user permission to execute the script (x = execute in symbolic mode)
> 6.  Run the script ./hello.sh and you will see "Hello World" .

`$@` = An array construct
`$1, $2,...` = parameters that pass to the command (1-based indexing, 0 is the command itself)

### **_Conditional statements_**

`test EXPRESSION` = `[ EXPRESSION ]` = Evaluates the expression and returns 0 (true) or 1 (false)  
Use the following operators to constructing expression:

- Logical operators
   - `!` = Negation
   - `-a` = And
   - `-o` = Inclusive or (at least one expression is true)
- Mathematical operators
   - `-eq` = Numerically Equivalent
   - `-ne` = Numerically non-equivalent
   - `-gt` = Greater than
   - `-ge` = Greater than or equal
   - `-lt` = Less than
   - `-le` = Less than or equal
- String comparing
   - `=` = Two strings are equivalent
   - `!=` = Two strings are not equivalent
- File checking
   - `-e` = File exists
   - `-d` = File exists and is a directory
   - `-f` = File exists and is a regular file

`if...then...elif...then...else...fi`

```sh
STRING="Mary had a little lamb"

   # start of the if...fi block
if [ $STRING = "morning" ]; then
   # executes if the expression following "if" keyword returns 0 (true)
   echo "Good morning!"

elif [ $STRING = "afternoon" ]; then
   # executes if the expression following "elif" keyword returns 0 (true)
   echo "Good afternoon!"

else
   # executes if the expressions above does not return 0 (true)
   echo "What should I say?"

   # end of the if...fi block
fi

   # The result is "What should I say?"
```

`case...esac`

```sh
   # start of the case block;
   # $1 refers to the first parameter passed into the program
case $1 in

   # specify the case
-s)
   # execute if the first parameter is -s
    echo "you have -s in args"
   # the end of this case, skip the rest and jump to the end of the entire case block
;;

   # specify the case
*)
   # execute if $1 match this case
    echo "Unknown"
   # same as the above
;;

   # end of the case block
esac
```

### **_Loop statements_**

`for...in...do...done`

```sh

   # start of the looping block
   # $@ is an array-like construct of all parameters
for arg in "$@"

   # start of the computation
do

    echo $arg

   # end of the computation
done
```

---

## **Fetching data from internet**

`curl [OPTIONS] URL` = Display the fetched data on the terminal

<!-- > NOTE: Install curl using Homebrew: `brew install curl`   -->

- `-C -` = Resume download
- `-I` = Display information of the response header
- `-k` = Allow insecure connections when using SSL
- `-O` = Save data to a file with it original name
- `-o FILE_NAME` = Save data to a file with custom file name
- `--retry NUMBER` = Set number of retries
- `-#` = Show progress bar

`wget URL` = Save the fetched data in a file

- `-b` = Download in the background
- `-c` = Resume download
- `-i FILE_NAME` = Download all files from the urls listed in the FILE
- `-O FILE_NAME` = Custom file name
- `-P PATH/` = Save to specific directory
- `-t NUMBER` = Set number of retries
- `--progress=SYMBOL_TYPE` = Show progress bar (SYMBOL_TYPE can be dot or bar)

<!-- > NOTE: Install wget using Homebrew: `brew install wget`   -->
<!-- > NOTE: Basically, the differences between `curl` and `wget` are:
>
> 1. `curl` supports a lot of protocol, wget supports only http and ftp
> 2. -->

---

## References:

1. Commands  
   [awk](https://phoenixnap.com/kb/awk-command-in-linux)

2. User and Group management  
   [How to create users in linux using useradd command](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) \
   [Linux user group management](https://www.redhat.com/sysadmin/linux-user-group-management) \
   [local group accounts](https://www.redhat.com/sysadmin/local-group-accounts) \
   [Managing users and groups](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups)

3. Fetching data from internet  
   [wget vs crul](https://www.linuxfordevices.com/tutorials/linux/wget-vs-curl) \
   [wget](https://www.hostinger.com/tutorials/wget-command-examples/#Using_Wget_Command_to_Download_Single_Files)

4. Shell script  
   [Writing shell scripts](https://linuxcommand.org/lc3_writing_shell_scripts.php)

##### Brought to you by Suen Lam ;)

