# ���
## һ��ʲô�Ǳհ�? ��ʲô���á�
- �հ���Closure�����ֳƴʷ��հ���Lexical Closure�������հ���function closures���������������ɱ����ĺ�������������õ����ɱ��������������һͬ���ڣ���ʹ�Ѿ��뿪�˴������Ļ���Ҳ�����⡣��[ά���ٿ�](https://zh.wikipedia.org/wiki/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)#C.E8.AF.AD.E8.A8.80.E7.9A.84.E5.9B.9E.E8.B0.83.E5.87.BD.E6.95.B0)��
- ���ã�
1. ��ȡ�����ڲ��ı�����
2. ����Щ������ֵʼ�ձ������ڴ��С�

------
- �Ҿ��õô�Ϊʲô��Ҫ�հ������������
1. ��c�����У���Ҫ��**�õ������ڵľֲ��������޸�**����ֱ����ϵͳ��̬�����ڴ棬ʹ�ûص������ķ�ʽ��֧�ֻص������Ŀ���Ҫ����������һ������ָ�룬һ��������voidָ�����ڱ����û����ݡ�����������**����ص������ָ������ʱ��״̬**��
ע1���ص���������ƻص���ͨ�������������ݵ��������룬ĳһ���ִ�д�������á�
ע2������ָ�룺����ͨ����ȡַ��Ϊ��������ûʲô����
ע3����Ȼ��c�ﻹ��������չ��c++��Ҳ�������ڱհ�����ʽ��Ϊ���ڴ˲�ϸ��
2. ��Javascript�л���һЩ����ʽ�����У���Ҫ**��ʵ���������**������ʹ��ָ�롣��Js�ı����������У������ڲ�����ֱ�Ӷ�ȡȫ�ֱ������������ⲿ�޷���ȡ�����ڲ������ı�������������Ҫ�õ������ڲ��ľֲ�����ʱ���Ϳ��Բ����ڸú����ڲ��ٶ���һ������������ڲ��ĺ����������ⲿ�ĺ����ı���������ܲ����հ�������ʱ��һ��**�ⲿ�ĺ�����ִ��**��һ��**�հ�**��**�γ�**�ˡ��հ��а������ڲ������Ĵ��룬�Լ������ⲿ�����еı��������á���ʱ�����ǾͿ�����**��ȡ���ⲿ�����ڵı���**�ˡ�

---

- ����ʾ��
``` javascript
var a = function () {
  var test = {};
  setTimeout(function () {
    console.log(test);
  }, 1000);
}
```
����������У�text��a�ж��壬����setTimeout�����жԱ��������á���aִ���ˣ�����a�Ѿ�ִ���꣬��������˵a�������ִ�й����в����ı��������󶼿��Ա����١���text���ڱ����ã����Բ������������ִ�н����������٣�ֱ����ʱ����ĺ�����ִ�е���

- �ٿ�һ��
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
���� obj���������ִ����֮�������� �������ڶ�����Ӧ�ñ����յ�������ִ�к�ķ���ֵ b ����set��get�������������������a���������ã�����objִ�й����в�����a�Ͳ������١�ֱ��b�ȱ����գ����a�Ż���ա�

- �հ���ע��㣺
(1) ���ڱհ���ʹ�ú����еı��������������ڴ��У��ڴ����ĺܴ����Բ������ñհ�������������ҳ���������⣬��IE�п��ܵ����ڴ�й¶����������ǣ����˳�����֮ǰ������ʹ�õľֲ�����ȫ��ɾ����
(2) �Ͼ仰����˼�ǣ����������������һֱ���ڣ�������������Ҳ����ڣ��������ǵ��õ��Ǹ��������������� ����ʹ�õ��Ǹ��������������������ڴ��У�Ϊʲô����ʹ�õ��Ǹ�������һֱ���ڣ����ոյڶ��������Ѿ������ˡ�
- �հ��ؼ���
�����֣��ڴ档

---


## ����setTimeout 0��ʲô���ã�
- ԭ��
1. setTimeout��setInterval��**���л���**�ǽ��ƶ��Ĵ����Ƴ�����ִ�еȵ���һ��Event Loop���¼���·���ټ���Ƿ�ָ��ʱ�䡣������ˣ���ִ�ж�Ӧ�Ĵ��룬�����������**�ٵȵ���һ��** Event Loop�����жϡ�
2. setTimeout(f, 0)���ڶ���������Ϊ0����������f�����е����񣨽ű���ͬ������͡���Ϣ���С�ָ��������һ����������ִ�С�Ҳ����˵��setTimeout(f, 0)�������ǣ�**���������ִ��ָ�������񡣶������ǻ����̾�ִ���������**
3. ע����Ǽ�ʹ��Ϣ�����ǿյģ�0����ʵ����Ҳ�Ǵﲻ���ģ�����ʵ�ʲ��Եó����ֵ��4.7�������ҡ�����html5��׼��setTimeout�Ƴ�ִ�е�ʱ��������4���롣���С�����ֵ���ᱻ�Զ����ӵ�4������Ϊ�˷�ֹ��� setTimeout(f, 0)�������ִ������������⡣
- Ӧ��

(1) �����¼��ķ���˳����ص��������Ⱥ�
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
��������ڵ����ť���ȴ����ص�����A��Ȼ�󴥷�����C���ں���A�У�setTimeout������B�Ƴٵ���һ��Loopִ�У����������ˣ��ȴ�����Ԫ�صĻص�����C��Ŀ���ˡ�

�û��Զ���Ļص�������ͨ�����������Ĭ�϶���֮ǰ���������磬�û�������������ı���keypress�¼���������������ı�֮ǰ��������ˣ�����Ļص������Ǵﲻ��Ŀ�ĵġ�
```
document.getElementById('input-box').onkeypress = function(event) {
  this.value = this.value.toUpperCase();
}
```
������������û������ı����������ַ�תΪ��д������ʵ���ϣ���ֻ�ܽ���һ���ַ�תΪ��д����Ϊ�������ʱ��û���յ��ı�������this.valueȡ��������������Ǹ��ַ���ֻ����setTimeout��д������Ĵ�����ܷ������á�
```
document.getElementById('my-ok').onkeypress = function() {
  var self = this;
  setTimeout(function() {
    self.value = self.value.toUpperCase();
  }, 0);
}
```

(2) ���������󡢺�ʱ��������ֳɼ���С���ֱַ�ŵ�setTimeout(f, 0)ִ�С�
```
 var div = document.getElementsByTagName('div')[0];

// д��һ
for (var i = 0xA00000; i < 0xFFFFFF; i++) {
  div.style.backgroundColor = '#' + i.toString(16);
}

// д����
var timer;
var i=0x100000;

function func() {
  timer = setTimeout(func, 0);
  div.style.backgroundColor = '#' + i.toString(16);
  if (i++ == 0xFFFFFF) clearTimeout(timer);
}

timer = setTimeout(func, 0);
```
�������������д�������Ǹı�һ����ҳԪ�صı���ɫ��д��һ����������������������ΪJavaScriptִ���ٶ�Զ����DOM������ɴ���DOM�������ѻ�������д�����Ͳ��ᣬ�����setTimeout(f, 0)�ĺô���

��һ��ʹ�����ּ��ɵ������Ǵ�������Ĵ�����������ܴ�һ���Դ������ܻ��������ɺܴ��ѹ������ô����ֳ�һ����С�飬һ�δ���һ�飬����д��setTimeout(highlightNext, 50)�����ӣ�����ѹ���ͻ���ᡣ
# ����
## һ.����Ĵ���������٣��޸Ĵ�����`fnArr[i]()`���i��ʹ���������ϵķ�����
```
 var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        fnArr[i] =  function(){
            return i;
        };
    }
    console.log( fnArr[3]() );  //���10

    //����һ
    var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        (function (i) {
            fnArr[i] = function () {
                return i;
            };
        })(i);
    }
    console.log( fnArr[3]() );  //���3

    //������
    var fnArr = [];
    for (var i = 0; i < 10; ++i){
        (function () {
            var n = i; //var i = i;  //var i = i���У��ᱨ�� ˵fnArr[3] ����һ���������е���˼��
            fnArr[i] = function () {   //����Ҫ֪������ �����Ѿ��������ı������ٴ�var ����Ч�ģ����������
                return n; // ͬʱ��ֵ�Ļ����Ḳ��֮ǰ��ֵ
            }
        })();
    }
    console.log( fnArr[3]() );  //���3

    //������
    var fnArr = [];
    for (var i = 0; i < 10; i ++) {
        fnArr[i] = function (i) {
            return function () {
                return i;
            };
        }(i);
    }
    console.log( fnArr[3]() );  //���3
```

## ����ʹ�ñհ���װһ���������󣬿���ͨ�����·�ʽ��ȡ����״̬��
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
 var Car = function(){   //Car��һ�����󣡣�
        var speed = 0;
        function accelerate() {  //����
            speed += 10;
        }
        function decelerate() {  //����
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
## ����һ������ʹ��setTimeoutģ��setInterval�Ĺ���
```
function interval(func, wait){
        var interv = function(){
            func.call(null);
            setTimeout(interv, wait);
        };
        setTimeout(interv, wait);
    };
```
## �ġ�дһ������������setTimeoutƽ��[��ע���¼�]��Сʱ�����ȡ�
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
    getMini();  //�ǵõ��ú������Ҵ�����10�������ơ�   //Html5��׼��4ms
```
## �塢������δ�����������? Ϊʲô?
```
 var a = 1;
    setTimeout(function(){
        a = 2;          //setTimeout��������δ���ŵ���һ��event loop���ж��Ƿ�ʱ�䡣
        console.log(a);
    }, 0);//���õ���0�������Ĭ����С��4����࣬�������ǿ�������2��1��3�������������
    var a ;  //���������Ч��
    console.log(a);   //��˳�����1��3
    a = 3;
    console.log(a);   //���1 3 2��
```
## ����������δ�����������? Ϊʲô?
```
var flag = true;
setTimeout(function () { //setTimeout��flag = true�ŵ���һ��event loopִ�С�
    flag = false;
}, 0);
while (flag){}  //���������flagһֱ��true ������ѭ����
console.log(flag);  //�ű������򲻿��ˡ� ԭ�����������whileѭ����
```
## �ߡ�������δ��������������delayer: 0, delayer:1...��ʹ�ñհ���ʵ�֣�
```
for(var i = 0; i < 5; i++){
        setTimeout(function () {
            console.log('delayer:' + i ); // �����5��delayer:5 ��Ϊ��δ��뱻�ŵ���Ϣ�������ִ�С�
        }, 0);
        console.log(i);  //���������0 1 2 3 4
    }
    //��� 0 1 2 3 4 ����5��delayer:5
```
ʹ�ñհ�ʵ��
```
//�ñհ�ʵ��delayer: 0, delayer:1...
    //����һ
    for(var i = 0; i < 5; ++i){
        (function (i) {    //֪ʶ��عˣ���������Ĳ���δ���룬�ñ�������undefined
            setTimeout(function () {
                console.log('delayer:' + i);
            }, 0);
        })(i);
    }

    //������(ͬѧ�����󵼣�������Ǳհ�����Ӵ)
    for(var i = 0; i < 5; ++i){
            setTimeout(function (i) {
                console.log('delayer:' + i);
            }, 0, i); //�������ֻ����ie9���ϰ汾
    }
```
---
[ά���ٿ�](https://zh.wikipedia.org/wiki/%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)#gcc.E5.AF.B9C.E8.AF.AD.E8.A8.80.E7.9A.84.E6.89.A9.E5.B1.95)
[������ʦ](https://www.zhihu.com/question/27712980/answer/37768023)
[��һ����ʦ](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
[������ʦ](https://zhuanlan.zhihu.com/p/22486908?refer=study-fe)
_���̳̰�Ȩ�鱾�˺ͼ��˹����У�ת����˵����Դ_

