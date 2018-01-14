# 变量打印特性

Xdebug替换了PHP的`var_dump()`函数来显示变量。Xdebug的版本包含不同类型的不同颜色，并限制数组元素/对象属性的数量，最大深度和字符串长度。还有一些其他功能处理变量显示。

## 设置对var_dump的影响

有许多设置可以控制Xdebug修改的var_dump()函数的输出 ：xdebug.var_display_max_children，xdebug.var_display_max_data和xdebug.var_display_max_depth。这三个设置的效果最好用一个例子来展示。下面的脚本运行四次，每次都有不同的设置。您可以使用这些标签来查看差异。

**代码：**

```php
<?php
class test {
    public $pub = false;
    private $priv = true;
    protected $prot = 42;
}
$t = new test;
$t->pub = $t;
$data = array(
    'one' => 'a somewhat long string!',
    'two' => array(
        'two.one' => array(
            'two.one.zero' => 210,
            'two.one.one' => array(
                'two.one.one.zero' => 3.141592564,
                'two.one.one.one'  => 2.7,
            ),
        ),
    ),
    'three' => $t,
    'four' => range(0, 5),
);
var_dump( $data );
?>
```

**输出（默认值）：**

```
array
  'one' => string 'a somewhat long string!' (length=23)
  'two' => 
    array
      'two.one' => 
        array
          'two.one.zero' => int 210
          'two.one.one' => 
            array
              ...
  'three' => 
    object(test)[1]
      public 'pub' => 
        &object(test)[1]
      private 'priv' => boolean true
      protected 'prot' => int 42
  'four' => 
    array
      0 => int 0
      1 => int 1
      2 => int 2
      3 => int 3
      4 => int 4
      5 => int 5
```

**输出（xdebug.var_display_max_children = 2）：**

```
array
  'one' => string 'a somewhat long string!' (length=23)
  'two' => 
    array
      'two.one' => 
        array
          'two.one.zero' => int 210
          'two.one.one' => 
            array
              ...
  more elements...
```

**输出（xdebug.var_display_max_data = 16）：**

```
array
  'one' => string 'a somewhat long '... (length=23)
  'two' => 
    array
      'two.one' => 
        array
          'two.one.zero' => int 210
          'two.one.one' => 
            array
              ...
  'three' => 
    object(test)[1]
      public 'pub' => 
        &object(test)[1]
      private 'priv' => boolean true
      protected 'prot' => int 42
  'four' => 
    array
      0 => int 0
      1 => int 1
      2 => int 2
      3 => int 3
      4 => int 4
      5 => int 5
```

**输出（xdebug.var_display_max_depth = 2）：**

```
array
  'one' => string 'a somewhat long string!' (length=23)
  'two' => 
    array
      'two.one' => 
        array
          ...
  'three' => 
    object(test)[1]
      public 'pub' => 
        &object(test)[1]
      private 'priv' => boolean true
      protected 'prot' => int 42
  'four' => 
    array
      0 => int 0
      1 => int 1
      2 => int 2
      3 => int 3
      4 => int 4
      5 => int 5
```

**输出（xdebug.var_display_max_children = 3，xdebug.var_display_max_data = 8，xdebug.var_display_max_depth = 1）：**

```
array
  'one' => string 'a somewh'... (length=23)
  'two' => 
    array
      ...
  'three' => 
    object(test)[1]
      ...
  more elements...
```

## 相关设置

#### xdebug.cli_color

> 该功能仅适用于`Xdebug> = 2.2`

类型：整数，默认值：0，

