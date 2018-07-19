# Python配置虚拟环境

@toc

# Mact系统的python配置virtualenv

## 1.安装virtualenv

`pip3 install virtualenv` 

## 2.创建项目目录

```bash
mkdir Myproject
cd Myproject 
```

## 3.创建独立运行环境-命名

```
virtualenv --no-site-packages venv
#得到独立第三方包的环境
```


## 4.进入虚拟环境

```
source venv/bin/activate
#此时进入虚拟环境(venv)Myproject 
```

## 5.安装第三方包

```
(venv)Myproject: pip install django==1.9.8 
#此时pip的包都会安装到venv环境下，venv是针对Myproject创建的 
```



## 6.退出venv环境

`deactivate命令 `

## 7.virtualenv是如何创建“独立”的Python运行环境的呢？

原理很简单，就是把系统Python复制一份到virtualenv的环境，
用命令`source venv/bin/activate`进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。