# pycharm导入模块报No model named
@toc

## 引言

在PyCharm中同目录下import其他模块，出现`No model named ...`的报错，但实际可以运行的情况。
这很可能是因为PyCharm并没有将当前文件目录路径加入`source_path`而引起的。

## 解决办法
对目录右键`make_directory as-->Sources Root`
![pycharm_sources_root]($resource/pycharm_sources_root.jpg)
将当前目录路径加到PyCharm的环境变量中。

## python导入模块

 * 同一目录下在`x.py`中导入`y.py`
    * `import y ` 或者 `from y import 方法/函数`
    
* 不同目录下在`x.py`中导入`y.py`

```python
import sys
sys.path.append('y模块的绝对路径')
import y
```

2018-8-15 铁乐与猫
end