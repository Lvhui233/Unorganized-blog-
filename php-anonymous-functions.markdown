---

匿名函数

---

匿名函数也叫闭包函数,允许临时创建一个没有名称的临时函数,目前是通过Closure类实现的

## 从父作用域继承变量

继承父作用域中的变量需要用use关键字

### use传值赋值

函数继承的父作用域的变量值是函数定义时变量的值，而不是函数调用时变量的值

```
$message = 'hello';
$example = function() use ($message) {
	var_dump($message);
}
$message = 'world';
$example(); //输出 string(5) "hello"
```

### user传址赋值

父作用域的变量改变直接影响到匿名函数

```
$message = 'hello';
$example = function() use (&$message) {
	var_dump($message);
}
$message = 'world';
$example(); //输出 string(5) "world"
```

### 进行常规传参

```
$message = 'world';
$example = function ($arg) use ($message) {
    var_dump($arg . ' ' . $message);
};
$example("hello"); //输出 string(11) "hello world"
```