
Test isNaN
==========
什么是NaN?
---------
NaN（Not a Number, 非数字）, NaN 属性的初始值就是 NaN, 和 Number.NaN 的值一样 NaN 属性是一个不可配置（non-configurable）, 不可写（non-writable）的属性. 但在ES3中, 这个属性的值是可以被更改的, 但应该避免被覆盖.<br/>

**注意**: 虽然NaN本身名字为非数字,但是利用typeof来判断NaN的时候, 会返回number.

>document.write("Type of NAN : " + typeof NaN + "<br/>");    // number


NaN的产生与isNaN的产生
--------------------
**NaN的产生**: 一般编码中很少会直接出现或使用NaN, 通常通常都是在计算失败时, 作为 Math 的某个方法的返回值出现的（例如：Math.sqrt(-1)）或者尝试将一个字符串**解析**成数字但失败了的时候（例如：parseInt("bluetata")）; 另外, 任何涉及与NaN的计算, 返回结果都为NaN.

1. 计算失败时:

在对于未定义的运算(Infinity相关)或错误数学公式计算时(对负数进行开偶次方; 对负数进行对数运算; 对小于-1或大于+1的数进行反正弦或反余弦运算), 得到的计算结果为 NaN

```JavaScript
document.write(Math.sqrt(-1));      // 结果为 NaN  
document.write(0/0);                // 结果为 NaN  
document.write(5/0);                // 结果为 Infinity  
document.write(Infinity/Infinity)   // 结果为 NaN  
document.write(0/Infinity);         // 结果为 0  
document.write(0*Infinity);         // 结果为 NaN  
```

在进行对 加法(+) 以外的四则运算时, 即 对带有减号 (-) 乘号 (`*`) 或 除号 (/) 运算符时, JavaScript引擎会先尝试将待计算单项转换成Number类型(使用 Number(x) 做转换), 如果转换失败, 整个计算结果即为 NaN

```JavaScript
10 - '5a'           // NaN  
'5a' * 5            // NaN  
'10' / '5a';        // NaN  

undefined - 5;      // NaN  Number(undefined) == NaN  
'' * 5              // 0    Number('') == 0  
true * 5            // 5    Number(true) == 1  
[] * 5              // 0    Number([]) == 0  
null - 5;           // -5   Number(null) == 0  
```

对于四则运算 加法(+)时, JavaScript引擎不会对单项先进行Number强制转换, 如果加号(+)两端单项 有任意一个是字符串 js会进行拼接操作, 只有加号(+) 两端单项都是数字Number类型的时, 才会进行相加计算.  

```JavaScript
document.write(1 + 2 + '3'         + "<br/>");    // 计算结果为 33  
document.write(1 + '2' + 3         + "<br/>");    // 计算结果为 123  
document.write(1 + undefined + 3   + "<br/>");    // 计算结果为 NaN  
document.write(1 + '' + 3          + "<br/>");    // 计算结果为 13
```

[2]   解析数字失败时, 任何一个字符串当不能被 parseInt, parseFloat 或 Number 成功转换时, 就返回 NaN, 表示该字符串无法被识别为Number类型, 这是一个异常状态, 并不是一个确切的值 :
当被转换值中不包含任何数字时, 使用parseInt, parseFloat 或 Number进行转换时, 直接返回 NaN

```JavaScript
parseInt('bluetata')     // NaN  
parseFloat('bluetata')   // NaN  
Number('bluetata')       // NaN
```

当被转换值中含任何数字时, parseInt 和 parseFloat 会将被转换值中第一个无效字符之前的 数字字符串进行转换; Number 会将被转换值当做一个整体来转换, 这样对于Number()来说, 即使被转换值中包含数字, 也不会将数字转换出来.

```JavaScript
parseInt('123abc')         // 转换结果 123  
parseInt('123abc45')       // 转换结果 123  
parseInt('abc123def')      // 转换结果 NaN  
parseFloat('123.45abc')    // 转换结果 123.45  
parseFloat('123abc')       // 转换结果 123  
Number('123abc')           // 转换结果 NaN  
```

[3]   与 NaN 进行计算时:
任何涉及与NaN的计算, 返回结果既为NaN
```JavaScript
document.write("NaN === NaN     : " + (NaN === NaN)   + "<br/>");             // false  

document.write("isNaN(NaN + 10) : " + isNaN(NaN + 10) + "<br/>");             // true  
document.write("isNaN(NaN - 10) : " + isNaN(NaN - 10) + "<br/>");             // true  
document.write("isNaN(NaN * 10) : " + isNaN(NaN * 10) + "<br/>");             // true  
document.write("isNaN(0/NaN)    : " + isNaN(0/NaN)    + "<br/>");             // true  
document.write("isNaN(NaN/1)    : " + isNaN(NaN/1)    + "<br/>");             // true  
```


深入理解isNaN
isNaN:  NaN不能通过相等操作符（== 和 ===）来判断, 因为 NaN 不与任何值相等, 即使是NaN自己本身. 因此, 对于如何判断一个值是否为NaN, 函数isNaN 的产生就很有必要了.

isNaN的判断方法可以看做为：
--------------------
可以把isNaN看做：

```JavaScript
isNaN = function(value) {
    Number.isNaN(Number(value));
}```

如果isNaN函数的参数不是Number类型, isNaN函数会首先尝试将这个参数转换为数值, 然后才会对转换后的结果是否是NaN进行判断.
因此, 对于能被强制转换为有效的非NaN数值来说（空字符串和布尔值分别会被强制转换为数值0和1）, 返回false值也许会让人感觉莫名其妙. 比如说, 空字符串就明显"不是数值（not a number）". 这种怪异行为起源于："不是数值（not a number）"在基于IEEE-754数值的浮点计算体制中代表了一种特定的含义. isNaN函数其实等同于回答了这样一个问题：被测试的值在被强制转换成数值时会不会返回IEEE-754​中所谓的"不是数值（not a number）".

根据上述MDN的解释, 可以看出 `isNaN` 本身实际需要将参数转换成 `Number` 才可以判断其值是否为 NaN, 本身并没有能力判断一个值是否为 `NaN` ,所以可以利用 **NaN 本身不等于自身** 这一特性(因为本身NaN不能通过===来判断相等)来判断其变量`x`是否为`NaN`
```JavaScript
function reallyIsNaN(x){
    return x !== x
}
```

为了方便理解上述的判断方法, 以下举例说明了 其利用!==判断式的相关结果：
```JavaScript
var xVar;               // undefined
xVar !== xVar           // false

var xVar = "bluetata";
xVar !== xVar           // false

var xVar = NaN
xVar !== xVar           // true
```

在ECMAScript6之后, 判断一个值是否为NaN, 可以利用Number提供的isNaN进行判断
```JavaScript
Number.isNaN(x)
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
