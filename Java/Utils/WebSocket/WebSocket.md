# WebSocket

SpringBoot项目整合WebSocket功能

## 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

## 配置类

```java
package xyz.kbws.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

/**
 * @author hsy
 * @date 2023/7/4
 */
@Configuration
public class WebSocketConfig {
    @Bean
    public ServerEndpointExporter serverEndpointExporter(){
        return new ServerEndpointExporter();
    }
}
```

## WebSocket功能

```java
package xyz.kbws.wevSocket;

import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;


import javax.websocket.*;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.concurrent.ConcurrentHashMap;

/**
 * @author hsy
 * @date 2023/7/4
 */
@ServerEndpoint("/websocket/{userId}")
@Component
@Slf4j
public class WebSocketServer {
    //静态变量，用来记录当前在线连接数，应该设计成线程安全的
    private static int onlineCount=0;
    //concurrent包的线程安全集合
    private static ConcurrentHashMap<String, WebSocketServer> webSocketMap = new ConcurrentHashMap<>();
    //与某个客户端的连接会话，需要通过他来给客户端发送数据
    private Session session;
    //接收userId
    private String userId="";

    //连接成功调用的方法
    @OnOpen
    public void onOpen(Session session, @PathParam("userId") String userId){
        this.session=session;
        this.userId=userId;

        if (!webSocketMap.containsKey(userId)){
            //加入集合中
            webSocketMap.put(userId, this);
            //在线数加1
            addOnlineCount();
        }
        log.info("用户连接:"+userId+",当前在线人数为:"+getOnlineCount());
        try {
            sendMessage("连接成功");
        }catch (IOException e){
            log.error("用户:"+userId+",网络异常!!!");
        }

    }

    //连接关闭调用的方法
    @OnClose
    public void onClose(){
        if (webSocketMap.containsKey(userId)){
            webSocketMap.remove(userId);
            //从集合中删除
            subOnlineCount();
        }
        log.info("用户退出:"+userId+",当前在线人数为:"+getOnlineCount());
    }
    //服务器主动推送
    public void sendMessage(String message)throws IOException {
        this.session.getBasicRemote().sendText(message);
    }

    //收到客户端消息后调用的方法
    @OnMessage
    public void onMessage(String message, Session session){
        log.info("[websocket消息]收到客户端消息{}",message);
        String[] split = message.split("/");
        try {
            sendInfo(split[0], split[1]);
        }catch (IOException e){
            e.printStackTrace();
        }
    }

    @OnError
    public void onError(Session session, Throwable error){
        log.error("用户错误:"+this.userId+",原因:"+error.getMessage());
    }

    //发送自定义消息
    public static void sendInfo(String message, @PathParam("userId") String userId)throws IOException{
        log.info("发送消息到:"+userId+"，报文:"+message);
        if (message!=null&&webSocketMap.containsKey(userId)){
            webSocketMap.get(userId).sendMessage(message);
        }else {
            log.error("用户"+userId+"不在线");
        }
    }

    public static synchronized int getOnlineCount(){
        return onlineCount;
    }
    public static synchronized void addOnlineCount(){
        WebSocketServer.onlineCount++;
    }
    public static synchronized void subOnlineCount(){
        WebSocketServer.onlineCount--;
    }
}
```

## 接口测试

```java
package xyz.kbws.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import xyz.kbws.wevSocket.WebSocketServer;

import java.io.IOException;

/**
 * @author hsy
 * @date 2023/7/4
 */
@RestController
public class DemoController {
    @RequestMapping("/push/{toUserId}")
    public ResponseEntity<String> pushToWeb(String message, @PathVariable String toUserId)throws IOException{
        WebSocketServer.sendInfo(message,toUserId);
        return ResponseEntity.ok("MSG SEND SUCCESS");
    }
}
```

> 注：测试WebSocket功能时，链接应为ws://localhost:8080/websocket/1