# fpm-for-windows

一个windows下的简易的Nginx+FPM组合. 市面上的其它集成包多数可以启动多个php-cgi进程监听同一个端口, 但实际上并不能做到真正的多进程, 真正起作用的只有一个进程. 需要写如下代码就可以测出来:

```php
<?php

header('Content-Type: text/plain;charset=utf-8');

if (isset($_GET['echo'])) {
	echo $_GET['echo'];
	exit;
}

// 假如你当前的页面地址是http://127.0.0.2/test.php, 根据实际改一下. 
$data = file_get_contents('http://127.0.0.2/test.php?echo=123');

if ($data == '123') {
	echo '你的环境支持并发请求';
	exit;
}

echo '配置错误';

```

如果你的环境支持多个php-cgi进程同时工作, 这个脚本是可以正常执行的并输出`你的环境支持并发请求`
相反你不会得到任何输出, 因为它会产生一个死锁. 

而fpm-for-windows是一个真正的windows下实现多个php-cgi进程同时工作的集成环境. 

## 基本原理

golang充当fpm, electron编写的界面. 

* fpm-for-windows.exe充当一个fpm的角色监听在9001端口
* fpm-for-windows.exe启动多个php-cgi进程分别监听在比如127.0.0.2:30001~127.0.0.2:30005
* fpm-for-windows.exe与上述127.0.0.2:N端口建立连接并放到一个连接池中.
* fpm-for-windows.exe将nginx发到9001端口的fastcgi请求路由到一个空闲的php-cgi连接进行处理并发回相应的输出.

## 界面预览

<img src="https://github.com/gaohuia/fpm-for-windows/raw/master/ui.png" width="800" />
<img src="https://github.com/gaohuia/fpm-for-windows/raw/master/ui.png" width="800" />

