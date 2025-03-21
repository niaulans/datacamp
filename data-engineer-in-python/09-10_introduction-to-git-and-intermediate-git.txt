## [Introduction to Git](https://app.datacamp.com/learn/courses/introduction-to-git) and [Intermediate Git](https://app.datacamp.com/learn/courses/intermediate-git)

pwd = posisi saat ini
ls = list file and folder
cd = change directory
cd .. = back to parent directory
cd / = back to root directory
cd ~ = back to home directory
mkdir = make directory
touch = create file
rm = remove file
rm -r = remove folder
mv = move file
cp = copy file
cat = read file
head = read first 10 lines
tail = read last 10 lines
less = read file
nano = edit file
clear = clear terminal
exit = exit terminal

# Git
git --version = check git version
git config --global user.name "your name" = set user name in git
git config --global user.email "your email" = set user email in git
git config --list = check git configuration
git config --global --unset user.name = remove user name in git
git config --global --unset user.email = remove user email in git
git config --global --unset-all = remove all configuration in git

git init = initialize git repository/ create git repository
    git init mental-health-workspace
git status = check status of git repository
git add = add file to staging area
    git add . = add all file to staging area
    git add mental-health.txt
git commit -m "Adding a README." = commit file to repository

The commit structure:
- Commit 
    - Contains the metadata - author, log message, commit time
- Tree
    - Tracks the names and locations of files and directories in the repo
    - like a dictionary - mapping keys to files/directories
- Blob
    - Binary Large Object 
    - contain data of any kind
    - a compresses snapshot of a file's content

Git hash 
- Last commit : 7c35e3b
- Pseudo random number generator - hash function
- Hash allow data sharing between repositories
- Hash is unique to the data
- If two files are the same, they will have the same hash
- Git only needs to compare hashes

git log = check commit history
    - git log --oneline = check commit history in one line
    - space = move down
    - q = quit
    - git log report.md = check commit history of specific file
    - git log -2 mental_health_survey.csv = check last 2 commit history of specific file
    - git log --since='2021-01-01' = check commit history since specific date
    - git log --since='2021-01-01' --until='2021-01-02' = check commit history between specific date
    - git log --author='your name' = check commit history by specific author
    - git log --grep='keyword' = check commit history by specific keyword
    - git show 7c35e3b = check commit detail

git diff = check difference between all unstaged files and the latest commit
    - git diff mental-health.txt = check difference between an unstaged file and the latest commit
    - git diff --staged mental-healt.txt = check difference between staging area and the latest commit
    - git diff 7c35e3b 7c35e3b = check difference between two commit
    - git diff 7c35e3b..7c35e3b = check difference between two commit
    - git diff 7c35e3b..HEAD = check difference between commit and working directory
    - git diff HEAD~1..HEAD = check difference between commit and working directory
    - git diff HEAD~1..HEAD mental-health.txt = check difference between commit and working directory of specific file

git revert = revert commit (only works on commit, not on file)
    - git revert 7c35e3b = revert commit
    - git revert HEAD = revert last commit
    - git revert HEAD~1 = revert last second commit
    - git revert --no-commit 7c35e3b = revert commit without commit
    - git revert --no-commit HEAD~1 = revert last commit without commit
    - git revert --no-commit HEAD~1..HEAD = revert last commit without commit
    - git revert --no-commit HEAD~1..HEAD mental-health.txt = revert last commit without commit of specific file
    - git revert --no-commit HEAD~1..HEAD mental-health.txt report.md = revert last commit without commit of specific file
    - git revert --no-edit HEAD = revert last commit without edit commit message
    - git revert -n HEAD = revert last commit without commit (bring back to staging area)

git checkout = to revert a single file to a previous commit
    - git checkout 7c35e3b mental-health.txt = revert a single file to a previous commit
    - git checkout HEAD mental-health.txt = revert a single file to the latest commit
    - git checkout HEAD~1 mental-health.txt = revert a single file to the last second commit
    - git checkout -- mental-health.txt = revert a single file to the latest commit
    - git checkout -- . = revert all file to the latest commit

git restore --staged = unstage file
    - git restore --staged mental-health.txt = unstage file
    - git restore --staged . = unstage all file


branch = an individual version of a repo
- Git uses branches to systematically track multiple versions of a files
- In each branch:
    - Some files might be the same
    - Others might be different

Default branch = main/master

Why use branches?
- Multiple developers can work on a project simultaneously
- Compare the state of a repo between branches
- Combine contents, pushing new features to a live system
- Each branch should have a specific purpose

Identifying branches
git branch = listing all branch 
    - * = current branch
    - git branch -a = listing all branch including remote branch
    - git branch -r = listing remote branch
    - git branch -d branch_name = delete branch
    - git branch -m old_branch_name new_branch_name = rename branch
    - git branch -D branch_name = force delete branch

git switch = switch branch
    - git switch branch_name = switch branch
    - git switch -c branch_name = create and switch branch
    - git switch -c branch_name 7c35e3b = create and switch branch from specific commit

- Creating a new branch = "branching off"
- Creating speed-test from main = "branching off main"

Comparing branches
- git diff main speed-test = compare two branches

Merging branches
- source branch = branch to be merged
- destination branch = branch to merge into

- Move to the destination branch first
- git merge source branch = merge source branch into destination branch
    - git merge speed-test = merge speed-test branch into main branch
    - git merge --no-ff speed-test = merge speed-test branch into main branch without fast forward

Merge conflicts
- Conflicts:
    - Inability to resolve differences in the contents of one or more files between branches
    - Edit the same file in two branches
    - Git cannot determine which version to keep
    - Git will ask for help

Remotes = remote repositories

Cloning a repo = copying a repo from a remote server to your local machine

- git clone url = clone a repo
- git remote -v = check remote repository
    - origin = default name for remote repository
- git remote add remote_name url = add remote repository
- git remote remove remote_name = remove remote repository
- git remote rename old_name new_name = rename remote repository

Pulling from remote:
- git fetch origin = fetch changes from remote repository
- Fetch all remote branches
- Might create a new local branches if they only existed in the remote

- git fetch origin
- git merge origin

perbedaan git fetch dan git pull
- git fetch = hanya mengambil dari remote repo ke local tetapi tidak menggabungkannya ke branch yang sedang aktif
- git pull = mengambil dari remote repo ke local dan menggabungkannya ke branch yang sedang aktif


Pulling from remote:
git pull origin branch_name = pull changes from remote repository to local repository
    - git pull origin main = pull changes from remote repository to local repository

Pushing to remote:
git push remote local_branch = push changes from local repository to remote repository
    - git push origin main = push changes from local repository to remote repository

Pulling without editing:
- git pull --no-edit = pull changes from remote repository to local repository without editing commit message

Creating a new remote branch:
git push origin hotfix = push changes from local repository to remote repository and create a new branch
    - git push origin hotfix:hotfix = push changes from local repository to remote repository and create a new branch

