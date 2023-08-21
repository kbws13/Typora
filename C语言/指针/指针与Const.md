# 指针与Const
指针可以是const，指针所指的变量也可以是const

# 指针是const
表示一旦得到某个变量的地址，不能再指向其他变量

```cpp
int *const p=&i;//q是const
*q=26;//ok
q++;//ERROR
```
# 所指是const
表示不能通过这个指针去修改那个变量（并不能使得那个变量成为const）

```cpp
const int *p=&i;
*p=26;//ERROR!（*p）是const
i=26;//ok
p=&j;//ok
```
```cpp
int i;
const int* p1=&i;
int const* p2=&i;
//上面两个是不能通过指针去修改变量
int *const p3=&i;
//这个是指针不能被修改
```
判断哪个被const了的标志是const在\*的前面还是后面

# 转换
总是可以把一个非const的值转换成const的

```cpp
void f(const int* x);
int a=5;
f(&a);//ok
const int b=a;
f(&b);//ok
b=a+1;//Error
```
当要传递的参数的类型比地址大的时候，这是常用的手段：既能用比较少的字节数传递值给参数，又能避免函数对外面变量的修改。

# const数组
```cpp
const int a[]={1,2,3,4,5,6};
```
数组变量已经是const的指针了，这里的const表明数组的每个单元都是const int

所以必须通过初始化赋值

# 保护数组值
因为把数组传入函数时传递的是地址，所以函数可以在内部修改数组的值

为了保护数组不被函数破坏，可以设置参数为const

```cpp
int sum(const int a[],int length);
```
