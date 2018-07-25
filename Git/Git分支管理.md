# git 分支管理

by 铁乐猫
@toc

## 创建分支

创建本地dev分支 `local_dev`

    git branch local_dev

创建本地分支`local_dev`并切换到`local_dev`分支

     git checkout -b local_dev

## 切换分支

切换到分支`local_dev`

    git checkout local_dev

## 推送本地分支到远程分支

推送本地分支`local_dev`到远程分支 `remote_dev`并建立关联关系。

      a.远程已有remote_dev分支并且已经关联本地分支local_dev,且本地已经切换到local_dev

          git push

     b.远程已有remote_dev分支但未关联本地分支local_dev,且本地已经切换到local_dev

         git push -u origin/remote_dev

     c.远程没有有remote_dev分支并且本地已经切换到local_dev

        git push origin local_dev:remote_dev

## 删除分支

删除本地分支`local_dev`

      git branch -d local_dev

删除远程分支`remote_dev`

     git push origin  :remote_dev

     git branch -m | -M oldbranch newbranch 重命名分支，如果newbranch名字分支已经存在，则需要使用-M强制重命名，否则，使用-m进行重命名。

     git branch -d | -D branchname 删除branchname分支

     git branch -d -r branchname 删除远程branchname分支

## 查看分支

查看本地分支

      git branch

查看远程和本地分支

      git branch -a

## 合并分支

`git rebase <branch_name>`用于把一个分支的修改**合并到当前分支**。

`git merge <branch_name>`也是合并分支。

两者不同的是`git merge`会合并两个分支产生一个新commit对象【拥有两个parent】。
![git-merge]($resource/git-merge.jpg)

`git rebase` 【rebase】顾名思义**重新定义起点**，即重新定义分支指向commit对象。从而改变commit history。
![git-rebase]($resource/git-rebase.jpg)

`git rebase`也由此可以保持分支的整洁。
![git-rebase2]($resource/git-rebase2.jpg)

## 修复Bug

**问：在公司如果遇到要紧急修复的bug，怎么解决？**

* 在master分支上创建一个debug分支；
  * git branch debug 
* 切换到debug分支上进行修复；
  * git checkout debug
* 修复完毕后切换到master分支，再合并debug分支到master；
  * git checkout master
  * git merge debug 
* 删除debug分支；
  * git branch -d debug
* 切换回dev分支，继续进行开发。
  * git checkout dev

【end】