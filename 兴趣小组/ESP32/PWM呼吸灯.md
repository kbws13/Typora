
# 1、什么是PWM

`PWM`（Pulse Width Modulation）简称`脉宽调制`，是利用微处理器的数字输出来对模拟电路进行控制的一种非常有效的技术，广泛应用在测量、通信、工控等方面

# 2、Python控制PWM

官方给的示例代码如下

```python
from machine import Pin, PWM

pwm0 = PWM(Pin(0))         # 从引脚创建PWM对象
freq = pwm0.freq()         # 获取当前频率（默认5kHz）
pwm0.freq(1000)            # 将PWM频率设置为1Hz到40MHz

duty = pwm0.duty()         # 获取当前占空比，范围0-1023（默认512，50%）
pwm0.duty(256)             # 将从0到1023的占空比设置为占空比/1023（现在为25%）
```

其中占空比是调节LED灯亮度的关键

所以我们只需要循环改变占空比数值就好

# 3、呼吸灯

完整代码如下
```python
from machine import Pin, PWM
import time


led2 = PWM(Pin(2))
led2.freq(1000)


while True:
    for i in range(0, 1024):
        led2.duty(i)
        time.sleep_ms(1)
        
    for i in range(1023, -1, -1):
        led2.duty(i)
        time.sleep_ms(1)
```

运行代码之后可以看到LED灯在变亮和变暗之间循环往复