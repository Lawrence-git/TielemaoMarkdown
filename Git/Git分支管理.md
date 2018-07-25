# git 分支管理

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