如果此设置为1，则在CLI模式下以及输出为tty时，var_dumps和堆栈跟踪Xdebug将着色输出。在Windows上， 需要安装[ANSICON](http://adoxa.110mb.com/ansicon/)工具。

如果设置为2，那么无论是否连接到tty或是否安装ANSICON，Xdebug将始终为var_dumps和堆栈跟踪着色。在这种情况下，您最终可能会看到转义码。

看到[这篇文章](http://drck.me/clicolor-9cr)的一些更多的信息。

#### xdebug.overload_var_dump

> 该功能仅适用于`Xdebug> = 2.1`

当`php.ini`中html_errors设置为1或2时，Xdebug会默认更改var_dump输出。如果您不希望如此，您可以将其值设置为0，但是首先检查是否智能关闭html_errors。

该值设置为2时，除了很好的格式化var_dump()输出外，它还会将文件名和行号添加到输出中。

在Xdebug 2.4之前，这个设置的默认值是 `1`。

#### xdebug.var_display_max_children

类型：整数，默认值：128

当使用xdebug_var_dump()， xdebug.show_local_vars或通过函数轨迹显示变量时，控制数组的数量和子对象的属性。

要禁用任何限制，请使用-1作为值。

此设置对通过远程调试功能发送给客户端的子项数量没有任何影响。

#### xdebug.var_display_max_data

类型：整数，默认值：512

控制使用xdebug_var_dump()， xdebug.show_local_vars或通过函数轨迹显示变量时显示的最大字符串长度。

要禁用任何限制，请使用-1作为值。

此设置对通过远程调试功能发送给客户端的子项数量没有任何影响。

#### xdebug.var_display_max_depth

通过xdebug_var_dump()， xdebug.show_local_vars或函数轨迹显示变量时，控制数组元素和对象属性的嵌套级别。

您可以选择的最大值是1023。您也可以使用-1作为值来选择此最大值。

此设置对通过远程调试功能发送给客户端的子项数量没有任何影响。

## 相关函数

#### void var_dump( [mixed var [, ...]] )

显示有关变量的详细信息

这个函数被Xdebug重载，参见xdebug_var_dump()的描述 。

#### void xdebug_debug_zval( [string varname [, ...]] )

显示有关变量的信息

此功能显示有关一个或多个变量的结构化信息，其中包括其类型，值和引用计数信息。数组通过值递归地进行探索。这个函数的实现方式与PHP的debug_zval_dump()函数不同，是用来解决debug_zval_dump()函数存在的问题，因为变量本身实际上被传递给函数。Xdebug的版本更好，因为它使用变量名查找内部符号表中的变量，并直接访问所有属性，而不必处理实际将变量传递给函数。结果是这个函数返回的信息比PHP自己的显示zval信息的函数要准确得多。

自Xdebug 2.3以来， 支持除简单变量名称（如下面的“a [2]”）之外的任何其他内容。

**例：**

```php
<?php
    $a = array(1, 2, 3);
    $b =& $a;
    $c =& $a[2];

    xdebug_debug_zval('a');
    xdebug_debug_zval("a[2]");
?>
```

**输出：**

```
a: (refcount=2, is_ref=1)=array (
	0 => (refcount=1, is_ref=0)=1, 
	1 => (refcount=1, is_ref=0)=2, 
	2 => (refcount=2, is_ref=1)=3)
a[2]: (refcount=2, is_ref=1)=3
```

#### void xdebug_debug_zval_stdout( [string varname [, ...]] )

将有关变量的信息返回到stdout。

此功能显示有关一个或多个变量的结构化信息，其中包括其类型，值和引用计数信息。数组通过值递归地进行探索。与xdebug_debug_zval()的不同之处在于信息不是通过Web服务器API层显示的，而是直接显示在标准输出上（所以当你在单进程模式下运行Apache时，它将在控制台上输出）。

**例：**

```php
<?php
    $a = array(1, 2, 3);
    $b =& $a;
    $c =& $a[2];

    xdebug_debug_zval_stdout('a');
```

**输出：**

```
a: (refcount=2, is_ref=1)=array (
	0 => (refcount=1, is_ref=0)=1, 
	1 => (refcount=1, is_ref=0)=2, 
	2 => (refcount=2, is_ref=1)=3)
```

#### void xdebug_dump_superglobals()

显示有关超级全局的信息

这个函数按照`xdebug.dump.*`在php.ini的设置转储超级全局元素的值。对于下面的例子，php.ini中的设置是：

```ini
xdebug.dump.GET=*
xdebug.dump.SERVER=REMOTE_ADDR

Query string:
?var=fourty%20two&array[a]=a&array[9]=b
```

**返回：**

| Dump *$_SERVER*             |                                   |
| --------------------------- | --------------------------------- |
| `$_SERVER['REMOTE_ADDR'] =` | `string '127.0.0.1' *(length=9)*` |

| Dump *$_GET*       |                                          |
| ------------------ | ---------------------------------------- |
| `$_GET['var'] =`   | `string 'fourty two' *(length=10)*`      |
| `$_GET['array'] =` | `**array**  'a' => string 'a' *(length=1)*  9 => string 'b' *(length=1)*` |

#### void xdebug_var_dump( [mixed var [, ...]] )

显示有关变量的详细信息

此功能显示关于一个或多个表达式的结构化信息，包括其类型和值。数组通过值递归地进行探索。请参阅php.ini设置影响此功能的变量显示功能的介绍（上文）。

**例：**

```php
<?php
ini_set('xdebug.var_display_max_children', 3 );
$c = new stdClass;
$c->foo = 'bar';
$c->file = fopen( '/etc/passwd', 'r' );
var_dump(
    array(
        array(TRUE, 2, 3.14, 'foo'),
        'object' => $c
    )
);
?>  
```

**输出：**

```
array
  0 => 
    array
      0 => boolean true
      1 => int 2
      2 => float 3.14
      more elements...
  'object' => 
    object(stdClass)[1]
      public 'foo' => string 'bar' (length=3)
      public 'file' => resource(3, stream)
```

