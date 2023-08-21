# Frame窗口
```java
public class TestFrame {
    public static void main(String[] args) {
        //Frame,JDK,看源码
        Frame frame = new Frame("我的第一个Java图形界面窗口");
        //需要设置可见性
        frame.setVisible(true);
        //设置窗口大小
        frame.setSize(400,400);
        //设置背景颜色
        frame.setBackground(new Color(168, 168, 173));
        //弹出初始位置
        frame.setLocation(200,200);
        //设置大小固定
        frame.setResizable(false);
    }
```
![image](https://gitee.com/Enteral/images/raw/master/https://gitee.com/enteral/images/1652178816377.png)

