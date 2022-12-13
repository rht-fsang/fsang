---
title: 计分小程序介绍
date: 2022-12-13 10:36:15
tags: summary
tags:前端
typora-copy-images-to: upload
---

## 计分小程序介绍：计分小程序是一个打牌计分神器，麻将扑克均适用，主要分为单人计分模式和多人计分模式。

#### 技术栈：[TS](https://www.tslang.cn/docs/handbook/basic-types.html)、[微信小程序](https://developers.weixin.qq.com/miniprogram/dev/framework/)、[Vant Wapp](https://youzan.github.io/vant-weapp/#/goods-action)、[websocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

<!--more-->

### 遇到的问题

***

**问题：房间计分情况实时更新到每一位成员 **

解决思路：选择长连接，长连接有websocket，Long Pulling， 轮询。鉴于计分操作比较频繁且需要后台主动发送信息，所以我使用的是websocket来实现长连接。小程序官方有封装好的websocket，

使用websocket涉及到关闭重连和收到信息去做通知两种情况。为了更好的复用，我把websocket连接的方法initWebSocket挂在全局的app的实例上，当收到服务端发来的信息时，该实例方法会生成一个事件进行广播，我使用的是eventemitter2生成和监听事件。再全局我还写了一个实例listenEvent去监听事件，当监听到事件的时候会把传进来的方法执行一遍，传进来的方法就可以去拿数据。因为监听的事件在页面销毁后是需要同时去进行销毁的。但是在页面销毁的生命周期函数中获取不到最新的listener,所以我在使用listenEvent的时候也把当前的页面实例也传进来，然后在实例内部去改写页面的onUnload的页面生命周期函数，在页面销毁的同时去销毁当前监听的listener。执行完之后再还原页面的生命周期函数。这样就完成了监听和销毁的闭环。

关于websocket断线重连，我在全局写了一个方法去争对主动关闭和被动关闭的情况分别进行处理，该方法会根据入参判断是否是主动关闭，被动关闭的话会先销毁上一个调用的定时器，然后再生成一个新的定时器去执行连接websocket的方法initWebSocket.如果是主动关闭的话就会只销毁而不再去生成定时器去连接websocket了。下面是具体的方法

```typescript
//连接websocket的方法，在页面加载之前执行这个方法连接websocket
initWebSocket() {
        this.wsDisabledReconnect = false;
        if (this.ws) {
            return this.ws;
        }
        this.ws = new Promise((reslove, reject) => {
            const ws = wx.connectSocket({
                url: 'wss://scorex.dayangame.com:443/ws/?token=' + this.appData.token,
            });
            ws.onOpen(res => {
                console.log('websocket连接', res);
                this.timeFdScoketPing = setInterval(() => {
                    ws?.send({ data: JSON.stringify({ event: 'ping', data: {} }) });
                }, 3000);
                reslove(ws);
                //连接成功后刷新房间信息
                this.event.emit('room.refresh', res);
            });
            ws.onError(res => {
                console.log('未能正常连接', res);
                this.onCleanSocket(this.wsDisabledReconnect);
                reject(new Error('socket error'));
            });
            ws.onMessage(res => {
                console.log('收到服务器内容：' + res.data);
                this.event.emit('room.refresh', res);
                console.log('emit room.refresh');
            });
            ws.onClose(res => {
                console.log('WebSocket 已关闭！', res);
                this.onCleanSocket(this.wsDisabledReconnect);
                reject(new Error('socket close'));
            });
        });
        return this.ws;
    },
    
    // 判断是主动关闭wbsocket连接还是被动，主动销毁定时器，被动销毁定时器后再生成一个定时器来实现被动关闭重连
    onCleanSocket(wsDisabledReconnect: boolean) {
        if (this.ws) {
            const ws = this.ws as Promise<WechatMiniprogram.SocketTask>;
            this.ws = null;
            ws.then(e => e.close({}));
        }
        clearInterval(this.timeFdScoketPing as number);
        clearTimeout(this.timeFdSocketReconnect as number);
        this.wsDisabledReconnect = wsDisabledReconnect;
        if (!this.wsDisabledReconnect) {
            this.timeFdSocketReconnect = setTimeout(() => this.initWebSocket(), 1000);
        }
    },
    
    //去监听一个事件并且在页面销毁的时候销毁监听的方法
    listenEvent(event: string, page: WechatMiniprogram.Page.Instance<any, any>, callback: ListenerFn): void {
        const listener = this.event.on(event, callback, { objectify: true }) as Listener;
        const superOnUnload = page.onUnload || function () {};
        console.log('add listener ->' + event);
        page.onUnload = () => {
            listener.off();
            console.log('remove listener ->' + event);
            superOnUnload.apply(page);
        };
    },    
```

