#websocket

## 1. WebSocket 的背景
+ Polling
> 采用轮询的方式，浏览器隔个几秒就发送一次请求，询问服务器是否有新信息。
> ![Alt text](http://img.blog.csdn.net/20130517151509160)
>缺点：需要服务器有很快的处理速度和资源
+ Long Polling
>采用轮询的方式，客户端发起连接后，如果没消息，就一直不返回Response给客户端。直到有消息才返回，返回完之后，客户端再次建立连接，周而复始。
>![Alt text](http://img.blog.csdn.net/20130517151612871)
> 缺点: 需要有很高的并发处理能力
## 2. WebSocket 是什么
> WebSocket 是 HTML5 一种新的协议。它实现了浏览器与服务器全双工通信，能更好的节省服务器资源和带宽并达到实时通讯。
+ ### 基于TCP协议
>握手的时序图
>![Alt text](https://raw.githubusercontent.com/laubrence/static/master/websocket.gif)
+ ### 双向通信
>类似于Socket，服务器和客户端(Browser)都能主动的向对方发送或接收数据
>传统 HTTP 请求响应客户端服务器交互图 
>![Alt text](http://www.ibm.com/developerworks/cn/java/j-lo-WebSocket/img001.jpg "传统 HTTP 请求响应客户端服务器交互图")
>WebSocket 请求响应客户端服务器交互图 
>![Alt text](http://www.ibm.com/developerworks/cn/java/j-lo-WebSocket/img002.jpg "WebSocket 请求响应客户端服务器交互图")
## 3. WebSocket 机制
+ WebSocket 客户端连接报文
```text
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

+ WebSocket 服务端响应报文
```text
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```
+ WebSocket API
> 客户端API
```javascript
var ws = new WebSocket(“ws://echo.websocket.org”);
ws.onopen = function(){ws.send(“Test!”); };
ws.onmessage = function(evt){console.log(evt.data);ws.close();};
ws.onclose = function(evt){console.log(“WebSocketClosed!”);};
ws.onerror = function(evt){console.log(“WebSocketError!”);};
```
> 服务器API
```
@ServerEndpoint("/echo")
public class EchoEndpoint {
 @OnOpen
 public void onOpen(Session session) throws IOException {
 //以下代码省略...
 }
 @OnMessage
 public String onMessage(String message) {
 //以下代码省略...
 }
 @Message(maxMessageSize=6)
 public void receiveMessage(String s) {
 //以下代码省略...
 } 
 @OnError
 public void onError(Throwable t) {
 //以下代码省略...
 }
 @OnClose
 public void onClose(Session session, CloseReason reason) {
 //以下代码省略...
 } 
 }
```
> Tomcat从7.0.27开始支持 WebSocket，从7.0.47开始支持JSR-356
## 4. WebSocket 应用场景
> 决定手头的工作是否需要使用WebSocket技术的方法很简单：
+ 你的应用提供多个用户相互交流吗？
+ 你的应用是展示服务器端经常变动的数据吗？
### 应用场景
1. WebIM
> 新浪微博 [私信聊天](http://weibo.com/)
2. 在线聊天室 
> [聊天室demo](http://chat.workerman.net/)
3. 多玩家游戏
> [小蝌蚪游戏](http://kedou.workerman.net/)
> [BrowserQuest](http://browserquest.mozilla.org/)
4. 视频直播弹幕
> [摄像头录制页面](http://www.workerman.net/demos/live-camera/camera.html)
>
> [实时接收视频流页面](http://www.workerman.net/demos/live-camera/)
5. WEB消息推送
> [推送demo](http://www.workerman.net:2123/)
6. 基于位置的应用
7. 社交订阅
1. 协同编辑
1. ...
