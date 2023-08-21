# 分支结构之switch语句

语句结构 :

```c
switch（表达式）{
	case 常量表达式1：执行代码块1；
        break；
	……………
	case 常量表达式n：执行代码块n；
        break；
	default：执行代码块n+1；
}
```

注：

1. 在case后的各常量表达式的值各不相同
2. 在case子句后如没有break；会一直往后执行一直遇到break才会跳出switch
3. switch后面的表达式语句只能是整型或者字符型
4. 在case后，允许有多个语句，可以不用{ }括起来
5. 各case和default子句的顺序可以变动，而不会影响程序
6. default子句可以省略不用 