感谢廖老师！留个足迹\~个人网站：[https://jyzhu.top/](https://jyzhu.top/)

# Git Setting Up

> reference:
>
> [廖雪峰 - Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
>
> [Coursera - Server-side Development with NodeJS, Express and MongoDB](https://www.coursera.org/learn/server-side-nodejs/home/welcome)

## 0. Git Diagram

工作区(Working Directory) **--add-->** 暂存区(Staging area) **--commit-->** Master

HEAD refers to master

## 1. Global Configuration for Git

* Check to make sure that Git is installed and available on the command line, by typing the following at the command prompt:

```bash
git --version
```

* To configure your user name to be used by Git, type the following at the prompt:

```bash
git config --global user.name "Your Name"
```

* To configure your email to be used by Git, type the following at the prompt:

```bash
git config --global user.email <your email address>
```

* Check your default Git global configuration:

```bash
git config --list
```

## 2. Start

- Initializes the current folder as a git repository, just go to the folder and type the following at the prompt:

``` bash
git init
```

Now the folder will be mark as a master

- Current status of the foler:
  - current branch
  - untracked files: files that are not been added to Git repository. Use `add` to add.
  - Changes not staged for commit: still use `add` to update
  - changes to be committed: files listed below are ready to be committed

``` bash
git status
```

- See change details of a file:

```bash
git diff <file>
```

- Add files to staging area:

``` bash
git add <file(s)/folder(s)>
git add . # the "." means all files
```

- After adding some files, you can commit the current state of the folder into Git repository:

```bash
git commit
# use -m <message> to add message
```

- Show the brief log of all the commits with messages

```bash
git log --oneline
```

## 3. Checkout/Reset/Restore

- Discard changes of the last commit, and checkout the file from an older commit. **last commit -> any commit (staged)**

```bash
git checkout <commit> <file>
# commit means commit id, using first several chars is ok
```

The file changes will be automatically set into the staging area, all you need to do is submitting.

- Reset a file from staged to untracked: **staged -> untracked**

```bash
git reset <commit [default=HEAD]> <file>
```

This command essentially has deeper meaning: You can return to past versions by config the `<commit>`.

`HEAD`: current version; `HEAD^`: previous version; `HEAD^^`: previous previous version...;`HEAD-100`: 100 versions before

- Reset the staging area to untracked. (Just status, not actually discarding your changes on the files)

```bash
git reset
```

- Discard changes of an untracked file: **untracked -> last commit**

```bash
git restore <file>
# or
git checkout -- <file>
```

- Return to a future version? Still use commit id as reference in `git reset`. But how to know the commit id of a future version? Use `reflog` to see command history.

```bash
git reflog
```

### 说人话

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- <file>`或者`git restore <file>`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，用`git reset HEAD^`，不过前提是没有推送到远程库。

## Github

###0. Connect with your Github account using SSH

### 1. Connect with a remote repo

- Suppose you have a blank repo on Github, and an existing repo on your computer
- Connect local repo with a remote one:

```bash
git remote add origin git@github.com:username/repoName.git
```

`origin` is the default name of a remote repo.

- Push local repo to the remote one:

```bash
git push -u origin master
```

command `-u` is for: Branch 'master' set up to track remote branch 'master' from 'origin'.

After the first push, you don't need the `-u`.

```bash
git push origin master
```

- Show info of remote repo:

```bash
git remote -v
```

- Disconnect remote repo by name:

```bash
git remote rm <name>
```

### 2. Clone an existing remote repo

```bash
git clone git@github.com:username/repoName.git
```

Instead of ssh, https is also available.

## Branch

- Create a new branch

```bash
git branch <name>
```

- Switch to a branch

```bash
git switch <name>
```

- Show branchs:

```bash
git branch
```

Current bransh has a `*` ahead.

- Merge branch to current one:

```bash
git merge <name>
```

- Delete a branch

```bash
git branch -d <name>
```

### Solve conflicts

When you merge a branch to current branch, sometimes there are conflicts.

The conflicts will be directly showed in the files as:

```bash
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

- Just fix it manually, then `add`, then `commit`.
- Show merge logs:

```bash
git log --graph --pretty=oneline --abbrev-commit
git log --graph
```

### `stash` to save current work

```bash
git stash # save the work
git stash list # list saved works
git stash pop # restore the saved works and delete the stash
```

### `cherry-pick` to copy a specific commit

```bash
git cherry-pick <commit id>
```

### Cooperation

* 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
* 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## Tag

Tag is a name that we give to a commit.

- Set a new tag to the newest commit:

```bash
git tag <name>
# add message:
git tag -a v0.1 -m "version 0.1 released"
```

To a past commit?

```bash
git tag <name> <commit id>
```

- List all tag:

```bash
git tag
```

Show info of a tag:

```bash
git show <tagname>
```

- Delete a tag:

```bash
git tag -d <name>
```