### 实现算法

***



算法解决的问题：在计分小程序单人模式给分中最后以为玩家根据合分为零的规则自动填充好分数

![image-20221213110516315](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213110516315.png)



如图1.1是记录玩家分数的数据结构，我通过遍历player数据去，通过判断score是否为空来记录目前有多少个玩家还没有填分数同时计算出来目前已填分数玩家的总分。然后在遍历结束后针对只有一个玩家未填分数进行处理，把已填玩家的总分的相反数给最后一位玩家。

```typescript
 autofill() {
        let num = 0;
        let index = 0;
        let sum = 0;
        const copyUsers = this.data.player;
        for (let i = 0; i < copyUsers.length; i++) {
            if (!copyUsers[i].score) {
                num++;
                index = i;
            }
            if (copyUsers[i].win === 1) {
                sum += Number(copyUsers[i].score);
            } else {
                sum -= Number(copyUsers[i].score);
            }
        }
        if (num === 1) {
            if (sum >= 0) {
                copyUsers[index].win = 2;
            } else {
                copyUsers[index].win = 1;
            }
            copyUsers[index].score = Math.abs(sum).toString();
            this.setData({
                player: copyUsers,
            });
        }
    },
```

#### 接口封装

***



```typescript
  //使用query-string三方包
     import { stringifyUrl, StringifiableRecord } from 'query-string';
     
     //配置三个开发环境对应的路由
     const API_URLS: { [index: string]: string } = {
         develop: 'https://scorex.dayangame.com/api',
         trial: 'https://scorex.dayangame.com/api',
         release: 'https://scorex.dayangame.com/api',
     };
     
     //在小程序加载完毕时获取下程序的配置
    onLaunch() {
        const accountInfo = wx.getAccountInfoSync();
        this.config.appid = accountInfo.miniProgram.appId;
        this.config.env = accountInfo.miniProgram.envVersion;
        this.config.version = accountInfo.miniProgram.version || '0.0.0';
        this.systemInfo = wx.getSystemInfoSync();
        this.config.url = API_URLS[this.config.env] || API_URLS.develop;
        //获取JWT
        try {
            this.appData.token = wx.getStorageSync('jwt') || '';
        } catch (e) {
            console.error('getStorageSync[jwt] error', e);
        }
    },
    
    //封装接口
     doApiRequest<O, T extends WechatMiniprogram.IAnyObject>(
        method: WxRequestMethods,
        path: string,
        data?: T,
        query?: StringifiableRecord,
    ): Promise<O> {
        //拼接好路由
        const baseUrl = `${this.config.url}${path}`;
        //利用stringifyUrl把路由上的参数拼接在后面
        //queryString.stringifyUrl({url: 'https://foo.bar', query: {foo: 'bar'}});  
        //=> 'https://foo.bar?foo=bar'
        const url = stringifyUrl({ url: baseUrl, query }, { arrayFormat: 'index' });
        const header: WechatMiniprogram.IAnyObject = {
            'x-platform': ['wxmp', this.config.appid, this.config.env, this.config.version].join('/'),
            // 'x-debug-user': '7b2ee6da-1de7-406c-bf64-5e4afbbc31f1',
        };
        if (this.appData.token) {
            //在每次请求接口的时候都会带上token值，如果把token直接放在cookie里面会有跨域的问题，所以这里我把token放在http协议头上，避免跨域的问题
            header.Authorization = `Bearer ${this.appData.token}`;
        }
        return new Promise((reslove, reject) => {
            wx.request({
                url,
                header,
                method,
                data,
                dataType: 'json',
                success: result => {
                    if (result.statusCode != 200 && result.statusCode != 201) {
                        return reject(new Error(`request err: ${result.statusCode}`));
                    }
                    const data: { code: number; message: string; resource: O } = result.data as any;
                    if (data.code) {
                        return reject(new Error(data.message));
                    } else {
                        reslove(data.resource);
                    }
                },
                fail: e => {
                    reject(new Error(e.errMsg));
                },
            });
        });
    },
    doApiPostRequest<O, T extends WechatMiniprogram.IAnyObject>(
        path: string,
        data?: T,
        query?: StringifiableRecord,
    ): Promise<O> {
        return this.doApiRequest('POST', path, data, query);
    },
    doApiPutRequest<O, T extends WechatMiniprogram.IAnyObject>(
        path: string,
        data?: T,
        query?: StringifiableRecord,
    ): Promise<O> {
        return this.doApiRequest('PUT', path, data, query);
    },
    doApiDeleteRequest<O, T extends WechatMiniprogram.IAnyObject>(
        path: string,
        data?: T,
        query?: StringifiableRecord,
    ): Promise<O> {
        return this.doApiRequest('DELETE', path, data, query);
    },
    doApiGetRequest<O>(path: string, query?: StringifiableRecord): Promise<O> {
        return this.doApiRequest('GET', path, undefined, query);
    },

```

