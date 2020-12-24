##### 1、自动加载

自动加载小项目可以自己搞一下，大项目最好按照PSR4 规范走，脉络清晰

这里主要理解的是对` spl_autoload_register`  方法的理解

```php
class Loader
{
    /* 路径映射 */
    public static $vendorMap = array(
        'app' => __DIR__ . DIRECTORY_SEPARATOR,//项目的根目录
    );

    /**
     * 自动加载器
     */
    public static function autoload($class)
    {
        $file = self::findFile($class);
        if (file_exists($file)) {
            self::includeFile($file);
        }
    }

    /**
     * 解析文件路径
     */
    private static function findFile($class)
    {
        $vendor = substr($class, 0, strpos($class, '\\')); // 顶级命名空间
        $vendorDir = self::$vendorMap[$vendor]; // 文件基目录
        $filePath = substr($class, strlen($vendor)) . ".php"; // 文件相对路径
        return strtr($vendorDir . $filePath, '\\', DIRECTORY_SEPARATOR); // 文件标准路径


    }

    /**
     * 引入文件
     */
    private static function includeFile($file)
    {
        if (is_file($file)) {
            include $file;
        }
    }
}

-------------------------------------
namespace app;

class demo1
{
    public function __construct()
    {
        echo "demo1加载成功";
    }
}
-------------------------------------
include_once ('autoload.php');

spl_autoload_register('Loader::autoload');

new app\demo1();
```

##### 2、cgi 、fastcgi 、php-fpm

###### 2.1 cgi

1. 网关接口：简单理解就是服务器和请求处理程序之间传输数据的一种标准，或者叫协议叫标准。
2. 数据传递：    入：服务器传递给处理程序的时候，php解析器（php-cgi）拿到后程序进行处理。 出：程序把数据响应给服务器，服务器拿到后 用cgi解析 返回给客户端。
3. 是不是好像很完美？  当发生web 请求的时候  必须要实现cgi的进程，都会fork 一个进程，这样频繁的创建销毁进程，必定占据资源啊。

###### 2.2 fastcgi

和cgi 一样是和语言无关的协议， 为了解决cgi频繁的创建销毁而生的。

###### 2.3  FPM

1. 进程管理器，官方推出了一个叫php-cgi

<img src="/Users/duanyaqiang/Library/Application Support/typora-user-images/image-20201223232909297.png" alt="image-20201223232909297" style="zoom:50%;" />



##### 3、 PHP 数组的实现

###### 先来看看PHP数组的特性

* 查找元素的时间复杂度为o1
* PHP 中的数组是使用 **HashTable** + 链表的数据结构 实现的
* HashTable 的占用空间是 **2 的幂次方**
* HashTable 通过 Key-Value 映射关系实现随机读取
* HashTable 通过**中间映射表**实现顺序读取，中间映射表和元素数组（Bucket）使用连续的内存空间
* PHP 通过链地址法解决 HashTable 中的哈希冲突
* 在空间已满时，会触发自动扩容机制，导致**重建索引**

###### 底层基本实现 ：hashtable+链表

1. 随机读
2. 顺序读

###### hash 冲突 ：链地址法

- 将旧元素的下标储存到新元素的 `next` 中
- 将新元素的下标储存到中间映射表中

###### 查找

* 使用 `time 33` 算法对 key 值计算得到 `hash code`
* 使用散列函数计算 hash code 得到散列值 `nIndex`，即元素在中间映射表的下标
* 通过 nIndex 从中间映射表中取出元素在 Bucket 中的下标 `idx`
* 通过 idx 访问 Bucket 中对应的数组元素，该元素同时也是一个`静态链表`的头结点
* 遍历链表，分别判断每个元素中的 key 值是否与我们要查找的 key 值相同
* 如果相同，终止遍历

###### 扩容 ：链地址法

- 将旧元素的下标储存到新元素的 `next` 中
- 将新元素的下标储存到中间映射表中



##### 5. PHP垃圾回收机制



##### 6、PHP运行机制和原理

###### 设计理念 特性

1. 多进程模型：现在也支持多线程
2. 弱语言类型
3. 引擎zend+组件ext  降低耦合
4. 中间层：隔绝web server 和PHP
5. 语法太灵活，拿来就用，易学

###### 架构图

1. Zend：  翻译PHP代码为可执行的opcode  实现数据结构，内存分配管理。
2. exstention: 常见的内置函数 在这里实现。
3. Sapi: 钩子函数通过 sapi 使PHP和外围应用实现交互 如 apache 、 cgi
4. Application：我们平时写的PHP代码

###### PHP的运行模式

SAPI 是中间媒介

* CLI : 基本不用
* CGI：



* fastCGI：

##### 7、PHP的变量









