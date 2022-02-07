
## array_merge <!-- {docsify-ignore} -->

在PHP的开发中，我们经常需要合并两个数组，通常我们会使用 ```array_merge```函数

文档的解释为
> array_merge() 函数用于把一个或多个数组合并为一个数组; 如果两个或更多个数组元素有相同的键名，则最后的元素会覆盖其他元素

但是在我们合并两个以数字为key的数组时却发现它没能像我们想象中那样合并

例如:
```
$a1=array(1=>"red",2=>"green");
$a2=array(2=>"blue",3=>"yellow");
print_r(array_merge($a1,$a2));

输出:

(
    [0] => red
    [1] => green
    [2] => blue
    [3] => yellow
)

```

我们发现并没有把key=2的给覆盖掉，也许你会说为字符串2时呢

```
$a1=array('1'=>"red",'2'=>"green");
$a2=array('2'=>"blue",'3'=>"yellow");
print_r(array_merge($a1,$a2));

输出：

Array
(
    [0] => red
    [1] => green
    [2] => blue
    [3] => yellow
)

```

我们发现并没有变化，解决办法：可以使用 $a1 + $a2 来达到覆盖数字作为key的情况


**总结**

> 在合并字符串为key的数组时 使用array_merge后一个会覆盖前一个
> 在合并以数字作为key的数组时 使用array_merge不会覆盖 
> 可以使用 ``` $a1 + $a2 ``` 需要注意的时相同的key名、前一个会覆盖后一个


