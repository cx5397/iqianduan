+++
title = "设计模式"
description = "设计模式"
date = 2022-07-25T10:20:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "计算机基础"
]
series = [
  "编码"
]
tags = [
  '设计模式'
]

images = []
+++

<!--more-->

### 策略模式

- 允许封装用于特定任务的备选算法的设计模式。它可以定义一系列算法，并以这样一种方式封装它们。它们在运行时可以互换调用顺序，而不需要编写额外的代码

```
//策略列表
const jobList = ['FE', 'BE'];
var strategies = {
  checkRole: function(value) {
    if (value === 'registered') {
      return true;
    }
    return false;
  },
  checkGrade: function(value) {
    if (value >= 1) {
      return true;
    }
    return false;
  },
  checkJob: function(value) {
    if (jobList.indexOf(value) > 1) {
      return true;
    }
    return false;
  },
  checkType: function(value) {
    if (value === 'active user') {
      return true;
    }
    return false;
  }
};
//验证逻辑
var Validator = function() {
  // Store strategies
  this.cache = [];
  // add strategy to cache
  this.add = function(value, method) {
    this.cache.push(function() {
      return strategies[method](value);
    });
  };
  // check all strategies
  this.check = function() {
    for (let i = 0; i < this.cache.length; i++) {
      let valiFn = this.cache[i];
      var data = valiFn();
      if (!data) {
        return false;
      }
    }
    return true;
  };
};
//调用
var compose1 = function() {
  var validator = new Validator();
  const data1 = {
    role: 'register',
    grade: 3,
    job: 'FE',
    type: 'active user'
  };
  validator.add(data1.role, 'checkRole');
  validator.add(data1.grade, 'checkGrade');
  validator.add(data1.type, 'checkType');
  validator.add(data1.job, 'checkJob');
  const result = validator.check();
  return result;
};
```

### 发布-订阅模式

- 一种消息传递范例，消息的发布者不直接将消息发送给特定的订阅者，而是通过消息通道广播消息，订阅者通过订阅获得他们想要的消息。

```
const EventEmit = function() {
  this.events = {};
  this.on = function(name, cb) {
    if (this.events[name]) {
      this.events[name].push(cb);
    } else {
      this.events[name] = [cb];
    }
  };
  this.trigger = function(name, ...arg) {
    if (this.events[name]) {
      this.events[name].forEach(eventListener => {
        eventListener(...arg);
      });
    }
  };
};

let event = new EventEmit();
MessageCenter.fetch() {
  event.on('success', () => {
    console.log('update MessageCenter');
  });
}
Order.update() {
  event.on('success', () => {
    console.log('update Order');
  });
}
Checker.alert() {
  event.on('success', () => {
    console.log('Notify Checker');
  });
}
event.trigger('success');
```

```
//类似事件触发
window.addEventListener('event_name', function(event){
    console.log('得到标题为：', event.detail.title);
});
var myEvent = new CustomEvent('event_name', {
    detail: { title: 'This is title!'},
});
// 随后在对应的元素上触发该事件
if(window.dispatchEvent) {
    window.dispatchEvent(myEvent);
} else {
    window.fireEvent(myEvent);
}
```

### 装饰者模式

- 种给对象动态地增加职责的方式称为装饰者（decorator）模式。装饰者模式能够在不改变对象自身的基础上，在程序运行期间给对象动态地添加职责。跟继承相比，装饰者是一种更轻便灵活的做法，这是一种“即用即付”的方式

```
//原始对象
// 建立一架飞机
var Plane = function(){}
// 飞机有发射普通子弹的方法
Plane.prototype.fire = function(){
    console.log( '发射普通子弹' );
}


//接下来增加两个装饰类，分别是导弹和原子弹：
var MissileDecorator = function( plane ){
    this.plane = plane;
}
MissileDecorator.prototype.fire = function(){
    this.plane.fire();
    console.log( '发射导弹' );
}
var AtomDecorator = function( plane ){
    this.plane = plane;
}
AtomDecorator.prototype.fire = function(){
    this.plane.fire();
    console.log( '发射原子弹' );
}

//调用
var plane = new Plane();
plane = new MissileDecorator( plane );
plane = new AtomDecorator( plane );
plane.fire();
// 分别输出： 发射普通子弹、发射导弹、发射原子弹
```

### 责任链模式

- 为请求创建了一个接收者对象的链。多个对象均有机会处理请求，从而解除发送者和接受者之间的耦合关系。这些对象连接成为“链式结构”，每个节点转发请求，直到有对象处理请求为止

```
    class Handler {
        constructor() {
             this.handler = null;
        }
        // 将下一个需要处理的操作缓存起来
        setNextHandler(handler) {
            this.next = handler;
        }
    }

    class LogHandler extends Handler {
        constructor() {
            super();
            this.name = 'log';
        }

        handle(type, msg) {
            if (type == this.name) {
                console.log(`${type} ---------- ${msg}`);
                return;
            }
            // 假如下一个处理操作存在就继续往下处理，走到这里表示当前并非终点
            this.next && this.next.handle(...arguments);
        }
    }

    class WarnHandler extends Handler {
        constructor() {
            super();
            this.name = 'warn';
        }

        handle(type, msg) {
            if (type == this.name) {
                //假如传过来的类型与名字相同， 说明是当前操作处理
                console.log(`${type} ---------- ${msg}`);
                return;
            }
            // 假如下一个处理操作存在就继续往下处理，走到这里表示当前并非终点
            this.next && this.next.handle(...arguments);
        }
    }

    class ErrorHandler extends Handler {
        constructor() {
            super();
            this.name = 'error';
        }

        handle(type, msg) {
            if (type == this.name) {
                console.log(`${type} ---------- ${msg}`);
                return;
            }
            // 假如下一个处理操作存在就继续往下处理，走到这里表示当前并非终点
            this.next && this.next.handle(...arguments);
        }
    }

    const logHandler = new LogHandler();
    const warnHandler = new WarnHandler();
    const errorHandler = new ErrorHandler();

    //设置下一个处理的节点
    logHandler.setNextHandler(warnHandler);
    warnHandler.setNextHandler(errorHandler);

    // 开始依次处理满足条件的日志
    logHandler.handle('log', 'this is some logs!'); // log ---------- this is some logs!
    logHandler.handle('warn', 'this is some warnings!'); // warn ---------- this is some warnings!
    logHandler.handle('error', 'this is some errors!'); // error ---------- this is some errors!
```
