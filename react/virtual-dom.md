1 平时书写的jsx是纯javascript 代码
参考： https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060
比如：
<ul className="list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>

babel  将其编译成：

```js
var h = Reate.createElement

h('ul',{class: "list"},[
  h('ul',{},['1']),
  h('ul',{},['2']),
  h('ul',{},['3']),
 ]
)

Reate.createElement 主要作用是创建对象，描述真实的dom结构的对象

function h(type, props, …children) {
  return { type, props, children };
}

```

2 react中渲染组件有-组件初次渲染/status or props 改变，组件随之渲染

初次渲染： 利用我们上面生成的纯js对象，对应的组件dom tree


更新后渲染：diff 对比oldtree 和 newtree 两个js对象的difference，由于 old tree 的结构跟真实dom的结构是一样的（树），而difference是基于oldtree遍历对比的，所以difference应用在dom tree，进行相应的操作

3 如何根据以上生成的js对象生成dom树

1）根据节点类型--createTextNode or  createElement
2）props setAtrr
3) children 递归处理
4）插入到真实的dom父节点
```js
function createElement(node) {
  if (typeof node === ‘string’) {
    return document.createTextNode(node);
  }
  return document.createElement(node.type);
}
```

4 diff算法的原理
diff算法参考： https://github.com/livoras/blog/issues/13
status和props变化的时候 ==》 生成一个新的js对象，将其和之前的js对象进行对比

1）关键，根据ui特性：同层对比, 如果节点不同直接替换
2）每个节点比对 {type:  REPLACE  || REORDER || PROPS || TEXT }
  节点type不同： REPLACE
  子节点顺序改变：REORDER
  props改变：PROPS
  文本节点改变：TEXT



5 diff算法patch如果应用到dom上
diff算法参考： https://github.com/livoras/blog/issues/13

遍历dom树，获取每个patch的type和数据进行相应的操作

2）每个节点比对 {type:  REPLACE  || REORDER || PROPS || TEXT }

 ```js
  appendChild || removeChild || replaceChild
 ```


 整个过程： https://medium.com/@rajaraodv/the-inner-workings-of-virtual-dom-666ee7ad47cf
