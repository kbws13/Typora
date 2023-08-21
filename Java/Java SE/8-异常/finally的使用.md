# finally的使用

1、finally是可选的

2、finally中声明的是一定会被执行的代码。即使catch中又出现了异常，try中有return语句。catch中有return语句等

3、像数据库连接、输入输出流、网络编程Socket等资源，JVM是不能自动回收的，我们需要自己手动的进行资源的释放。此时的资源释放，就需要声明在finally中