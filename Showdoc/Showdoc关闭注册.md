Showdoc关闭注册
===


## 关闭注册

编辑文件 `server/Application/Api/Controller/UserController.class.php`，
在`register()`里面，这个方法的开头处加上两行代码：
```
$this->sendError(10101,'不开放注册');
return  ;
```