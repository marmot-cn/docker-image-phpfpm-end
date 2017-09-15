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