#### 思考

****

 **for, forEach，for in，for of, map区别**

```typescript
const a = [1,2,3,4]
const b = {a: 1,b: 2};
//forEach使用起来更加简洁
a.forEach(i => {
	console.log(i);
});
//for in一般用来遍历对象
for (const i in b) {
	console.log(i);
}
//for of可以遍历类数组结构
for (const i of a) {
	console.log(i);
}
//map遍历数组,会返回一个新的数组
a.map(i => {
	console.log(i);
});
```

#### 微信小程序登录流程

****

```typescript
   export async function doAuthJsCode(): Promise<IWechatLogin> {
        const result = await wx.login();
        return app.doApiPostRequest('/wechat/auth/jscode', { code: result.code });
    }
    //先调用wx.login获取code，通过doAuthJsCode接口把code传给后端，后端通过code向微信客户端获取session_key和openid。然后加密返回给客户端，客户端保存在全局。
    const data = await doAuthJsCode();
    this.setData({
       loginInfo: data,
    });
    //当用户点击微信授权的时候，调用getUserProfile方法，通过微信提供的getUserProfile方法拿到用户的信息，，注意这里的用户信息还是加密的，之后把加密的用户信息和登录的加密信息一起返回给后端，后端拿到信息后就会注册一个用户，并且为这个用户生成一个token值返回给客户端，客户端把token值放到http的请求头上，每次调用接口的时候都要带上这个token值。
    getUserProfile() {
        if (!this.data.loginInfo) {
            return false;
        }
        wx.getUserProfile({
            desc: '展示用户信息',
            success: async res => {
                const encryptedData = res.encryptedData || '';
                const iv = res.iv || '';
                const session = await requestWechatMobileLogin({
                    encryptedData,
                    iv,
                    loginEncryptedData: this.data.loginInfo.encryptedData,
                    loginIv: this.data.loginInfo.iv,
                });
                await app.setJwt(session.jwt);
                const userInfo = await app.doApiGetRequest<UserInfo>('/user');
                this.setData({ hasToken: app.hasToken(), userInfo });
                await app.setUserInfo(userInfo);
                this.getAllRooms();
                return true;
            },
        });
    },

```

#### 记录列表翻页

****

```typescript
   //先定义一个变量去记录当前页有多少条数据，每次滚轮触底的时候会触发小程序的生命周期函数onReachBottom，在里面给size加10，然后再去调用一次接口
    onReachBottom() {
        let size = this.data.size;
        size += 10;
        this.setData({
            size,
        });
        this.getAllRooms();
    },
```

#### js中的同步异步问题

js是一个单线程的语言，很多地方都需要去异步去操作。不然会产生堵塞，导致有些地方拿不到最新的信息。所以作为一个合格的js开发人员，一定要有一个很好的异步思维。我在项目中好几个应该使用异步的地方没有异步去执行。导致没有拿到最新的信息，产生了一些bug。比如我在结束房间的时候会去调用一个结算房间的接口，然后我会跳转到房间结算的页面，由于没有异步，导致我在结算页面查询房间信息的时候房间还是处于未结算的状态。具体的一些异步方法有`promise`,`async` `await`，`setTimeout`,`setInterval`

#### 时间格式转换

由于微信小程序不支持moment去做时间格式的转换，不然的话的会在打包的时候报错，所以我手写了一个方法去实现时间格式化，具体是利用Date自带的一些api很容易去实现。

```typescript
const formatServerTime = (str: string) => {
    const date = new Date(str);
    const year = date.getFullYear();
    const month = date.getMonth() + 1;
    const day = date.getDate();
    const hour = date.getHours();
    const minute = date.getMinutes();
    const second = date.getSeconds();
    return [year, month, day].map(formatNumber).join('-') + ' ' + [hour, minute, second].map(formatNumber).join(':');
};
```

