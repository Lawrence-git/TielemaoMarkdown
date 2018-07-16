[[toc]]

## 文件图标: file-icons
根据不同文件后缀名显示不同类型图标

## 标签栏根据不同文件格式显示色彩: filetype-color
在标签栏不同格式文件显示不同的颜色的标题，支持二度设置。

## 小地图：minimap
用过 Sublime Text 的友友们都知道有一个很实用的功能，就是内部编辑那里有一个小小的代码图,
这货就是补全 Atom 这个功能的,支持高亮代码,还可控，具体看内部设置。

## 实时预览HTML页面: atom-html-preview
Atom编辑器内实时预览的工具。
* 快捷键
    * 默认快捷键： CTRL + P 调用，会和内置核心插件冲突(切换文件那个)
    * 修改版快捷键： CTRL + F12 (感觉方便使用且没有冲突的快捷键)
* 修改自定义键盘映射文件：    
```
#实时浏览器调用快捷键
  'atom-text-editor':
    'ctrl-F12':'atom-html-preview:toggle'
```
atom 内置了 Dev Live Reload 这个插件；
这个插件的作用就是重新加载所有样式和规则，
有点类似 linux 的 source xx.sh 一样..即时生效。
调用快捷键是 CTRL + SHIFT + ALT +R
当然，关闭 atom 再开 atom 编辑器也能达到重新加载所有样式规则的效果......

## 同步备份atom设置到github: Sync-settings
同步 Atom 的设置文件，自定义快捷键，用户风格，初始化脚本及代码片段，还支持已安装的插件同步。

* 初始化设置
    * 进入设置中心找到该插件，点击该插件的 setting 项进入到二步设置；
    * 打开自己的 github 创建一个[personal access token](https://github.com/settings/tokens/new)
    * 复制生成的 token 序列，粘贴到插件的 sync-settings 插件的二步设置相关的 Token 项中。
    * 打开 github 的 gist 服务，创建一个 gist。
    * 将 gistID 粘贴到 sync-settings 插件的二步设置相关的 gistID 项中。
      * 注:gistID就是在你的gist页面网址最后的那串32位的16进制数字。
      * 类似39d20859457f059a84b66577c80d4d38
![](https://www.tielemao.com/wp-content/uploads/2018/05/atom-sync-settings.jpg)

* 使用方法(配置完毕后)
      * 在文档编辑页面，按下全局命令搜索面板(Ctrl+shift+p)
      * 搜索sync- ，会有可选项
          * sync-settings:backup – 备份当前配置;
          * sync-settings:restore – 恢复配置，(将gits上的配置文件)直接覆盖(本地);
          * sync-settings:view-backup – 执行备份后到线上查询备份,访问gist;
          * sync-settings:check-backup – 查询最后一次备份是否正常;
          * 备份成功和失败都有一条信息提醒。
![](https://www.tielemao.com/wp-content/uploads/2018/05/atom-sync-git.jpg)

## 自动补全 autocomplete-plus
完善自带 autocomplete,有二度设置，接下来列出的一些有二度设置

* autocomplete-python — 你懂得，更加细致
* autocomplete-paths — 实用派，路径补全
* autocomplete-html — 你懂得，更加细致
* autocomplete-bibtex — Github的markdown语法
* autocomplete-snippets — 如名字
* autocomplete-css — 你懂得，更加细致
    * less-autocompile — 实时编译
    * docblockr — 注释插件，非常的实用

## 支持html和css格式文件中代码速写插件: emmet

### html中用法

```html
a、新建空文件后,改变文本格式为html(点击atom最右下角显示的文本格式来改变),
然后在文本空白处输入标签名html、head、div、article、a…后按tab键.
b、多倍数个的同类标签: li*3
c、快速设置class/id属性默认的div标签: .clsName/idName然后tab会出来class/id为clsName/idName的div标签
d、快速设置class/id属性的任意标签: 如h1.title/h1#title出来id/class为title的h1标签
e、>嵌套符来速写嵌套关系的标签: ul>li*3>a
f、{}设置标签内value值:如ul>li{value1}+li{value2}
```

### css中用法

新建空文件后,改变文本格式为css(点击atom最右下角显示的文本格式来改变),
只需书写属性和值的第一个缩写字母+值。

```
//典型速写举例
//1、输入db后按tab键
display: block;
//2、输入dib后按tab键
display: inline-block;
//3、输入mb10
margin-bottom: 10px;
//4、输入m10%
margin: 10%;
```

更多缩写用法请查看[emmet官网](http://docs.emmet.io/cheat-sheet/)

## webserver服务器插件: atom-live-server

用法介绍:
```
ctrl-shift-3 launch live server on port 3000.
ctrl-shift-4 launch live server on port 4000.
ctrl-shift-5 launch live server on port 5000.
ctrl-shift-8 launch live server on port 8000.
ctrl-shift-9 launch live server on port 9000.
```
更多请参考[atom-live-server官网](https://atom.io/packages/atom-live-server)

关于atom的绝赞插件有好的会随时再更新。
end
