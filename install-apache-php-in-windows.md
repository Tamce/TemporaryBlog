# 手动在 Windows 下安装 Apache + PHP
 手动安装步骤我自己也进行过好几次了，但有时候依旧会忘记一些步骤，这篇文章主要用作备忘，顺便也可以分享给大家。  
 本文章使用的版本分别是 `Apache 2.4` + `php 5.6`。

## 安装 PHP
### 下载 & 解压
 在 php 官网上下载需要的版本，我还比较保守并没有升级到 `PHP7` 因此一直使用的是 `PHP5.6`。  
 把 php 解压到想要的位置比如 `D:\php56`，然后可以直接重命名 php 目录下的 `php.ini-development` 或者 `php.ini-production` 为 `php.ini`，可选的可以复制到系统目录 `C:\Windows` 下。  

### 配置
 解压完毕后就是对 `php.ini` 的配置和修改，具体如下：  
 1. 找到 `extension_dir` 所在行，将其值修改为 php 目录下的 ext，如 `D:\php56\ext`。
 2. 找到 `include_path` 所在行，共两处，将其注释标明为 `windows` 的那行取消注释。
 3. 找到 `extension` 的位置将常用的拓展取消注释。
 4. （可选） 修改 `date.timezone`。

### 设置环境变量
 将 `D:\php56\ext` 和 `D:\php56` 添加到 `PATH` 环境变量中。  

### PHP 准备完毕
 至此， php 的安装和配置完成，如果要从命令行中使用的话已经没有问题了。  

## 安装 Apache
### 下载 & 解压
 Apache 官网是不提供 Windows 版本的二进制文件的，但是提供了一些其他组织为 Windows 构建的二进制文件的链接，通过官网找寻到相关站点下载。下载完成后解压到任意目录如： `D:\Apache24`。  

### 配置 Apache
 打开 `conf\httpd.conf` 修改配置：  
 1. 将 `Define SRVROOT` 后的路径改为安装目录如 `D:\Apache24`。
 2. 找到 `DirectoryIndex` ，末尾添加 `index.php index.phtml`。（是为了支持 php 文件作为目录默认文件）
 3. 找到 `AddType` 所在位置，添加 `AddType application/x-httpd-php .php .phtml`。 （这是为了使 php 拓展能够识别其该解释的文件后缀名，否则其不会自动解释 `.php` 后缀的文件）
 4. 在 `LoadModule` 处加载 php 拓展。 `LoadModule php5_module D:\php56\php5apache2_4.dll`，注意指向 php 安装目录下的 apache 2.4 拓展文件。
 5. 在载入拓展之后手动指定 `php.ini` 的位置。 `PHPIniDir D:\php56` （如果 `php.ini` 在 php 根目录下的话可以省略这步）

> 如果要让命令行中和 `Apache` 的 php 使用不同配置的话就可以通过 `Apache` 的配置文件指定 php 的配置文件，php 搜索配置文件的顺序可以在官网上查到。

### 设置环境变量，配置网站根
 在 `PATH` 环境变量中添加 Apache 安装目录下的 `bin` 文件夹（来让其中的可执行文件可被命令行直接调用）。  

 依旧在 Apache 的配置文件中，默认自动创建的网站根位置在安装目录下的 `htdocs`，可以通过修改 `DocumentRoot` 来改变网站根位置。  

> 如果要实现本地多站点可以配置 `VHost` 指定不同的 `ServerName`，如访问 `localhost` 和 `task.localhost` 时使用不同的目录作为网站根。另外建议使用 `Include` 来使配置文件组织更有条理。  

### 安装服务并运行
 打开命令行（管理员权限），使用 `httpd -k install` 安装 Apache 的服务，使用 `httpd -k start` 来启动服务。  
 在需要的时候可以使用 `httpd -k stop` 和 `httpd -k restart` 来停止和重启服务。  

 如果不希望以服务的形式运行的话也可以直接执行 `httpd`（双击执行和从命令行执行都可），这样在关闭窗口的时候 http 服务即被终止。  

## 确认安装完成
 在设置好的网站根目录添加 `index.php` 文件并写入 `<?php phpinfo();` 后访问本地站点检查 php 是否成功执行以及 php 的拓展的启用情况。  

 **显示的是 Apache 的欢迎页面？**
 因为没有删除 `index.html` 且在 `Apache` 的配置文件中 `html` 的优先级高于 `php`。  

> `DirectoryIndex` 决定了访问一个目录时搜索其中主页文件的优先顺序。

## 其他步骤
 其他配套软件的安装都是相对独立的（不需要和服务器软件或 php 有耦合），而且在 Windows 上大多提供了安装向导如 `MySQL` 等，直接安装即可。
