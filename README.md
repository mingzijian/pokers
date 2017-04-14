# pokers for robot or AI research

You can edit this client for game robot AI research,
the websoket api in texasJS/message.js.

Texas Holdem Poker and three card poker games client. 

A poker client useing html,jquery,websocket,canvas. 

Only contains login,regist,three basic play rooms,user's chips rank. 

You can see this project at this website below. 

http://120.26.217.116:8080/texas/texasIndex.html. 

德州扑克的客户端代码， 

可以根据texasJS/message.js文件中的websocket方式访问，编写自己的机器人用于人工智能研究

包含了德州扑克和欢乐拼三张2个游戏的客户端部分。 

包含登陆注册功能，筹码排行榜，德州扑克仅有3种下注额度的房间。 

参考效果地址http://120.26.217.116:8080/texas/texasIndex.html 

message.js below:

var websocket = null;
var wsip = "120.26.217.116:8080/texas/ws/texas";
// 发送消息映射
var mapping = {
    // 注册
    regist: 0,
    // 登陆
    login: 1,
    // 进入房间
    enterRoom: 2,
    // 退出房间
    exitRoom: 3,
    // 坐下
    sitDown: 4,
    // 站起
    standUp: 5,
    // 过牌
    check: 6,
    // 下注
    betChips: 7,
    // 弃牌
    fold: 8,
    //获取排行榜
    getRankList: 9
};
function wsInit() {
    var url = "ws://" + wsip;
    if ('WebSocket' in window) {
        websocket = new WebSocket(url);
    } else if ('MozWebSocket' in window) {
        websocket = new MozWebSocket(url);
    }
    bindWsFunction(websocket);
}
function wsReOpen() {
    // if (websocket.readState != 1) {
    // websocket = new WebSocket("ws:/" + wsip );
    // }
    // bindWsFunction(websocket);
}
function bindWsFunction(ws) {
    ws.onerror = function (event) {
        onError(event);
    };
    ws.onopen = function (event) {
        onOpen(event);
    };
    ws.onmessage = function (event) {
        onMessage(event);
    };
    ws.onclose = function (event) {
        onClose(event);
    };
}
/**
 * 接收服务器消息
 */
function onMessage(event) {
    if (event.data != null) {
        var dataJson = JSON.parse(event.data);
        var func = eval(dataJson.c);
        new func(null, dataJson);
        console.log(dataJson.c + " is call by server!");
    }
}
/**
 * 建立连接
 */
function onOpen(event) {
    document.getElementById('messages').innerHTML = 'Connection established';
}
function onError(event) {
    console.log(event.data);
    window.location.reload();
    alert(event.data);
}
function onClose(event) {
    document.getElementById('messages').innerHTML = 'Connection onCloseed';
    window.location.reload();
    wsReOpen();
}

// 发送消息
function sendMessage() {
    var data = {};
    data.c = mapping.sendMessage;
    websocket.send(JSON.stringify(data));
}
// 错误消息返回
function onException(e, data) {
    console.log(data.message);
    alert(data.message);
}
