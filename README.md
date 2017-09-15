# docker-image-phpfpm-end

---


慢日志

request_slowlog_timeout

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

## apt

安装: `libmemcached-dev` `zlib1g-dev`

memcached 需要指定 `--disable-memcache-session`, 否则会出现`Cannot find php_session.h`

redis 需要指定 `--disable-redis-session`

mongodb 需要安装 `libssl-dev`