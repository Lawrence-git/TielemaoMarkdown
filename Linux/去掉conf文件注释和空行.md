# 去掉conf文件注释和空行

方法一：
`[root@centos conf]# sed -e '/^.*#/d' -e '/^$/d' httpd.conf`

方法二：
`[root@centos conf]# cat httpd.conf | grep -v ^$ |grep -v ^.*#`

方法三：
`[root@centos conf]# awk '$0&&$0!~/.*#/' httpd.conf`

方法四：
`[root@centos conf]# awk '$0!~/^$/&&$0!~/.*#/' httpd.conf`