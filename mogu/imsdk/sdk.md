# IM JavaScript SDK

### sdk.init(option)

> 初始化使用SDK所需的配置

参数:

* `option` - 对象，配置参数，属性可以为一项或多项

```javascript
option = {
  uid: null, // 必选，登录用户uid
  dao: null, // 必选，登录用户服务端dao
  token: null, // 必选，登录用户服务端token
  faceData: 'http://s6.mogujie.cn/__/js/module/module-data-imface.js$2014041601.js', // 可选，表情数据
  faceTabData: 'http://s6.mogujie.cn/__/js/module/module-data-face.js$2014041601.js', // 可选，表情tab数据
  siteHost: 'http://www.mogujie.com', // 可选，服务端host
  serviceHost: 'xxx.xxx.xxx.xxx:xxx.xxx.xxx.xxx:80', // 可选，flash连接服务器
  ajaxServiceHost: 'http://xxx.xxx.xxx.xxx:80', // 可选，长连接服务器host
  ajaxServiceBakHost: 'http://xxx.xxx.xxx.xxx:80', // 可选，长连接备用服务器host
  connectMethod: [ // 可选，所用连接方式
     'serviceHost'
    ,'ajaxServiceHost'
    ,'ajaxServiceBakHost'
  ],
  debug: false, // 可选，调试模式
  imclient: false, // 可选，客户端选用的验证方式，默认非客户端
  version: '1.1' // sdk，版本
};
```

返回: 无

### sdk.connect(callback)

> 建立socket连接

参数:

* `callback` - 函数，可选，连接成功后的回调函数

返回: 无

### sdk.listen(ename, callback)

> 事件通知监听

参数:

* `ename` - 字符串，监听事件名
* `callback` - 函数，事件通知回调函数

返回: 无

### sdk.parseCmd(cmd, param, callback)

> 调用api接口

参数:

* `cmd` - 字符串，api接口名
* `param` - 对象，api所需参数
* `callback` - 函数，api回调函数

返回: 无

### Upgrade

#### v 1.1

* 1. 新增表情tab数据
* 2. 新增flash连接方式，支持设置连接方式
* 3. `faceData` 数据上cdn
* 4. `faceTabData` 数据抽离
* 5. 黑名单支持、黑名单缓存
* 6. 自己发送的消息通过服务端同步其他socket连接（本连接发送的消息还是需要自行处理显示）
* 7. 本地接口新增 `_isUserBlock`, `_svrScore`
* 8. 分配客服默认值更改为23售前
* 9. 未读消息clean bug fixed
* 10. 离线客服不发送欢迎语
* 11. msgtype新增评分消息
* 12. 新增imclient配置

#### v 1.0

* sdk模式实现

## License

Copyright (c) 2014-03-27 MOGU F2E  
Licensed under the MIT license.
