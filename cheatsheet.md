# **Palowan Linux guru Cheatsheet**

## **Table of Contents**

   <ol>
   <li>Introduction</li>
   <li>ZSH tips</li>
   <li>Path tips</li>
   <li>User and Group management</li>
   <li>File management</li>
   <li>Useful utility</li>
   <li>Fetching data from internet</li>
   <li>Docker</li>
   </ol>

   <!--

   awesome git commands section
   common docker commands
   curl wget get
   net requests like a ninja with curl -->

---

## **Introduction**

1.  What is terminal and shell?

    - Terminal: an Interface to interact with the kernel.
    <!-- Kernel: the heart of the OS helping software to communicate with hardware -->
    - Shell: Pass the command on to the operating system for execution

2.  What is zsh?

    - based on bash (bourne again shell)
    - Also, it is the default shell on mac.

3.  Why zsh?
    - Fit the POSIX (Portable Operating System Interface) standard, so the portability of programs to different UNIX variants is guaranteed.
    - more interactive
    - more plugins (Oh-my-zsh has a large community of contributors and users, you may find a bunch of useful tools in it.)

---

> **NOTES**
>
> 1.  For the sake of consistency, here is the structure of commands in this cheat sheet: `command [OPTIONS] PARAMETER`
>
>     - Commands are indicated in lowercase.
>     - Parameters are indicated in uppercase.
>     - Parameters inside square brackets "[ ]" are optional.
>
> 2.  You may know more by typing `command --help` or `man command`
>
>     - for example:
>       - `useradd --help` or `man useradd` displays the user manual of command `useradd`
>       - `man echo` for the user manual of command echo
>
> 3.  Most of the command accepts compound options
>     - for example: `ls -alt` combines three options which are `-a (list all including hidden entries )`, `-l (view in long listing format)` and `-t (sort by time)`.

---

## **ZSH tips**

<!-- list of awesome plugins we palowans love
how color profile and understanding history is super helpful -->

1.  Theme

    - Oh-my-zsh: https://github.com/ohmyzsh/ohmyzsh

> Install oh-my-zsh:
>
> - Type in the terminal (make sure it is using the z shell): `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)
> Change the theme of your z shell (after installing oh-my-zsh):
>
> 1.  copy the name of the theme found here: https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
> 2.  open file: `~/.zshrc` and paste the name of the theme to the variable `ZSH_THEME`
>     Right at the moment, the author is using the theme "alien": https://github.com/eendroroy/alien (let's follow the guideline and customise, that's fuuuuuuuuuun! )

2.  Plugins (More plugins: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins)

    - dirhistory: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/dirhistory
    - git: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git
    - history: https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/history
    - zsh Autosuggestions: https://github.com/zsh-users/zsh-autosuggestions

3.  alias  
    You can customise your own shortcut to run command for daily operation! (However, if the program is too sophisticated, you should write a shell script instead.)

    - `alias` = List all alias entries

    > Create your own alias:
    >
    > 1. `nano ~/.zshrc`
    > 2. add: `alias ts="echo 'this is my first alias'"`
    > 3. cmd + x to leave and save the changes
    > 4. type `source ~/.zshrc` to reload the profile
    > 5. type `ts` and you should see the string "this is my first alias" is printed on the terminal.

4.  Builtin powerful tab auto-completion and auto-correction.

    - for example: “/u/lo/b” --> press tab --> “/usr/local/bin”

5.  Select listed options with arrow keys

6.  `exec zsh` = Restart zsh

---

## **Path tips**

`/` = root path

`~` = home path

`.` = this path

`..` = parent path

`-` = last path

`pwd` = print working directory (current location)

`cd PATH` = Change working directory to PATH

- for example: `cd ~/Document` = change working directory to "Document" inside home directory

`ls [OPTIONS] [PATH]` = List contents of the directory (the path would be current directory if not specified.) if list all files

- `-a` = List all contents including entries starting with "." , for example: .ssh, .zshrc
- `-l` = List with details
- `-h` = Display file sizes in human readable format
- `-R` = List subdirectories recursively
- `-r` = List in reverse order
- `-S` = Sort by size
- `-S` = Sort by size
- `-t` = Sort by time
- `-X` = Sort by extension

---

## **File management**

`file PATH` = Display the file type of the PATH

`stat PATH` = Display the status of file or directory

`du PATH` = Display disk usage of directories (recursively)
`-a` = Display all the size of files along with directories
`-s` = Display a total for each argument
`-h` = human readable

`mkdir [OPTIONS] DIRECTORY` = Create directory
- `-p` = create directory with intermediate directories if they don't exist.

`touch NEW_FILE` = Create new file

`cp FILE DIRECTORY` = Copy file to directory

`cat FILE` = Print the content of file
- `cat > FILE` Write the content from standard input to the file (cmd + D to leave standard input and make changes to the file)
- `cat FILE_1 > FILE_2 ` Write the content of file_1 to
  file_2

> You may use ">>" instead of ">" in certain case, but BE CAREFUL, their behaviors are different:
>
> - `>` = Write content to the file (overwrite if the file has content);
> - `>>` = Append content to the end of the file

`chmod MODE FILE_NAME` = Change the permission of files or directory

> Mode can be symbolic or absolute, and absolute mode would be used for the demonstration. Know more about mode, see article on chmod under header "3." in References.  
> Absolute mode consists of 3 integers and each position represents the permission for Owner, Group, Others respectively.
> There are three kinds of permissions and each is represented by a positive integer:
>
> - read = 4
> - write = 2
> - execute = 1

> Let say we want to modify the permissions of a file named `TEST_FILE` such that it can be
>
> - read (4), written (2), executed (1) by the Owner;
> - read (4) and written (2) by the Group;
> - read (4) by Others,
>   its mode would be 7 (Owner) 6 (Group) 4 (Others),  
>   and here is the command: `chmod 764 TEST_FILE`
>   type `stat TEST_FILE`and you should see (0764/-rwxrw-r--) at the Access field

`echo "CONTENT"` = Print content to standard output (terminal by default)
- `echo "CONTENT" > FILE` Write content to file
> if you want to read the manual of command "echo", type `/bin/echo --help` instead of `echo --help`, which simply print --help to the output.

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
password file (All passwords are encrypted): `/etc/shadow`  
Group info file: `/etc/group`

`getent passwd` = List all users

`getent group` = List all groups

`sudo useradd [OPTIONS] USER_NAME` = Create user

- `-d PATH` = Specify home directory for the new user
- `-m` = Create home directory for the new user if it does not exist
- `-s PATH` = Set user's shell (Default shell is /bin/bash)

`passwd USER_NAME` = Change user's password

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
for example:

1.  `ls -al ~ | grep .git`
2.  `ls -alr ~ | more`

<!-- `awk` TODO -->

`grep [OPTIONS] "PATTERN" FILE_NAME` = Search the file for lines containing particular pattern and print it on terminal (or you may use I/O redirection symbol: ">" and ">>" to write the results to file)

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

> for example:
>
> - `ls -alR ~| grep .json | sort -r | more`

<!-- `xargs` TODO -->

`eval $COMMAND_VARIABLES` = Run shell command stored in variable(s)
for example:

1.  `command="ls -alR | grep .json | more"` \
     run `eval $command` is equivalent to run `ls -alR | grep .json | more"`

