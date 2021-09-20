---
title: 工作上常用的数组方法
---

##### reduce()

1. reduce接收好几个参数 reduce(function(initValue,currentValue,currentIndex,currentArry),init)

   initValue => 初始值(上次一回调函数的返回值),由最后一个参数init 赋予

   currentValue => 当前所操作数组的值

   currentIndex => 当前所操作数组的下标

   currentArry => 当前数组

   init => 初始值,值是传递给initValue

   => 求和

```js
    const arry = [1,3,5,4,3,1,8,9,7,7];
    // 数字求和 每次计算的结果都会被赋到init上面
    const arrList = arry.reduce((init,currentValue)=>{
        return init + currentValue
    // 初始值为0 则 init一开始的数据就为0
    },0)
    console.log(arrList) // 48
```

​		=> 求每一项出现的次数

```js
    const arry = [1,3,5,4,3,1,8,9,7,7];
    const arrList = arry.reduce((init,currentValue)=>{
        if(currentValue in init){
            init[currentValue] ++;
        }else{
            init[currentValue] = 1;
        }
        return init;
    // 初始值设置为{} 因为返回出的结果 我期望是一个对象 来展现这样一个数据
    },{})
    // console.log(arrList); 
    // 打印结果
    // {
    // 1: 2,
    // 3: 2,
    // 4: 1,
    // 5: 1,
    // 7: 2,
    // 8: 1
    // }
```

​	=> 数组去重

```js
    const arry = [1,3,5,4,3,1,8,9,7,7];
    const arrList = arry.reduce((init,currentValue)=>{
        //includes 查找每一项有没有 有的返回true 没有则flase
        return init.includes(currentValue) ? init : init.concat(currentValue);
    },[])
    console.log(arrList); 
```



##### includes()

1. includes 接收俩个参数 includes(value,index)

   value =>  查找的数据

   index => 开始查找的下标(如果是负数则从末尾还是向右找)

```js
    const arry = [1,3,7,8];
    // 查找数组里面是否有3 有true 无flase;
    const bool = arry.includes(3);
    console.log(bool); //true
```

```js
    const arry = [1,3,7,8];
    // 查找数组里面是否有3 有true 无flase;
	// -2的位置是7 向右边找3 找不到 则flase
    const bool = arry.includes(3,-2);
    console.log(bool); //flase

    const arry = [1,3,7,8];
    // 查找数组里面是否有3 有true 无flase;
	// -2的位置是3 向右边找3 找到了 则true
    const bool = arry.includes(3,-3);
    console.log(bool); //true
```

##### map()

```js
    const arry = [2,3,7,8];
    const num = arry.map((v,i)=>{
        // v 是数组的每一项 i每一项数组的下标
        // 并且会返回一个新的数组
        return v;
    })
    console.log(num) // [2,3,7,8]
```

##### filter()

```js
    const arry = [2,3,7,8];
    const num = arry.filter((v,i)=>{
        // v 是数组的每一项 i每一项数组的下标
        // 过滤出大于3 返回新的数组
        return v > 3;
    })
    console.log(num) //[7,8]

    const arry = [2,3,7,8];
    const num = arry.filter((v,i)=>{
        // v 是数组的每一项 i每一项数组的下标
        // 过滤出不等于3 返回新的数组
        return v != 3;
    })
    console.log(num) //[2,7,8]
```



##### fill()

1. fill 是替换掉原数组的内容,会改变原数组

2. fill接收三个参数 fill(value,start,end)

   value => 替换的内容

   start => 开始的数组下标(位置)

   end => 结束的数组下边(位置)

```js
    const arry = [1,3,7,8];
    // 替换的内容 66 1是开始的数组元素下标 3是结束的数组元素下标
	// 如果不写开始下标开始位置和结束位置 则默认替换掉数组所有内容
	// 会改变原数组
    const fillArry = arry.fill(66,1,3);
    console.log(fillArry); //[1, 66, 66, 8]
```



##### find()

1. find始查找满足条件的第一个元素

2. find(function(currentValue, index, arr))

   => currentValue 当前值

   => index 数组元素下边下标

   => arr 当前值所属数组

```js
    // 找到了返回满足条件的第一个值
	const arry = [1,3,7,8];
    const findArry = arry.find((value,index,arr)=>value > 3);
    console.log(findArry); // 7
	// 找不到则返回 undefined
    const arry = [1,3,7,8];
    const findArry = arry.find((value,index,arr)=>value > 8);
    console.log(findArry); // undefined
```





##### findIndex()

1. findIndex是查找满足条件的第一个元素的位置

2. findIndex(function(currentValue, index, arr))

   => currentValue 当前值

   => index 数组元素下边下标

   => arr 当前值所属数组

```js
    // 找到了返回满足条件的第一个值的下标
	const arry = [1,3,7,8];
    const findArry = arry.findIndex((value,index,arr)=>value > 3);
    console.log(findArry); // 2

    // 找不到则返回 -1
	const arry = [1,3,7,8];
    const findArry = arry.findIndex((value,index,arr)=>value > 3);
    console.log(findArry); // -1
```



##### push()

```
    const arry = [2,3,7,8];
    const arr = [];
    for(let i = 0 ;i<arry.length;i++){
    // 将数组arry里面的每一项都添加到新数组里面
        arr.push(arry[i])
    }
    console.log(arr) // [2,3,7,8]
```



##### concat()

```
    const arry = [2,3,7,8];
    const num =  [4,5,6,9,8,7];
    // concat 可以将俩个数组 连接起来
    const arNum = arry.concat(num)
    console.log(arNum) // [2, 3, 7, 8, 4, 5, 6, 9, 8, 7]
```









##### 





