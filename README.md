# konlib

由于网页版不支持闭源的cpp插件，那就用命令行来编译和调试就可以了。

## 编译

打开命令行，然后运行

```bash
gcc -g -o hello.bin main.c hello.c
```

> -g 参数表示要生成gdb调试器所需的信息
> -o main.bin 表示编译结果要命名为 hello.bin，后缀名本来不加也可以，只是为了方便 .gitignore 识别，所以还是加一下。毕竟这个项目结构太简单了，都没有把源代码和二进制分开。嗯，这样不分开更方便你学习，之后你学会了你就可以分开了。

## 调试

参考这个帖子：http://c.biancheng.net/view/8153.html

```bash
gdb hello.bin
```

然后就可以各种调试了，例如：
1. 先list 看一下代码

`list`

```bash
gitpod /workspace/konlib (main) $ gcc -g -o hello.bin main.c hello.c
gitpod /workspace/konlib (main) $ gdb hello.bin
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04.1) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from hello.bin...
(gdb) list
1       #include <stdlib.h>
2
3       #include "hello.h"
4
5       int main(int argc, char** argv) {
6
7           int v = 10;
8
9           say_hello(v);
10
(gdb) 
```

2. 接下来在地8行设置断点：

`break 8`

```bash
(gdb) list
1       #include <stdlib.h>
2
3       #include "hello.h"
4
5       int main(int argc, char** argv) {
6
7           int v = 10;
8
9           say_hello(v);
10
(gdb) break 8
Breakpoint 1 at 0x1183: file main.c, line 9.
(gdb) 
```

3. 断电设置好了，就来运行程序

`run`

```bash
(gdb) list
1       #include <stdlib.h>
2
3       #include "hello.h"
4
5       int main(int argc, char** argv) {
6
7           int v = 10;
8
9           say_hello(v);
10
(gdb) break 8
Breakpoint 1 at 0x1183: file main.c, line 9.
(gdb) run
Starting program: /workspace/konlib/hello.bin 
warning: Error disabling address space randomization: Operation not permitted

Breakpoint 1, main (argc=1, argv=0x7ffd7af65d88) at main.c:9
9           say_hello(v);
(gdb) 
```

4. 这个时候可以查看一下变量v的值

`print v`

```bash
(gdb) list
1       #include <stdlib.h>
2
3       #include "hello.h"
4
5       int main(int argc, char** argv) {
6
7           int v = 10;
8
9           say_hello(v);
10
(gdb) break 8
Breakpoint 1 at 0x1183: file main.c, line 9.
(gdb) run
Starting program: /workspace/konlib/hello.bin 
warning: Error disabling address space randomization: Operation not permitted

Breakpoint 1, main (argc=1, argv=0x7ffd7af65d88) at main.c:9
9           say_hello(v);
(gdb) print v
$1 = 10
(gdb) 
```

5. 如果要退出调试

`quit`

```bash
(gdb) list
1       #include <stdlib.h>
2
3       #include "hello.h"
4
5       int main(int argc, char** argv) {
6
7           int v = 10;
8
9           say_hello(v);
10
(gdb) break 8
Breakpoint 1 at 0x1183: file main.c, line 9.
(gdb) run
Starting program: /workspace/konlib/hello.bin 
warning: Error disabling address space randomization: Operation not permitted

Breakpoint 1, main (argc=1, argv=0x7ffd7af65d88) at main.c:9
9           say_hello(v);
(gdb) print v
$1 = 10
(gdb) quit
A debugging session is active.

        Inferior 1 [process 5108] will be killed.

Quit anyway? (y or n) y
gitpod /workspace/konlib (main) $ 
```