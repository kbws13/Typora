
# 1、连接需要控制的LED灯

![](img/Pasted%20image%2020230808184053.png)

同学们也可以根据自己的想法连接别的灯

# 2、整体思路

![](img/Pasted%20image%2020230808184138.png)

电脑向路由器发送数据，ESP32接收数据，并做出相应的响应

# 3、ESP32端代码

```python
# 整体流程
# 1. 链接wifi
# 2. 启动网络功能（UDP）
# 3. 接收网络数据
# 4. 处理接收的数据


import socket
import time
import network
import machine


ssid="test"
password="test"
def do_connect():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print('connecting to network...')
        wlan.connect(ssid, password)
        i = 1
        while not wlan.isconnected():
            print("正在链接...{}".format(i))
            i += 1
            time.sleep(1)
    print('network config:', wlan.ifconfig())


def start_udp():
    # 2. 启动网络功能（UDP）

    # 2.1. 创建udp套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 2.2. 绑定本地信息
    udp_socket.bind(("0.0.0.0", 7788))

    return udp_socket


def main():
    # 1. 链接wifi
    do_connect()
    # 2. 创建UDP
    udp_socket = start_udp()
    # 3. 创建灯对象
    led = machine.Pin(2, machine.Pin.OUT)
    # 4. 接收网络数据
    while True:
        recv_data, sender_info = udp_socket.recvfrom(1024)
        print("{}发送{}".format(sender_info, recv_data))
        recv_data_str = recv_data.decode("utf-8")
        try:
            print(recv_data_str)
        except Exception as ret:
            print("error:", ret)
        
        # 5. 处理接收的数据
        if recv_data_str == "light on":
            print("这里是要灯亮的代码...")
            led.value(1)
        elif recv_data_str == "light off":
            print("这里是要灯灭的代码...")
            led.value(0)


if __name__ == "__main__":
    main()
```

> 注意：ESP32和电脑要处在同一个WiFi网络下

ESP32运行程序后，会在控制台打印出如下信息：

![](img/Pasted%20image%2020230808184511.png)

箭头所指的是ESP32的IP，后面电脑端的代码会用到

输出上面这些信息，就代表ESP32连接WiFi成功了

# 4、电脑端代码

```python
import socket


esp32_ip = "192.168.220.34"  # 这是你的ESP32的IP地址，要根据上面输出的进行修改
esp32_port = 7788  # 这是你的ESP32监听的端口号
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

while True:
    message = input("请输入要发送的数据: ")
    if message.lower() == "exit":
        break
    client_socket.sendto(message.encode("utf-8"), (esp32_ip, esp32_port))
    
client_socket.close()
```

运行上面的代码

当电脑端发送`light on`时，LED灯会点亮；当电脑端发送`light off`时，LED灯会熄灭。