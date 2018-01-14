# 基础特性

### 相关设置参数

#### xdebug.default_enable

类型：boolean，默认值：1
参数设置为1时，在错误事件中会显示堆栈跟踪信息。您可以使用`xdebug_disable()`来禁用显示你的代码的栈跟踪。由于这是Xdebug的基本功能之一，建议将此设置设置为1。

#### xdebug.force_display_errors

> 该功能仅适用于`Xdebug> = 2.3`

类型：int，默认值：0，
如果该参数被设置为1，那么总是会显示错误 ，不管PHP的display_errors 是什么设置。

#### xdebug.force_error_reporting

> 该功能仅适用于`Xdebug> = 2.3`

类型：int，默认值：0，
这个参数的设置是一个位掩码，就像error_reporting。该位掩码将与由error_reporting表示的位掩码**进行逻辑“或”运算**，从而确定应显示哪些错误。此设置只能在php.ini中进行，无论应用程序使用`ini_set()`设置何值，都会强制显示某些错误。

#### xdebug.halt_level

> 该功能仅适用于`Xdebug> = 2.3`

类型：int，默认值：0，
这个参数允许您配置掩码，以确定显示哪些通知和(或)警告转换为错误。您可以配置由PHP生成的通知和警告，以及您自己生成的通知和警告（通过`trigger_error()`）。

例如，要将`strlen()`（不带参数）的警告转换为错误，您应该这样做：

```php
ini_set('xdebug.halt_level', E_WARNING);
strlen();
echo "Hi!\n";
```

这将导致错误信息的显示以及脚本的中止。`echo "Hi!\n";`将不会被执行。

该设置是一个位掩码，所以要将所有通知和警告转换为所有应用程序的错误，可以在php.ini中设置：

```ini
xdebug.halt_level = E_WARNING | E_NOTICE | E_USER_WARNING | E_USER_NOTICE
```

位掩码只支持上面提到的四个级别。

#### xdebug.max_nesting_level

类型：int，默认值：256
控制无限递归保护的保护机制。此设置的值是在脚本中止之前所允许的嵌套函数的最大级别。

在Xdebug 2.3之前，默认值是100。

#### xdebug.max_stack_frames

> 该功能仅适用于Xdebug> = 2.3

类型：int，默认值：-1。
控制堆栈跟踪中显示的堆栈帧的数量，包括PHP错误堆栈跟踪期间的命令行以及HTML跟踪的浏览器。

#### xdebug.scream

> 该功能仅适用于`Xdebug> = 2.1`

类型：boolean，默认值：0。
如果此设置为1，则Xdebug将禁用`@操作符`，以便通知，警告和错误不再隐藏。

### 相关函数

#### string xdebug_call_class()[int $ depth = 1])

正常情况返回调用类，如果堆栈帧不存在返回`NULL`，堆栈帧没有类信息返回`FALSE`

此函数返回定义当前方法的类的名称，或者 `FALSE如果没有类与此调用关联。

**Example:**

```php
<?php
class Strings
{
    static function fix_string($a)
    {
        echo
            xdebug_call_class().
            "::".
            xdebug_call_function().
            " is called at ".
            xdebug_call_file().
            ":".
            xdebug_call_line();
    }
}

$ret = Strings::fix_string( 'Derick' );
?>
```

**Returns:**

```
Called @ /home/httpd/html/test/xdebug_caller.php:17 from ::{main}
```

要从较早的堆栈帧中检索信息，请使用可选的 `$depth`参数。参数为`1`时返回执行`xdebug_call_class()`方法的调用信息的值：

**Example:**

```php
<?php
class Strings
{
    static function fix_string( $a )
    {
        echo
            xdebug_call_class( 1 ).
            "::".
            xdebug_call_function( 1 ).
            " is called at ".
            xdebug_call_file( 1 ).
            ":".
            xdebug_call_line( 1 );
    }
}

$ret = Strings::fix_string( 'Derick' );
?>
```

**Returns:**

```
Strings::fix_string is called at /home/httpd/html/test/xdebug_caller:17
```

参数为`2`（默认值）时，返回调用当前方法的方法调用信息：

**Example:**

```php
<?php
class Strings
{
    static function fix_string( $a )
    {
        echo
            xdebug_call_class( 2 ).
            "::".
            xdebug_call_function( 2 ).
            " is called at ".
            xdebug_call_file( 2 ).
            ":".
            xdebug_call_line( 2 );
    }

    static function fix_strings( array $a )
    {
        foreach ( $a as $element )
        {
            self::fix_string( $a );
        }
    }
}

$ret = Strings::fix_strings( [ 'Derick' ] );
?>
```

**Returns:**

```
Strings::fix_strings is called at /home/httpd/html/test/xdebug_caller:25
```

参数为`0`时，返回相应的`xdebug_call_*` 调用的信息：

**Example:**

```php
<?php
class Strings
{
    static function fix_string( $a )
    {
        echo
            xdebug_call_class( 0 ).
            "::".
            xdebug_call_function( 0 ).
            " is called at ".
            xdebug_call_file( 0 ).
            ":".
            xdebug_call_line( 0 );
    }

