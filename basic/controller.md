## 介绍
与一般的框架不同，为了更好的保证框架的安全性，控制器不能通过url直接访问，而需要先配置路由。`route\web.php`。 

例如：
```php
Route::get('/home', 'HomeController@index');
```
这样就创建了一条路由，指向到 `HomeController` 下的 `index()` 方法。

默认控制器位于`app\Http\Controllers`下，如果在此目录下新建目录`app\Http\Controllers\Api`则需要将控制器命名空间对应设置为 `namespace App\Http\Controllers\Api;`
对应的路由也设置为`Route::get('/home', 'Api\HomeController@index');`


## 约束
- 文件夹名称需要与命名空间一致，区分大小写
- 文件名和类名一致，以`Controller`结尾,
- 控制器继承于`Controller`

## 编写一个控制器
```php
<?php

namespace App\Http\Controllers;

use System\Response;

class HomeController extends Controller {

    public function index(){
        $data = 'Hello Coder!';
        return Response::json($data);
    }

}
```

> 控制器主要作用：参数接收和校验，调用对应的服务获取结果，捕获异常以及返回Json。
> 建议： 不要在控制器中直接调用Model。