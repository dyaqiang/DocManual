#### 1、安装 composer

1、很简单按照操作步骤安装即可

2、工具idea  、设计模式、概念的理解、跟代码

#### 2、类的自动加载

##### 2.1 spl_autoload_register：类的自动加载基础原理：

```php
//PHP的内置函数处理类的自动加载
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

##### 2.2 tp自动加载类库 主要是分析composer类库的自动加载流程

自动加载流程分析：

![image-20201221205841072](/Users/duanyaqiang/github/DocManual/doc/image-20201221205841072.png)

#### 3、配置文件

##### 3.1 配置文件的种类

惯例配置：thinkphp/convention.php
应用配置：项目根目录下的config文件夹内的配置文件
模块配置：如：index/config/database.php
动态配置：在控制器或者其他行为中的配置

##### 3.2 arrayAccess：

访问数组一样访问对象的能力接口(使对象的操作看起来像操作数组一样)

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
        retu1rn $this->testDate[$offset];
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

##### 3.3 配置文件的加载应用流程分析

![image-20201222113937357](/Users/duanyaqiang/github/DocManual/doc/image-20201222113937357.png)

#### 4、容器的制作

##### 4.1 单例模式

##### 4.2 注册树模式

对象放在一个数组中

##### 4.3 依赖注入

减少代码之间的耦合（方法中把对象拿出来 作为参数传递）

##### 4.4 反射机制

```php
function rel(){
   $obj = new \ReflectionClass($obj1);
   $obj2 = $obj->newInstance();
}
//获取类
//获取属性
```

##### 4.5容器的制作

1、

```php
//制作容器
class Container
{
    /**
     * 寸放容器的数据
     * @var array
     */
    public $instances = [];

    /**
     * 对象实例
     * @var
     */
    protected static $instance;

    private function __construct()
    {
    }

    /**
     * 获取容器的实例（单例）
     * @return mixed
     */
    public static function getInstance()
    {
        if (is_null(static::$instance)) {
            static::$instance = new static;
        }
        return static::$instance;
    }

    /**
     * @return array
     */
    public function get($key)
    {
        return $this->instances[$key];
    }

    /**
     * @param array $instances
     */
    public function set($key, $value)
    {
        $this->instances[$key] = $value;
    }
}

//使用容器
class containerTest
{
    public function test()
    {
         //容器中放对象
        \di\Container::getInstance()->set("person",new \di\Persion()); 
         //容器中放对象
        \di\Container::getInstance()->set("car",new \di\Car());
        //获取容器中对象
        $obj = \di\Container::getInstance()->get("person");
        //对象的依赖注入
        $obj->buy(\di\Container::getInstance()->set("car"));
    }
}
//使用容器2
class containerTest
{
    public function test()
    {
         //容器中放对象
        \di\Container::getInstance()->set("person",new \di\Persion(new \di\Car())); //?????
         //容器中放对象
        \di\Container::getInstance()->set("car",new \di\Car());
        //获取容器中对象
        $obj = \di\Container::getInstance()->get("person");
        //对象的依赖注入
        $obj->buy(\di\Container::getInstance()->set("car"));
    }
}




```

2、

```php
<?php
/**
 * Created by PhpStorm.
 * User: yq
 * Date: 12/22/20
 * Time: 11:49 AM
 */

namespace di;
class Container
{
    /**
     * 寸放容器的数据
     * @var array
     */
    public $instances = [];

    /**
     * 对象实例
     * @var
     */
    protected static $instance;

    private function __construct()
    {
    }

    /**
     * 获取容器的实例（单例）
     * @return mixed
     */
    public static function getInstance()
    {
        if (is_null(static::$instance)) {
            static::$instance = new static;
        }
        return static::$instance;
    }

    /**
     * @return array
     */
    public function get($key)
    {
        return $this->instances[$key];
    }

    /**
     * @param array $instances
     */
    public function set($key, $value)
    {
        $this->instances[$key] = $value;
    }


    public function get($key)
    {
        if (!empty($this->instances[$key])) {
            $key = $this->instances[$key];
        }
        $reflect = new \ReflectionClass($key);
        $c = $reflect->getConstructor();
        if (!$c) {
            return new $key;
        }
        $params = $c->getParameters();
        if (empty($params)) {
            return new $key;
        }
        foreach ($params as $param) {
            $class = $param->getClass();
            if (!$class) {

            } else {
                $args[] = $this->get($class->name);
            }
        }
        return $reflect->newInstanceArgs($args);

    }
}

//容器的应用
class containerTest
{
//    public function test()
//    {
//
//        \di\Container::getInstance()->set("person",new \di\Persion());
//        \di\Container::getInstance()->set("car",new \di\Car());
//
//        $obj = \di\Container::getInstance()->get("person");
//        $obj->buy(\di\Container::getInstance()->set("car"));
//
//    }

    public function test2()
    {
        \di\Container::getInstance()->set("person","\di\person");
        \di\Container::getInstance()->set("car","\di\Car");
      
        $obj = \di\Container::getInstance()->get("person");
        $obj->buy();
    }
}

```

3、获取容器中的应用实例，需要用到反射机制。

![image-20201222125307313](/Users/duanyaqiang/github/DocManual/doc/image-20201222125307313.png)

##### 4.6 门面模式facade

静态的方法调用容器中的实例，再从实例中调用相关的方法

![image-20201222140948422](/Users/duanyaqiang/github/DocManual/doc/image-20201222140948422.png)

#### 5、框架执行流程、路由解析

##### 5.1 执行流程分析

##### 5.2 路由解析



#### 6、控制器解析

#### 7、模型、视图解析

#### 8、缓存 、异常处理架构

#### 9、架构层







