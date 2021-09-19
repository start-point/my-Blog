---
title: generator的简单用法
---

#### generator

介绍 => es6中新增的数据类型generator函数 需要在函数申明的时候在函数名字和function之间加上*号,yield可以去暂停函数的执行,可以执行多次.

1.generator基本用法

```js
function *generator(){
    console.log(1);
    yield; // 执行碰到yield 会去执行暂停
    console.log(2);
    yield;
    console.log(3);
    return 10;
}
const str = generator();
console.log(str.next())

// 打印结果:
// 1
// {value: undefined, done: false}
// value的值是yield后面所赋的值 done是一个布尔值代表的是有没有执行完
```

2.next()里面也可以传参

```js
function *generator(){
    console.log(1);
    const a1 = yield 5;
    console.log(a1);
    yield;
    console.log(3);
    return 10;
}
const str = generator();
console.log(str.next(111)) //第一次的传参111 是没有意义的,函数里也拿不到结果
console.log(str.next(222)) //第二次传参的结果才会在第一次yield结束后拿到
//执行结果:
//1
//{value: 5, done: false} //value为5 因为yield 后面赋值了为5
//222
//{value: undefined, done: false}
```



3.generator的迭代器委托

```js
function *generatorArry() {
    var arry = ['这','个','世','你','好'];
    var idx = 0;
    while(idx < arry.length) yield arry[idx++];
}
function *Iterator() {
    yield "我是被第一次执行...";
    // 这里可以暂停去执行generatorArry这个函数,当执行完了 done会为true 表示执行完毕
    yield *generatorArry();
}
var ite = Iterator();
console.log(ite.next().value);
console.log(ite.next().value);
console.log(ite.next().value);
console.log(ite.next().value);
// 运行结果:
// 我是被第一次执行...
// 这
// 个
// 世
```

