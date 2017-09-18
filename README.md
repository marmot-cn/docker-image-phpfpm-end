# docker-image-phpfpm-end

---

需要把框架的`index.php`移动至`public`目录下.
创建`cache\`文件夹


慢日志`www.conf`

request_slowlog_timeout = 5s

slowlog = log/$pool.log.slow

docker 需要 --cap-add=SYS_PTRACE


enable_dl?


##

* `--disable-session`禁用`session`
* `--enable-zip`
* `-enable-pcntl`
* `--enable-sysvmsg`
* `--enable-sysvsem`
* `--enable-sysvshm`
* `--enable-bcmath` rabbitmq 需要

## ini

* `allow_url_fopen` 开启, composer 需要

## apt

安装: `libmemcached-dev` `zlib1g-dev`

memcached 需要指定 `--disable-memcache-session`, 否则会出现`Cannot find php_session.h`

redis 需要指定 `--disable-redis-session`

mongodb 需要安装 `libssl-dev`





## `open_basedir` 不使用

open_basedir开启后会影响I/O，因为每个调用的文件都需要判断是否在限制目录内.

使用open_basedir可以限制程序可操作的目录和文件,提高系统安全性.但会影响I/O性能导致系统执行变慢,因此需要根据具体需求,在安全与性能上做平衡.

## 错误

`html_errors = Off`