# **Palowan Guru Cheatsheet - Git CLI**

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [Git](#git)
2. [Git configuration](#git-configuration)
3. [Git repository](#git-repository)
4. [Get status](#get-status)
5. [Remote repository](#remote-repository)
6. [Pull and Push](#pull-and-push)
7. [Undo changes](#undo-changes)
8. [Git branch](#git-branch)
9. [Merge conflict](#merge-conflict)
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
>       - `git push --help` or `man git push` displays the user manual of command `git push`

---

## **TL;DR**

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

---

## **Git**

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

---

## **Git configuration**

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

---

## **Git repository**

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

---

## **Get status**

`git status [OPTIONS]` = List all uncommitted files 
- `-s` = Display a short summary

`git diff [OPTIONS]` = Show all NON-STAGED changes
- `--check` = Identify possible error
- `--staged` = Show staged changes ready for commit

`git difftool` = Show changes with external diff tool

---

## **Record changes**

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

---

## **Remote repository**

> Before pushing your committed changes to the github or any git repositories you have to connect yours to it. 

`git remote add [OPTIONS] REMOTE_NAME REMOTE_URL` = Connect to remote repository (custom REMOTE_NAME)
- `-f` = Immediately fetch remote repository after setup the connection

`git remote rename` = Rename remote 

`git remote remove [OPTIONS] REMOTE_NAME` = Remove the connection

`git remote` = List all remote connections
`git remote get-url REMOTE_NAME` = Display the URL of the remote
- `-all` = List all URLs of the remote

---

## **Pull and Push**

`git fetch` = Fetch from the remote repository
- `--all` = Fetch from all remote repositories

`git merge BRANCH_NAME` = Merge the target branch into the current branch

`git pull` = Fetch from the remote repository and merge the branch HEAD into the current branch (combination of `git fetch`and `git merge`)

`git push` = Update the remote repository

---

## **Undo changes**

`git reset [OPTIONS] COMMIT_ID` = Set the current HEAD to the specific COMMIT_ID and undo all the commits after that version \
`git reset [OPTIONS] HEAD~#` = Set the HEAD to the # number before the current HEAD
- `--soft` = Keep the undone commits being staged
- `--hard` = Delete all undone commits

`git revert COMMIT_ID` = Undo all the commits after the specific COMMIT_ID and create a new commit \
`git revert HEAD~#` = Undo the # number of commits before the current HEAD and create a new commit

---

## **Git branch**
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

---

## **Merge conflict**
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

## References:
1. [Git-tower](https://www.git-tower.com/)
2. [Atlassian ](https://www.atlassian.com/git)
3. [Trunk Based Development](https://trunkbaseddevelopment.com/)
4. [Tutorials Point - Git Tutorial](https://www.tutorialspoint.com/git/index.htm)
5. [Branching strategies](https://paloit2016.sharepoint.com/sites/WOW/SitePages/Source-Control-Guidelines.aspx)
6. [How to handle merge conflicts in git](https://www.freecodecamp.org/news/how-to-handle-merge-conflicts-in-git/)

#### Brought to you by Suen Lam ;)

