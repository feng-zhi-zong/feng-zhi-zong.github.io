---
title: JS中的数学计算
date: 2016-10-03 10:41:17
tags: 
    - JavaScript
    - 计算
---

在数学中 0.1 + 0.2 等于 0.3。

```
0.1 + 0.2 === 0.3
```



但是在JS中这个是错误的
[![js小数点计算错误](http://yiluyanxia.site/e34e0359/js%E5%B0%8F%E6%95%B0%E7%82%B9%E8%AE%A1%E7%AE%97%E9%94%99%E8%AF%AF.png)](http://yiluyanxia.site/e34e0359/js小数点计算错误.png)
### why

JavaScript 里的数字的数据类型是 Number，整数和浮点数都是采用 IEEE 754 标准的 64 位双精度浮点数形式存储的。
JS在编译时会将 0.1 和 0.2 转换为二进制，在变为二进制时浮点小数时不精确的，浮点数用二进制表达时是无穷的，[IEEE 754 标准](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)的 64 位双精度浮点数的小数部分最多支持 53 位二进制位，所以它们相加后再转换为十进制结果为：0.30000000000000004。
[![计算结果](http://yiluyanxia.site/e34e0359/%E8%AE%A1%E7%AE%97%E7%BB%93%E6%9E%9C.png)](http://yiluyanxia.site/e34e0359/计算结果.png)

### 解决方式

#### 使用容差。

什么是容差？
在PS中，用来选择色彩颜色类似区域的魔棒工具有一个特别重要的属性叫容差，容差控制其近似程度，容差值越大，所选的区域越大，容差值越小，所选的区域越小。

*可以接受的最常见的做法是使用一个很小的“错误舍入”值作为比较的容差。这个很小的值经常被称为“机械极小值（machine epsilon）”，对于 JavaScript 来说这种 number 通常为 2^-52（2.220446049250313e-16）。*

ES6中，可以使用 Number.EPSILON，为了兼容前 ES6 的浏览器，可以使用以下方法简单实现这个功能。

```
if (!Number.EPSILON) {	Number.EPSILON = Math.pow(2,-52);}
```

现在可以使用 Number.EPSILON 来比较两个数是否相等。

```
function numbersCloseEnoughToEqual(n1,n2) {	return Math.abs( n1 - n2 ) < Number.EPSILON;}var a = 0.1 + 0.2;var b = 0.3;numbersCloseEnoughToEqual( a, b );					// truenumbersCloseEnoughToEqual( 0.0000001, 0.0000002 );	// false
```



#### 去掉小数点

这个方法有点牵强，属于人类战略性欺骗。但是只要达到目的，何乐而不为。

简单的小数点相加，思路就是将小数点去掉再进行计算，并将小数点的位数记下来。

```
// 来自网络上的大佬function add(x, y) {  const xLen = (x.toString().split('.')[1] || '').length;  const yLen = (y.toString().split('.')[1] || '').length;  const num = Math.pow(10, Math.max(xLen, yLen));  return (x * num + y * num) / num;}
```



简单的小数点相乘，同样也是去掉小数点。

```
// 来自网络上的大佬function multiplyDecimal = (m, n) => {  let len = 0  let mStr = m.toString()  let nStr = n.toString()  try {    len += mStr.split('.')[1].length  } catch (f) {}  try {    len += nStr.split('.')[1].length  } catch (f) {}  return Number(mStr.replace('.', '') * Number(nStr.replace('.', '')) / Math.pow(10, len))}
```



### 你以为整数就不计算错误了吗？

猜猜看 *(手动滑稽)*

```
console.log(20190511224538123);console.log(20190511224538123 === 20190511224538124);console.log(201905112245381234);
```



### Oop!
[![js整数问题](http://yiluyanxia.site/e34e0359/js%E6%95%B4%E6%95%B0%E9%97%AE%E9%A2%98.png)](http://yiluyanxia.site/e34e0359/js整数问题.png)

### why

究其原因则是[JS数字类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures#数字类型)

根据 ECMAScript 标准，JavaScript 中只有一种数字类型：基于 IEEE 754 标准的双精度 64 位二进制格式的值（-(2^63 -1) 到 2^63 -1）。而其中只有 +／- (2^53 -1) 才能正确的表示整数。
2^53 -1 = 9,007,199,254,740,991
在这个 +／- (2^53 -1) 范围以内，称之为**安全整数范围**。

ES6 中 Number.MAX_SAFE_INTEGER 常量表示在 JavaScript 中最大的安全整数（maxinum safe integer)（2^53 - 1）。
同样的 Number.MIN_SAFE_INTEGER 代表在 JavaScript中最小的安全的integer型数字 (-(2^53 - 1)).
