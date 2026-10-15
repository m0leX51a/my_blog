# Git & Github

# Version control Types

- Local Version control (LVC)
    - on local host on local database through patch
- Centeralized Version control (CVC)
    - on remote server
    - only one server
- Distributed Version control
    - CVC + LVC
    - store project on cloud
    - developer clone copy of project on his local host
    - example (git & github )

## Git Flow 3 tree

1. create a remote repo on cloud 
2. upload project on repo 
3. DIV clone project on local host 
    1. working directory : have all project files 
        1. after editing add it to staging area
    2. staging area 
        1. commit collection of edits as a snapshot and commit it to git directory 
    3. git directory : only have the edits 
        1. we push this to remote 
4. DIV push edits on remote repo 

# GIT

```bash
git config --global user.name "value"
git config --global user.email "value"

pwd // current dir 
cd //change dir 
git init //Initialized empty Git repository 
git status  //current branch and additional info 
git add file_name //to add file to tracked status 
git add . OR git add -A // to add all files to tracked status
git rm --cached file_name //to remove file to tracked status

```

### ignore top secrets files

to not upload secret files to remote repo 

1. create file in local repo with “.gitignore” 
2. open it with text editor 

1. add this commands to it 

```bash
# ignore all files end with .txt
*.txt

```

### Commit

snapshot of current file with it’s content 

```bash
git commit -m "message with commit or version "
```

- after commit status will show that there is no file to commit
- but when editing on any file the status of edited file will be “modified:   file_name”
- to know what change that done on this file
    
    ```csharp
    git diff
    ```
    
- to commit direct from working environment without add to staged directory
    
    ```bash
    git commit -a -m "message with commit or version"
    ```
    
- to delete and restore file from commit directory
    
    ```bash
    git rm "file_name" // to remove file 
    
    git restore --staged file_name
    																	//to restore this file
    git restore  file_name
    
    ```
    
- to rename file
    
    ```bash
    git mv "old_name" "new_name"
    git commit -m "update name "
    ```
    
- to show history of commits
    
    ```bash
    git log 
    git log --oneline  //show commits with less info 
    ```
    
- update message of commits
    
    ```bash
    git commit -m "update name of commit" --amend
    ```
    
- to reset commit
    
    ```bash
    git reset commit_id 
    ```
    

## Branch Concept

divide working environment to allow different developers to work together by create a branch from master branch (project) and work with this copy but in his own branch and push it to the repo then if the edits  are accepted it will be merged to the main branch 

![image.png](Git%20&%20Github/image.png)

- branch commands
    
    ```bash
    git branch # to show all branches 
    git branch branch_name # to create new branch 
    git checkout branch_name  #to switch branch
    git switch branch_name  #to switch branch
    ```
    
- to merge branches with master stand in master write
    
    ```bash
    git merge -m "message of merge" branch_namegi
    ```
    
- to delete branch
    
    ```bash
    git branch -d branch_name 
    ```
    

# Github

it is remote environment  to work with team on remote repo and this repo as backup 

### Repo Type

- public : any one can see it and commit on it as default
- private :

- we can add  gitignore template to repo
- we can add a license as a signature for copy writing

### push an existing repository from the command line

```bash
git remote add origin https://github.com/m0leX51a/Test_Repo.git
git branch -M main
git push -u origin main
```

- to push all branches
    
    ```bash
    git push --all
    ```
    

- to get updates that done on remote repo to local host
    
    ```bash
    git pull origin branch_name
    git fetch 
    ```
    

## Taps

### issues

to add note for all collaborated members to tell them an issue about code or another 

by creating as issue 

### Pull requests

get permission to merge edits on external branch to master branch 

- to merge edits from forked repo to the main repo we gonna make a pull request from contribute tap

### Action

to integrate repo to another application such app store 

## Fork

to take a copy of another people repo and creating a new repo with this copy of people repo without affect on another people repo 

to request for permission to edit on project doesn’t mine 

### Clone

to download the files of repo to edit it with IDE

```bash
git clone REPO_LINK
```