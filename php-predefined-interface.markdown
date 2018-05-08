---

预定义接口

---

## Closure(闭包)

代表一个匿名函数的类，我们所用到的匿名函数，都是Closure的一个实例，主要方法有:bind ,bindTo

1. bindTo
他有两个参数

`public Closure Closure::bindTo ( object $newthis [, mixed $newscope = 'static' ] )`

$newthis是指需要绑定的对象，newscope是设置类的作用域
第一个参数可以设置null或者是一个对象的实例，主要取决于我们要在函数中操作的属性和方法是否是静态的，如果是静态的，那么第一个参数必须设置null，且第二个参数要设置一个类作用域

```
class A {
	private $number = 'one';
}

$newfunc = function(){
	$this->number = 'two';
	echo $this->number;
}
$a = new A;
$func = $newfunc->bindTo($a,A::class);
$func();
//输出 'two'

//操作静态属性
class A {
	private static $number = 'one';
}
$newfunc function(){
	self::number = 'two';
	echo self::number;
}
$func = $newfunc->bindTo(null,A::class);
$func();
//输出 'two'
```

2. 静态方法 bind
他有三个参数

`public static Closure Closure::bind ( Closure $closure , object $newthis [, mixed $newscope = 'static' ] )`

$closure是指我们需要绑定的匿名函数，$newthis是指需要绑定的对象，newscope是设置类的作用域
bind其实和bindTo用法一致，只不过在于使用的方式不一样而已

```
class A {
	private $number = 'one';
}

$a = new A;

$func = Closure::bind(function(){
	$this->number = 'two';
	echo $this->number;
	},$a,A::class);

$func();
//输出 'two'
```