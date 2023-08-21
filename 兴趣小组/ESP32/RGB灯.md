
本次课程中我们使用ESP32控制RGB灯循环点亮，且按照红、橙、黄、绿、蓝、靛、紫的颜色循环变化

# 1、连接引脚

使用杜邦线按下图所示连接引脚

![](img/Pasted%20image%2020230808171229.png)

# 2、导入依赖

接下来我们需要导入我们需要的模块

```python
from machine import Pin

from neopixel import NeoPixel

import time
```

# 3、定义变量

设置需要控制的引脚和RGB灯串联的个数

```python
pin = 16

rgb_num=5

rgb_led = NeoPixel(Pin(pin,Pin.OUT),rgb_num)
```

# 4、定义RGB颜色

既然我们想让RGB灯变化颜色，那我们就需要定义颜色，来让其按照我们定义的参数显示颜色

```python
RED = (255,0,0)

ORANGE = (255,165,0)

YELLOW=(255,150,0)

GREEN=(0,255,0)

BLUE=(0,0,255)

INDIGO=(75,0,130)

VIOLET=(138,43,226)
```

然后我们将这些颜色变量放在一个元组中，表示不可变，方便我们遍历

```python
COLORS = (RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET)
```

接下来我们创建一个主方法，不断让其修改颜色

```python
if __name__ == "__main__":
    while True:
        for color in COLORS:
            for i in range(rgb_num):
                rgb_led[i]=(color[0], color[1], color[2])
                rgb_led.write()
                time.sleep_ms(100)
            time.sleep_ms(1000)
```