    static function fix_strings( array $a )
    {
        foreach ( $a as $element )
        {
            self::fix_string( $a );
        }
    }
}

$ret = Strings::fix_strings( [ 'Derick' ] );
?>
```

**Returns:**

```
::xdebug_call_function is called at /home/httpd/html/test/xdebug_caller:13
```

#### string xdebug_call_file( [int $depth = 1] )

返回调用文件，如果堆栈帧不存在返回`NULL`

该函数返回从当前函数/方法执行的文件名。

要从较早的堆栈帧中检索信息，请使用可选的 `$depth`参数。

有关示例和更广泛的信息，请参见`xdebug_call_class()`

#### string xdebug_call_function( [int $depth = 1] )

如果堆栈帧不存在返回`NULL`，如果堆栈帧没有函数/方法信息返回`FALSE`。

该函数返回当前函数/方法的名称。

要从较早的堆栈帧中检索信息，请使用可选的 `$depth`参数。

有关示例和更广泛的信息，请参见`xdebug_call_class()`

#### int xdebug_call_line( [int $depth = 1] )

返回改函数调用所在行号，如果堆栈帧不存在返回`NULL`。

这个函数返回从当前函数/方法被调用的行号。

要从较早的堆栈帧中检索信息，请使用可选的 `$depth`参数。

有关示例和更广泛的信息，请参见`xdebug_call_class()`

#### void xdebug_disable()

禁用堆栈跟踪

禁止在错误情况下显示堆栈跟踪。

#### void xdebug_enable()

启用堆栈跟踪

在错误情况下启用显示堆栈跟踪。

#### string xdebug_get_collected_errors( [int clean] )

> 该功能仅适用于`Xdebug> = 2.1`

返回所有收集的错误消息。

此函数返回收集缓冲区中的所有错误，其中包含使用`xdebug_start_error_collection()`开始错误收集时存储在那里的所有错误 。

默认情况下，这个函数不会清除错误收集缓冲区。当该函数的参数为`true`时缓冲区内容将被清除。

此函数返回一个字符串，其中包含格式化为“Xdebug table”的所有收集的错误。

#### array xdebug_get_headers()

> 该功能仅适用于`Xdebug> = 2.1`

通过调用PHP的`header()`函数返回所有的头文件。

返回所有使用PHP的`header()`函数设置的头文件，或者PHP内部设置的任何其他头文件（比如通过`setcookie()`），作为返回数组内容。

**Example:**

```php
<?php
header( "X-Test", "Testing" );
setcookie( "TestCookie", "test-value" );
var_dump( xdebug_get_headers() );
?>
```

**Returns:**

```
array(2) {
  [0]=>
  string(6) "X-Test"
  [1]=>
  string(33) "Set-Cookie: TestCookie=test-value"
}
```

#### bool xdebug_is_enabled()

返回是否启用堆栈跟踪

返回是否在出现错误时显示堆栈轨迹。

#### int xdebug_memory_usage()

返回当前的内存使用情况

返回脚本使用的当前内存量。在PHP 5.2.1之前，这只有在使用`--enable-memory-limit`进行编译时才有效。从5.2.1版本开始，这个函数总是可用的。

#### int xdebug_peak_memory_usage()

返回峰值内存使用情况

返回脚本直到现在使用的最大内存量。在PHP 5.2.1之前，这只有在使用`--enable-memory-limit`进行编译时才有效。从5.2.1版本开始，这个函数总是可用的。

#### void xdebug_start_error_collection()

开始记录所有的通知，警告和错误，并阻止他们的显示。该功能仅适用于`Xdebug> = 2.1`

当执行这个函数时，Xdebug将导致PHP不显示任何通知，警告或错误。相反，它们根据Xdebug的正常错误格式化规则（即带有红色感叹号的错误表）格式化，然后存储在缓冲区中。这将继续，直到你调用`xdebug_stop_error_collection()`。

这个缓冲区的内容可以通过调用`xdebug_get_collected_errors()`来获取 ，然后显示。如果你想防止Xdebug强大的错误报告功能破坏你的布局，这是非常有用的。

#### void xdebug_stop_error_collection()

停止记录由`xdebug_start_error_collection()`启动的所有通知，警告和错误。该功能仅适用于`Xdebug> = 2.1`

执行此函数时，由`xdebug_start_error_collection()`启动的错误收集将 被中止。存储在收集缓冲区中的错误不会被删除，仍然可以通过`xdebug_get_collected_errors()`获取 。

#### float xdebug_time_index()

返回当前时间索引

从脚本开始以秒为单位返回当前时间索引。

**Example:**

```php
<?php
echo xdebug_time_index(), "\n";
for ($i = 0; $i < 250000; $i++)
{
    // do nothing
}
echo xdebug_time_index(), "\n";
?>
```

**Returns:**

```
0.00038003921508789 
0.76580691337585
```

