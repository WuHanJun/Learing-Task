# 简答
## 一、什么是闭包? 有什么作用。
- 闭包（Closure）：又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。【[维基百科](https://zh.wikipedia.org/wiki/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)#C.E8.AF.AD.E8.A8.80.E7.9A.84.E5.9B.9E.E8.B0.83.E5.87.BD.E6.95.B0)】
- 作用：
1. 读取函数内部的变量。
2. 让这些变量的值始终保持在内存中。

------
- 我觉得得从为什么需要闭包来理解这个概念。
1. 在c语言中，我要想**得到函数内的局部变量并修改**可以直接让系统动态分配内存，使用回调函数的方式。支持回调函数的库需要两个参数，一个函数指针，一个独立的void指针用于保存用户数据。这样的做法**允许回调函数恢复其调用时的状态**。
注1：回调函数：简称回调，通过函数参数传递到其他代码，某一块可执行代码的引用。
注2：函数指针：和普通变量取址作为参数传递没什么区别。
注3：当然在c里还有两种扩展，c++里也有类似于闭包的显式行为。在此不细表。
2. 在Javascript中或者一些函数式语言中，我要**想实现这个功能**并不能使用指针。在Js的变量作用域中，函数内部可以直接读取全局变量，但函数外部无法读取函数内部声明的变量。当我们需要得到函数内部的局部变量时，就可以采用在该函数内部再定义一个函数。如果内部的函数引用了外部的函数的变量，则可能产生闭包。运行时，一旦**外部的函数被执行**，一个**闭包**就**形成**了。闭包中包含了内部函数的代码，以及所需外部函数中的变量的引用。这时候我们就可以在**获取到外部函数内的变量**了。

---

- 看个示例
``` javascript
var a = function () {
  var test = {};
  setTimeout(function () {
    console.log(test);
  }, 1000);
}
```
上面的例子中，text在a中定义，但在setTimeout函数中对保持了引用。当a执行了，尽管a已经执行完，理论上来说a这个函数执行过程中产生的变量、对象都可以被销毁。但text由于被引用，所以不能随这个函数执行结束而被销毁，直到延时器里的函数被执行掉。

- 再看一个
``` javascript
var obj = function () {
  var a = '';
  return {
    set: function (val) {
      a = val;
    },
    get: function () {
      return a;
    }
  }
};

var b = obj();
b.set('new val');
b.get();
```
以上 obj这个函数在执行完之后理论上 函数体内东西都应该被回收掉。但它执行后的返回值 b 具有set和get方法。这两个方法里对a保持了引用，所以obj执行过程中产生的a就不会销毁。直到b先被回收，这个a才会回收。

- 闭包的注意点：
(1) 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
(2) 上句话的意思是：父级函数作用域会一直存在，所以其他变量也会存在？不单单是调用的那个变量？错！！！！ 除了使用的那个变量，其他都不会在内存中！为什么我们使用的那个变量会一直存在？，刚刚第二个例子已经解释了。
- 闭包关键：
两个字：内存。

---


## 二、setTimeout 0有什么作用？
- 原理
1. setTimeout和setInterval的**运行机制**是将制定的代码移出本次执行等到下一轮Event Loop（事件回路）再检查是否到指定时间。如果到了，就执行对应的代码，如果不到，就**再等到下一轮** Event Loop重新判断。
2. setTimeout(f, 0)将第二个参数设为0，作用是让f在现有的任务（脚本的同步任务和“消息队列”指定的任务）一结束就立刻执行。也就是说，setTimeout(f, 0)的作用是，**尽可能早地执行指定的任务。而并不是会立刻就执行这个任务。**
3. 注意的是即使消息队列是空的，0毫秒实际上也是达不到的，根据实际测试得出这个值是4.7毫秒左右。根据html5标准，setTimeout推迟执行的时间最少是4毫秒。如果小于这个值，会被自动增加到4。这是为了防止多个 setTimeout(f, 0)语句连续执行造成性能问题。
- 应用

(1) 调整事件的发生顺序（如回调函数的先后）
``` Javascript
var input = document.getElementsByTagName('input[type=button]')[0];

input.onclick = function A() {
  setTimeout(function B() {
    input.value +=' input';
  }, 0)
};

document.body.onclick = function C() {
  input.value += ' body'
};
```
上面代码在点击按钮后，先触发回调函数A，然后触发函数C。在函数A中，setTimeout将函数B推迟到下一轮Loop执行，这样就起到了，先触发父元素的回调函数C的目的了。

用户自定义的回调函数，通常在浏览器的默认动作之前触发。比如，用户在输入框输入文本，keypress事件会在浏览器接收文本之前触发。因此，下面的回调函数是达不到目的的。
```
document.getElementById('input-box').onkeypress = function(event) {
  this.value = this.value.toUpperCase();
}
```
上面代码想在用户输入文本后，立即将字符转为大写。但是实际上，它只能将上一个字符转为大写，因为浏览器此时还没接收到文本，所以this.value取不到最新输入的那个字符。只有用setTimeout改写，上面的代码才能发挥作用。
```
document.getElementById('my-ok').onkeypress = function() {
  var self = this;
  setTimeout(function() {
    self.value = self.value.toUpperCase();
  }, 0);
}
```

(2) 将计算量大、耗时长的任务分成几个小部分分别放到setTimeout(f, 0)执行。
```
 var div = document.getElementsByTagName('div')[0];

// 写法一
for (var i = 0xA00000; i < 0xFFFFFF; i++) {
  div.style.backgroundColor = '#' + i.toString(16);
}

// 写法二
var timer;
var i=0x100000;

function func() {
  timer = setTimeout(func, 0);
  div.style.backgroundColor = '#' + i.toString(16);
  if (i++ == 0xFFFFFF) clearTimeout(timer);
}

timer = setTimeout(func, 0);
```
上面代码有两种写法，都是改变一个网页元素的背景色。写法一会造成浏览器“堵塞”，因为JavaScript执行速度远高于DOM，会造成大量DOM操作“堆积”，而写法二就不会，这就是setTimeout(f, 0)的好处。

另一个使用这种技巧的例子是代码高亮的处理。如果代码块很大，一次性处理，可能会对性能造成很大的压力，那么将其分成一个个小块，一次处理一块，比如写成setTimeout(highlightNext, 50)的样子，性能压力就会减轻。
# 代码
## 一.下面的代码输出多少？修改代码让`fnArr[i]()`输出i，使用两种以上的方法。
```
 var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        fnArr[i] =  function(){
            return i;
        };
    }
    console.log( fnArr[3]() );  //输出10

    //方法一
    var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        (function (i) {
            fnArr[i] = function () {
                return i;
            };
        })(i);
    }
    console.log( fnArr[3]() );  //输出3

    //方法二
    var fnArr = [];
    for (var i = 0; i < 10; ++i){
        (function () {
            var n = i; //var i = i;  //var i = i不行，会报错。 说fnArr[3] 不是一个函数。有点意思。
            fnArr[i] = function () {   //我需要知道的是 对于已经声明过的变量，再次var 是无效的，不过，如果
                return n; // 同时赋值的话，会覆盖之前的值
            }
        })();
    }
    console.log( fnArr[3]() );  //输出3

    //方法三
    var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        fnArr[i] = function (i) {
            return function () {
                return i;
            };
        }(i);
    }
    console.log( fnArr[3]() );  //输出3
```

## 二、使用闭包封装一个汽车对象，可以通过如下方式获取汽车状态。
```
var Car = //todo;
Car.setSpeed(30);
Car.getSpeed(); //30
Car.accelerate();
Car.getSpeed(); //40;
Car.decelerate();
Car.decelerate();
Car.getSpeed(); //20
Car.getStatus(); // 'running';
Car.decelerate(); 
Car.decelerate();
Car.getStatus();  //'stop';
//Car.speed;  //error
```
```
 var Car = function(){   //Car是一个对象！！
        var speed = 0;
        function accelerate() {  //加速
            speed += 10;
        }
        function decelerate() {  //减速
            speed -= 10;
        }
        function getSpeed() {
            console.log(speed);
        }
        function getStatus() {
            if (speed > 0) {
                console.log('running');
            }else {
                console.log('stop');
            }
        }
        function setSpeed(i) {
            speed = i;
        }
        return {
            accelerate: accelerate,
            decelerate: decelerate,
            getSpeed: getSpeed,
            getStatus: getStatus,
            setSpeed: setSpeed
        }
    }();
    Car.setSpeed(20);
    Car.getSpeed();
    Car.setSpeed();
```
## 三、一个函数使用setTimeout模拟setInterval的功能
```
function interval(func, wait){
        var interv = function(){
            func.call(null);
            setTimeout(interv, wait);
        };
        setTimeout(interv, wait);
    };
```
## 四、写一个函数，计算setTimeout平均[备注：新加]最小时间粒度。
```
function getMini(){
        var i = 0;
        var start = Date.now();
        var clock = setTimeout(function(){
            ++i;
            if(i === 1000){   //10000-4.7803ms  //1000-4.766ms
                clearTimeout(clock);
                var end = Date.now();
                console.log((end-start)/i);
            }
            clock = setTimeout(arguments.callee, 0);
        }, 0);
    };
    getMini();  //记得调用函数，找错找了10分钟郁闷。   //Html5标准是4ms
```
## 五、下面这段代码输出结果是? 为什么?
```
 var a = 1;
    setTimeout(function(){
        a = 2;          //setTimeout函数将这段代码放到下一个event loop中判断是否到时间。
        console.log(a);
    }, 0);//设置的是0，浏览器默认最小是4毫秒多，所以我们看到的是2在1和3后面立即输出。
    var a ;  //这个声明无效。
    console.log(a);   //按顺序输出1和3
    a = 3;
    console.log(a);   //输出1 3 2。
```
## 六、下面这段代码输出结果是? 为什么?
```
var flag = true;
setTimeout(function () { //setTimeout将flag = true放到下一轮event loop执行。
    flag = false;
}, 0);
while (flag){}  //所以这里的flag一直是true 进入死循环。
console.log(flag);  //脚本根本打不开了。 原因在于上面的while循环。
```
## 七、下面这段代码输出？如何输出delayer: 0, delayer:1...（使用闭包来实现）
```
for(var i = 0; i < 5; i++){
        setTimeout(function () {
            console.log('delayer:' + i ); // 这输出5个delayer:5 因为这段代码被放到消息队列最后执行。
        }, 0);
        console.log(i);  //这里先输出0 1 2 3 4
    }
    //输出 0 1 2 3 4 后面5个delayer:5
```
使用闭包实现
```
//用闭包实现delayer: 0, delayer:1...
    //方法一
    for(var i = 0; i < 5; ++i){
        (function (i) {    //知识点回顾：如果给定的参数未传入，该变量就是undefined
            setTimeout(function () {
                console.log('delayer:' + i);
            }, 0);
        })(i);
    }

    //方法二(同学别被我误导，这个不是闭包方法哟)
    for(var i = 0; i < 5; ++i){
            setTimeout(function (i) {
                console.log('delayer:' + i);
            }, 0, i); //这个方法只有在ie9以上版本
    }
```
---
[维基百科](https://zh.wikipedia.org/wiki/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)#gcc.E5.AF.B9C.E8.AF.AD.E8.A8.80.E7.9A.84.E6.89.A9.E5.B1.95)
[朴灵老师](https://www.zhihu.com/question/27712980/answer/37768023)
[阮一峰老师](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
[芳芳老师](https://zhuanlan.zhihu.com/p/22486908?refer=study-fe)
_本教程版权归本人和饥人谷所有，转载须说明来源_

