# 结束语句之break语句 

```c
#include<stdio.c>
int main()
{
    for(m=2;m<=50;m++)
    {
	for(n=2;n<m;n++)
	{
	    if(m%n==0)
	    break;
	}
	if(m=n)
	printf("%d",m);
    }
    return 0;
}
```

注：

1. 在没有循环结构的情况下，break不能在单独的if-else中使用 
2. 在多层循环中，一个break语句只能跳出当前循环 