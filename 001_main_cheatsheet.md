# **Palowan Linux guru Cheatsheet**

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [ZSH tips](#zsh-tips)
2. [Path tips](#path-tips)
3. [User and Group management](#user-and-group-management)
4. [File management](#file-management)
5. [Useful utility](#useful-utility)
6. [Shell Scripting](#shell-scripting)
    - [Conditional Statements](#conditional-statements)
    - [Loop Statements](#loop-statements)
7.  [Fetching data from internet](#fetching-data-from-internet)
8.  [Git](#git)
    - [Git configuration](#git-configuration)
    - [Git repository](#git-repository)
    - [Get status](#get-status)
    - [Remote repository](#remote-repository)
    - [Pull and Push](#pull-and-push)
    - [Undo changes](#undo-changes)
    - [Git branch](#git-branch)
    - [Merge conflict](#merge-conflict)
9. [Docker](#docker)
    - [Image](#image)
    - [Volume](#volume)
    - [From image to container](#from-image-to-container)
    - [Container](#container)
    - [Docker Compose](#docker-compose)
10. [References](#references) 

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


### **_Zsh_**

- Start Z-shell: `zsh`
- Restart Z-shell: `exec zsh`


### **_Path_**

- Print working directory: `pwd`
- List all entries and sort by time in descending order: `ls -alht`
- List all entries and sort by time in ascending order: `ls -alhtr`
- Change working directory: `cd PATH`


### **_File_**

- Check file type: `file PATH`
- Check file status: `stat PATH`
- Check disk usage: `du PATH`
- Create new directory: `mkdir NEW_DIRECTORY`
- Create new file: `touch NEW_FILE`
- Change the permissions of files or directory: `chmod MODE FILE`
- Move file: `mv FILE DESTINATION`
- Move directory: `mv DIRECTORY DESTINATION`
- Copy file or directory: `cp SOURCE DESTINATION`
- Copy content: `clipcopy FILE` (then press command/ctrl + v to paste)
- Print content: `cat FILE`
- Remove file: `rm FILE`
- Remove directory: `rm -rf DIRECTORY`


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

### **_Git_**

**Get information**

- Display repository status: `git status`
- Display non-staged changes: `git diff`


**Record changes**

- Stage changes: `git add FILE`
- Stage all changes: `git add -A`
- Commit staged changes: `git commit -m "COMMIT_MESSAGE"`
- Pull (fetch and merge) from remote branch: `git pull`
- Push local changes to remote branch: `git push REMOTE BRANCH` 


**Undo changes**

- Discard changes: `git restore FILE`
- Unstage staged changes: `git restore --staged FILE`
- Undo commit: 
    - `git reset --soft COMMIT_ID`
    - `git reset --soft HEAD~#`
    - `git revert COMMIT_ID`
    - `git revert HEAD~#`


**Remote**

- List all remote connections: `git remote`
- Add remote connection: `git remote add REMOTE_NAME URL`
- Check remote URL: `git remote get-url REMOTE_NAME`
- Remove remote connection: `git remote remove REMOTE_NAME` 


**Branch**

- List all branches: `git branch -avv`
- Create branch:
    - `git checkout -b BRANCH`
    - `git switch -c BRANCH`
- Switch to branch: 
    - `git checkout BRANCH`
    - `git switch BRANCH`
- Remove branch: `git branch -d BRANCH`


### **_Docker_**

**Image**

- List all images: `docker images`
- Build image: `docker build -t IMAGE_NAME:TAG PATH`
- Remove image: `docker rmi -f IMAGE_NAME`
- Remove all unused images: `docker image prune`
- Remove all images: `docker image prune -a`

**Volume**

- List all volumes: `docker volume list`
- Display information: `docker volume inspect VOLUME_NAME`
- Create a volume: `docker volume create VOLUME_NAME`
- Remove volume: `docker volume rm VOLUME_NAME`
- Remove all unmounted volumes: `docker volume prune`


**Container**

- List all running containers: `docker ps`
- List all containers: `docker ps -a`
- Log container console: `docker logs CONTAINER_NAME`
- Create and run a container: `docker run --name CONTAINER_NAME IMAGE`
- Remove container `docker rm CONTAINER_NAME`
- Remove all stopped containers: `docker container prune`

**System**

- Remove all unused data: `docker system prune`

**Docker compose**

- Create services: `docker-compose create`
- Start services: `docker-compose start`
- Create containers and immediately start services: `docker-compose up`
- pause services: `docker-compose pause`
- unpause services: `docker-compose unpause`
- Stop services: `docker-compose stop`
- Remove stopped service containers: `docker-compose rm`
- Stop and remove containers: `docker-compose down`

---


## **ZSH tips**
> What is terminal and shell?
>    - Terminal: An Interface to interact with the kernel, basically, it is a program that receive the input from and display the output to user.
>    - Shell: A program that pass the command on to the operating system for execution
> 
> What is zsh?
>    - Based on bash (bourne again shell)
>    - Default shell on mac.
> 
> Why zsh?
>    - It fits the POSIX (Portable Operating System Interface) standard, the portability of programs to different UNIX variants is guaranteed.
>    - more interactive
>    - more plugins (Oh-my-ZSH has a large community of contributors and users, you may find a bunch of useful tools in it.)

<!-- list of awesome plugins we palowans love
how color profile and understanding history is super helpful -->

1.  Theme
    - [Oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)

> Install oh-my-zsh:
> - Type in the terminal (make sure it is using the z shell): `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)`
> 
> Change the theme of your z shell (after installing oh-my-zsh):
> 1.  copy the name of the theme found [here](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)
> 2.  open file: `~/.zshrc` and paste the name of the theme to the variable `ZSH_THEME`
>     Right at the moment, the author is using the theme "[alien](https://github.com/eendroroy/alien)" (let's follow the guideline and customise, that's fuuuuuuuuuun! )

2.  Plugins (Click [here](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins) for more plugins)

    - [dirhistory](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/dirhistory)
    - [git](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git)
    - [history](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/history)
    - [ZSH Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

3.  alias  
    You can customise your own shortcut to run command for daily operation! (However, if the program is too sophisticated, you should write a shell script instead.)

    - `alias` = List all alias entries

    > Create your own alias:
    >
    > 1. `nano ~/.zshrc` (nano is a terminal editor, you are free to learn or use others like vim, neovim, emacs)
    > 2. add: `alias ts="echo 'this is my first alias'"`
    > 3. cmd + x to leave and save the changes
    > 4. type `source ~/.zshrc` to reload the profile
    > 5. type `ts` and you should see the string "this is my first alias" is printed on the terminal.

4.  Builtin powerful tab auto-completion and auto-correction.

    - For example: “/u/lo/b” --> press tab --> “/usr/local/bin”

5.  Select listed options with arrow keys

6.  `exec zsh` = Restart zsh

---

## **Path tips**

`/` = Root path

`~` = Home path

`.` = This path

`..` = Parent path

`-` = Last path

`pwd` = Print working directory (current location)

`cd PATH` = Change working directory to PATH
> For example:
> `cd ~/Document` = Change working directory to "Document" inside home directory

`ls [OPTIONS] [PATH]` = List contents of the directory (the path would be current directory if not specified.) if list all files

- `-a` = List all contents including entries starting with "." , such as .ssh, .zshrc
- `-l` = List with details
- `-h` = Display file sizes in human readable format
- `-R` = List subdirectories recursively
- `-r` = List in reverse order
- `-S` = Sort by size
- `-t` = Sort by time
- `-X` = Sort by extension

---

## **File management**

`file PATH` = Display the file type of the PATH

`stat PATH` = Display the status of file or directory

`du PATH` = Display disk usage of directories (recursively)
- `-a` = Display all the size of files along with directories
- `-s` = Display a total for each argument
- `-h` = Human readable

`mkdir [OPTIONS] DIRECTORY` = Create directory
- `-p` = Create directory with intermediate directories if they don't exist.

`touch NEW_FILE` = Create new file

`cp SOURCE DESTINATION` = Copy source(s) to destination
- `-r` = Copy recursively (used when the source is a directory)
- `-v` = Verbose mode, display process

`clipcopy FILE` = Copy a file's contents to clipboard

`cat FILE` = Print the content of file
- `cat > FILE` Write the content from standard input to the file (cmd + D to leave standard input and make changes to the file)
- `cat FILE_1 > FILE_2 ` Write the content of file_1 to
  file_2

> You may use ">>" instead of ">" in certain case, but BE CAREFUL, their behaviors are different:
>
> - `>` = Write content to the file (overwrite if the file has content);
> - `>>` = Append content to the end of the file

`chmod MODE FILE` = Change the permission of files or directory

> Mode can be symbolic or absolute, and absolute mode would be used for the demonstration. Know more about mode, see article on chmod under header "3." in References.  
> Absolute mode consists of 3 integers and each position represents the permission for User, Group, Others respectively.
> There are three kinds of permissions and each is represented by a positive integer:
>
> - read = 4
> - write = 2
> - execute = 1

> Let say we want to modify the permissions of a file named `TEST_FILE` such that it can be
>
> - read (4), written (2), executed (1) by the User;
> - read (4) and written (2) by the Group;
> - read (4) by Others.
> 
> Then its mode would be 7 (User) 6 (Group) 4 (Others) and here is the command: `chmod 764 TEST_FILE`.
> Type `stat TEST_FILE`and you should see (0764/-rwxrw-r--) at the Access field
> (In fact, symbolic mode is simple. As shown, -rwxrw-r-- is the mode of the TEST_FILE in the symbolic format. The first position is the type of the file, then we can separate the remaining into three groups. The first group represents the permission for the User, the second for the Group, and the third for the Others. The first position of each group is the permission to read (r) that file , the second the permission to write (w), the third the permission to execute (x).)

`echo "CONTENT"` = Print content to standard output (terminal by default)
- `echo "CONTENT" > FILE` Write content to file
> If you want to read the manual of command "echo", type `/bin/echo --help` instead of `echo --help`, which simply print --help to the output.

`mv PATH_1 PATH_2` = Move file or directory (this command is often used to rename file and directory.)

`rm [OPTIONS] FILE` = Remove file(s)
- `-rf` = Remove directory including its contents.

> Another command designed for removing directory in particular:  
> `rmdir [OPTIONS] DIRECTORY`
> - `-p` = Remove empty directory with its parent directory(ies)
> 
> As you can see, `rmdir -p` support recursive deletion as well. However, `rm-rf DIRECTORY` and `rmdir -p PATH` perform differently.
> - `rm-rf DIRECTORY` removes all contents of the directory(ies).
> - `rmdir -p PATH` removes directory starting from the lowest to the highest of the tree and stops if the directory being removed is not empty.
>   However, it is rarely used since it removes ONLY empty directory and requires specification when performing recursive deletion.
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

## **Git**

What is Git?
- Distributed version control system
> Two types of version control systems: Centralised version control system vs Distributed version control system

- Snapshot-based version control
> Delta-based version control vs Snapshot-based version control

> **Connect Github with SSH**
> 1. Generate SSH key
>     - Open terminal and type `ssh-keygen`
>     - Set the path to store the keys. For example: `/home/YOUR_USER_NAME_HERE/.ssh/github_paloit` (absolute path is required)
>     - Set passphrase (or just press enter to leave it blank)
>     - Then it generates two files, the one with .pub extension is the public key and the one without the private key.
> 2. Copy the public key 
>     - `cd ~/.ssh`
>     - `clipcopy github_paloit.pub`
> 3. Add the public key to your github account
>     - Direct to [Github](https://www.github.com) and login your account
>     - Open the user dropdown menu by clicking your profile pic at the top-right corner
>     - Select "Settings" and go to "SSH and GSP keys" under the "Access" header 
>     - Click "New SSH key" button, then input the "Title" of the key (as always, it should be meaningful) and paste the copied public key to the "Key" field.
> 4. Add info to the connection config file
>     - Open `~/.ssh/config` file (create if not exists) and add the following:   
>        ```
>        Host github.com  
>            IdentityFile ~/.ssh/github_paloit
>        ```
>     - Save the file.


### **_Git configuration_**

`git config [OPTIONS] [KEY] [VALUE]` = Git configuration
> NOTE: Setting up your profile is important because git have to record all the information of every single changes including WHO making the changes
> - `git config --global user.name YOUR_NAME` = Set your user name
> - `git config --global user.email YOUR_EMAIL` = Set your email
>
> You may also change the default branch name for the git repository whenever you create it locally
> - `git config --global init.defaultBranch main` = Set the default branch name
- `--global` = Global git config
- `-l` = List all config variables and values
   - `--name-only` = Show variables only
   - `--show-origin` = Show the path of the config file of all variables used in the git repository

### **_Git repository_**

`git init` =  Create a .git subdirectory and now your directory is being version controlled (every single modifications are exposed to git)


`git clone GIT_REPOSITORY` = Copy the existing git repository from remote or local


> **.gitignore**  
> (File names or patterns listed here will neither be scanned, tracked nor committed by git, this file should be set up before continue. (files and folders like libraries folders, secrets and sensitive info)  
> Learn more about .gitignore, see [page](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)  
> or you may find useful templates for different projects or languages [here](https://github.com/github/gitignore)


> **Lifecycle**  
> Each file in the working directory is either tracked or untracked.
> - untracked = New files or directories that were not in the previous snapshot AND not staged.
> - tracked = One of the following
>   - modified = Files that were in the previous snapshot AND is modified.
>   - staged = Files that is labeled to be ready for the commit.
>   - unmodified = Files that were in the previous snapshot AND is NOT modified.

### **_Get status_**

`git status [OPTIONS]` = List all uncommitted files 
- `-s` = Display a short summary

`git diff [OPTIONS]` = Show all NON-STAGED changes
- `--check` = Identify possible error
- `--staged` = Show staged changes ready for commit

`git difftool` = Show changes with external diff tool

### **_Record changes_**

`git add [OPTIONS] [PATH_or_FILE_or_DIRECTORY]` = Stage modified file(s) (A new file (untracked) will be staged (then, logically, is also tracked).)
- for example: `git add .` = Stage all of the files inside the current directory recursively
- `-A` = Stage all changes
- `-i` = Interactive mode

`git restore [OPTIONS] PATH_or_FILE_or_DIRECTORY` = Discard all changes that are not staged
- `--staged ` = Unstage staged changes

`git commit [OPTIONS]` = Commit all changes (staged by `git add`)
- `-F FILE` = use content from specific file as the commit message 
- `-m COMMENT` = Commit with the given COMMENT
- `--amend -m NEW_COMMENT` = Rewrite commit message without changing the snapshot

`git log` = View commit History

`git blame FILE` = Display when did who make what changes

### **_Remote repository_**

> Before pushing your committed changes to the github or any git repositories you have to connect yours to it. 

`git remote add [OPTIONS] REMOTE_NAME REMOTE_URL` = Connect to remote repository (custom REMOTE_NAME)
- `-f` = Immediately fetch remote repository after setup the connection

`git remote rename` = Rename remote 

`git remote remove [OPTIONS] REMOTE_NAME` = Remove the connection

`git remote` = List all remote connections
`git remote get-url REMOTE_NAME` = Display the URL of the remote
- `-all` = List all URLs of the remote


### **_Pull and Push_**

`git fetch` = Fetch from the remote repository
- `--all` = Fetch from all remote repositories

`git merge BRANCH_NAME` = Merge the target branch into the current branch

`git pull` = Fetch from the remote repository and merge the branch HEAD into the current branch (combination of `git fetch`and `git merge`)

`git push` = Update the remote repository

### **_Undo changes_**

`git reset [OPTIONS] COMMIT_ID` = Set the current HEAD to the specific COMMIT_ID and undo all the commits after that version \
`git reset [OPTIONS] HEAD~#` = Set the HEAD to the # number before the current HEAD
- `--soft` = Keep the undone commits being staged
- `--hard` = Delete all undone commits

`git revert COMMIT_ID` = Undo all the commits after the specific COMMIT_ID and create a new commit \
`git revert HEAD~#` = Undo the # number of commits before the current HEAD and create a new commit


### **_Git branch_**
`git branch [OPTIONS]` = List local branches
- `-r` = List remote-tracking branches
- `-a` = List local and remote-tracking branches
- `-v` or `-vv` = Display HEAD with subject line
- `-d BRANCH_NAME` = Delete branch 
- `-D BRANCH_NAME` = Delete branch regardless its status
- `-m OLD_BRANCH_NAME NEW_BRANCH_NAME` = Rename/move a branch

`git checkout [OPTIONS] BRANCH_NAME` = Switch to branch BRANCH_NAME
- `-b` = Create a new branch and switch to it

`git switch [OPTIONS] BRANCH_NAME` = Switch to branch BRANCH_NAME
- `-c` = Create a new branch and switch to it

### **_Merge conflict_**
> NOTE: In a normal situation, git automatically merge contents from different sources for you as long as there is no conflict at all.
> 
> However, every time we integrate commit from one branch to another, if the changes made are at the same location, a conflict occurs. For example, you and your colleague have added difference contents to the same line or a file was deleted by you whilst was modified by he/she. It's always good to avoid multiple workers editing the same file if possible, otherwise, you have to deal with merge conflict.
> 
> Since git is unable to determine which is the correct change, it cannot perform auto merge for you. So what you need to do is:
> 1. look through the changes via editor
>    - the conflicting contents are enclosed with <<<<<<< ======= >>>>>>
>    - contents before ======= are the local changes and contents after ======= the remote changes
> 2. communicate with your teammates about the conflict
> 3. keep one of the changes and remove the other, or keep both of the changes
> 4. preform `git merge`, `git commit` after editing

---

## **Docker**

Virtualisation
- Container
  - OS virtualisation
  - process isolation
- Virtual Machine
  - Hardware virtualisation  
  (Virtual environments are built on top of the hardware level, so it necessarily includes the OS, hardware config, etc in virtual environment)
  - machine isolation

Implementations of containerisation
<!-- keywords: chroot, cgroup, isolated namespaces -->
- LXC
- Docker

### **_Image_**

`docker pull IMAGE` = Download the image (default from [DockerHub](https://hub.docker.com/))

`docker images` = List all images

`docker build [OPTIONS] PATH` = Create an image from a dockerfile
- `--no-cache` = Build image without using cache
- `-t IMAGE_NAME:TAG` = Custom image name and tag

`docker rmi -f IMAGE_NAME` = Remove an image

`docker image prune [OPTIONS]` = Remove all unused images
- `-a` = Remove ALL images
- `-f` = Remove without confirmation

`docker save IMAGE | pv > IMAGE_FILE` = Export image to image file

`docker load IMAGE_FILE` = Import image from image file

### **_Volume_**

> Volume is an external filesystem connecting to container(s) that allows us storing data inside container persistently. Since it is an external system, it is not affected by the lifecycle of the container. In other words, the data is accessible after the container was stopped, killed or removed. A volume can be mounted to a container by adding `--mount` or `-v` to `docker run` or `docker create` commands.

`docker volume create VOLUME_NAME` = Create a volume

`docker volume list` = List all volumes

`docker volume inspect VOLUME_NAME` = Display information about the volume

`docker volume rm [OPTIONS] VOLUME_NAME` = Remove a volume
- `-f` = Remove without confirmation

`docker volume prune [OPTIONS]` = Remove all unmounted local volumes
- `-f` = Remove without confirmation

<!-- TODO　permission -->

### **_From image to container_**

`docker create [OPTIONS] IMAGE` = ONLY create a container from a docker image
- `--name CONTAINER_NAME` = Custom container name

`docker start CONTAINER_ID_or_NAME` = Run the containerised application (or re-start from the beginning)

`docker run [OPTIONS] IMAGE ` = Create a container from a docker image and start it immediately
- `docker run -it [OPTIONS] IMAGE TERMINAL` = Run image with interactive mode
- `--mount source=VOLUME_NAME, destination=PATH` = mount a volume to the directory inside container
- `--name CONTAINER_NAME` = Custom container name
- `--rm` = Automatically remove container when it exits

### **_Container_**

`docker ps [OPTIONS]` = List containers in progress

- `-a` = List all containers
- `--filter=FILTER` = List all filtered containers
  - some useful filter: status=created, status=running, status=exited, status=paused
- `-l` = Show the latest created container

`docker exec CONTAINER_ID_or_NAME COMMAND` = Run a command in a running container

`docker pause CONTAINER_ID_or_NAME` = Pause the running container (not to stop it)

`docker unpause CONTAINER_ID_or_NAME` = Resume the paused container (not to re-start it)

`docker stop [OPTIONS] CONTAINER_ID_or_NAME` = Terminate the running container (or kill it if no response after 10 seconds)
- `-t SECONDS` = Seconds for waiting before killing it

`docker rename CONTAINER_ID_or_NAME NEW_CONTAINER_NAME` = Rename a container

`docker rm CONTAINER_ID_or_NAME` = Remove container
- `-f` = Remove without confirmation

`docker container prune [OPTIONS]` = Remove all stopped containers
- `-f` = Remove without confirmation

`docker logs CONTAINER_ID_or_NAME` = Log container console

### **_Docker system_**

`docker [OPTIONS] df` = Display information on space usage
- `-v` = Display detailed information


`docker event` = Display real-time events for the docker daemon 

`docker system prune` = Remove all unused data
- `-a` = Remove ALL data
- `-f` = Remove without confirmation
- `--volumes` = Remove all unmounted volumes

### **_Context_**

<!-- TODO -->

### **_Docker Compose_**

> Normally, each container serves ONLY ONE purpose. That is to say, for example, a database service should not be contained in the container running the server. Since every container is a isolated run-time environment, how could we integrate multiple services into one application if multipurpose container is always a bad idea? Well, that's why docker-compose comes into play.  
> Docker-compose is the tool that allows us to write only ONE docker-compose.yml file where we define the services needed and to run all containers at the same time with just one single command.  
> (Please note that you still have to write dockerfile for your custom image.)
<!-- keywords: networking -->

```yaml
# docker-compose.yml example

version: "3"
services:
  api_service:
    build: ./api_service
    env_file:
      - .env
    environment:
      NODE_ENV: production
    ports:
      - "8100:80"
    volumes:
      - .:/code
    depends_on:
      - database

database:
  image: 'postgres:14'
  env_file:
    - docker.env
  environment:
    POSTGRES_PORT: 5432
  volumes:
    - ./pgdata:/var/lib/postgresql/data

# explanation:

version: # Top-level element, specify the version of docker-compose tool, optional

services: # Top-level element, all services go under this field

  api_service: # service name, custom but should be semantically meaningful (api_service in the above example)

    image: # prebuilt image

    build: # directory that storing docker file from which the custom image for the service is built

    env_file: # file that storing environment variables

    environment: #  you may place some environment variables under this field (no credentials or sensitives)

    ports: # map the host port to the port of container (8100 and 80 respectively in this case)

    volumes: # As said above, volumes is a filesystem external to the container storing persistent data. By mounting the current directory . to the working directory inside the container (/code in this case), we can modify the code without re-building the image.

    depends_on: # services that the current service depends on
```

> Note: Go to the directory where the file compose.yaml is stored and run the following command:

`docker-compose create` = Create services

`docker-compose start` = Start services

`docker-compose up` = Create containers and immediately start services

`docker-compose pause` = pause services

`docker-compose unpause` = unpause services

`docker-compose stop` = Stop services

`docker-compose rm` = Remove stopped service containers

`docker-compose down` = Stop and remove containers

---

## References:

0. Basic concepts  
   [Terminal, Console, Shell and Command line](https://www.geeksforgeeks.org/difference-between-terminal-console-shell-and-command-line/) \
   [Difference between shell console terminal](https://fossbytes.com/difference-between-shell-console-terminal/) \
   [Console vs terminal vs shell](https://medium.com/@Abhishek_kumar_/console-vs-terminal-vs-shell-difference-betweeen-them-b9acd3270dae)

1. General shell command  
   [Linuxcommand.org (highly recommanded)](https://linuxcommand.org/) \
   [Java T point - Linux/Unix Tutorial](https://www.javatpoint.com/linux-tutorial) \
   [Tutorials Point - UNIX / LINUX Tutorial](https://www.tutorialspoint.com/unix/index.htm) \
   [Cardiff School of Computer Science & Informatics - Linux Shell Commands](https://docs.cs.cf.ac.uk/notes/linux-shell-commands/) \
   [Computer Hope - Unix and Linux commands help](https://www.computerhope.com/unix.htm) \
   [Unix Tutorial](https://afni.nimh.nih.gov/pub/dist/edu/data/CD.expanded/AFNI_data6/unix_tutorial/index.html#)

2. ZSH  
   [oh-my-zsh plugin](https://github.com/ohmyzsh/ohmyzsh) \
   [Top 10 ZSH plugins](https://travis.media/top-10-oh-my-zsh-plugins-for-productive-developers/) \
   [ZSH vs Bash](https://www.educba.com/zsh-vs-bash/)  \
   [ZSH vs Bash - UNIX shell in comparison](https://kruschecompany.com/zsh-vs-bash-unix-shell/) \
   [Practical differences between ZSH and bash](https://apple.stackexchange.com/questions/361870/what-are-the-practical-differences-between-bash-and-zsh) \
   [Why use ZSH](https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/)

3. Commands  
   [chmod](https://tutorialshut.com/file-permissions-chmod-command/) \
   [awk](https://phoenixnap.com/kb/awk-command-in-linux)

4. User and Group management  
   [How to create users in linux using useradd command](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) \
   [Linux user group management](https://www.redhat.com/sysadmin/linux-user-group-management) \
   [local group accounts](https://www.redhat.com/sysadmin/local-group-accounts) \
   [Managing users and groups](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups)

5. Fetching data from internet  
   [wget vs crul](https://www.linuxfordevices.com/tutorials/linux/wget-vs-curl) \
   [wget](https://www.hostinger.com/tutorials/wget-command-examples/#Using_Wget_Command_to_Download_Single_Files)

6. Shell script  
   [Writing shell scripts](https://linuxcommand.org/lc3_writing_shell_scripts.php)

7. Git  
   [Git-tower](https://www.git-tower.com/) \
   [Atlassian ](https://www.atlassian.com/git) \
   [Trunk Based Development](https://trunkbaseddevelopment.com/) \
   [Tutorials Point - Git Tutorial](https://www.tutorialspoint.com/git/index.htm) \
   [Branching strategies](https://paloit2016.sharepoint.com/sites/WOW/SitePages/Source-Control-Guidelines.aspx) \
   [How to handle merge conflicts in git](https://www.freecodecamp.org/news/how-to-handle-merge-conflicts-in-git/)

8. Docker  
   [Containers vs Virtual machines](https://www.ibm.com/cloud/blog/containers-vs-vms) \
   [Docker image and container](https://phoenixnap.com/kb/docker-image-vs-container) \
   [Container lifecycle](https://linuxhandbook.com/container-lifecycle-docker-commands/) \
   [Docker volumes](https://phoenixnap.com/kb/docker-volumes)

##### Brought to you by Suen Lam ;)

