#安装

## 服务器要求

对于高性能的要求，框架采用PHP7的语法。建议使用php7.2及以上。Openssl用于接口加密开发时候，内置的非对称加密RSA和对称加密RES等， DB类采用PDO Prepare的方式操作数据库，这样可以有效防止Sql注入。系统使用mb_开头的函数，因此需要安装Mbstring扩展。 以上这些，基本是目前市面上最常用的扩展。

- PHP >= 7.0
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension

## 下载

官网下载：
> http://getcoder.cn/download

Github下载：
> https://github.com/wk331100/coder_php_framework

下载完成后，直接解压到网站目录即可。


## Nginx配置
在Nginx配置文件中，Server{} 里面配置：
```nginx
location / {
        try_files $uri $uri/ /index.php?_url=$uri&$args;
}

location ~ \.php$ {
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index /index.php;

        include fastcgi_params;
        fastcgi_param env            "dev";   #配置系统环境为开发，线上部署配置为："production"
        fastcgi_param PATH_INFO       $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

```