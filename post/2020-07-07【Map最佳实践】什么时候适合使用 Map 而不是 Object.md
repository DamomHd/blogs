# 【Map最佳实践】什么时候适合使用 Map 而不是 Object

![](https://imgkr.cn-bj.ufileos.com/a541e3e7-8ef7-48be-a822-739959f2cf0c.png)

首先我们先有请**Map**简单介绍下自己
![](https://imgkr.cn-bj.ufileos.com/041c61c5-62ef-41bd-8b5d-89f079b3720a.jpg =60%x)
![](https://imgkr.cn-bj.ufileos.com/c714cf0c-67d8-4ab4-bc29-a859b54f28ee.png)
**Map**映射是一种经典的数据结构类型，其中数据以 **key/value** 的键值对形式存在

|        |                 Map                  |                                                               Object |
| :----- | :----------------------------------: | -------------------------------------------------------------------: |
| 默认值 | 默认不包含任何值，只包含显式插入的键 | 一个 Object 有一个原型，原型上的键名有可能和自己对象上设置的键名冲突 |
| 类型   |                 任意                 |                                                 _String_ 或 _Symbol_ |
| 长度   |     键值对个数通过 size 属性获取     |                                               键值对个数只能手动计算 |
| 性能   |    频繁增删键值对的场景下表现更好    |                               频繁添加和删除键值对的场景下未作出优化 |

## Map 基本用法

> 接受任何类型的键 划重点，是任何 any!!!

![](https://imgkr.cn-bj.ufileos.com/9321d796-b29d-4117-9b75-ff8d4472ba05.jpg =60%x)

```javascript
const testMap = new Map()

let str = '今天不学习',
    num = 666,
    keyFunction = function () {},
    keySymbol = Symbol('Web'),
    keyNull = null,
    keyUndefined = undefined,
    keyNaN = NaN
//添加键值对
//基本用法
testMap.set('key', 'value') // Map(1) {"key" => "value"}

testMap.set(str, '明天变辣鸡')
testMap.set(num, '前端Sneaker')
testMap.set(keyFunction, '你的函数写的好棒棒哦')
testMap.set(keySymbol, '大前端')
testMap.set(keyNull, '我是个Null')
testMap.set(keyUndefined, '我是个Undifined')
testMap.set(keyNaN, '我是个NaN')

testMap.get(function () {}) //undefined
testMap.get(Symbol('Web')) //undefined

//虽然NaN !== NaN 但是作为Map键名并无区别
testMap.get(NaN) //"我是个NaN"
testMap.get(Number('NaN')) //"我是个NaN"

```

> 除了*NaN*比较特殊外，其他**Map**的*get*方法都是通过对比键名是否相等（_===_）来获取，不相等则返回*undefined*

## 比较 Map 和 Object

### 定义

```javascript
//Map
const map = new Map();
map.set('key', 'value'); // Map(1) {"key" => "value"}
map.get('key'); // 'value'

//Object
const someObject = {};
someObject.key = 'value';
someObject.key; // 'value'
```

这里可以明显看出其实其定义行为是十分相似的，想必看到这里大家还没看出来**Map**到底在何时使用才是最佳实践，别急接着来。

### 键名类型

JavaScript **Object**只接收两种类型的键名 _String_ 和 _Symbol_，你可以使用其他类型的键名，但是最终 JavaScript 都会隐式转换为*字符串*

```javascript
const obj = {}
//直接看几种比较特殊的键名
obj[true] = 'Boolean'
obj[1] = 'Number'
obj[{'前端':'Sneaker'}] = '666'

Object.keys(obj) // ["1", "true", "[object Object]"]
```

再来看看 **Map** 的，其接收*任何类型*的键名并保留其键名类型 (此处简单举例，详细可看文章开头**Map**基本使用)

```javascript
const map = new Map();
map.set(1, 'value');
map.set(true, 'value');
map.set({'key': 'value'}, 'value');
for (const key of map.keys()) {
  console.log(key);
}
// 1
// true
// {key: "value"}

//除此之外，Map还支持正则作为键名
map.set(/^1[3456789]\d{9}$/,'手机号正则')
//Map(1) {/^1[3456789]\d{9}$/ => "手机号正则"}
```

> **Map**支持*正则表达式*作为键名，这在*Object*是不被允许的直接报错

### 原型 Prototype

**Object**不同于**Map**，它不仅仅是表面所看到的。**Map**只包含你所定义的键值对，但是**Object**对象具有其原型中的一些内置属性

```javascript
const newObject = {};
newObject.constructor; // ƒ Object() { [native code] }
```

如果操作不当没有正确遍历对象属性，可能会导致出现问题，产生你意料之外的 bug
![](https://imgkr.cn-bj.ufileos.com/7a3860ff-27f2-41ac-b8cb-c71202a2308d.jpg =60%x)
![](https://imgkr.cn-bj.ufileos.com/27ad3c65-3823-440b-97aa-de93d6b2dfd5.jpg =60%x)

```javascript
const countWords = (words) => {
  const counts = { };
  for (const word of words) {
    counts[word] = (counts[word] || 0) + 1;
  }
  return counts;
};
const counts = countWords(['constructor', 'creates', 'a', 'bug']);
// {constructor: "function Object() { [native code] }1", creates: 1, a: 1, bug: 1}
```

这个例子灵感来源于[《Effective TypeScript》](https://www.oreilly.com/library/view/effective-typescript/9781492053736/ "Effective TypeScript")一书

### 迭代器

**Map** 是可迭代的，可以直接进行迭代，例如*forEach*循环或者*for*..._of_...循环

```javascript
//forEach
const map = new Map();
map.set('key1', 'value1');
map.set('key2', 'value2');
map.set('key3', 'value3');
map.forEach((value, key) => {
  console.log(key, value);
});
// key1 value1
// key2 value2
// key3 value3

//for...of...
for(const entry of map) {
  console.log(entry);
}
// ["key1", "value1"]
// ["key2", "value2"]
// ["key3", "value3"]
```

但是对于**Object**是不能直接迭代的，当你尝试迭代将导致报错
![](https://imgkr.cn-bj.ufileos.com/12128aa8-6e25-4d1a-aac0-a677cd6f1a10.jpg =60%x)

```javascript
const object = {
  key1: 'value1',
  key2: 'value2',
  key3: 'value3',
};
for(const entry of object) {
  console.log(entry);
}
// Uncaught TypeError: object is not iterable
```

这时候你就需要一个额外的步骤来检索其键名、键值或者键值对

```javascript
for(const key of Object.keys(object)) {
  console.log(key);
}
// key1
// key2
// key3

for(const value of Object.values(object)) {
  console.log(value);
}
// value1
// value2
// value3

for(const entry of Object.entries(object)) {
  console.log(entry);
}
// ["key1", "value1"]
// ["key2", "value2"]
// ["key3", "value3"]

for(const [key,value] of Object.entries(object)) {
  console.log(key,value);
}
//"key1", "value1"
//"key2", "value2"
//"key3", "value3"
```

当然也可以使用*for*..._in_...进行遍历循环键名

```javascript
for(const key in object) {
  console.log(key);
}
// key1
// key2
// key3
```

### 元素顺序和长度

Map 保持对长度的跟踪，使其能够在*O(1)复杂度*中进行访问

```javascript
const map = new Map();
map.set('key1', 'value1');
map.set('key2', 'value2');
map.set('key3', 'value3');
map.size; // 3
```

而另一方面，对于**Object**而言，想要获得对象的属性长度，需要手动对其进行迭代，使其为*O(n)复杂度*，属性长度为*n*

在上文提及的示例中，我们可以看到**Map**始终保持按插入顺序返回键名。但**Object**却不是。从 ES6 开始，*String*和*Symbol*键是按顺序保存起来的，但是通过隐式转换保存成*String*的键就是乱序的

```javascript
const object = { };
object['key1'] = 'value1';
object['key0'] = 'value0';
object; // {key1: "value1", key0: "value0"}
object[20] = 'value20';
object; // {20: "value20", key1: "value1", key0: "value0"}

Object.keys(object).length; //3
```

## Object/Map 应用场景

如上就是 **Map** 和 **Object** 的基本区别，在解决问题考虑两者的时候就需要考虑两者的区别。

- 当*插入顺序*是你解决问题时需要考虑的，并且当前需要使用除 _String_ 和 _Symbol_ 以外的键名时，那么 **Map** 就是个最佳解决方案
- 如果需要*遍历键值对*（并且需要考虑顺序）,那我觉得还是需要优先考虑 **Map**。
- **Map**是一个纯哈希结构，而**Object**不是（它拥有自己的内部逻辑）。*Map* 在*频繁增删键值对*的场景下表现更好，性能更高。因此当你需要频繁操作数据的时候也可以优先考虑 *Map*
- 再举一个实际的例子，比如有一个自定义字段的用户操作功能，用户可以通过表单自定义字段，那么这时候最好是使用 Map，因为很有可能会*破坏原有的对象*

```javascript
const userCustomFields = {
  'color':    'blue',
  'size':     'medium',
  'toString': 'A blue box'
};
```

此时用户自定义的 _toString_ 就会破坏到原有的对象
而 **Map** 键名接受任何类型，没有影响

```javascript
function isMap(value) {
  return value.toString() === '[object Map]';
}

const actorMap = new Map();

actorMap.set('name', 'Harrison Ford');
actorMap.set('toString', 'Actor: Harrison Ford');

// Works!
isMap(actorMap); // => true
```

- 当你需要处理一些属性，那么 **Object** 是完全受用的，尤其是需要处理 JSON 数据的时候。由于 **Map** 可以是任意类型，因此没有可以将其转化为 JSON 的原生方法。

```javascript
var map = new Map()
map.set('key','value')
JSON.stringify(map)  //"{}"
```
- 如果需要在对象中保持独有的逻辑和属性，只能使用Object 
```javascript
var obj = {
    id: 1, 
    name: "前端Sneaker", 
    speak: function(){ 
        return `Object Id: ${this.id}, with Name: ${this.name}`;
    }
}
console.log(obj.speak());//Object Id: 1, with Name: 前端Sneaker.
```
- 当你需要通*正则表达式*判断去处理一些业务逻辑时，**Map**将是你的最佳解决方案

```javascript
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  const functionC = ()=>{/*send log*/}
  returnnewMap([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    [/^guest_.*$/,functionC],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```

利用数组循环的特性，符合正则条件的逻辑都会被执行，那就可以同时执行公共逻辑和单独逻辑，因为正则的存在，你可以打开想象力解锁更多的玩法,更多相关 Map 用法样例可以查看[JavaScript 复杂判断的更优雅写法](https://mp.weixin.qq.com/s/JkZZbWOesqWDVGkUh2lRvg)

## 总结：

**Object**对象通常可以很好的保存结构化数据，但是也有相应的局限性：

1. 键名接受类型只能用 _String_ 或者 _Symbol_
2. 自定义的键名容易与原型继承的属性*键名冲突*（例如 _toString_，_constructor_ 等）
3. 对象/正则无法用作键名
   而这些问题通过 **Map** 都可以解决，并且提供了诸如迭代器和易于进行大小查找之类的好处

> 不要将**Map**作为普通**Object**的替代品，而应该是普通对象的补充

## 参考

- 《Effective TypeScript》 Dan Vanderkam

![](https://imgkr.cn-bj.ufileos.com/04742fa4-bfb6-4db0-aaaf-4bd0a4ef99c4.png)

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map "MDN")

[dmitripavlutin](https://dmitripavlutin.com/maps-vs-plain-objects-javascript "dmitripavlutin")

[medium](https://medium.com/javascript-in-plain-english "medium")
