# Git

下载地址:[Git - Downloads (git-scm.com)](https://git-scm.com/downloads)

## 配置

1. 打开Git Bash
2. 设置用户信息
   1. git config --global user.name "niuheshui"
   2. git config --global user.email "niuhesui@gmail.com"
   3. git config --global core.editor vim 
   4. git config --global color.ui true
3. 查看配置信息
   1. git config --global user.name
   2. git config --global user.email

## 设置命令别名

编辑~/.bash文件

```shell
# 输出git提交日志
alias git-log="git log --pretty=oneline --all --graph --abbrev-commit"
# ls
alias ll="ls -al"
```

## 本地仓库

```bash
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ touch file01.txt

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ ll
total 20
drwxr-xr-x 1 lenovo 197121 0 Jul  1 13:33 ./
drwxr-xr-x 1 lenovo 197121 0 Jul  1 13:20 ../
drwxr-xr-x 1 lenovo 197121 0 Jul  1 13:20 .git/
-rw-r--r-- 1 lenovo 197121 0 Jul  1 13:33 file01.txt
# 查看状态
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file01.txt

nothing added to commit but untracked files present (use "git add" to track)
# 将工作区文件添加到暂存区
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git add .

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   file01.txt

# 将暂存区的文件提交到仓库
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git commit -m "add file01"
[master (root-commit) a8468a8] add file01
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 file01.txt

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git status
On branch master
nothing to commit, working tree clean
# 查看提交日志
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git log
commit a8468a897c800288ba8a050f0a3813bd9b4d0efe (HEAD -> master)
Author: niuheshui <793347740@qq.com>
Date:   Sat Jul 1 13:35:28 2023 +0800

    add file01
# 修改文件
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ vim file01.txt

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file01.txt

no changes added to commit (use "git add" and/or "git commit -a")

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git add .
warning: in the working copy of 'file01.txt', LF will be replaced by CRLF the ne
xt time Git touches it

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git commit -m "update file01"
[master 191433d] update file01
 1 file changed, 1 insertion(+)

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git log
commit 191433dec63c4ee1477ad272b64e983808737f87 (HEAD -> master)
Author: niuheshui <793347740@qq.com>
Date:   Sat Jul 1 13:38:16 2023 +0800

    update file01

commit a8468a897c800288ba8a050f0a3813bd9b4d0efe
Author: niuheshui <793347740@qq.com>
Date:   Sat Jul 1 13:35:28 2023 +0800

    add file01
    
lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$
#########################################################
   				===   版本回退   === 

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git-log
* 191433d (HEAD -> master) update file01
* a8468a8 add file01

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ cat file01.txt
update count=1

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git reset --hard a8468a8
HEAD is now at a8468a8 add file01

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git log
commit a8468a897c800288ba8a050f0a3813bd9b4d0efe (HEAD -> master)
Author: niuheshui <793347740@qq.com>
Date:   Sat Jul 1 13:35:28 2023 +0800

    add file01

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ cat file01.txt

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git reset --hard 191433d
HEAD is now at 191433d update file01

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git-log
* 191433d (HEAD -> master) update file01
* a8468a8 add file01

lenovo@LENOVO MINGW64 ~/Desktop/test (master)
$ git reflog
191433d (HEAD -> master) HEAD@{0}: reset: moving to 191433d
a8468a8 HEAD@{1}: reset: moving to a8468a8
191433d (HEAD -> master) HEAD@{2}: commit: update file01
a8468a8 HEAD@{3}: commit (initial): add file01

```

## 配置远程仓库

创建 git 仓库:

```shell
mkdir sadf
cd sadf
git init 
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:niuheshui/sadf.git
git push -u origin "master"
```

已有仓库

```shell
cd existing_git_repo
git remote add origin git@gitee.com:niuheshui/sadf.git
git push -u origin "master"
```

