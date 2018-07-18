## [git rebase](https://www.cnblogs.com/chenweichu/p/5852335.html)

git rebase用于把一个分支的修改合并到当前分支

git merge <branch_name>也是合并分支，与git rebase不同的是git merge会合并两个分支产生一个新commit对象【拥有两个parent】。

git rebase 【rebase】顾名思义重新定义起点，即重新定义分支指向commit对象。从而改变commit history。

![](https://images2015.cnblogs.com/blog/736876/201609/736876-20160908113135082-2046493307.jpg)

![](https://images2015.cnblogs.com/blog/736876/201609/736876-20160908113221519-1143791214.jpg)

![](https://images2015.cnblogs.com/blog/736876/201609/736876-20160908113239316-1733271263.jpg)