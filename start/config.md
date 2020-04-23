## 介绍
所有的配置，建议用户都写在`.env`文件里，用户可以根据系统环境创建`.env_test`文件和`.env_production`文件，分别配置对应的参数。
> tips：开发人员通常只需要配置`.env`文件即可。系统上线的时候，运维可配置Jenkins等部署工具，自动创建`.env_production`文件，并且设置线上数据库等账号密码。可有效防止密码泄露。

## config文件配置
配置文件位于：config目录下。采用php 数组配置对应的参数。已内置`app.config`配置系统常用信息,`database.php`配置数据库和缓存信息，`logging.php`配置系统日志相关信息。如果用户希望自定义配置文件，可以参考这三个文件，在config目录创建一个新的php文件,例如: myconfig.php。
> tips: 自定义配置，建议采用env()的方式加载.env文件获取配置

读取方式：
```php
$myConfig = config('myconfig');
```

## env
.env为环境配置文件。系统默认.env文件和部分常用配置。用户可以自由添加其他配置。
在其他文件中获取.env里面的参数：第二个参数为默认值，如果获取不到则返回默认值。
```php
$dbName = env(DB_NAME, 'blog');
```

通常配置项：
```
APPNAME=Coder 1.0

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=123456

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=123456
REDIS_PORT=6379

```