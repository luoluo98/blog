# git 基本使用

- 初始化：创建一个 git 仓库，创建之后就会在当前目录生成一个.git 的文件

  - `git init`

- 添加文件：把文件添加到缓冲区

  - `git add filename`

- 添加所有文件到缓冲区（从目前掌握的水平看，和后面加“.”的区别在于，加 all 可以添加被手动删除的文件，而加“.”不行）：

  - `git add .`
  - `git add --all`

- 删除文件

  - git rm filename

- 提交：提交缓冲区的所有修改到仓库(注意：如果修改了文件但是没有 add 到缓冲区，也是不会被提交的)

  - `git commit -m "提交的说明"`

- 查看 git 库的状态，未提交的文件，分为两种，add 过已经在缓冲区的，未 add 过的

  - `git status -b`

## 1. 拉取子模块

```
git clone 仓库地址
git submodule update --init --recursive
```

或直接

```
git clone --recursive 仓库地址
```

## 2. 相关信息查看

- 查看远程仓库地址

  - `git remote -v`

- 查看本地分支所跟踪的远程分支

  - `git branch -vv`

- 查看用户名和邮箱

  - `git config user.name`
  - `git config user.email`

- 修改用户名和邮箱

  - `git config --global user.name "NewUserName"`
  - `git config --global user.email "NewEmail"`

## 3. 删除本地/远程分支

```
git branch -d <BranchName>       git branch -d <BranchName>(强删)
git push origin --delete <BranchName>
```

## 4. 放弃本地未提交的修改

`git checkout .`

## 5. 强制同步远程仓库

```
git fetch --all
git reset --hard
```

## 6. git rebase

用于把一个分支的修改合并到当前分支

`git rebase dev`

## 7. 强推远程

`git push --force-with-lease`

## 8. 分支

- 创建分支

  `git branch "分支名"`

- 切换当前分支到指定分支

  `git checkout "分支名"`

- 创建分支并切换到创建的分支

  `git checkout -b "分支名"`

- 合并某分支的内容到当前分支

  `git merge "分支名"`

- 删除分支

  `git branch -d "分支名"`

- 如果两个分支同时进行了同一个文件的修改和提交，在 merge 时就会产生冲突，首先要手动打开文件解决冲突，再提交，就相当于进行了`merge`

  `git log --graph`

<hr>