`export ENV_VARIABLE=VALUE` = Assign value to a environment variable which can be accessed from all the shells and processes under the same original shell.

`set VARIABLE=VALUE` = Assign value to a local variable.

> Know more about the difference between `export` and `set`, see: https://www.theunixschool.com/2010/04/what-is-difference-between-export-set.html

`source PATH` = Load a profile to current session

---

## **Shell Scripting**

> Create a shell script
>
> 1.  Create a file with .sh extension. e.g. hello.sh
> 2.  Type `#!/bin/zsh` at the very beginning of the file telling the loader executes this script program using zsh (it would be executed using bash if you type `#!/bin/bash` instead.)
> 3.  Type `echo "Hello World"`(or any other commands you want)
> 4.  Save the file
> 5.  Run `chmod +x hello.sh` in order to give user permission to execute the script (x = execute in symbolic mode)
> 6.  Run the script ./hello.sh and you will see "Hello World" .

`$@` = An array construct
`$1, $2,...` = parameters that pass to the command (1-based indexing, 0 is the command itself)

### **_Conditional statements_**

`test EXPRESSION` = `[ EXPRESSION ]` = Evaluates the expression and returns 0 (true) or 1 (false)  
 The following operators would help you constructing expression:

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

### **_Loop statement_**

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
- `-k` =
- `-O` = Save data to a file with it original name
- `-o FILE_NAME` = Save data to a file with custom file name
- `--retry NUMBER` = Set number of retries
- `-#` = Show progress bar
- `-t` =
- `-x` =

`wget URL` = Save the fetched data in a file

- `-b` = Download in the background
- `-c` = Resume download
- `-i FILE_NAME` = Download all files from the urls listed in the FILE
- `-O FILE_NAME` = Custom file name
- `-P PATH/` = Save to specific directory
- `-t NUMBER` = Set number of retries
- `--progress=SYMBOL_TYPE` = Show progress bar (SYMBOL_TYPE can be dot or bar)

<!-- > NOTE: Install wget using Homebrew: `brew install wget`   -->
<!-- > Note: Basically, the differences between `curl` and `wget` are:
>
> 1. `curl` supports a lot of protocol, wget supports only http and ftp
> 2. -->

---

## **Git**

Version control systems
- Centralised version control system
  - Subversion(SVN)
  - Concurrent Version System(CVS)
  <!-- keywords: hierarchical models -->
- Distributed version control system
  - Git
  - Mercurial
  - Bazaar 
  - Darcs

- Delta-based version control
- Snapshot-based version control

What is Git?

`git clone`
`git fetch`
`git pull`
`git diff`
`git add`
`git commit`
`git push`
`git branch`
`git checkout`

---

## **Docker**

