# Git学习

## Git的工作流程

一般工作流程：

* 克隆Git资源作为工作目录
* 在克隆的资源上添加或修改文件
* 如果其他人修改了，你可以更新资源
* 在提交前查看修改
* 提交修改
* 在修改完成后，如发现错误，可以撤回提交并再次修改并提交

## Git工作区、暂存区、版本库

* **工作区(workspace)**：在电脑里能看到的目录
* **暂存区(stage/index)**：一般存放在`.git`目录下的index文件中，所以暂存区也可称为索引
* **版本库**：工作区有一个隐藏目录`.git`，不算工作区，而是Git的版本库

## Git创建仓库

### **git init**

用于初始化Git仓库，是使用Git的第一个命令

在执行完成`git init`后，Git仓库会生成一个`.git`目录，其中包含了资源的所有元数据，其他的项目目录保持不变

**使用方法**

使用当前目录作为Git仓库，只需将它初始化

`git init`

执行完成后将会在当前目录生成一个`.git`目录

***

使用指定目录作为Git目录

`git init newrepo`

执行完成后在`newrepo`目录下会出现`.git`目录

***

如果当前目录下有文件想要纳入版本控制，需要先用`git add`命令告诉Git开始对这些文件进行跟踪，然后提交

```
git add *.c
git add README
git commit -m '初始化版本'
```

这些命令会将目录下.c结尾的和README文件提交到仓库中

### **git clone**

用于从现有Git仓库中拷贝项目

* 克隆仓库 `git clone <repo>`

* 克隆到指定目录 `git clone <repo> <directory>`

### **git config**

`git config --list` 显示当前的Git配置信息

`git config -e` 编辑Git配置文件(针对当前仓库)

`git config -e --global` 针对系统上所有仓库

设置提交时的用户信息

`git config --global user.name "Red"`

`git config --global user.email xxx@xxx.com`

## Git基本操作

![Git命令](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

### 创建仓库

* `git init`初始化仓库
* `git clone`克隆仓库

### 提交与修改

#### git add

`git add`将文件添加到**暂存区**

* `git add [file1] [file2] ...`添加一个或多个文件
* `git add [dir]`将指定目录及其子目录添加到暂存区
* `git add .`将目录下所有文件添加到暂存区

文件修改后，一般需要进行`git add`操作，从而保存历史版本

#### git status

`git status`查看仓库状态，显示有变更的文件

通常用`-s`参数来获得简短的输出结果

#### git diff

`git diff`比较文件的不同，即比较文件在暂存区和工作区的差异

* `git diff`尚未缓存的改动
* `git diff --cached`查看已缓存的改动
* `git diff HEAD`查看已缓存的与未缓存的所有改动
* `git diff --stat`显示摘要而非整个diff

例如：

* 显示暂存区和工作区的差异：

  `git diff [file]`

* 显示暂存区和上一次提交的差异：

  `git diff --cached [file]`

  `git diff --staged [file]`

* 显示两次提交之间的差异：

  `git diff [first-branch]...[second-branch]`

#### git commit

`git commit`用于将暂存区内容添加到本地仓库中

* `git commit -m [message]`  提交暂存区到本地仓库中，message可以是一些备注信息

* `git commit [file1] [file2] ... -m [message]`  提交暂存区的指定文件到仓库区
* `git commit -a`  **\-a**参数设置修改文件后不需要执行`git add`命令，直接提交

#### git reset

用于回退版本，可以指定退回某一次提交的版本

* `git reset [--soft | --mixed | --hard] [HEAD]`      

**--mixed**为默认参数，可以不带该参数，用于重置暂存区的文件与上一次的提交保持一致，工作区文件内容保持不变

例如：

```
git reset HEAD^ #回退所有内容到上一个版本
git reset HEAD^ hello.php #回退hello.php文件的版本到上一个版本
git reset 052e #回退到指定版本
```

**--soft**用于回退到某个版本

例如：

```
git reset --soft HEAD~3 #回退到上上上个版本
```

**--hard**用于撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前所有的信息提交

例如：

```
git reset --hard HEAD~3 #回退到上上上个版本
git reset --hard bae128 #回退到某个版本回退点之前的所有信息
git reset --hard origin/master #将本地的状态退回到和远程的一样
```

**注意：--hard会删除回退点之前的所有信息**

***

**HEAD说明**

* HEAD表示当前版本
* HEAD^ 上一个版本
* HEAD^^ 上上个版本
* 以此类推

使用数字表示

- HEAD~0 表示当前版本
- HEAD~1 表示上一个版本
- HEAD^2 上上个版本
- 以此类推

#### git rm

用于删除文件

``` 
git rm -f <file> #如果删除之前修改过且已经放到暂存区，需要使用强制选项-f
git rm --cached <file> #仅从跟踪清单中删除，仍保留在当前工作目录中
git rm -r * #递归删除，删除目录下所有文件和子目录
```

#### git mv

用于移动或者重命名文件、目录、软连接

`git mv [file] [newfile]`
