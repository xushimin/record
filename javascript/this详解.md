# this 详解
## 规则
1. 通过new 关键字调用函数，那么函数中的this是由JavaScript 引擎创建的全新的对象，
2. 通过apply，call，bind 去调用function， 在函数内部的this是调用时传的参数-对象，如果传的是null，那非严格模式下，默认是window
3. 如果函数是作为方法调用的，也就是通过.去调用函数，this是函数归属的那个对象，也就是点的左边的那个对象
4. 如果一个函数free function invocation，那么this就是全局对象，在浏览器中是window
5. 如果上面的多种方式一起使用，那么其中一个设置this这个值， 2vs3,规则2取胜,1vs3 1取胜

```js
  function Car(){
    console.log(this); // {}
    this.color = 'skyblue';
    console.log(this);// {color: skyblue}
  }

  new Car();
```


```js
var mycar = {
  color: 'skblue',
  price: 17

}

var yourcar = {
  color: 'red',
  price: 50

}

function getThis(){
  console.log(this);
}
getThis();//Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
getThis.apply(yourcar); // yourcar = {color: 'red',price: 50}
getThis.apply(mycar); // mycar = {color: 'skblue',price: 17}
getThis.call(yourcar); // yourcar = {color: 'red',price: 50}
getThis.call(mycar); // mycar = {color: 'skblue',price: 17}
var bindMyCarFun = getThis.bind(mycar);
var bindYourCarFun = getThis.bind(yourcar);
bindYourCarFun(); // yourcar = {color: 'red',price: 50}
bindMyCarFun(); // mycar = {color: 'skblue',price: 17}

```
tips: bindMyCarFun() 调用方式特别想规则5，但是this不是window，所以看到从词法上判断的this和实际情况不一样时，可能是因为bind处理过的函数

```js
var yourcar = {
  color: 'red',
  price: 50,
  getThis(){
    console.log(this);
  }
}

yourcar.getThis()//yourcar = {color: 'red',price: 50}

```

```js
function fn() {
    console.log(this);
}

// If called in browser:
fn(); // -> Window {stop: ƒ, open: ƒ, alert: ƒ, ...}
```

## 当多个规则同时应用时：2（bind,apply,call）+3(.) and 1(new)+3(.)

```js
var yourcar = {
  color: 'red',
  price: 50,
  getThis(){
    console.log(this);
  }
}

yourcar.getThis.apply(window)//Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```

```js
var yourcar = {
  color: 'red',
  price: 50,
  getThis: function(){
    this.instance = "myinstance"
    console.log(this);
  }
}

new yourcar.getThis()//getThis {instance: "myinstance"}
```

```js

function foo(something) {
   this.a = something;
}

var obj1 = {};

var bar = foo.bind( obj1 );

bar( 2 );
console.log( obj1.a ); // 2
var baz = new bar(3);
console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```

## 具体原理
1 this 是被自动定义在所有函数的作用域中
2 this是在运行时候进行绑定的，并不是在编写进行绑定的，所以this的取值取决于函数调用的方式

### 确定this的值
1 调用栈，栈中的第二个就是函数的调用位置
