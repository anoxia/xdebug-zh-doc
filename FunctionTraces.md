# 函数跟踪

Xdebug允许你记录所有的函数调用数据（包括参数和返回值）到不同格式的文件。

之所以称之为**函数跟踪**，是因为它可以帮助你在使用新的应用程序，或者当你试图找出应用程序运行时到底发生了什么。函数跟踪还可以选择显示传递给函数和方法的变量的值，并返回值。在默认跟踪中，这两个元素不可用。

## 输出格式

有三种输出格式。第一种是人类可读的调用栈，第二种更适合计算机程序，因为它更容易解析，最后一种使用HTML格式化踪迹。您可以通过设置[xdebug.trace_format](https://xdebug.org/docs/all_settings#trace_format)参数在两种不同格式之间切换。有几个设置可以控制将哪些信息写入跟踪文件。例如，包含变量（[xdebug.collect_params](https://xdebug.org/docs/all_settings#collect_params)）和包含返回值（[xdebug.collect_return](https://xdebug.org/docs/all_settings#collect_return)）的设置。下面的例子显示了不同的设置对人类可读的函数调用栈有什么影响。

**Example:**

```php
<?php
$str = "Xdebug";
function ret_ord( $c )
{
    return ord( $c );
}

foreach ( str_split( $str ) as $char )
{
    echo $char, ": ", ret_ord( $char ), "\n";
}
```

**结果：**

以下是使用不同设置的[xdebug.collect_params](https://xdebug.org/docs/all_settings#collect_params) 设置的结果。由于这不是Web环境，工具提示在文本文件中不起作用，所以2的值没有任何意义。

**输出（默认）**

```
TRACE START [2007-05-06 14:37:06]
    0.0003 114112  - > {main}（）../trace.php:0
    0.0004 114272  - > str_split（）../trace.php:8
    0.0153 117424  - > ret_ord（）../trace.php:10
    0.0165 117584  - > ord（）../trace.php:5
    0.0166 117584  - > ret_ord（）../trace.php:10
    0.0167 117584  - > ord（）../trace.php:5
    0.0168 117584  - > ret_ord（）../trace.php:10
    0.0168 117584  - > ord（）../trace.php:5
    0.0170 117584  - > ret_ord（）../trace.php:10
    0.0170 117584  - > ord（）../trace.php:5
    0.0172 117584  - > ret_ord（）../trace.php:10
    0.0172 117584  - > ord（）../trace.php:5
    0.0173 117584  - > ret_ord（）../trace.php:10
    0.0174 117584  - > ord（）../trace.php:5
    0.0177 41152
TRACE END [2007-05-06 14:37:07]
```

**输出（collect_params=1）：**

```
TRACE START [2007-05-06 14:37:11]
    0.0003     114112   -> {main}() ../trace.php:0
    0.0004     114272     -> str_split(string(6)) ../trace.php:8
    0.0007     117424     -> ret_ord(string(1)) ../trace.php:10
    0.0007     117584       -> ord(string(1)) ../trace.php:5
    0.0009     117584     -> ret_ord(string(1)) ../trace.php:10
    0.0009     117584       -> ord(string(1)) ../trace.php:5
    0.0010     117584     -> ret_ord(string(1)) ../trace.php:10
    0.0011     117584       -> ord(string(1)) ../trace.php:5
    0.0012     117584     -> ret_ord(string(1)) ../trace.php:10
    0.0013     117584       -> ord(string(1)) ../trace.php:5
    0.0014     117584     -> ret_ord(string(1)) ../trace.php:10
    0.0014     117584       -> ord(string(1)) ../trace.php:5
    0.0016     117584     -> ret_ord(string(1)) ../trace.php:10
    0.0016     117584       -> ord(string(1)) ../trace.php:5
    0.0019      41152
TRACE END   [2007-05-06 14:37:11]
```

**输出（collect_params=3）：**

```
TRACE START [2007-05-06 14:37:13]
    0.0003     114112   -> {main}() ../trace.php:0
    0.0004     114272     -> str_split('Xdebug') ../trace.php:8
    0.0007     117424     -> ret_ord('X') ../trace.php:10
    0.0007     117584       -> ord('X') ../trace.php:5
    0.0009     117584     -> ret_ord('d') ../trace.php:10
    0.0009     117584       -> ord('d') ../trace.php:5
    0.0010     117584     -> ret_ord('e') ../trace.php:10
    0.0011     117584       -> ord('e') ../trace.php:5
    0.0012     117584     -> ret_ord('b') ../trace.php:10
    0.0013     117584       -> ord('b') ../trace.php:5
    0.0014     117584     -> ret_ord('u') ../trace.php:10
    0.0014     117584       -> ord('u') ../trace.php:5
    0.0016     117584     -> ret_ord('g') ../trace.php:10
    0.0016     117584       -> ord('g') ../trace.php:5
    0.0019      41152
TRACE END   [2007-05-06 14:37:13]
```

**输出（collect_params=4）：**

```
TRACE START [2007-05-06 14:37:16]
    0.0003     114112   -> {main}() ../trace.php:0
    0.0004     114272     -> str_split('Xdebug') ../trace.php:8
    0.0007     117424     -> ret_ord($c = 'X') ../trace.php:10
    0.0007     117584       -> ord('X') ../trace.php:5
    0.0009     117584     -> ret_ord($c = 'd') ../trace.php:10
    0.0009     117584       -> ord('d') ../trace.php:5
    0.0010     117584     -> ret_ord($c = 'e') ../trace.php:10
    0.0011     117584       -> ord('e') ../trace.php:5
    0.0012     117584     -> ret_ord($c = 'b') ../trace.php:10
    0.0013     117584       -> ord('b') ../trace.php:5
    0.0014     117584     -> ret_ord($c = 'u') ../trace.php:10
    0.0014     117584       -> ord('u') ../trace.php:5
    0.0016     117584     -> ret_ord($c = 'g') ../trace.php:10
    0.0016     117584       -> ord('g') ../trace.php:5
    0.0019      41152
TRACE END   [2007-05-06 14:37:16]
```

除了[xdebug.collect_params](https://xdebug.org/docs/all_settings#collect_params)设置之外，还有一些影响跟踪文件输出的设置。选项`show_mem_delta = 1`显示输出文件中两条不同行之间的内存使用率差异。

**输出（show_mem_delta = 1）：**

```
TRACE START [2007-05-06 14:37:06]
    0.0003 114112  - > {main}（）../trace.php:0
    0.0004 114272  - > str_split（）../trace.php:8
    0.0153 117424  - > ret_ord（）../trace.php:10
    0.0165 117584  - > ord（）../trace.php:5
    0.0166 117584  - > ret_ord（）../trace.php:10
    0.0167 117584  - > ord（）../trace.php:5
    0.0168 117584  - > ret_ord（）../trace.php:10
    0.0168 117584  - > ord（）../trace.php:5
    0.0170 117584  - > ret_ord（）../trace.php:10
    0.0170 117584  - > ord（）../trace.php:5
    0.0172 117584  - > ret_ord（）../trace.php:10
    0.0172 117584  - > ord（）../trace.php:5
    0.0173 117584  - > ret_ord（）../trace.php:10
    0.0174 117584  - > ord（）../trace.php:5
    0.0177 41152
TRACE END [2007-05-06 14:37:07]
```

**输出（collect_return = 1）：**

```
TRACE START [2007-05-06 14:37:35]
    0.0003     114112   -> {main}() ../trace.php:0
    0.0004     114272     -> str_split('Xdebug') ../trace.php:8
                          >=> array (0 => 'X', 1 => 'd', 2 => 'e', 3 => 'b', 4 => 'u', 5 => 'g')
    0.0007     117424     -> ret_ord($c = 'X') ../trace.php:10
    0.0007     117584       -> ord('X') ../trace.php:5
                            >=> 88
                          >=> 88
    0.0009     117584     -> ret_ord($c = 'd') ../trace.php:10
    0.0009     117584       -> ord('d') ../trace.php:5
                            >=> 100
                          >=> 100
    0.0011     117584     -> ret_ord($c = 'e') ../trace.php:10
    0.0011     117584       -> ord('e') ../trace.php:5
                            >=> 101
                          >=> 101
    0.0013     117584     -> ret_ord($c = 'b') ../trace.php:10
    0.0013     117584       -> ord('b') ../trace.php:5
                            >=> 98
                          >=> 98
    0.0015     117584     -> ret_ord($c = 'u') ../trace.php:10
    0.0016     117584       -> ord('u') ../trace.php:5
                            >=> 117
                          >=> 117
    0.0017     117584     -> ret_ord($c = 'g') ../trace.php:10
    0.0018     117584       -> ord('g') ../trace.php:5
                            >=> 103
                          >=> 103
                        >=> 1
    0.0021      41152
TRACE END   [2007-05-06 14:37:35]
```

所有函数调用的返回值也是可见的。

**输出（trace_format=1）：**

```
Version: 2.0.0RC4-dev
TRACE START [2007-05-06 18:29:01]
1	0	0	0.010870	114112	{main}	1	../trace.php	0
2	1	0	0.032009	114272	str_split	0	../trace.php	8
2	1	1	0.032073	116632
2	2	0	0.033505	117424	ret_ord	1	../trace.php	10
3	3	0	0.033531	117584	ord	0	../trace.php	5
3	3	1	0.033551	117584
2	2	1	0.033567	117584
2	4	0	0.033718	117584	ret_ord	1	../trace.php	10
3	5	0	0.033740	117584	ord	0	../trace.php	5
3	5	1	0.033758	117584
2	4	1	0.033770	117584
2	6	0	0.033914	117584	ret_ord	1	../trace.php	10
3	7	0	0.033936	117584	ord	0	../trace.php	5
3	7	1	0.033953	117584
2	6	1	0.033965	117584
2	8	0	0.034108	117584	ret_ord	1	../trace.php	10
3	9	0	0.034130	117584	ord	0	../trace.php	5
3	9	1	0.034147	117584
2	8	1	0.034160	117584
2	10	0	0.034302	117584	ret_ord	1	../trace.php	10
3	11	0	0.034325	117584	ord	0	../trace.php	5
3	11	1	0.034342	117584
2	10	1	0.034354	117584
2	12	0	0.034497	117584	ret_ord	1	../trace.php	10
3	13	0	0.034519	117584	ord	0	../trace.php	5
3	13	1	0.034536	117584
2	12	1	0.034549	117584
1	0	1	0.034636	117584
TRACE END   [2007-05-06 18:29:01]
```

最后一个标签显示了一个不同的输出格式，它更容易解析，但难以阅读。该[xdebug.trace_format](https://xdebug.org/docs/all_settings#trace_format)因此设置主要是对分析工具有用的。