---

预定义接口

---

## Closure 闭包

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

## ArrayAccess 数组式访问

很多框架中,明明是数据格式是对象,却能以数组的形式访问,这就是用到了数组式访问

```
class A implements ArrayAccess{
    public $name;
    public function __construct(){
        $this->name = 'one';
    }
    public function offsetSet($offset, $value) {
        $this->$offset = $value;
    }
    public function offsetExists($offset) {
        return isset($this->$offset);
    }
    public function offsetUnset($offset) {
        unset($this->$offset);
    }
    public function offsetGet($offset) {
        return $this->$offset;
    }
}
$a = new A;
// 这里会调用offsetGet
echo $a['name'];
// 这里会调用offsetSet
$a['name'] = 'two';
echo $a['name'];
// 这里会调用offsetExists
var_dump(isset($a['name']));
// 这里会调用offsetUnset
unset($a['name']);
var_dump(isset($a['name']));
```

## Traversable 遍历

检测一个类是否可以用foreach进行遍历,无法被单独实现的基本抽象接口

```
var_dump(new testClass instanceof Traversable);

//输出 false
```

## Iterator 迭代器

主要功能是用来遍历一个对象,正常遍历对象只能获取pulic属性,不能获取protect域private属性.在不了解对象结果的情况下,Iterator可以帮助我们去获取他的所有的属性内容

```
class Test implements Iterator {
    private $position = 0;
    private $array = ['nine' , 'seven'];

// 该方法主要用户项目初始化
    function rewind() {
        echo __METHOD__ . PHP_EOL;
        $this->position = 0;
    }
//用来获取当前游标所对应的值
    function current() {
        echo __METHOD__ . PHP_EOL;
        return $this->array[$this->position];
    }
//获取当前游标
    function key() {
        echo __METHOD__ . PHP_EOL;
        return $this->position;
    }
//下移游标
    function next() {
        echo __METHOD__ . PHP_EOL;
        ++$this->position;
    }
//判断是否还有值
    function valid() {
        echo __METHOD__ . PHP_EOL;
        return isset($this->array[$this->position]);
    }
}

$obj = new Test;

$obj->rewind();

while($obj->valid()){
    echo $obj->current() . PHP_EOL;
    $obj->next();
}

//输出
Test::rewind
Test::valid
Test::current
nine
Test::next
Test::valid
Test::current
seven
Test::next
Test::valid
```

## IteratorAggregate 聚合式迭代器

IteratorAggregate接口继承了Traversable,主要用来创建外部迭代器的接口,只需要实现一个getIterator方法即可
