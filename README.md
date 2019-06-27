# Git学习笔记

## 1. 集中式和分布式版本控制系统
- - -
* Git是分布式版本控制系统
* CVS、SVN、VSS等为集中式版本控制系统
* 集中式VS分布式：  
1. 集中式版本控制系统，版本库集中存放在中央服务器，必须要联网才能工作,开发人员电脑上没有版本库。   
2. 分布式版本控制系统，没有“中央服务器”，每个开发人员电脑上都有一个完整的版本库，无需联网也能工作。
3. 分布式优势：无需联网，分支管理，安全性更高，若“中央服务器”故障，可从其他人电脑复制版本库。  
3. 主要区别在于历史版本库的存放，集中式历史版本只存在于中央服务器，而分布式则是每个本地库都有历史版本库。

## 2. 相关概念 
- - -
1. 工作区：电脑中能看到的目录。
2. 版本库：工作区有一个隐藏目录`.git`，这是Git的版本库 。
3. 暂存区：版本库中存放了很多东西，最重要的就是称为 stage 的暂存区。`git add`实际上就是把文件修改添加到暂存区，`git commit` 只负责提交暂存区的内容。
4. 分支：创建版本库时，git自动为我们创建了一个 master 分支。

## 3. 安装Git
- - -
* Windows系统  

[官网下载](https://git-scm.com/downloads)，安装完成后，如在开始菜单找到“Git”->“Git Bash”，打开出现命令窗口及安装成功。

* Git配置
``` 
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
注：  `--global`参数表示你这台机器上所有的Git仓库都会使用这个配置；也可以对某个仓库指定用户名和 Email地址。

## 4. Git命令
- - -
#### 创建版本库
1. 选择一个合适目录(Windows系统请确保目录名（包括父目录）不包含中文)
* a. `$ mkdir <name>`，创建库名
* b. `$ cd <name>`，进入库内  
2. 在当前目录初始化库
```
$ git init
```

#### 把文件添加到版本库
```
$ git add <file-name>
$ git commit -m "description"
```
注：可以多次`git add `，最后一次`git commit`

#### 查看工作区当前状态
```
$ git status
```
#### 查看修改内容
```
$ git diff
```

#### 查看提交日志
```
$ git log
```

#### 查看简化提交日志
```
$ git log --pretty=oneline
````
**注：**  
1. ` 1094adb... `表示 commit id(版本号)。
2. `HEAD`，当前版本；`HEAD^`，上个版本 ；`HEAD^^`，上上个版本；`HEAD~100`，往上100个版本。
#### 查看命令历史
```
$ git reflog
````

#### 版本回退
1.回退到上一个版本
```
$ git reset --hard HEAD^
````
2.回退到指定版本(需要 commit id)
```
$ git reset --hard 1094a
````

#### 管理修改
- - -
git 管理的是修改，而不是文件。如果修改后不 `git add`，那 `git commit` 的时候就不会被提交。

* 查看工作区和版本库里最新版本的区别
```
git diff HEAD -- fileName
```
* 比较所有修改文件的不同
```
$ git diff
```
* 比较指定修改文件的不同
```
$ git diff <file-name>
```
#### 撤销修改
- - -
* 场景1：工作区已修改但未add到暂存区，用命令 `$ git checkout -- <file-name>`。
* 场景2：工作区修改且已经 `git add` 到暂存区，分两步，第一步用命令 `$ git reset HEAD <file-name>`，就回到了场景1，然后再按场景1操作。
* 场景3：已经commit但没有推送到远程库，使用版本回退。



#### 删除文件
- - -
文件已经 commit，可直接手动删除或使用 `rm  filename` 命令删除。
此时有两个选择：
1. 确实要从版本库中删除该文件：使用`git rm filename` 命令，并且 `git commit`。
2. 删错了，要恢复:  使用`git checkout -- filename` 命令 ，就可以把误删的文件恢复到最新版本。

注： 从来没有添加到版本库的文件，删除后无法恢复。


## 远程仓库
- - -
本地 git 仓库与 GitHub 仓库之间的传输通过 SSH 加密的

#### 创建SSH Key
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```  
注：在用户主目录下， .SSH 目录里面有 id_rsa 和 id_rsa.pub 两个文件分别是 SSH 的私钥(不可泄露)和公钥(可公开)。

#### 关联远程仓库
```
$ git remote add origin <url>
```
 
注：远程库默认名字为 origin

#### 删除已经关联的远程库
```
git remote rm  name
```

#### 推送到远程仓库
1.第一次推送到 master 分支  
```
$ git push -u origin master
```
2.后续推送
```
$ git push origin master
```

#### 从远程库克隆
```
$ git clone <url>
```  

#### 查看已经连接的远程库
```
$ git remote
```
#### 详细查看远程库详情
```
git remote -v
```

#### 在本地创建和远程分支对应的分支
```
$ git checkout -b <branch-name> origin/<branch-name>
```
#### 建立本地分支和远程分支的关联
```
$ git branch --set-upstream <branch-name> origin/<branch-name>
```  
或  
```
$ git branch --set-upstream-to=origin/<branch-name> <branch-name>
```
#### 从远程抓取分支
```
$ git pull
```


## 分支管理
- - -
#### 查看分支
```
$ git branch
```
#### 创建分支
```
$ git branch <name>
```
#### 删除分支
```
$ git branch -d <branch-name>
```
#### 强行删除分支
```
$ git branch -D <branch-name>
```
#### 切换分支
```
$ git checkout <name>
```
#### 创建+切换分支
```
$ git checkout -b <name>
```
#### 合并某分支到当前分支
```
$ git merge <name>
```
注：通常进行分支合并时，如果可以，Git会使用`Fast forward`模式，这种模式删除分支后，分支历史信息会丢失。  

#### 禁用 Fast forward 模式合并分支
```
$ git merge --no-ff -m "description" <name>
```  
注：`--no-ff`表示禁用`Fast forward`模式，Git 就会在 merge 时生成一个新的 commit，这样可以从分支历史上看出分支信息。

#### 查看分支合并图
```
$ git log --graph
```  

简洁查看
```
$ git log --graph --pretty=oneline --abbrev-commit
```

#### 保存工作现场
```
$ git stash
```
#### 查看保存的工作现场
```
$ git stash list
```
#### 恢复工作现场
```
$ git stash apply
```
#### 删除工作现场
```
$ git stash drop
```
#### 恢复并删除工作现场
```
git stash pop
```

## 标签管理
- - -
发布一个版本时，我们通常现在版本库中打一个标签(tag)，这样就确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所有，标签是版本库的一个快照。

#### 创建标签
```
$ git tag <tag-name>
```
默认标签是打在在最新commit上

#### 根据 commit id 打标签
```
$ git tag <tag-name> commit-id
```

#### 创建带有说明的标签
```
$ git tag -a <tag-name> -m "description" 
```

#### 查看所有标签
```
$ git tag
```  
#### 查看对应标签的信息
```
$ git show <tag-name>
```
注：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

#### 推送某个标签到远程
```
$ git push origin <tag-name>
```
#### 一次性推送全部尚未推送的标签到远程
```
git push origin --tags
```
#### 删除一个本地标签
```
$ git tag -d <tag-name>
```
#### 删除一个远程标签
先从本地删除  
```
$ git tag -d <tag-name>
```
再从远程删除
```
$ git push origin :refs/tags/<tag-name>
```


