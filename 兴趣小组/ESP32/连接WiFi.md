
# 1、连接LED

![](img/Pasted%20image%2020230808174122.png)

连接LED，用LED表示WiFi是否连接成功

# 2、导入模块

导入需要的模块

```python
# 导入Pin模块
from machine import Pin
import time
import network
```

# 3、定义对象和常量

```python
# 定义LED控制对象
led1 = Pin(2, Pin.OUT)
# 路由器WiFi名字和密码
ssid = "test"
password = "123456"
```

> 注意：替换成自己WiFi的名字和密码，这里只是举个例子

# 4、WiFi连接函数

这里是最核心的部分

```python
# WiFi连接
def wifi_connect():
    # STA模式
    wlan = network.WLAN(network.STA_IF)
    # 激活
    wlan.active(True)
    # 记录时间超时判断
    start_time = time.time()
    if not wlan.isconnected():
        print("connecting to network...")
        # 输入WiFi账号和密码
        wlan.connect(ssid,password)
        while not wlan.isconnected():
            led1.value(1)
            time.sleep_ms(300)
            led1.value(0)
            time.sleep_ms(300)
  
            # 超时判断，15秒没连接成功判定为超时
            if time.time()-start_time>15:
                print("timeout!")
                break
		print("network information:", wlan.ifconfig())
    else:
        led1.value(0)
        print("network information:", wlan.ifconfig())
```

运行程序，连接WiFi成功后，控制台会输出信息

![](img/Pasted%20image%2020230808175900.png)

> 注意：如果连接失败，可能是因为WiFi频度的原因，请连接自己手机的共享热点再试一次

