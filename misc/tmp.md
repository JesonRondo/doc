前端MV\*设计模式作为SPA(single page application)比较成熟的解决方案，在很多应用场景下被广泛运用。数据模型驱动的优势不言而喻，我们可以清晰的划分服务端数据与交互表现，更多关注前端模块层级之间的逻辑处理，拥有更好的可维护性和扩展性。然而模块化的层级结构让MV\*和通常意义上的前端页面相比显得非常臃肿，一个MV\*的组织架构可以将项目提升到App级别，所以MV\*的选用使应用更加系统化的同时也失去了一些灵活性。

Web-IM有着比较明显映射的数据模型，前有响应用户的富应用交互模式，后与服务端数据通讯有着多种链接模式，模块结构划分比较明显，MV\*的使用可以更好的处理这三者之间的关系。

![mvc](http://s13.mogujie.cn/pic/140607/ubzlo_kq2fg3kbkrbf6ssugfjeg5sckzsew_560x346.jpg)

数据模块，界面模块这两个必不可少，考虑到与服务端的通讯涉及到flash socket、ajax poll多套链接（将来还可能扩展更多链接方式，如websocket），所以把服务端细化到应用接口层，包括socket的调用接口、回调接口和控制接口操作的socket交互模块，由socket交互模块来实现不同的连接方式，不同连接方式公用调用接口和回调接口，走相同的接口调用，然后加上一个相当于控制器的模块把前面所有模块联系起来，先称其为通知模块，负责模块间的连接，最后再加上前端神级模块util，我们就可以组装起一个基于MV\*思想的Web-IM。

```javascript

    /**
     * MOGU.IM.Data
     * @namespace
     * @desc IM数据Model － IM数据存储对象
     */
    MOGU.IM.Data = {
    };

    /**
     * MOGU.IM.Interface
     * @namespace
     * @desc 界面模块 - 界面构成 & 控制界面现实相关
     */
    MOGU.IM.Interface = (function() {
    }());

    /**
     * MOGU.IM.SocketInteface
     * @namespace
     * @desc Socket接口映射 - 向服务端的请求接口
     */
    MOGU.IM.SocketInteface = {
    };

    /**
     * MOGU.IM.SocketCallback
     * @namespace
     * @desc Socket回调映射 - 接受服务端消息推送的接口
     */
    MOGU.IM.SocketCallback = {
    };

    /**
     * MOGU.IM.SocketNotification
     * @namespace
     * @desc Socket交互模块
     */
    MOGU.IM.SocketNotification = (function() {
    }()};

    /**
     * MOGU.IM.Notification
     * @namespace
     * @desc 消息通知模块 - 控制IM的Model数据变化，发送消息给界面元素显示
     */
    MOGU.IM.Notification = (function() {
    }()};

    /**
     * MOGU.IM.Util
     * @namespace
     * @desc IM工具模块 － 工具函数集合
     */
    MOGU.IM.Util = {
    };

```

看到这里，好像已经实现了逻辑分离，模块各司其职，组成了一个拥有整体功能的Web-IM，jsdoc把文档生成下，IM就可以开始投入使用了。在满足了当前需求的情况下，这样的设计就是我们蘑菇街的初代Web-IM，没错，只是初代...

软件工程中永远不变的是需求的变化，下面我们来接受需求的洗礼吧，不，我们换个方向，来一起向前端开心的提需求。

“我们服务端这边加了登录验证，你那边改一下”
“这里几个店铺是系统账号，不让用户回复，只接受消息，就把发送的输入框界面隐藏下，不需要UI的”
“我们手机这边要做个wapIM，和web的差不多，在手机客户端里面用的，功能只有发送接受消息就可以了，应该很快吧”
“怎么转客服这边没提示的，要帮用户把原来的客服关掉，然后告诉用户接下来由新的客服来服务”
…

嗯，很熟悉的场景，回来吧，开始做需求了，不急，一个一个来。

登录验证，之前设计的SocketNotification用来管理所有链接方式的，在这加一下就好了；系统账号隐藏输入框，好吧，在Interface里面加判断，如果是系统账号就隐藏掉...怎么代码看起来又变丑了；wapIM，还好是模块划分的，把Interface层重新写个wap的好了，诶，好像不对，加过登录验证，客户端的验证方式和web的验证方式不同，不是吧，所有的接口都要带上client参数，在这里写if代码没法看了，哎，拷一份出来好了；转客服，还好有数据模型，把Data里面的userlist操作下好了，之前的客服删掉，新的客服加进来，完了，提示怎么办…又是一堆煞风景的代码。

为了提供可维护的jsdoc文档，之前把模块都放到一个文件，然后经过一轮一轮的轰炸，我们的代码达到了

![code line](http://s13.mogujie.cn/pic/140607/ubzlo_kq2gk3kbkrbdivtwgfjeg5sckzsew_404x185.jpg)

没错，4702行，呵呵。

“这里加个东西吧”
“不要改了，改不动了= =。。。”

程序的臃肿越来越明显，设计之初的想法不是这样的，最后达到了无法维护的地步，重构任务急不可待。

先总结下之前设计的不足，后面需要考虑和改进的地方有哪些。首先模块化最初的想法是好的，但是改来改去，发现模块里面的额外逻辑越来越多，有时一个功能修改可能牵扯到几个模块，模块功能是独立了，但是低耦合却没做到；多版本共存时，很多代码达到了数学几何中的相似不相同；为了提供代码目录文档，所有模块放一个文件中，导致文件过大，无法维护，就算是按兴趣命名也得分文件；和服务端的交互除了socket链接外，还有一般的服务端接口（如商品数据查询etc.），socket模块虽然独立了，但是服务端接口分散到各个需要使用的模块中，没法统一管理；服务端和界面所使用的数据模型是同一套，服务端修改数据模型虽然有分操作类别，但是与数据模型直接联系的界面在模型修改后立马做出反应，并不区分修改是来自哪种情况（如转客服提示不能跟着userlist的更改一起通知过来）。总结新设计的前端架构需要解决以下问题：

* 项目分文件
* 模块解耦合
* 增加代码复用
* 服务层独立
* 不同版本只针对interface层

新的模块大概分成两大块。服务层，解决所有和服务端交互的数据处理，自己维护一套数据模型，可以推送数据更改、消息变更。界面层，多版本开发，接受服务层推送数据，实现界面展示和交互功能。基于以上划分，SDK的解决方案应运而生。

新的目录层级如下：

    im
     |— web // 界面层mvc
     |     |— scripts
     |     |     |— collection +
     |     |     |— helper +
     |     |     |— model +
     |     |     `— view +
     |     |— style +
     |     `— tpl +
     |— wap +
     `— sdk // sdk即服务层
           |— im-1.1.js // sdk接口
           `— 1.1
                 |— connect.js // 链接模块，多套链接方式的实现
                 |— core.js // sdk接口中转
                 |— data.js // 数据模块，包括im运行时的用户数据和sdk配置数据
                 |— event.js // sdk自定义消息推送模块
                 |— inter-local.js // sdk本地接口
                 |— inter-socket.js // sdk socket接口
                 |— inter-svrs.js // sdk服务端接口
                 |— socket.js // socket请求、调用模块
                 `— util.js // 工具函数

界面层依旧采用MV\*模式，用Backbone来组织结构，不同的是界面层的数据模型完全是展示所用，更新数据从SDK获取。SDK是和服务端的通信桥梁，维护服务端推送的用户信息和消息队列，更新主动查询的数据，推送相应的数据消息。

参考weibo的JavaScript SDK设计，SDK应该有SDK参数初始化、数据请求这两个接口。考虑IM的特殊性，对应的登录验证其实可以抽象成建立链接，数据、消息推送可以抽象成自定义事件监听。所以SDK提供的接口如下

```javascript

    /**
     * @desc 初始化
     * @param {object} SDK参数
     */
    init: function(option) {
    }

    /**
     * @desc 建立连接
     * @param {function} 回调函数
     */
    connect: function(callback) {
    }

    /**
     * @desc 数据请求
     * @param {string} 接口名
     * @param {object} 参数
     * @param {function} 回调函数
     */
    parseCmd: function(cmd, param, callback) {
    }

    /**
     * @desc 自定义事件监听
     * @param {string} 自定义事件名
     * @param {function} 回调函数
     */
    listen: function(ename, callback) {
    }

```

由于业务层基本集中于上层界面，只要数据结构不动，相应版本只需建立好链接，然后就可以处理自己的业务相关逻辑，不用再关心如何与服务端通讯，相反的，只要是与服务端做的数据修改，只需在sdk层实现就可以实现多版本同时更新，不用担心代码冗余带来的巨大维护成本。SDK接口调用方式如图

![sdk call](http://s13.mogujie.cn/pic/140607/ubzlo_kq2eiy2mkrbhgvsugfjeg5sckzsew_516x508.jpg)

这里包括了除SDK listen接口之外的所有接口，listen接口的使用在上图的notice模块中。上面说过数据和消息推送是IM与其他web应用相比特殊性的存在，listen接口正是为了解决推送问题设计的，考虑数据的单向传递和低耦合，jQuery里面的自定义事件正好满足了这两个需求，对自定义事件做一次简单的封装

```javascript

    /**
     * @desc 绑定事件
     */
    on: function(ename, callback) {
      $(document).on(pre + ename, function(e, data) {
        callback && callback(data);
      });
    },

    /**
     * @desc 触发事件
     */
    trigger: function(ename, data) {
      if (events[ename]) {
        $(document).trigger(pre + ename, [data]);
      }
    }

```

在SDK内部只需要trigger操作就能给外部（界面层）推送消息和数据，在外部实现需要监听的自定义消息即可，在现在的im中，就是notice模块的实现

```javascript

    // 用户信息更新
    im.listen('userinfo:update', function(userinfo) {
    });

    // 用户列表更新
    im.listen('userlist:update', function(usersinfo) {
    });

    // 即时消息
    im.listen('message:new', function(msginfo) {
    });

    ...

```

至此，SDK的解决方案弥补了之前简单模块化的不足，有利于多平台版本的维护，可以支持服务端协议版本的单独升级，同时对外开放SDK API也成为可能。

能满足当时需求的解决方案就是好的解决方案，随着需求的变化，一层不变必定会带来巨大的维护成本，Web-IM的解决方案也将不断完善下去。

原文地址:
[Web-IM前端解决方案](http://vicbeta.com/code/2014/06/07/webim.html)

相关文档：
[IM JavaScript API](https://github.com/JesonRondo/doc/blob/master/mogu/im/sdk/api.md)
[IM JavaScript SDK](https://github.com/JesonRondo/doc/blob/master/mogu/im/sdk/sdk.md)
