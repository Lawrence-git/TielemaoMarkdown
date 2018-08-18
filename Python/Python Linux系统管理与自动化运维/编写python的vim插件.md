# 编写python的vim插件

## 一键执行
一键执行功能不是一个插件，而是自定义的vim配置。
如果我们写的python代码是一些较为简单的脚本，那么，这个一键执行的功能会非常实用。

将下面的配置放在vim的配置文件当中，编写完python代码以后，按F5就实现了一键执行的功能。

该功能最实用的是编写单元测试，写完测试不用退出vim，立即执行就能看到结果，非常方便。

### vim的配置文件
```
/etc/vimrc     System wide Vim initializations.

~/.vimrc       Your personal Vim initializations.

/etc/gvimrc    System wide gvim initializations.

~/.gvimrc      Your personal gvim initializations.
```

在用户家目录`(/home/用户名)`下面有一个`.vimrc`,`/etc`下面也有一个vimrc

一般只改用户家目录下面的 `.vimrc `此配置文件只针对用户有效

一般我们只需要在`~/.vimrc`修改个人用户下的配置即可：
```
""""""""""""""""""""""
"Quickly Run
""""""""""""""""""""""
map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'java'
        exec "!javac %"
        exec "!time java %<"
    elseif &filetype == 'sh'
        :!time bash %
    elseif &filetype == 'python'
        exec "!time python2.7 %"
    elseif &filetype == 'html'
        exec "!firefox % &"
    elseif &filetype == 'go'
"        exec "!go build %<"
        exec "!time go run %"
    elseif &filetype == 'mkd'
        exec "!~/.vim/markdown.pl % > %.html &"
        exec "!firefox %.html &"
    endif
endfunc
```

### 代码补全插件snipmate
代码补全能够显著减少敲键的次数，将我们从琐碎的语法中解放出来。
例如，使用snipmate插件，输入ifmain后按tab键将会自动生成：

```python
if __name__ == '__main__':
    main()
```


### 语法检查插件 Syntastic
当我们保存源文件时，它就会执行。执行完以后，会提示我们哪些代码存在语法错误，哪些代码不符合编码规范，并给出具体的提示信息。
python代码风格默认设置为PEP8。

### 编程提示插件 jedi-vim
jedi-vim是基于jedi的自动补全插件，与snipmate不同的是，该插件更加智能。jedi-vim更贴切的称呼是“编程提示”，而不是代码补全插件。jedi是一个自动补全和静态分析的python库，直接使用pip安装即可：
 `pip install jedi`

jedi-vim这个插件是使用vim写python的标配。
并且，真正让vim写python变成一件轻松愉快的事情。

参考
《Python Linux系统管理与自动化运维》第2章
