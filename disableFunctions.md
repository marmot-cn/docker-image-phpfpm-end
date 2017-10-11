# disable functions

## 函数列表

* `disk_total_space`
* `disk_free_space`
* `diskfreespace`
* `passthru`
* `exec`
* `system`
* `chroot`
* `chgrp`
* `chown`
* `shell_exec`
* `phpinfo`
* `chmod`
* `dl`
* `ini_set`
* `ini_alter`
* `ini_restore`
* `popen`打开一个指向进程的管道,该进程由派生给定的 command 命令执行而产生.
* `proc_open`执行一个命令，并且打开用来输入/输出的文件指针

---

## 函数

### `disk_total_space`

#### 定义

`float disk_total_space ( string $directory )`

返回一个目录的磁盘总大小.

#### 示例

```php
var_dump(disk_total_space("/"));

输出: float(67371577344) 
```

### `disk_free_space` 和 `diskfreespace`

#### 定义

`float disk_free_space ( string $directory )`

返回目录中的可用空间.

#### 示例

```php
var_dump(disk_free_space("/"));

输出: float(58478755840)
```

### `passthru`

#### 定义

`void passthru ( string $command [, int &$return_var ] )`

来执行外部命令`command`的.

当所执行的`Unix`命令输出二进制数据,并且需要**直接传送**到浏览器的时候,需要用此函数来替代`exec()`或`system()`函数.

`CLI`中也会输出命令的执行结果.

#### 参数

* `command`: 要执行的命令.
* `return_var`: 如果提供 return_var 参数， Unix 命令的返回状态会被记录到此参数.

#### 示例

```shell
marmot-sourcecode git:(dev) ✗ cat test.sh
#!/bin/bash
echo 'run from command test.sh';
exit 2;

marmot-sourcecode git:(dev) ✗ ./test.sh
1
marmot-sourcecode git:(dev) ✗ echo $?
2
```

在`php`代码里面写入:

```php
passthru('./test.sh', $err);
echo '<br />';
var_dump($err);

通过浏览器输入地址, 输出脚本的执行结果:

run from command test.sh
int(2)
```

### `exec`

#### 定义

`string exec ( string $command [, array &$output [, int &$return_var ]] )`

`exec()`执行`command`参数所指定的命令.

如果你需要获取未经处理的全部输出数据, 请使用`passthru()`函数.

#### 参数

* `command`: 要执行的命令.
* `output`: 如果提供了该参数, 会用命令执行的输出填充此数组, 每行输出填充数组中的一个元素.数组中的数据不包含行尾的空白字符,例如`\n`字. 如果`output`有数据, 会在数组末尾追加内容. 如果不想追加, 在传入`exec()`函数之前对数组使用`unset()`函数.
* `return_var`: 命令执行后的返回状态会被写入到此变量.

#### 示例

在`php`代码里面写入:

```php
exec('./test.sh', $output, $err);
var_dump($output);
echo '<br />';
var_dump($err);

通过浏览器输入地址, 输出脚本的执行结果:

array(1) { [0]=> string(24) "run from command test.sh" }
int(2)
```

### `system`

#### 定义

`string system ( string $command [, int &$return_var ] )`

同`C`版本的`system()`函数一样, 本函数执行`command`参数所指定的命令,并且输出执行结果.

如果`PHP`运行在服务器模块中,`system()`函数还会尝试在每行输出完毕之后,自动刷新`web`服务器的输出缓存.

#### 参数
 
* `command`: 要执行的命令.
* `return_var`: 则外部命令执行后的返回状态将会被设置到此变量中.

#### 示例

```php
$output = system('./test.sh', $err);
echo '<br />';
var_dump($err);
 
通过浏览器输入地址, 输出脚本的执行结果:
run from command test.sh
int(2)

$output 变量也会保存输出的结果
```

### `chroot`

#### 定义

`bool chroot ( string $directory )`

将当前进程的根目录改变为`directory`.

本函数仅在系统支持且运行于`CLI`,`CGI`或嵌入`SAPI`版本时才能正确工作. 此外本函数还需要`root`权限.

#### 示例

我编译镜像提示该函数未定义.

### `chgrp`

#### 定义

改变文件所属的组.

`bool chgrp ( string $filename , mixed $group )`

尝试将文件`filename`所属的组改成`group`.

只有超级用户可以任意修改文件的组,其它用户可能只能将文件的组改成该用户自己所在的组.

#### 参数

* `filename`: 文件的路径.
* `group`: 组的名称或数字.

#### 示例

