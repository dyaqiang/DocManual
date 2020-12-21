#### 1、自动加载

自动加载小项目可以自己搞一下，大项目最好按照PSR4 规范走，脉络清晰

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

