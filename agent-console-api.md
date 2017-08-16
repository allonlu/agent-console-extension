# 如何使用

所有的JS API调用需要在 JS API SDK加载完成以后调用, 即 `LiveChat.onReady` 之后调用

```javascript
LiveChat.onReady = function () {
  // when this callback is triggered,
  // all APIs are ready to use
}
```


API 提供四个接口，分别是

1. 获取数据的 get 接口

  ```javascript
  /**
   * @param {string} type - Type of data
   * @param {...any} args - Any specific argument required
   * @return {any} result
   */
  LiveChat.get('type', ...args);
  ```

2. 设置数据的 set 接口

  ```javascript
  /**
   * @param {string} type - Type of data
   * @param {any} value - Value
   * @param {...any} args - Any specific data needs to be set
   */
  LiveChat.set('type', value, ...args);
  ```

3. 注册事件回调函数的 on 接口

  ```javascript
  /**
   * @param {string} type - Type of event
   * @param {function} callback - Callback function. Params will be provided accordingly. Callback
   * will be triggered after event, which means callback cannot prevent default behavior of event
   * from happening.
   */
  LiveChat.on('type', function () { });
  ```

4. 模拟访客动作的 do 接口

  ```javascript
  /**
   * @param {string} type - Type of action
   * @param {...any} args - Any args required
   * @return {Promise<void>} Return promise to indicate if action is done successfully
   */
  LiveChat.do('type', args);
  ```
5. 命名规则

   get/set/render函数的名字为namespace，do/on函数的名字为namespace.verb


# 全局

## 基本数据结构

  ```javascript
  // visitor
  const visitor = {
    id: 1, 
    name: '',
    email: '',
    status: '',
    enterSiteTime: '',
    referrer: '',
    landingPage: {
      title: '',
      url: '',
    }
    currentPage: {
      title: '',
      url: '',
    },
    currentPageStayTime: '',
    numOfPages: '',
    location: {
      ip: '',
      city: '',
      state: '',
      country: '',
    },
    device: {
      os: '',
      browser: '',
      screenResolution: '',
      language: '',
      timezone: '',
    },
    history: {
      firstVisitorTime: '',
      visitTimes: 0,
      chatTimes: 0,
    }
    
    variables: [
      {
        name: '',
        value: '',
      },
    ]
  }

  // chat
  const chat = {
    id: 1,  // number
    status: ''  // chatting
    department: '' // string
    prechat: {

    },
    agents: [], // array<string>
    requestPage: {
      title: '',
      url: '',
    },
    requestTime: '',
    duration: '',
  }

  // agent
  const  agent = {
    name: '',
    email: '',
    isAdmin: false,

  }
  ```
## Agent Console 页面上的接口

1. 在chats Tab中增加一个tab, 指定具体的地址, 方便用户快速调试程序

  ```javascript
    // 该接口需要用户在Agent　Cosnole页面上直接调用, 针对开发人员使用
    LiveChat.extensions.add(name, url);
  ```

# Agent

## get/set

1. 获取当前登录的agent信息

  ```javascript
  /** @type {object(agent)} **/
  const agent = LiveChat.get('livechat.agent');
  ```

## 事件

1. 当前agent的状态变化, online/away

  ```javascript
  /** @param {string} status - new status of agent, online/away **/
  LiveChat.on('livechat.agent.status.change', function(status) { });
  ```

# Chat

## get/set

1. 获取当前聊天信息

  ```javascript
  /** @type {object(chat)} **/
  const chat = LiveChat.get('livechat.chats.current');
  ```

2. 获取当前聊天的访客信息

  ```javascript
  /** @type {object(visitor)}**/
  const visitor = LiveChat.get('livechat.chats.current.visitor');
  ```

3. 设置当前聊天的pre-chat信息

  ```javascript
  const prechat = {
    name: '',     // string
    email: '',    // string 
    company: '',  // string
    phone: '',    // string
    productService: '', // string
    department: '',     // string
    customFields: [
      {
        name: '',   // string
        value: '',  // string
      }
    ]
  }
  LiveChat.set('livechat.chats.current.prechat', prechat);
  ```

4. 设置当前聊天的wrapup信息

  ```javascript
  const wrapup = {
    category: '',     // string, category simple mode
    categoryList: [], // array<string>, category advanced mode
    customFields: [
      {
        name: '',   // string
        value: '',  // string
      }
    ]
    
  };
  LiveChat.set('livechat.chats.current.wrapup', wrapup);
  ```

## 事件

1. 当前选中的chat变化

  ```javascript
  /** @param {object(chat)} chat **/
  LiveChat.on('livechat.chats.select', function(chat) { });
  ```

2. 当前选中的chat的状态变化, 开始聊天/结束聊天

  ```javascript
  LiveChat.on('livechat.chats.current.start', function() { });
  LiveChat.on('livechat.chats.current.end', function() { });
  ```

3. 当前选中的chat收到访客消息

  ```javascript
  /** @param {string} message **/
  LiveChat.on('livechat.chats.current.receive', function(message) { });
  ```

# 动作

1. 发送聊天消息到当前chat

  ```javascript
  /** @param {string} message **/
  LiveChat.do('livechat.chats.current.send', message);
  ```
2. 插入消息到当前chat的输入框中

  ```javascript
  /** @param {string} message **/
  LiveChat.do('livechat.chats.current.input', message)
  ```

# Tab

## get/set

1. 当前Tab是否被选中

  ```javascript
  /** @type {boolean} **/
  const isActive = LiveChat.get('tab.isActive');
  ```

2. 当前Tab的徽标

  ```javascript
  /** @type {string} **/
  LiveChat.get('tab.badge');

  LiveChat.set('tab.badge',  badge);
  ```


## 事件

1. 当前Tab选中与否的状态变化

  ```javascript
  LiveChat.on('tab.active', function() { });
  LiveChat.on('tab.inactive', function() { });
  ```

## 动作

1. 选中当前Tab

  ```javascript
  LiveChat.do('tab.active')
  ```
