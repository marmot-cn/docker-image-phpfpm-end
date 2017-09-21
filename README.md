# 后端镜像

---

### `ini`配置文件

* `allow_url_fopen`开启,`composer`需要
* `open_basedir`因为性能问题不使用
* `post_max_size`设置`post`传输限制
* `date.timezone`设定时区
* `memory_limit`设定内存限制
* `file_uploads = off`上传文件功能关闭
* `upload_tmp_dir`虽然上传文件功能关闭, 还是把上传文件路径指向到别的位置
* `display_errors = off`不输出错误
* `error_reporting = E_ALL`错误级别,设定到`notice`
* `log_errors = on`开启记录错误日志, 因为默认配置文件会把错误输出指向到标准错误输出
* `expose_php = off`不返回`php版本号`
* `html_errors = off`关闭错误引导链接

##### `open_basedir` 不使用

open_basedir开启后会影响I/O，因为每个调用的文件都需要判断是否在限制目录内.

使用open_basedir可以限制程序可操作的目录和文件,提高系统安全性.但会影响I/O性能导致系统执行变慢,因此需要根据具体需求,在安全与性能上做平衡.

### `phpfpm`配置文件

* 修改`pm.max_children/`从`5`到`100`.
* 修改`pm.start_servers`从`2`到`40`.
* 修改`pm.min_spare_servers`从`1`到`20`. 
* 修改`pm.max_spare_servers`从`3`到`60`.
* 开启慢日志`slowlog`,并指向`标准错误输入`.
* 设定慢日志的`php`执行时间标准`request_slowlog_timeout`为`5`秒.

#### `docker`启动项修改

因为开启了慢日志,`docker`需要添加`--cap-add=SYS_PTRACE`权限.

### 其他配置项修改

* 修改默认`docker`时区

### 问题

`enable_dl`

#### cig-fcgi ?

* fastcgi.logging
* cgi.fix_pathinfo