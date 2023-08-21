# 结束语句之continue语句

例：计算1到20之间不能被3整除的数字之和 

```c
#include<stdio.h>
int main()
{
    int i,sum;
    for(i=1,sum=0;i<=20;i++){
        if(i%3==0){
	    continue;
        }
        sum+=i;
    }
}
```

continue的作用是结束本次循环开始执行下一次循环

==注：break是跳出当前整个循环，continue是结束本次循环开始下一次==