Virtualization
- Container
  - OS virtualization
  - process isolation
- Virtual Machine
  - Hardware virtualization  
  (Virtual environments are built on top of the hardware level, so it necessarily includes the OS, hardware config, etc in virtual environment)
  - machine isolation

Implementations of containerisation
<!-- keywords: chroot, cgroup, isolated namespaces -->
- LXC
- Docker

### **_Image_**

`docker pull IMAGE` = Download the image

> DockerHub: https://hub.docker.com/

`docker images` = List all images

`docker build PATH [OPTIONS]` = Create an image from a dockerfile
- `--no-cache` = Build image without using cache
- `-t IMAGE_NAME:VERSION` = Custom image name and version

`docker rmi -f IMAGE_NAME` = Remove an image

`docker image prune [OPTIONS]` = Remove all unused images
- `-a` = Remove ALL images
- `-f` = Remove without confirmation

`docker save IMAGE | pv > IMAGE_FILE` = Export image to image file

`docker load IMAGE_FILE` = Import image from image file

### **_Volumes_**

> Volume is an external filesystem connecting to container(s) that allows us storing data inside container persistently. Since it is an external system, it is not affected by the life cycle of the container. In other words, the data is accessible after the container was stopped, killed or removed. A volume can be mounted to a container by adding `--mount` or `-v` to `docker run` or `docker create` commands.

`docker volume create VOLUME_NAME` = Create a volume

`docker volume list` = List all volumes

`docker volume inspect VOLUME_NAME` = Display information about the volume

`docker volume rm [OPTIONS] VOLUME_NAME` = Remove a volume
- `-f` = Remove without confirmation

`docker volume prune [OPTIONS]` = Remove all unmounted local volumes
- `-f` = Remove without confirmation

### **_From image to container_**

`docker create [OPTIONS] IMAGE` = ONLY create a container from a docker image
- `--name IMAGE_NAME` = Custom image name

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

`docker stop [OPTIONS] CONTAINER_ID_or_NAME` = Terminate the running container (or kill it if 10 seconds is over since the signal sent to the process)

- `-t SECONDS` = Seconds for waiting before killing it

`docker rename CONTAINER_ID_or_NAME NEW_CONTAINER_NAME` = Rename a container

`docker rm CONTAINER_ID_or_NAME` = Remove container
- `-f` = Remove without confirmation

`docker container prune [OPTIONS]` = Remove all unused containers
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

### **_Docker Compose_**

> Normally, each container serves ONLY ONE purpose. That is to say, for example, a database service should not be contained in the container running the server. Since every container is a isolated run-time environment, how could we integrate multiple services into one application if multipurpose container is always a bad idea? Well, that's why docker-compose comes into play.  
> Docker-compose is the tool that allows us to write only ONE compose.yaml file where we define the services needed and to run all containers at the same time with just one single command.  
> (Please note that you still have to write dockerfile for your custom image.)

```yaml
# compose.yaml example

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

1. General shell command  
   https://linuxcommand.org/ (highly recommanded)  
   https://www.javatpoint.com/linux-tutorial  
   https://www.tutorialspoint.com/unix/index.htm  
   https://docs.cs.cf.ac.uk/notes/linux-shell-commands/
   https://www.computerhope.com/unix.htm
   https://afni.nimh.nih.gov/pub/dist/edu/data/CD.expanded/AFNI_data6/unix_tutorial/index.html#

2. Zsh  
   oh-my-zsh plugin: https://github.com/ohmyzsh/ohmyzsh  
   top 10 zsh plugins: https://travis.media/top-10-oh-my-zsh-plugins-for-productive-developers/  
   zsh vs bash: https://www.educba.com/zsh-vs-bash/  
   https://kruschecompany.com/zsh-vs-bash-unix-shell/  
   practical differences between zsh and bash: https://apple.stackexchange.com/questions/361870/what-are-the-practical-differences-between-bash-and-zsh  
   why use zsh: https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/

3. Commands  
   chmod: https://tutorialshut.com/file-permissions-chmod-command/  
   awk: https://phoenixnap.com/kb/awk-command-in-linux

4. User and Group management  
   https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/  
   https://www.redhat.com/sysadmin/linux-user-group-management  
   https://www.redhat.com/sysadmin/local-group-accounts  
   https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups

5. Fetching data from internet  
   wget vs crul: https://www.linuxfordevices.com/tutorials/linux/wget-vs-curl
   wget: https://www.hostinger.com/tutorials/wget-command-examples/#Using_Wget_Command_to_Download_Single_Files

6. Shell script  
   https://linuxcommand.org/lc3_writing_shell_scripts.php

7. Git

8. Docker  
   containers vs virtual machines: https://www.ibm.com/cloud/blog/containers-vs-vms
   docker image and container: https://phoenixnap.com/kb/docker-image-vs-container  
   container lifecycle : https://linuxhandbook.com/container-lifecycle-docker-commands/  
   docker volumes: https://phoenixnap.com/kb/docker-volumes

##### Brought to you by Suen Lam ;)
