
# 1、原理

想要让一个LED灯亮，那么就需要让他有电流通过

浏览MicroPython官方的文档
http://docs.micropython.org/en/latest/esp32/quickref.html

![](img/Pasted%20image%2020230808160812.png)

通过上面的文档我们知道，想要让一个引脚输出高电平，只需要找到对应的GPIO然后通过`on()`或者`value(1)`操作就可以，同理如果想要输出低电平让LED灯灭，只需要调用`off()`或者`value(0)`就行

下面我们来试试看

## 2、点亮LED灯

各位手里有开发板的同学请先使用杜邦线按照下面的图示将引脚和LED灯相连

![](img/Pasted%20image%2020230808162055.png)


```python
from machine import Pin 

pin2 = Pin(2, Pin.OUT) 
pin2.value(1)
```

通过Thonny编写上述代码，然后运行，此时会看到开发板上的蓝色LED灯亮了

# 3、闪烁的LED灯

原理：调用`value(1)`然后延时一小会，再调用`value(0)`延时一小会，然后重复上述操作

代码如下：

```python
import machine 
import time 

pin2 = machine.Pin(2, machine.Pin.OUT) 

while True: 
	pin2.value(1) 
	time.sleep(1) 
	pin2.value(0) 
	time.sleep(1)
```

运行之后就可以见到LED灯闪烁了