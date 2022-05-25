# **Palowan Linux guru Cheatsheet - Z shell 1**

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [ZSH tips](#zsh-tips)
2. [Path tips](#path-tips)
3. [File management](#file-management)
4. [References](#references) 

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


**_Zsh tips_**

- Start Z-shell: `zsh`
- Restart Z-shell: `exec zsh`


**_Path tips_**

- Print working directory: `pwd`
- List all entries and sort by time in descending order: `ls -alht`
- List all entries and sort by time in ascending order: `ls -alhtr`
- Change working directory: `cd PATH`


**_File management_**

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
> - Type in the terminal (make sure it is using the z shell): `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
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
   [chmod](https://tutorialshut.com/file-permissions-chmod-command/)

##### Brought to you by Suen Lam ;)

