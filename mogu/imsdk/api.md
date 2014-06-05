# IM JavaScript API

`param`: 参数对象
`callback`: 回调函数，回调参数为接口返回值

## 本地接口

查询完通过callback参数返回

### _getConfig

> 获取配置

参数: null
返回: 配置obj对象

### _getuserinfo

> 获取登录用户信息

参数: null
返回: 用户信息

### _getuserlist

> 获取用户列表

参数: null
返回: 用户列表

### _popMessageQueue

> 获取消息队列，并且清空当前队列

参数:

* `uid` - 字符串，需要查询的uid

返回: 查询的消息队列

### _addNewBusinessUser

> 获取店铺的客服id

参数:

* `bid` - 字符串，店铺id#（售前｜售后）

返回: 被分配客服的uid

### _addNewUser

> 获取用户信息，本地有的查询本地，没有的就查询远程接口，查询完通过消息事件通知

参数:

* `uids` - 字符串，逗号分割的uid

返回: 无

### _getHistoryMessage

> 获取历史消息，查询完通过消息事件通知

参数:

* `offset` - 整数，跳步
* `count` - 整数，查询条数
* `uid` - 字符串，自己的uid
* `otheruid` - 字符串，对方的uid

返回: 无

### _getGoodsInfo

> 获取浏览商品信息

参数:

* `uid` - 字符串，客服用户id

返回: 浏览的商品信息

### _setGoodsinfo

> 存储浏览的商品队列

参数:

* `bid` - 字符串，店铺id
* `goodsId` - 字符串，商品id

返回: 无

### _isUserBlock

> 获取黑名单 (sdk 1.1 up)

参数:

* `uid` - 字符串，待查询uid

返回: 布尔值，是否在黑名单

### _svrScore

> 给客服评分 (sdk 1.1 up)

参数:

* `id` - 字符串，评分id
* `score` - 字符串，评分

返回: 无

## socket接口

查询完通过消息事件通知返回

### addNewUser

> 新建用户连接

参数:

* `uids` - 字符串，逗号分割的uid
* `isSelectStatus` - 布尔值，可选，是否需要查询及时状态

返回: 无

### addNewBusinessUser

> 新建商家用户连接

参数:

* `bid` - 字符串，店铺id#（售前｜售后）

返回: 无

### sendMessage

> 发送消息

参数:

* `fromUid` - 字符串，发送用户id
* `toUid` - 字符串，接收用户id
* `content` - 字符串，消息内容

返回: 无

### sendData

> 发送数据包

参数:

* `fromId` - 字符串，发起请求的id
* `toId` - 字符串，接受请求的id
* `content` - 对象，{action: 消息类型, data: 数据}

返回: 无

### setOnlineStatus

> 设置用户状态

参数:

* `value` - 整数，1、在线 2、离线 3、离开

返回: 无

### get2HistoryMsg

> 查询历史记录

参数:

* `fromId` - 字符串，用户id
* `offset` - 整数，跳步
* `count` - 整数，查询条数

返回: 无

### getAllHistoryMsg

> 查询和所有客服的历史记录

参数:

* `shopId` - 字符串，店铺id
* `userId` - 字符串，用户id
* `displayId` - 字符串，显示id，一般为对方id
* `offset` - 整数，跳步
* `count` - 整数，查询条数

返回: 无

### get2UnreadMsg

> 查询离线消息

参数:

* `fromId` - 字符串，待查询用户id

返回: 无

### msgReadConfirm

> 标记已读

参数:

* `fromId` - 字符串，待确认用户id

返回: 无

### setToken

> token设置

参数:

* `token` - 字符串，token

返回: 无

### getUserStatus

> 获取用户在线状态

参数:

* `userid` - 字符串，待查询用户id

返回: 无

### initRecentlyList

> 初始化最近联系人列表

参数: 无

返回: 无

### initServiceList

> 初始化客服列表

参数: 无

返回: 无

### getMyUnreadMsgCount

> 获取未读消息

参数: 无

返回: 无

## 消息事件

接受sdk推送的自定义消息

### userinfo:update

> 用户信息更新

### userlist:update

> 用户列表更新

### userpanel:update

> 用户面板更新

### message:new

> 即时消息

### message:unread

> 未读消息

### message:history

> 历史消息

### assignsvrs:finish

> 分配客服完毕

### switchsvrs:finish

> 被转客服

### loading:status

> 加载状态 - reconnect 重连，finish 加载完毕，timeout 超时

### notice:show

> 提示信息

### history:empty

> 历史消息为空

## License

Copyright (c) 2014-03-27 MOGU F2E  
Licensed under the MIT license.
