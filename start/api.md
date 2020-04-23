> 出于安全的考虑，系统每一个对外的API都需要手动配置路由。

## 配置路由
在 routes\web.php中添加配置
```php
Route::get('/home', 'HomeController@index');
```
`Route::get`表示添加一个Method为GET方式的路由。还可以配置POST等其他形式的路由。
第一个参数为 url访问的路径。第二个参数为指定对应的控制器和方法，中间需要`@`符号连接。

## 编写控制器
控制器系统默认都写在 app\Http\Controllers下。我们创建一个`HomeController.php`，写入以下的内容：
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

**第一行:** 为定义文件的命名空间，命名空间和文件路径一致，区分大小写。系统自动加载机制依赖命名空间，如果命名空间设置的不正确，则系统报错找不到对应的控制器。

**类名：** 和文件名保持一致，已`Controller`结尾， 统一继承自`Controller`，用户可以在Controller里面自定义全局控制器的功能。

**Response：** Response::json 将结果返回为默认的Json格式


这时，访问`http://localhost/home`，将得到以下的结果：

```php
{
    "code": 200,
    "msg": "成功",
    "data": "Hello Coder!"
}
```

至此，第一个简单的API就完成了。 下一节，将介绍一个标准的规范的完整的Web Service 服务API如何编写