#### 1、安装 composer

很简单按照操作步骤安装即可

#### 2、类的自动加载

类的自动加载基础知识原理：

```php
spl_autoload_register('autoload', true, true);

function autoload($classname)
{
    include_once ($classname.'.php');
}

new demo1();
//1、加载类
//2、没有就去找spl_autoload_register的方法执行
//3、包含文件名 就有类了
```

tp自动加载类库 主要是分析composer类库的自动加载流程

#### 3、配置文件

1、代码中很多场景在用的时候，配置文件的作用就是容易改写

2、配置的优先级（惯例配置、应用配置、模块配置、动态配置）

3、arrayAccess：访问数组一样访问对象的能力接口(使对象的操作看起来像操作数组一样)

```php
<?php
/**
 * Created by PhpStorm.
 * User: yq
 * Date: 12/21/20
 * Time: 10:52 AM
 */

class ObjArray implements \ArrayAccess
{
    private $testDate = [
        'title' => "singwa",
    ];
    //存在与否
    public function offsetExists($offset)
    {
        echo "offsetExists" . $offset;
        return isset($this->testDate[$offset]);
    }

    //获取
    public function offsetGet($offset)
    {
        echo "offsetGet" . $offset;
        return $this->testDate[$offset];
    }

    //设置
    public function offsetSet($offset, $value)
    {
        echo "offsetSet" . $offset;
        return $this->testDate[$offset] = $value;
    }

    //销毁
    public function offsetUnset($offset)
    {
        echo "offsetUnset" . $offset;
        unset($this->testDate[$offset]);
    }
}
```

4、配置文件的加载流程

#### 4、容器的制作

1、单例模式
2、注册树模式
3、依赖注入：减少代码之间的耦合（方法中把对象拿出来 作为参数传递）

4、反射机制：

```php
function rel(){
   $obj = new \ReflectionClass($obj1);
   $obj2 = $obj->newInstance();
}
//获取类
//获取属性
//。。。。
```

5、容器的制作：

获取容器中的应用实例，需要用到反射机制。

6、门面模式facade：静态的方法调用容器中的实例，实际类中的方法

#### 5、框架执行流程、路由解析

#### 6、控制器解析

#### 7、模型、视图解析

#### 8、缓存 、异常处理架构

#### 9、架构层







