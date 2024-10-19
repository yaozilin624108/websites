#介绍

Shizuku可以帮助普通应用程序使用 adb/root you you you API，Java you appy_process you

Shizuku这个名字来源于[一个角色](https://danbooru.donmai.us/posts/3553474).

##静久为什么出生？

“世祖”的诞生主要有两个目的。

1.提供一种使用系统 API
2.方便一些只需要 adb

##Shizuku vs“老派”方法

###“老派”方法

例如，为了启用/禁用组件，一些需要root权限的应用程序执行`pm禁用`直接`苏`.

1.执行`苏`
2.执行`pm禁用`
3.（pre-Pie）用app_process（[看这里](https://android.googlesource.com/platform/frameworks/base/+/oreo-release/cmds/pm/pm))
4.（Pie+）执行本机程序`cmd` ([看这里](https://android.googlesource.com/platform/frameworks/native/+/pie-release/cmds/cmd/))
5. Process the parameters, interact with the system server through the binder, and process the result to output the text result.

Each of the "Execute" means a new process creation, su internally uses sockets to interact with the su daemon, and a lot of time and performance are consumed in such process. (Some poorly designed app will even execute `su` **every time** for each command)

The disadvantages of this type of method are:

1. **Extremely slow**
2. Need to process the text to get the result
3. Features are subject to available commands
4. Even if adb has sufficient permissions, the app requires root privileges to run

###Shizuku法

Shizuku应用程序将引导用户使用root或adb运行一个进程（Shizuku服务进程）。

1.当app进程启动时，Shizuku服务进程将活页夹发送到app进程。
2.app通过绑定器与Shizuku服务交互，Shizuku服务进程通过绑定器与系统服务器交互。

Shizuku的优点是：

1.最小的额外时间和性能消耗
2.它几乎与直接调用 API在我的面前（在我的面前）
