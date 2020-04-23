## 主目录结构
```
- app
    - Exception
    - Http
		- Controllers
		- Middleware
	- Libs
	- Models
	- Services
- bootstrap
	- app.php
- config
	- app.php
	- database.php
	- logging.php
- public
	- index.php
- routes
	 web.php
- storage

```


主目录结构
- App为应用目录，里面包括控制器、中间件、Lib库、Model模型、Service服务等。
- bootstrap目录为系统引导启动目录，只含有web.php, 用于fpm类的启动引导，开发过程中不需要修改。
- config目录之前介绍过，为系统配置文件目录。里面内置了 app.php，database.php，logging.php，用户可以自定义配置文件。
- public目录下仅包含index.php，为系统的统一入口文件。
- routes目录，用于配置API的路由。为了最大限度的保证系统安全性，为注册的路由，系统一律不予解析。
- 

## 入口文件
入口文件为 public下的`index.php`，该文件为系统的统一入口，代码极为简单，载入启动程序bootstrap.php后`run()`。
```php
<?php

$app = require_once __DIR__.'/../bootstrap/app.php';

$app->run();

```

## 启动程序
系统的启动程序为`bootstrap.php`， 该程序主要加载配置项，实例化APP类，加载中间件、路由等功能，返回最终的APP对象。

```php
<?php
define('ROOT_PATH', __DIR__ . DIRECTORY_SEPARATOR . '..');
define('APP_PATH', ROOT_PATH . DIRECTORY_SEPARATOR . 'app');

//require __DIR__.'/../vendor/autoload.php';
require __DIR__ . '/../system/autoload.php';
require_once __DIR__ . '/../system/helpers.php';

$app = new System\Application(dirname(__DIR__), ['web']);

$file = '.env';
if (isset($_SERVER["env"])) {
    if ($_SERVER["env"] == 'test') {
        $file = '.env_test';
    } else if ($_SERVER["env"] == 'production') {
        $file = '.env_production';
    }
}

$app->loadEnv($file);

$app->beforeMiddleware([
    App\Http\Middleware\BeforeMiddleware::class,
]);
$app->afterMiddleware([
    App\Http\Middleware\AfterMiddleware::class
]);
$app->routeMiddleware([
    'auth' => App\Http\Middleware\AuthMiddleware::class,
    'auth2' => App\Http\Middleware\Auth2Middleware::class
]);

return $app;


```
