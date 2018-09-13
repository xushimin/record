# new Array(length).map((_ , i)=>{i})函数没有执行的原因
```js
const arr = new Array(100).map((_ , i)=>{i});


```

没有执行map中的函数，首先 我们明确new Array(100) 返回一个怎么样的对象：
我们说数组其实也是对象
```js
const arr = ["a", "b", "c", "d"];

```
其实内部表示如下

```js
arr  = {
  0: "a",
  1: "b",
  2: "c",
  3: "d",
  length: 4
}
```
那么通过new的方式

```js
var myarr = new Array(4)

```

```js
//！！没有索引键值
myarr  = {
  length: 4
}

```

，高阶函数map，reduce，filter和forEach迭代数组对象的索引键从0到length，但只有在对象上存在键时才执行回调。 这就解释了为什么我们的回调从未被调用过，当我们在数组上调用map函数时没有任何反应 - 因为没有索引键！

##还有一个注意的点，对象有这个键值，存的是undefined以及没有这个键值，打印出来都是undefined

##解决方案
```js
const arr = [...Array(4)].map((_, i) => i);
console.log(arr[0]); //0

```
为什么上面这种方式生效了呢

```js
[...Array(5)]
{
  0: undefined,
  1: undefined,
  2: undefined,
  3: undefined,
  length: 4
}
```

主要原因是：
这是因为扩展运算符比map函数更简单。 它简单地遍历数组（或任何可迭代的）从0到length，并在数组中创建一个新的索引键，它的值从数组当前索引的值。 由于JavaScript在每个索引处从我们的数组中返回undefined（没有这个键值，返回的undefined)
