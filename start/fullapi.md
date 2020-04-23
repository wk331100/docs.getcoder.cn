**完整的API：** 这里以用户的增删改查为例子。包含：路由、控制器、服务、数据模型、错误码、抛出异常等功能。主要是：帮助新同学熟悉 API服务的开发流程和规范。

## 配置参数
在`.env`文件中配置Mysql数据库的参数
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=coder
DB_USERNAME=coder
DB_PASSWORD=123456
```

## 创建数据库 `coder`
```
create database `coder` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
这里我们默认字符集为`utf8mb4`,改字符集支持emoji类的表情符号。


## 创建user表
```
CREATE TABLE `user` (
   `uid` int(11) NOT NULL AUTO_INCREMENT,
   `username` varchar(24) DEFAULT NULL,
   `password` varchar(64) DEFAULT NULL,
   `enabled` enum('1','0') DEFAULT '1',
   `create_time` datetime DEFAULT NULL,
   PRIMARY KEY (`id`)
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```

至此，数据库部分准备完成。

## 添加路由
在routes\web.php中添加路由
```php
Route::get('/user/list', 'UserController@index   //获取列表
Route::get('/user/info', 'UserController@info');  //获取详情
Route::post('/user/create', 'UserController@create');  //添加用户
Route::post('/user/update', 'UserController@update');  //更新用户
Route::post('/user/delete', 'UserController@delete'); //删除用户

```

## 添加控制器
在app\Http\Controllers下添加UserController.php

写入如下代码:
```php
<?php

namespace App\Http\Controllers;  //声明Controller命名空间

use App\Exceptions\ServiceException;
use App\Libs\MessageCode;
use App\Services\UserService;
use System\Request;
use System\Response;
use System\Validator;

class UserController extends Controller {
    //获取列表
    public function index(Request $request){
        try {
            $data = UserService::getList();
            return Response::json($data);
        } catch (ServiceException $e){
            return Response::error($e);
        }
    }

    //获取详情
    public function info(Request $request){
        try {
            $params = $request->all();
            $validate = Validator::make($params, [  //参数验证，具体规则参考 Validator类
                'uid' => 'required',
            ]);
            if($validate->fails()){
                throw new ServiceException(MessageCode::ILLEGAL_PARAMETERS);
            }
            $data = UserService::getInfo($params);  //调用服务
            return Response::json($data);  //Response 类按照Json格式输出
        } catch (ServiceException $e){  //捕获异常
            return Response::error($e);
        }
    }

    public function create(Request $request){
        try {
            $params = $request->all();
            $validate = Validator::make($params, [
                'username' => 'required|min:4',
                'password' => 'required|between:6,20'
            ]);
            if($validate->fails()){
                throw new ServiceException(MessageCode::ILLEGAL_PARAMETERS);
            }

            $result = UserService::create($params);
            return Response::json($result);
        } catch (ServiceException $e){
            return Response::error($e);
        }
    }

    public function update(Request $request){
        try {
            $params = $request->all();
            $validate = Validator::make($params, [
                'uid' => 'required',
            ]);
            if($validate->fails()){
                throw new ServiceException(MessageCode::ILLEGAL_PARAMETERS);
            }

            $result = UserService::update($params);
            return Response::json($result);
        } catch (ServiceException $e){
            return Response::error($e);
        }
    }

    public function delete(Request $request){
        try {
            $params = $request->all();
            $validate = Validator::make($params, [
                'uid' => 'required',
            ]);
            if($validate->fails()){
                throw new ServiceException(MessageCode::ILLEGAL_PARAMETERS);
            }

            $result = UserService::delete($params);
            return Response::json($result);
        } catch (ServiceException $e){
            return Response::error($e);
        }
    }

}
```

## 添加Service
在app\Services下添加`UserService.php`文件，写入如下内容：
```php
<?php
namespace App\Services;  //声明Service命名空间

use App\Exceptions\ServiceException;
use App\Libs\MessageCode;
use App\Models\UserModel;

class UserService{

    public static function getList(){
        return UserModel::getInstance()->getList();
    }

    public static function getInfo($data){
        if(empty($data['uid'])){
            throw new ServiceException(MessageCode::PARAMETERS_ERROR);
        }
        return UserModel::getInstance()->getInfo($data['uid']);
    }

    public static function create($data){
        if(empty($data['username']) || empty($data['password'])){
            throw new ServiceException(MessageCode::PARAMETERS_ERROR); //抛出异常
        }

        $insertData = [
            'username' => $data['username'],
            'password' => md5($data['password'])
        ];

        return UserModel::getInstance()->create($insertData); //调用Model
    }

    public static function update($data){
        if(empty($data['uid'])){
            throw new ServiceException(MessageCode::PARAMETERS_ERROR);
        }
        $updateData = [];

        if(!empty($data['username'])){
            $updateData['username'] = $data['username'];
        }

        if(!empty($data['password'])){
            $updateData['password'] = md5($data['password']);
        }

        return UserModel::getInstance()->update($updateData, $data['uid']);
    }

    public static function delete($data){
        if(empty($data['uid'])){
            throw new ServiceException(MessageCode::PARAMETERS_ERROR);
        }
        return UserModel::getInstance()->delete($data['uid']);
    }

}

```

## 添加Model
在app\Models下添加`UserModel.php`,写入如下内容：
```php
<?php

namespace App\Models;  //声明Model的命名空间

use System\DB;

class UserModel extends DB  {

    protected $table = 'user';
    private static $_instance;
    protected $_pk = 'uid';

    public static function getInstance() {
        if (!self::$_instance) {
            self::$_instance = new self();
        }
        return self::$_instance;
    }
}
```

至此，一个简单的完整的API就编写完成了。