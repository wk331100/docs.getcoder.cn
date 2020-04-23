## 介绍
Request类用于请求相关的数据处理和验证。

## 使用
在控制器中，可以通过依赖注入的方式使用Request对象
```php
<?php

namespace App\Http\Controllers;

use System\Request;
use System\Response;

class HomeController extends Controller {

    public function index(Request $request){
        $params = $request->all();
        return Response::json($params);
    }

}
```

## 获取参数
获取所有参数:`all()`
```php
$request->all();
```

获取指定参数, 第二个参数表示未命中时返回的默认值，可选: `input($param, $default)`
```php
$request->input('id');
```

判断是否包含参数: `has($param)`
```php
$request->has('id');
```

## 获取url
获取URL路径：`path()`，结果为：`/home`
```php
$request->path();
```

获取URL,：`url()`, 结果为： 'http://getcoder.cn/home'
```php
$request->url();
```

获取完整URL,：`fullUrl()`, 结果为： 'http://getcoder.cn/home?id=1'
```php
$request->rullUrl();
```

## 判断Method
获取当前请求的Method： `method()`, 结果为：`GET/POST`
```php
$request->method();
```

判断当前请求Method： `isMethod($method)`, 结果为：`true/false`
```php
$request->isMethod('post');
```

判断当前请求Method是否为POST： `isPost()`, 结果为：`true/false`
```php
$request->isPost();
```

判断当前请求Method是否为GET： `isGet()`, 结果为：`true/false`
```php
$request->isGet();
```

## 文件上传

判断是否有指定文件上传：`hasFile('image')`
```php
$request->hasFile('image')
```

获取文件对象：`file('image')`
```php
$file = $request->file('image');
```

判断上传的文件是否正确：`isValid()`
```php
$file->isValid()
```

从文件对象中获取临时文件路径：`path()`
```php
$file->path()
```

从文件对象中获取文件扩展：`extension()`: 结果`.jpg`
```php
$file->extension()
```

从文件对象中获取文件类型：`getType()`：结果`image/jpeg`
```php
$file->getType()
```


从文件对象中获取文件类型简写：`getMimeType()`： 结果 `jpeg`
```php
$file->getMimeType()
```

从文件对象中获取文件大小：`getClientSize()`： 结果 `110663` 字节
```php
$file->getClientSize()
```

从文件对象中获取原始文件名称：`getClientOriginalName()`： 结果 `image.jpeg`
```php
$file->getClientOriginalName()
```

