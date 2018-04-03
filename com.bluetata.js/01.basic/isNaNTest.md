
Test isNaN
==========
什么是NaN?
---------
NaN（Not a Number, 非数字）,NaN 属性的初始值就是 NaN, 和 Number.NaN 的值一样.NaN 属性是一个不可配置（non-configurable），不可写（non-writable）的属性。但在ES3中，这个属性的值是可以被更改的，但应该避免被覆盖。<br/>

**注意**: 虽然NaN本身名字为非数字,但是利用typeof来判断NaN的时候, 会返回number.

>document.write("Type of NAN : " + typeof NaN + "<br/>");    // number


NaN的产生与isNaN的产生
--------------------
**NaN**: 一般编码中很少会直接出现或使用NaN, 通常通常都是在计算失败时, 作为 Math 的某个方法的返回值出现的（例如：Math.sqrt(-1)）或者尝试将一个字符串**解析**成数字但失败了的时候（例如：parseInt("balabala")）.

**isNaN**: 与 JavaScript 中其他的值不同, NaN不能通过相等操作符（== 和 ===）来判断, 因为 NaN == NaN 和 NaN === NaN 都会返回 false. 因此, isNaN 的产生就很有必要了.

怎么理解NaN(NaN的本质)
--------------------
可以把isNaN看做：

```JavaScript
isNaN = function(value) {
    Number.isNaN(Number(value));
}```

如果isNaN函数的参数不是Number类型, isNaN()会首先尝试将这个参数转换为数值，然后才会对转换后的结果是否是NaN进行判断。因此，对于能被强制转换为有效的非NaN数值来说（ 值得一提的是，空字符串和布尔值会被强制转换为数值0或1），返回false值也许会让人感觉莫名其妙。比如说，空字符串就明显”不是数值“（not a number）。这种怪异行为起源于：“不是数值”（not a number）在基于IEEE-754数值的浮点计算体制中代表了一种特定的含义。isNaN函数其实等同于回答了这样一个问题：这个值被强制转换成数值时会不会返回IEEE-754​中所谓的”不是数值“（not a number）。

根据上述MDN的解释, 可以看出 `isNaN` 本身实际需要将参数转换成 `Number` 才可以判断其值是否为 NaN, 本身并没有能力判断一个值是否为 `NaN` ,所以可以利用 **NaN 本身不等于自身** 这一特性来判断(因为本身NaN不能通过===来判断相等)其变量`x`是否为`NaN`
```JavaScript
function reallyIsNaN(x){
    return x !== x
}
```
test
```JavaScript
var xVar;               // undefined
xVar !== xVar           // false

var xVar = "bluetata";
xVar !== xVar           // false

var xVar = NaN
xVar !== xVar           // true
```

一些特殊的NaN判断结果
-------------------
```JavaScript
isNaN(NaN);                 // true
isNaN(undefined);           // true
isNaN({});                  // true

isNaN(Infinity)             // false
isNaN(5/0)                  // false  5/0返回Infinity 认为是无限大数字
isNaN(0/0)                  // true   为NaN
isNaN("0/0")                // true   为NaN
isNaN(Infinity/Infinity);   // true

isNaN(true);                // false: true被转换成1 同样如果是'false'会被转换成0
isNaN(null);                // false  null被转换成0
isNaN(37);                  // false

// strings
isNaN("37");                // false: 可以被转换成数值37
isNaN("37.37");             // false: 可以被转换成数值37.37
isNaN("");                  // false: 空字符串被转换成0
isNaN(" ");                 // false: 包含空格的字符串被转换成0
isNaN("bluetata")           // true: "blabla"不能转换成数值

// dates
isNaN(new Date());                // false 时间会被转换成毫秒 + 1
isNaN(new Date().toString());     // true
```

**总结**:在上面举例中可以看到,在js中




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


bluetataX
