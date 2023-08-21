# goto语句

使用格式为:  goto语句标号：

其中语句标号是一个标识符，这个标识符加上一个”：“出现在函数某处，执行goto语句后，程序将跳转到该处并执行后面的语句

例：goto和if语句构成循环求10以内的数的和 

```c
#include<stdio.c>
	int main(){
		int sum=0;
		int i=0;
		//LOOP就是一个有效标识符
		LOOP:if(i<=10){
			sum+=i;
			i++;
			//转义到LOOP所在位置继续执行
			goto LOOP;
		}
		printf("sum=%d\n",sum);
		return 0;
	}
```

