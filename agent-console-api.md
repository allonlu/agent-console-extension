# 如何使用

所有的JS API调用需要在 JS API SDK加载完成以后调用, 即 `Comm100AgentConsoleAPI.onReady` 之后调用

```javascript
Comm100AgentConsoleAPI.onReady = function () {
  // when this callback is triggered,
  // all APIs are ready to use

  Comm100AgentConsoleAPI.init();
}
```


API 提供四个接口，分别是

1. 获取数据的 get 接口

  ```javascript
  /**
   * @param {string} name - Name of data
   * @param {...any} args - Any specific argument required
   * @return {Promise<void>} result
   */
  Comm100AgentConsoleAPI.get(name, ...args).then(function(data) {

  });
  ```

2. 设置数据的 set 接口

  ```javascript
  /**
   * @param {string} name - Name of data
   * @param {any} value - Value
   * @param {...any} args - Any specific data needs to be set
   */
  Comm100AgentConsoleAPI.set(name, value, ...args);
  ```

3. 注册事件回调函数的 on 接口

  ```javascript
  /**
   * @param {string} type - name of event
   * @param {function} callback - Callback function. Params will be provided accordingly. Callback
   * will be triggered after event, which means callback cannot prevent default behavior of event
   * from happening.
   */
  Comm100AgentConsoleAPI.on(name, function () { });
  ```

4. 执行Agent动作的 do 接口

  ```javascript
  /**
   * @param {string} name - name of action
   * @param {...any} args - Any args required
   * @return {Promise<void>} Return promise to indicate if action is done successfully
   */
  Comm100AgentConsoleAPI.do(name, args);
  ```
5. 命名规则

   get/set/render函数的名字为namespace，do/on函数的名字为namespace.verb


# 全局

## 基本数据结构

  ```javascript
  // visitor
  const visitor = {
    id: 1,    // number
    name: '', // string
    email: '',// string
    status: '', // string, waiting for chat/chatting/prechat/inviting/offline message/refused by agent/refused by visitor/chat ended/in site/out of site/transferring/system processing
    enterSiteTime: 1502934947,  //number, unix time
    referrer: '', // string, the referrer url
    landingPage: {
      title: '',  // string
      url: '',    // string
    }
    currentPage: {
      title: '',  // string
      url: '',    // string
    },
    currentPageStayTime: 20, // number, time of this visitor stay in current page, unit(second)
    numOfPages: 20, // number, the number of this visitor view pages
    location: {
      ip: '',   // string, ip address in dotted form
      city: '', // string
      state: '',// string
      country: '',  // string
    },
    device: {
      os: '', // string
      browser: '',  // string
      screenResolution: '', //string
      language: '', // string
      timezone: '', // string
    },
    history: {
      firstVisitorTime: 1502934947, // number, unix time
      visitTimes: 0,  // number, visit times of this visitor
      chatTimes: 0,   // number, chat times of this visitor
    }
    visitorSegmentations: [], // array<string>, names of segmentation
    customVariables: [
      {
        name: '', // string
        value: '',// string
      },
    ]
  }


  // chat
  const chat = {
    id: 1,  // number
    visitorId: 1, // number
    status: ''  // string,  chatting/waiting/transferring/chat ended/audio chatting/video chatting/inviting
    name: '',     // string
    email: '',    // string 
    company: '',  // string
    phone: '',    // string
    productService: '', // string
    department: '',     // string, name of department
    fields: [
      {
        name: '',   // string
        value: '',  // string
      }
    ]
    rating: {
      score: 1,   // number, 0 - 5
      comment: '',  // string,
    }
    agents: [], // array<string>, agent's name
    requestPage: {
      title: '',  // string
      url: '',    // string
    },
    requestTime: 1502934947,  // number, unix time
    waitingTime: 15,  // number, how long the visitor waiting time, unit(second)
    duration: 180, //number, how long the chat duration, unit(second)、
    messages: [
      {
        sender: '',
        senderType: '', // string, system/agent/visitor
        time: 1502934947,
        content: '',  // string, html 
      }
    ]
  }

  // agent
  const  agent = {
    id: 1,    // number
    name: '', // string
    email: '',// string
    status: '', // string, online/away
    apikey: '', // string, 
    chats: 3,   // number, ongoing chats
    isAdmin: false, // boolean
  }
  ```

  


## Agent Console 页面上的接口

1. 在chats Tab中增加一个tab, 指定具体的地址, 方便用户快速调试程序

  ```javascript
    // 该接口需要用户在Agent　Cosnole页面上直接调用, 针对开发人员使用
    Comm100LiveChat.extensions.add(name, url);
  ```

# Agent

## get/set

1. 获取当前登录的agent信息

  ```javascript
  /** @type {object(agent)} **/
  Comm100AgentConsoleAPI.get('agentconsole.currentAgent').then(function(agent){

  });
  ```

# Chat

## get/set

1. 获取当前聊天信息

  ```javascript
  /** @type {object(chat)} **/
  Comm100AgentConsoleAPI.get('agentconsole.currentChat').then(function(chat){

  });
  ```

2. 获取当前聊天的访客信息

  ```javascript
  /** @type {object(visitor)}**/
  Comm100AgentConsoleAPI.get('agentconsole.currentChat.visitor').then(function(visitor){
    
  });
  ```

3. 设置当前聊天的pre-chat信息

  ```javascript
  /** @param {string} name **/
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.name', name);

  /** @param {string} email **/
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.email', email);

  /** @param {string} phone **/
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.phone', phone);

  /** @param {string} productService **/
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.productService', productService);

  /** @param {string} department **/
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.department', department);

  /** @param {array<field>} fields **/
  const fields = [{
    name: '',
    value: '',
  }];
  Comm100AgentConsoleAPI.set('agentconsole.currentChat.fields', fields)

  ```

## 事件

1. 聊天开始/结束的事件

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.chats.chatStart', function(chat) {});

  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.chats.chatEnd', function(chat) {}); 
  ```

2. 当前聊天的访客状态发生变化

  ```javascript
  /** @param {object(visitor)} visitor **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.visitor.status.change', function(visitor) {});
  ```

3. agent为当前聊天提交了wrapup

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.submitWrapup', function(chat) {});
  ```

4. 当前选中的chat变化

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.selectChange', function(chat) { });
  ```

5. 当前选中的chat收到访客消息

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.receiveVisitorMessage', function(message) { });
  ```

# 动作

1. 发送聊天消息到当前chat

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.do('agentconsole.currentChat.send', message);
  ```
2. 插入消息到当前chat的输入框中

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.do('agentconsole.currentChat.input', message)
  ```
