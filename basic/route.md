
路由支持get，post，any，group 四种加载方式。 第一个参数表示Url Path，第二个参数表示对应的控制器。控制器需要指定命名空间，如果不指定则匹配默认Controller空间。 `@`符号用于指定对应的方法。


## 固定路由配置
```
Route::get('/', 'HomeController@index');
Route::post('/home', 'HomeController@test')
Route::any('/home/test2', 'HomeController@test2')
```
## 路由组配置
```
Route::prefix('account')->middleware(['auth','auth2'])->group(function (){
    Route::any('user', 'Account\UserController@index');
    Route::get('user/test', 'Account\UserController@test');
});
```

> 路由组配置支持添加Middleware，当请求访问该路由组时，加载执行对应的中间件。这里的中间件需要在bootstrap\app.php中注册