```shell
创建一个用户
root@cca544d074cd:/var/www/html# useradd tom
root@cca544d074cd:/var/www/html# id tom
uid=1000(tom) gid=1000(tom) groups=1000(tom)

创建一个文件, 属主和属组都是 tom
root@cca544d074cd:/var/www/html# touch tom_file
root@cca544d074cd:/var/www/html# chown tom:tom tom_file
root@cca544d074cd:/var/www/html# ls -l tom_file
-rw-r--r-- 1 tom tom 0 Sep  7 06:16 tom_file
```

`php`运行`chgrp`

```php
chgrp('./tom_file', 'www-data');
```

```shell
发现文件的属组已经被修改了
root@cca544d074cd:/var/www/html# ls -l tom_file
-rw-r--r-- 1 tom www-data 0 Sep  7 06:16 tom_file
```

### `chown`

#### 定义

改变文件的所有者.

`bool chown ( string $filename , mixed $user )`

#### 参数

* `filename`: 文件路径.
* `user`: 用户名或数字.

#### 示例

```shell
root@98bf7ad95cd4:/var/www/html# useradd tom
root@98bf7ad95cd4:/var/www/html# touch tom_file
root@98bf7ad95cd4:/var/www/html# chown tom:tom tom_file
root@98bf7ad95cd4:/var/www/html# ls -l tom_file
-rw-r--r-- 1 tom tom 0 Sep  7 08:15 tom_file
```

`php`运行`chown`

```php
33是www-data用户的uid
chown('./tom_file', 33);
```

```shell
发现文件的属主已经被修改了
root@98bf7ad95cd4:/var/www/html# ls -l tom_file
-rw-r--r-- 1 www-data tom 0 Sep  7 08:15 tom_file
```

### `shell_exec`

#### 定义

通过`shell`环境执行命令, 并且将完整的输出以字符串的方式返回.

`string shell_exec ( string $cmd )`

命令执行的输出. 如果执行过程中发生错误或者进程不产生输出, 则返回`NULL`.

#### 参数

* `cmd`: 要执行的命令

#### 示例

```php
$output = shell_exec('ls -lart');
echo "<pre>$output</pre>";

输出:
total 284
drwxr-xr-x  3 root     root       4096 Oct  1  2016 ..
drwxr-xr-x  8 www-data www-data    272 May 26 09:49 tests
-rw-r--r--  1 www-data www-data   1340 May 26 09:49 phpunit.xml
-rw-r--r--  1 www-data www-data     18 May 26 09:49 phpinfo.php
-rwxr-xr-x  1 www-data www-data    763 May 26 09:49 phpcs.xml
drwxr-xr-x  4 www-data www-data    136 May 26 09:49 deployment
...
```

### `phpinfo`

禁用`phpinfo`函数, 需要查阅配置信息需要运行`php -i`查看.

```shell
root@cca544d074cd:/var/www/html# php -i | grep "expose_php"
expose_php => Off => Off
```

### `getcwd` 和 `posix_getcwd`

#### 定义

取得当前工作目录.

`string getcwd ( void )`

#### 示例

```php
var_dump(getcwd());

输出:
string(13) "/var/www/html" 
```

### `chmod`

#### 定义

`bool chmod ( string $filename , int $mode )`

尝试将`filename`所指定文件的模式改成`mode`所给定的.

#### 参数

* `filename`: 文件路径.
* `mode`: 要确保正确操作, 需要给前面加上`0`.

成功时返回`TRUE`, 或者在失败时返回`FALSE`. 

### `dl`

#### 定义

`bool dl ( string $library )`

在运行时加载`php`扩展

#### 参数

* `$library`: 扩展的文件名, 根据不同的平台名称后缀会不同
	* `windows`: `*.dll`
	* `unix`: `*.so`

#### 示例

```php
// Example loading an extension based on OS
if (!extension_loaded('sqlite')) {
    if (strtoupper(substr(PHP_OS, 0, 3)) === 'WIN') {
        dl('php_sqlite.dll');
    } else {
        dl('sqlite.so');
    }
}

// Or using PHP_SHLIB_SUFFIX constant
if (!extension_loaded('sqlite')) {
    $prefix = (PHP_SHLIB_SUFFIX === 'dll') ? 'php_' : '';
    dl($prefix . 'sqlite.' . PHP_SHLIB_SUFFIX);
}
```

### `passthru`

#### 定义

`void passthru ( string $command [, int &$return_var ] )`

执行外部程序并且显示原始输出.

