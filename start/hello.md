框架默认，配置了路由`Route::get('/', 'IndexController@index');`, 只需要修改app\Http\Controller\IndexController下的index()方法的内容即可。

```php
public function index(){
    return 'Hello World!';
}

```

浏览器访问`http://localhost/` 即可得到Hello World
