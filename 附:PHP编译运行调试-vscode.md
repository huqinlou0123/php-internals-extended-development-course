# VSCODE配置调试PHP代码

## 下载php源代码
    * 官网下载:https://secure.php.net/downloads.php
    * 或GITHUB:git clone http://github.com/php/php-src
---


## 我们要进行的配置和编译命令:
    ./configure --disable-all --enable-cli --enable-debug
        (--enable-debug启用调试模式,具有多重效果:
        编译将使用 -g运行以生成包括行号、变量的类型和作用域、函数名字、函数参数和函数的作用域等源文件特性的调试信息.
        另外使用-O0,会让gcc编译时不对代码优化.
        此外,调试模式定义了 ZEND_DEBUG宏,它将启动引擎中的各种调试助手.除其他事项外,还将报告内存泄漏以及某些数据结构的不正确使用.)
    make -jN
        (N为CPU数量,作用:make --help查看)


## vscode配置
    1:单击左边栏Extentions搜索 `C/C++`,安装第一个`C/C++ for Visual Studio Code`扩展并重启编辑器,
    2:File->Open 打开源码目录
    3:单击左边栏Debug,选择Add Configuration,并编辑launch.json内容为：
```json
    {
      "version": "0.2.0",
      "configurations": [
          {
              "name": "php-debug",
              "type": "cppdbg",
              "request": "launch",
              "program": "/your/php-source-code-path/sapi/cli/php",
              "args": [
                  "-f","/your/php-test-script-path/test.php"
              ],
              "stopAtEntry": false,
              "cwd": "/your/php-source-code-path/",
              "environment": [],
              "externalConsole": true,
              "MIMode": "lldb"
          }
      ]
    }
```
#### launch.json配置参数讲解：
* program为要调试的PHP程序,前面configure && make之后 会在源码目录里生成/sapi/cli/php（cli）程序，也是我们要调试的程序.
* args为程序运行的附加参数，我们这里指定了要运行的测试脚本，以空格分隔的多个参数对应args数组里的多个元素。
* cwd为我们的源码目录


#### 配置完成，配置比较简单，功能较eclipse较弱，可以开始调试我们的代码了。
