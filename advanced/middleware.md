中间件可以根据实际使用需求，可以配置前置中间件、后置中间件或者路由组中间件。并且每种中间件可以注册多个，按照注册顺序依次执行。

## 配置Bootstrap
打开 bootstrap/app.php， 在`return $app;` 之前加入中间件配置代码：
```php
$app->beforeMiddleware([
    App\Http\Middleware\BeforeMiddleware::class,
]);
$app->afterMiddleware([
    App\Http\Middleware\AfterMiddleware::class
]);
$app->routeMiddleware([
    'auth' => App\Http\Middleware\AuthMiddleware::class,
]);

```

- `beforeMiddleware()`为配置全局前置中间件，该中间件会在加载控制器之前执行。
- `afterMiddleware()`为全局后置中间件，该中间件会在加载控制器之后执行
- `routeMiddleware()`为路由组中间件，该中间件只会在路由组配置的地方执行，并且只能是前置中间件，在控制器之前执行

上面`auth` 为别名，在路由组中使用。

## 配置路由
```php
Route::prefix('account')->middleware(['auth','auth2'])->group(function (){
    Route::any('user', 'Account\UserController@index');
    Route::get('user/test', 'Account\UserController@test');
});

```

`middleware()` 方法设定需要执行的中间件。

## 编写中间件
在app\Http\Middleware下添加中间件文件。`BeforeMiddleware.php`, 写入代码：
```php
<?php

namespace App\Http\Middleware;

use App\Libs\Util;
use System\Request;

class BeforeMiddleware{

    public function handle(Request $request){
        $requestId = Util::randChar();
        $request->setParam('request_id', strtoupper($requestId));
    }

}
```

> 中间件默认执行方法为： `handle()`
