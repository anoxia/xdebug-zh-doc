# 安装

### Windows预编译的模块

有几个Windows预编译模块，它们都是PHP的非调试版本。你可以在[下载](https://xdebug.org/download.php) 页面获得这些信息。按照[这些指示](https://xdebug.org/wizard.php)安装Xdebug。

### 类UNIX系统下使用PECL安装

从Xdebug 0.9.0开始，你可以通过PEAR / PECL安装Xdebug。这只适用于PEAR版本0.9.1-dev或更高版本。

使用PEAR / PECL进行安装非常简单：

```shell
pecl install xdebug
```

然后将xdebug扩展配置添加到您的php.ini中：（配置时注意扩展所在路径，推荐使用绝对路径）

```ini
zend_extension="/usr/local/php/modules/xdebug.so"
```

**注意：**请不要将配置写成`extension = xdebug.so`， 这会导致加载xdebug扩展失败。

### macOS上通过Homebrew安装

PHP和Xdebug可以通过非官方的macOS包管理器[Homebrew](http://brew.sh/)进行安装。如果您是使用Homebrew安装的PHP（使用Homebrew安装PHP的[安装指南](https://github.com/Homebrew/homebrew-php#installation)）那么很容易通过brew install来安装Xdebug：

```shell
brew install homebrew/php/<php-version>-xdebug
```

例如：

```shell
brew install homebrew/php/php71-xdebug
```

您也可以使用brew搜索找到您需要的特定软件包：

```shell
brew search xdebug
```

通过Homebrew安装的Xdebug扩展将在安装后默认启用，扩展的额外配置通过向/usr/local/etc/php/<php-version>/conf.d/添加自定义的ini文件来完成。有关更多详细信息，请参考安装结束时brew在终端的输出。

## 通过编译源码进行安装

#### 获取源码

您可以通过三种渠道获得Xdebug源码：

1. PHP官方扩展库网站：[下载页面链接](http://pecl.php.net/package/xdebug)
2. Xdebug官网：[下载页面链接](https://xdebug.org/download.php#releases)
3. github release：[下载链接](https://github.com/xdebug/xdebug/releases)

#### 编译

您从PHP的其余部分分别编译Xdebug。但请注意，您需要访问脚本“phpize”和“php-config”。如果你的系统没有“phpize”和“php-config”，你将需要首先编译和安装PHP源代码，因为这些脚本是PHP编译和安装过程的副产品。（Debian用户可以安装所需的工具 `apt-get install php5-dev`）。源版本与安装的版本相匹配非常重要，因为PHP版本之间存在轻微但重要的区别。一旦你有权访问“phpize”和“php-config”，请执行以下操作：

1. 解压源码压缩包。请注意，您不需要解压缩PHP源代码树中的压缩包。如上所述，Xdebug是独立编译的。

   ```shell
   tar -xzf xdebug-2.5.5.tgz。
   ```

2. 进入Xdebug目录

   ```shell
   cd xdebug-2.5.5
   ```

3. 运行phpize（如果phpize不在你的路径中，使用绝对路径/ path / to / phpize）。确保你使用属于你想使用Xdebug的PHP版本的phpize。如果您在查找要使用的phpize时遇到问题，请参考常见问题解答。

   ```shell
   phpize
   ```

4. 检查编译条件

   ```shell
   ./configure --enable-xdebug
   ```

5. 执行源码编译操作

   ```shell
   make
   ```

6. 安装

   ```shell
   make install
   ```