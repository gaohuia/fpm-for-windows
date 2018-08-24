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

而fpm-for-windows是一个真正的windows下实现多个php-cgi进程同时工作的集成环境. 

