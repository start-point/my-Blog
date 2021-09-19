---
title: 实现 pow(x, n)
---

实现 pow(x, n) ，即计算 x 的 n 次幂函数（即，xn）。

示例 1：

```js
输入：x = 2.00000, n = 10
输出：1024.00000
```

示例 2：

```js
输入：x = 2.10000, n = 3
输出：9.26100
```

示例 3：

```js
输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

用的一个递归方式分为四种情况 等于0 小于0 奇次幂和偶次幂

```js
var myPow = function (x, n) {
    // 当n为0次幂时 直接返回0
    if (n === 0) return 1 
    // 当n<0时候 x的n次方 1/x 的 -n 次方 
    if (n < 0) {
        return 1 / myPow(x, -n)
    }
    // 当n为奇次幂时 x的n次方就等于 x*x的n-1次方
    if (n % 2) {    
        return x * myPow(x, n - 1)
    }
    // 当n为偶次幂时 就等于x*x的n/2次方 
    return myPow(x * x, n / 2)
}
```



当然你要是觉得这么写很麻烦你也可以这么写

```js
var myPow = function (x, n) {
    return Math.pow(x,n);
}
```

还可以这么写

```js
var myPow = function (x, n) {
    return x ** n;
}
```

