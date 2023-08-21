# LED

点亮LED灯

```python
import time
import machine

pin2 = machine.Pin(2, machine.Pin.OUT)

while True:
    pin2.value(1)
    time.sleep(1)
    pin2.value(0)
    time.sleep(1)
```

LED闪烁

```python
from machine import Pin
import time

led1 = Pin(15, Pin.OUT)
while True:
    led1.value(1)
    time.sleep(5)
    led1.value(0)
    time.sleep(0.5)
```

流水灯

```python
from machine import Pin
import time

led_pin = [15,2,0,4,16,17,5,18]
leds = []
for i in range(8):
    leds.append(Pin(led_pin[i],Pin.OUT))
    
if __name__ == "__main__":
    for n in range(8):
        leds[n].value(0)
    
    while True:
        for n in range(8):
            leds[n].value(1)
            time.sleep(0.05)
        for n in range(8):
            leds[n].value(0)
            time.sleep(0.05)
```

# 蜂鸣器

```python
from machine import Pin
import time

beep= Pin(25,Pin.OUT)

if __name__ == "__main__":
    i = 0
    while True:
        i = not i
        beep.value(i)
        time.sleep_us(250)
```

# 继电器



# PWM呼吸灯

```python
from machine import Pin
from machine import PWM
import time

led1 = PWM(Pin(15),freq = 1000, duty = 0)

if __name__ == "__main__":
    duty_value = 0
    fx = 1
    while True:
        if fx == 1:
            duty_value+=10
            if duty_value>1010:
                fx = 0
        else:
            duty_value-=10
            if duty_value<10:
                fx = 1
        led1.duty(duty_value)
        time.sleep_ms(10)
```



# RGB彩灯实验

```python
from machine import Pin
from neopixel import NeoPixel
import time

# 控制引脚为16，RGB灯串联5个
pin = 16
rgb_num=5
rgb_led = NeoPixel(Pin(pin,Pin.OUT),rgb_num)
#定义RGB颜色
RED = (255,0,0)
ORANGE = (255,165,0)
YELLOW=(255,150,0)
GREEN=(0,255,0)
BLUE=(0,0,255)
INDIGO=(75,0,130)
VIOLET=(138,43,226)
COLORS = (RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET)

if __name__ == "__main__":
    
    while True:
        for color in COLORS:
            for i in range(rgb_num):
                rgb_led[i]=(color[0], color[1], color[2])
                rgb_led.write()
                time.sleep_ms(100)
            time.sleep_ms(1000)

```



# 数码管

```python
from machine import Pin
import time
import tm1637

smg=tm1637.TM1637(clk=Pin(16),dio=Pin(17))

#smg.numbers(1,24)
if __name__=="__main__":
    n=0
    while True:
        smg.number(n)
        n+=1
        time.sleep(1)
```









# 链接WIFI

```python
import network
>>> wlan = network.WLAN(network.STA_IF)
>>> wlan.active(True)
True
>>> wlan.scan()
>>> wlan.isconnected()
False
>>> wlan.connect('502.2', '502502123')
>>> wlan.isconnected()
True
>>> wlan.ifconfig()
('192.168.3.49', '255.255.255.0', '192.168.3.1', '192.168.3.1')
# 测试

```

# 数码管

```python
from machine import Pin
import time
import tm1637

smg=tm1637.TM1637(clk=Pin(16),dio=Pin(17))

if __name__=="__main__":
    n=0
    while True:
        smg.number(n)
        n+=1
        time.sleep(1)

```



订阅者

```python
from machine import Pin
from machine import Timer
import json
import time
import network
from umqtt.simple import MQTTClient
import LED


ssid="Redmi K40"
password="123456789"



def wifi_connect():
    wlan=network.WLAN(network.STA_IF)
    wlan.active(True)
    start_time=time.time()
    
    if not wlan.isconnected():
        print("connecting to network...")
        wlan.connect(ssid,password)
        
        while not wlan.isconnected():
            if time.time()-start_time>15:
                print("timeout!")
                break;
            return False
    else:
        print("network information:", wlan.ifconfig())
        return True
    
def mqtt_callback(topic,msg):
    data = json.loads(str(msg.decode()))
    if 'msg' in data:
        if data['msg'] == True:
            on_off = LED.led_on()
            status = {
                'status': on_off    
            }
            client.publish(TOPIC, json.dumps(status))
        elif data['msg'] == False:
            on_off = LED.led_off()
            status = {
                'status': on_off    
            }
            client.publish(TOPIC, json.dumps(status))
    
    
def mqtt_rev(tim):
    client.check_msg()
    
    
if __name__=="__main__":
    if wifi_connect():
        server="kbws.xyz"
        port=1883
        CLIENT_ID="ESP32"
        TOPIC="django/mqtt"
        client = MQTTClient(CLIENT_ID, server, port)
        client.set_callback(mqtt_callback)
        
        client.connect()
        client.subscribe(TOPIC)
        
        tim=Timer(0)
        tim.init(period=1000, mode=Timer.PERIODIC, callback=mqtt_rev)



```

led.py

```python
import time
import machine

pin2 = machine.Pin(2, machine.Pin.OUT)

def led_on():
    pin2.value(1)
    return pin2.value()
def led_off():
    pin2.value(0)
    return pin2.value()


```





