# switch与if语句的应用 

例：计算2008年8月8日这一天是该年中的第几天 

```c
#include<stdio.c>
int main(){
int year=2008;
int month=8;
int day=8;
int sum,num,flag;
switch(month){
    case 1:num=0;break;
    case 2:num=31;break;
    ………..
    case 12:num=334;break;
    }
sum=num+day;
if(year%4==0){
    flag=1;
    }elae{
	flag=0;
    }
if(flag==1&month>2){
    sum=++sum;
}
printf("%d年%d月%d日是该年第%d天",year,month,day,sum);
}s
```

