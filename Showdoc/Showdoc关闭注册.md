Showdoc关闭注册
===


## 关闭注册

编辑文件 `server/Application/Api/Controller/UserController.class.php`，
在`register()`里面，这个方法的开头处加上两行代码：
```
$this->sendError(10101,'不开放注册');
return  ;
```
如图：
![showdoc_no_reg]($resource/showdoc_no_reg.jpg)

访问注册页面，点击注册时弹出不开放注册的提示
![showdoc_no_reg2]($resource/showdoc_no_reg2.jpg)

