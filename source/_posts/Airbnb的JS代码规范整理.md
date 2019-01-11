---
title: Airbnb的JS代码规范整理
date: 2019-01-11 09:09:52
tags: js规范
categories: 技术
---
## 为什么要重复写一些别人已经写好的东西？

1.每个团队都有一些自己的个性化的东西，不能完全照搬别个的。

2.整理的过程也是自己学习的一个过程，能让自己在无意识中规范自己的代码。

## 正文开始

### 定义变量方面

1.存取直接作用于它自身或它自身的引用

````javascript
var foo = [1, 2];
var bar = foo;
bar[0] = 9;
console.log(foo[0], bar[0]);
````

2.通过直接量创建类型且应避免使用保留字

补充：定义变量时最好用名词，定义方法时最好用动词

````javascript
var superman = { //避免使用new Object()
    defaults: { clark: 'kent' }, //避免使用default等保留字
    private: true
}
````

3.拷贝数据时最好用深拷贝，保证与原数据无关联

补充：可使用lodash的cloneDeep或者自己写的[h-toolbox](https://github.com/Qinhank/h-toolbox)

````javascript
import { cloneDeep } from 'h-toolbox'
let arr = [1,2,3]
let copyArr = cloneDeep(arr)
copyArr.push(4)
console.log(arr,copyArr)
````

4.不要在一个非函数代码块（if、while 等）中声明一个函数，浏览器允许你这么做，但它们的解析表现不一致，正确的做法是：在块外定义一个变量，然后将函数赋值给它。

````javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
var test;
if (currentUser) {
  test = function test() {
    console.log('Yup.');
  };
}
````

5.将未赋值的变量定义在最后面（可以引用前面的变量）

````javascript
var items = getItems();
var goSportsTeam = true;
var dragonball;
var length;
var i;
````

## 比较运算符 & 等号

在实际开发中有个问题其实很不起眼但是又很容易出错，那就是做判断的时候，因为js的弱类型，很多判断如果不精准的话可能会造成一些错误。如需要判断变量a是否存在，可能会很自然的写````a?true:false````，但是这样一来当a为0时可能也会被判断成false。

所以正确的写比较写判断是非常有必要且非常值得重视的。

先列一下js中一些常用的类型通过````ToBoolean````会变成什么吧：

- **对象** 被计算为 **true**
- **Undefined** 被计算为 **false**
- **Null** 被计算为 **false**
- **布尔值** 被计算为 **布尔的值**
- **数字** 如果是 **+0、-0 或 NaN** 被计算为 **false**，否则为 **true**
- **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

### 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`

### 判断是否存在一个长度大于0的数组

````javascript
if(arr && arr.length) {
    //something
}
````

## 注释

一般一个大型项目中，实际给每一个````function````做长注释是很累的，所以我更希望是这样一个原则：在业务中用单行注释简单说明，在公用函数中则必须把具体的信息全部做好注释。

### 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值

### 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行

## 代码规范

### 使用 2 个空格作为缩进（非常重要！！！非常重要！！！非常重要！！！）

### 在大括号前放一个空格

````javascript
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});
````

### 使用空格把运算符隔开

````javascript
// bad
var x=y+5;

// good
var x = y + 5;
````

### 使用链式调用时要进行缩进。使用前面的点 `.` 强调这是方法调用而不是新语句

````javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// good
var leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);
````

### 在块末和新语句前插入空行

````javascript
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;

// bad
var obj = {
  foo: function () {
  },
  bar: function () {
  }
};
return obj;

// good
var obj = {
  foo: function () {
  },

  bar: function () {
  }
};

return obj;
````

## 持续更新中...