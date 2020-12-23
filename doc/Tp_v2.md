#### 1、Loader自动加载

##### 1.1 register

###### 1.1.1加载composer

* 先来到入口文件 ` public/index.php`

* 然后 ` thinkphp\library\think\Loader.php`  的` register` 方法。

* 执行` spl_autoload_register `的方法  自动加载类文件 

```php
spl_autoload_register('autoload', true, true);

function autoload($classname)
{
    include_once ($classname.'.php');
}
```

* 加载composer 文件

  1. 引入` autoload_static.php`

  2. 设置  prefixLengthsPsr4 、 prefixDirsPsr4 属性的值

     ```php
     $prefixLengthsPsr4 = array (
             't' => 
             array (
                 'think\\composer\\' => 15,
             ),
             'a' => 
             array (
                 'app\\' => 4,
             ),
         );
     
     $prefixDirsPsr4 = array (
             'think\\composer\\' => 
             array (
                 0 => __DIR__ . '/..' . '/topthink/think-installer/src',
             ),
             'app\\' => 
             array (
                 0 => __DIR__ . '/../..' . '/application',
             ),
         );
     ```

###### 1.1.2 注册命名空间

* 注册 think 、traits 两个命名空间

  ````php
  self::addNamespace([
              'think'  => __DIR__,
              'traits' => dirname(__DIR__) . DIRECTORY_SEPARATOR . 'traits',
  ]);
  ````

  完成后：

  ```php
  Array
  (
      [t] => Array
          (
              [think\composer\] => 15
              [think\] => 6
              [traits\] => 7
          )
  
      [a] => Array
          (
              [app\] => 4
          )
  
  )
  Array
  (
      [think\composer\] => Array
          (
              [0] =>....tp5/vendor/composer/../topthink/think-installer/src
          )
  
      [app\] => Array
          (
              [0] => .../tp5/vendor/composer/../../application
          )
  
      [think\] => Array
          (
              [0] => ...tp5/thinkphp/library/think
          )
  
      [traits\] => Array
          (
              [0] => ...tp5/thinkphp/library/traits
          )
  
  )
  ```

* 加载类库映射文件

返回的是一个 类库对应的数组

* 加载extend 目录

  ` $fallbackDirsPsr4` 属性中赋值添加的是要加载的类库目录

##### 1.2 autoload 

* `findFile ` 找文件 借助之前设定的  prefixLengthsPsr4 、 prefixDirsPsr4 、fallbackDirsPsr4 找到文件的地址

##### 1.3 自定义文件的加载

这里可以仿照 extend.目录的加载进行添加

```
// 自动加载extend目录
self::addAutoLoadDir($rootPath . 'extend');
```



==认真跟代码 自动加载很简单==



#### 2 、配置文件的加载

##### 2.1 配置文件的种类

* 惯例配置
* 应用配置
* 模块配置
* 动态配置

##### 2.2 arrayaccess

就是提供像访问数组一样访问对象的接口

##### 2.3 Config 源码

* 加载文件`Init->load->loadFile` 
* 判断文件类型，set 进到  config  属性中
* Parse 解析配置文件（工厂模式）----- 很值得自己解析下





#### 3、容器解析

##### 3.1 单例模式

##### 3.2 注册树模式

##### 3.3 控制反转，依赖注入

控制反转：是站在容器的角度看待问题，容器控制着应用程序，由容器反向的向应用程序注入应用程序需要的外部资源。

依赖注入：是站在应用程序的角度看待问题，应用程序依赖容器创建并注入它所需要的外部资源

##### 3.4 反射

##### 3.5 自己的容器

```php
<?php
/**
 * Created by PhpStorm.
 * User: 咔咔
 * Date: 2020/9/21
 * Time: 19:04
 */

namespace container;


class Container
{
    /**
     * 存放容器
     * @var array
     */
    public $instances = [];

    /**
     * 容器的对象实例
     * @var array
     */
    protected static $instance;

    /**
     * 定义一个私有的构造函数防止外部类实例化
     * Container constructor.
     */
    private function __construct() {

    }

    /**
     * 获取当前容器的实例(单例模式)
     * @return array|Container
     */
    public static function getInstance ()
    {
        if(is_null(self::$instance)){
            self::$instance = new self();
        }

        return self::$instance;
    }

    public function set ($key,$value)
    {
        return $this->instances[$key] = $value;
    }

    /**
     * User : 咔咔
     * Notes: 获取容器里边的实例  使用反射
     * Time :2020/9/21 22:04
     * @param $key
     * @return mixed
     */
    public function get ($key)
    {
        if(!empty($this->instances[$key])){
            $key = $this->instances[$key];
        }

        $reflect = new \ReflectionClass($key);
        // 获取类的构造函数
        $c = $reflect->getConstructor();
        if(!$c){
            return new $key;
        }

        // 获取构造函数的参数
        $params = $c->getParameters();
        foreach ($params as $param) {
    		 /**
             ReflectionClass Object
            (
                [name] => container\dependency\Car
            )
             */
            $class = $param->getClass();
            if(!$class){

            }else{
                // container\dependency\Car
                $args[] = $this->get($class->name);
            }
        }
        // 从给出的参数创建一个新的类实例
        return $reflect->newInstanceArgs($args);
    }
}
这几行代码里面包含
1、单利模式 2、注册树模式 3、控制反转，依赖注入 4、反射等思想 共同组成了一个容器
```



##### 3.6 tp 中的容器

` Container::get('app')->run()->send();`

* 看看 `get->make()`



#### 4. 门面模式 



```php
  // 调用实际类的方法
    public static function __callStatic($method, $params)
    {
        return call_user_func_array([static::createFacade(), $method], $params);
    }
    
    //
    protected static function createFacade($class = '', $args = [], $newInstance = false)
    {
        $class = $class ?: static::class;

        $facadeClass = static::getFacadeClass(); //获取类名

        if ($facadeClass) {
            $class = $facadeClass;
        } elseif (isset(self::$bind[$class])) {
            $class = self::$bind[$class];
        }

        if (static::$alwaysNewInstance) {
            $newInstance = true;
        }

        //创建类实例子   返回
        return Container::getInstance()->make($class, $args, $newInstance);
    }
```

流程图：
![image-20201223182910709](/Users/duanyaqiang/github/DocManual/doc/image-20201223182910709.png)



#### 5.执行流程分析



#### 6.路由分析

先看到这里...  做项目