`passthru()`函数,也是用来执行外部命令的. 当所执行的 Unix 命令输出二进制数据， 并且需要直接传送到浏览器的时候, 需要用此函数来替代`exec()`或`system()`函数.

#### 参数

* `command`: 要执行的命令
* `return_var`: 如果提供`return_var`参数, Unix命令的返回状态会被记录到此参数.

#### 示例

```php
<?php
var_dump(passthru('ls', $err));

执行输出:
debug index.php phpinfo.php test.a NULL 
```

### `ini_set`

#### 定义

为一个配置选项设置值.

`string ini_set ( string $varname , string $newvalue )`

设置指定配置选项的值. 这个选项会在脚本运行时保持新的值, 并在脚本结束时恢复.

#### 参数

* `varname`: 选项
* `newvalue`: 选项新的值

#### 示例

```php
<?php
ini_set('display_errors', '1');
```

### `ini_alter`

#### 定义

`ini_set`函数的别名

### `ini_restore`

#### 定义

恢复配置选项的值.

`void ini_restore ( string $varname )`

恢复指定的配置选项到它的原始值.

#### 参数

* `varname`: 配置选项名称

### `copy`

#### 定义

拷贝文件.

`bool copy ( string $source , string $dest [, resource $context ] )`

将文件从`source`拷贝到`dest`.

#### 参数

* `source`: 源文件路径.
* `dest`: 目标路径.

#### 示例

```php
原来的文件:
ls
debug       index.php   phpinfo.php

<?php
var_dump(copy('./index.php', './index-2.php'));

浏览器访问页面, 返回 true, 查看并生成新的文件 index-2.php

ls
debug       index-2.php index.php   phpinfo.php
```
### `rename`

#### 定义

重命名一个文件或目录.

`bool rename ( string $oldname , string $newname [, resource $context ] )`

尝试把`oldname`重命名为`newname`.

#### 参数

* `oldname` 旧的文件名.
* `newname` 新的文件名.

#### 示例

```php
放文件可见文件名 a
ls
a           debug       index.php   phpinfo.php

<?php
var_dump(rename('./a', './b'));

浏览器访问页面, 返回 true, 可见 a 已经改为 b

ls
b           debug       index.php   phpinfo.php
```

### `unlink`

#### 定义

删除文件

`bool unlink ( string $filename [, resource $context ] )`

删除`filename`. 和`Unix C`的`unlink()`函数相似. 发生错误时会产生一个`E_WARNING`级别的错误.

#### 参数

* `filename`: 文件的路径

#### 示例

```php
原本有一个名为 c 的文件
ls
c           debug       index.php   phpinfo.php

<?php
var_dump(unlink('./c'));

浏览器访问, 返回true, 在查看 c 已经被删除
ls
debug       index.php   phpinfo.php
```

#### 示例

```php
ls
debug       index.php   phpinfo.php

创建文件夹
<?php
var_dump(mkdir('./a/b',0777, true));

浏览器访问, 返回true, 再次查看, 文件夹 a/b 都创建了
ls
a           debug       index.php   phpinfo.php
ls a
b
```

### `rmdir `

#### 定义

删除目录.

`bool rmdir ( string $dirname [, resource $context ] )`

尝试删除`dirname`所指定的目录. 

#### 参数

* `dirname`: 目录的路径

#### 示例

```php
创建文件夹 a
ls
a           debug       index.php   phpinfo.php

<?php
var_dump(rmdir('./a'));

浏览器访问, 返回true, 再次查看, a 文件夹不存在

ls 
debug       index.php   phpinfo.php
```

#### 参数

* `mask`新的`umask`值

### `popen`

#### 定义

打开进程文件指针.

`resource popen ( string $command , string $mode )`

打开一个指向进程的管道, 该进程由派生给定的`command`命令执行而产生.

#### 参数

* `command`: 命令
* `mode`: 模式

#### 示例

```php
<?php
$handle = popen('ls', 'r');
echo "'$handle'; " . gettype($handle) . "\n";
$read = fread($handle, 2096);
echo $read;
pclose($handle);

浏览器访问获取到 命令执行结果:
'Resource id #2'; resource debug index.php phpinfo.php 
```

### `proc_open`

#### 定义

执行一个命令, 并且打开用来输入/输出的文件指针.

`resource proc_open ( string $cmd , array $descriptorspec , array &$pipes [, string $cwd [, array $env [, array $other_options ]]] )`

类似`popen()`函数, 但是`proc_open()`提供了更加强大的控制程序执行的能力.

### `xxx`

#### 定义

#### 参数

#### 示例