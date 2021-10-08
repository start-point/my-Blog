---
title: ✨重复DNA序列
tags: [javascript,算法]
cover: /img/pic/lvxing.png
top_img: /img/pic/bg5.jpg
categories: javascript
comments: true
date: 2021-10-08 10:00:00
updated: 2021-10-08 10:00:00
---

重复DNA序列

难度中等228收藏分享切换为英文接收动态反馈

所有 DNA 都由一系列缩写为 `'A'`，`'C'`，`'G'` 和 `'T'` 的核苷酸组成，例如：`"ACGAATTCCG"`。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来找出所有目标子串，目标子串的长度为 10，且在 DNA 字符串 `s` 中出现次数超过一次。

**示例 1：**

```js
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```

**示例 2：**

```js
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```

思路：

循环截取长度为10的字符串存储起来，判断其出现的次数 出现俩次的 就是输出结果

```js
/**
 * @param {string} s
 * @return {string[]}
 */
var findRepeatedDnaSequences = function(s) {
    const map = new Map();
    // 用来存储结果 重复的结果
    const arr = [];
    for(let i = 0;i<s.length;i++){
        // 截取 目标长度
        const str = s.slice(i,i+10);
        // 判断之前是否存储过str  用来记录次数
        const m = map.get(str) || 0;
        map.set(str,m + 1);
        // 如果为2 就是出现过2次 取出这个结果即可
        if(map.get(str) === 2){
            arr.push(str);
        }
    }
    return arr;
};
```

