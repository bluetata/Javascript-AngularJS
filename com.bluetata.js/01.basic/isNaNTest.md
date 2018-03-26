<html>
<head>
    <meta charset="utf-8">
</head>
<h3>Test isNaN</h3>
#### 什么是NaN?<br/>
NaN（Not a Number, 非数字）,NaN 属性的初始值就是 NaN, 和 Number.NaN 的值一样.NaN 属性是一个不可配置（non-configurable），不可写（non-writable）的属性。但在ES3中，这个属性的值是可以被更改的，但应该避免被覆盖。<br/>

**注意**: 虽然NaN本身名字为非数字,但是利用typeof来判断NaN的时候, 会返回number.

    document.write("Type of NAN : " + typeof NaN + "<br/>");    // number

<p>
####NaN的产生与isNaN的产生<br/>
**NaN**: 一般编码中很少会直接出现或使用NaN, 通常通常都是在计算失败时, 作为 Math 的某个方法的返回值出现的（例如：Math.sqrt(-1)）或者尝试将一个字符串**解析**成数字但失败了的时候（例如：parseInt("balabala")）.
**isNaN**: 与 JavaScript 中其他的值不同, NaN不能通过相等操作符（== 和 ===）来判断, 因为 NaN == NaN 和 NaN === NaN 都会返回 false. 因此, isNaN 就很有必要了.


最新的 ECMAScript 标准定义了 7 种JavaScript数据类型: 基本原始类型 boolean, null, undefined, number, string, symbol (ECMAScript 6 新定义) 和引用类型object
<br/>
注意typeof返回值可能和JavaScript定义的类型有所不同, 具体的返回类型有:undefined
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof
isNaN() 函数用于检查其参数是否是非数字值。
</p>

    <script>
    document.write("Type of NAN : " + typeof NaN + "<br/>");
    document.write("Type of NAN : " + typeof NaN + "<br/>");
    document.write("=============================== <br/>");
    document.write("isNaN(NaN) : " + isNaN(NaN) + "<br/>");
    document.write("isNaN(5/0) : " + isNaN(5/0) + "<br/>");          // Infinity
    document.write("isNaN(0/0) : " + isNaN(0/0) + "<br/>");          // NaN
    </script>
