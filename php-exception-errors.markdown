---

php异常与错误
6.12

---

## 异常

异常是Exception类的对象，在遇到无法修复的状况时抛出，实例化时可传入两个参数,分别是提示消息与数字代码，数字代码不是必须的。

```
//抛出异常示例
throw new Exception('A unexpected problem');
```

### try/catch捕获异常

```
try{
	throw new Execption('A unexpected problem');
}catch{
	echo 'problem appear';
}finally{
	//这里的代码最后都会执行
}
```

### 全局异常处理程序

全局异常处理程序用`set_exception_handler()`函数注册，可以用来捕获所有未被捕获的异常

```
set_exception_handler(function(Execption $e){
		//处理异常代码
	});
```

## 错误

可以使用`error_reporting()`函数或者在php.ini文件中使用errorreporting指令告诉PHP报告或者忽略那些错误。这两种都是使用`E*`常量来确定

生产环境和开发环境应该用不同的配置

