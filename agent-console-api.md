# How to use Agent Console API SDK

The JavaScript SDK must be fully loaded before you can call the JavaScript APIs, which means you can use them when the callback function is assigned with `Comm100AgentConsoleAPI.onReady`.

```javascript
Comm100AgentConsoleAPI.onReady = function () {
  // when this callback is triggered,
  // all APIs are ready to use
  Comm100AgentConsoleAPI.init();
}
```

## What APIs We Provides

We provides 4 fundamental APIs to help you interact with the Comm100 Agent Console.

1. GET

  To get necessary information, you can use `Comm100AgentConsoleAPI.get`:

  ```javascript
  /**
   * @param {string} name - Name of data
   * @param {...any} args - Any specific argument required
   * @return {Promise<void>} result
   */
  Comm100AgentConsoleAPI.get(name, ...args).then(function(data) {

  });
  ```

2. SET

  To set information, you can use `Comm100AgentConsoleAPI.set`:

  ```javascript
  /**
   * @param {string} name - Name of data
   * @param {any} value - Value
   * @param {...any} args - Any specific data needs to be set
   */
  Comm100AgentConsoleAPI.set(name, value, ...args);
  ```

3. ON

  For register event callback function, please use `Comm100AgentConsoleAPI.on`:

  ```javascript
  /**
   * @param {string} type - name of event
   * @param {function} callback - Callback function. Params will be provided accordingly. Callback
   * will be triggered after event, which means callback cannot prevent default behavior of event
   * from happening.
   */
  Comm100AgentConsoleAPI.on(name, function () { });
  ```

4. DO

  To execute the action of the Agent, please use `Comm100AgentConsoleAPI.do`:

  ```javascript
  /**
   * @param {string} name - name of action
   * @param {...any} args - Any args required
   * @return {Promise<void>} Return promise to indicate if action is done successfully
   */
  Comm100AgentConsoleAPI.do(name, args);
  ```
5. Naming Rules

  GET/SET function are named as namespace. DO/ON function are named as  namespace.verb.


# General

## Basic Data Structure

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

  


## Interfaces on Agent Console Page

You can add one more tab in the My Chat tab in the Agent Console, please specify an URL which allows the users to debug the program.

  ```javascript
    // The following function, designed for developers, needs to be called in the Agent Console directly. 
    Comm100LiveChat.extensions.add(name, url);
  ```

# Agent

## get/set

1. Get the Information of Current Logged-in Agents

  ```javascript
  /** @type {object(agent)} **/
  Comm100AgentConsoleAPI.get('agentconsole.currentAgent').then(function(agent){

  });
  ```

# Chat

## get/set

1. Get the Information of Current Chats

  ```javascript
  /** @type {object(chat)} **/
  Comm100AgentConsoleAPI.get('agentconsole.currentChat').then(function(chat){

  });
  ```

2. Get the Information of the Current Chatting Visitors 

  ```javascript
  /** @type {object(visitor)}**/
  Comm100AgentConsoleAPI.get('agentconsole.currentChat.visitor').then(function(visitor){
    
  });
  ```

3. Set the Pre-chat Information for Current Chat

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

## Event

1. Chat started/Chat ended Event

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.chats.chatStart', function(chat) {});

  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.chats.chatEnd', function(chat) {}); 
  ```

2. Current Chatting Visitor Changes Status

  ```javascript
  /** @param {object(visitor)} visitor **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.visitor.status.change', function(visitor) {});
  ```

3. Agent Submit the Wrap-up for Current Chat

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.submitWrapup', function(chat) {});
  ```

4. Current Selected Chat Changes

  ```javascript
  /** @param {object(chat)} chat **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.selectChange', function(chat) { });
  ```

5. Current Selected Chat Receives Visitor Message

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.on('agentconsole.currentChat.receiveVisitorMessage', function(message) { });
  ```

# Action

1. Send Message to Current Chat

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.do('agentconsole.currentChat.send', message);
  ```
2. Insert Message to the Input Box of Current Chat

  ```javascript
  /** @param {string} message **/
  Comm100AgentConsoleAPI.do('agentconsole.currentChat.input', message)
  ```
