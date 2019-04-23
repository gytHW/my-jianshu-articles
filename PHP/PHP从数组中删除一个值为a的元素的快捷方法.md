一般常见的做法是对数组```foreach```循环判断，然后```unset()```掉这个值。这里发现了一种更简洁的方法：

> 使用```array_diff```函数，取数组和该元素的差集，即为需要的数组。
```
$new_arr = array_diff($arr, [$a]);
```
```array_diff```是不重排索引的，如果想重排索引再加一层```array_values```即可。
```
$new_arr = array_values(array_diff($arr, [$a]));
```
同理这方法也能适用于删除多个知道元素。
