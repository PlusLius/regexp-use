## 正则表达式

```js

//用法
1. /\d/g
2. new RegExp('/\d/','g')

//修饰符
g,i,m

//元字符
1.元字符是在正则表达式中有特殊含义的非字母字符
. * + ? $ ^ | \ () {} []
\t,\v,\n,\r,\0,\f,\cX

//字符类
1.元字符[]来构建一个简单的类
2.元字符^创建反向类
[abc] , [^abc]

//范围类
1.[a-z],[1-9]

//预定义类
.  [^\r\n]
\d [0-9]
\D [^0-9]
\s [\t\n\xOB\f\r]
\S [^\t\n\xOB\f\r]
\w [a-zA-Z_0-9]
\W [^a-zA-Z_0-9]

//边界
^ $ \b \B

"
@23
@45
@6
".replace(/^@\d/gm,'X')

//量词
? 零次或一次
+ 一次或多次
* 零次或多次
{n} 出现n次
{n,m} 出现m到n次
{n,} 至少出现n次

//贪婪模式
'12345678'.replace(/\d{3,6}/g,'X')
//匹配最大次

//非贪婪模式
在量词后面加?
'12345678'。replace(/\d{3,6}?/g,'X')
//匹配最少次


//分组
()可以达到分组的功能，使量词作用于分组

'a1b2c3d4'.replace(/([a-z]\d){3}/g,'X')

//反向引用
'2015-12-25'.replace(/(\d{4})-(\d{2})-(\d{2})/g,'$1$2$3')

//前瞻
正向前瞻exp(?=assert)
负向前瞻exp(?!assert)

'a2*3'.replace(/\w(?=\d)/g,'x')

//对象属性
//.test()
var reg2 = /\w/g
reg2.test('ab')
reg2.lastIndex 从上一次的记录开始匹配

//.exec()
// 没有加g的调用每次从第一次开始搜素，否则从上一次，返回一个结果数组[匹配项，匹配子项]
var reg3 = /\d(\w)\d/;
var ts = '1a2b3c4d5e'
var ret = reg3.exec(ts)

console.log(reg3.lastIndex + 
'\t' + ret.index + '\t' + ret.toStirng())


//string
'a1b2'.search(/1/g,'a')
var reg3 = /\d(\w)\d/;
var ts = '1a2b3c4d5e'
var ret = ts.match(reg3)

console.log(reg3.lastIndex + 
'\t' + ret.index + '\t' + ret.toStirng())

'a,b,c,d'.split(',')

'a1b1c1'.replace(//,(匹配字符串，分组，匹配项索引，原字符串)=>{
    
})

```

## g模式

```js
var str = 'hello world hello life'
reg1 = /hello/
reg2 = /hello/g

//match非全局模式下返回一个数组
/*
["hello", index: 0, input: "hello world, hello life", groups: undefined]

0: "hello" //匹配的字符
groups: undefined
index: 0 //匹配字符的起始位置
input: "hello world, hello life" //引用的文本
length: 1
*/
str.match(reg1)
//全局模式下返回匹配结果的数组
/*
(2) ["hello", "hello"]
0: "hello"
1: "hello"
length: 2
*/
str.match(reg2)


```

## m模式

```js
m模式

// multiline
var mStr = 'hello world\n hello life',
    mReg1 = /world$/,
    mReg2 = /world$/m;

// 结果为null，因为没有启用多行，world并不算mStr的结尾
mStr.match(mReg1);  // null
// 匹配成功，启用多行之后，world变成第一行的结尾
mStr.match(mReg2);  // [ 'world', index: 6, input: 'hello world\n he
```

## i模式

```js
var cStr = 'Hello world',
    cReg1 = /hello/,
    cReg2 = /hello/i;
    
cReg1.exec(cStr);   // null
cReg2.exec(cStr);   // [ 'Hello', index: 0, input: 'Hello world' ]

```

## RegExp实例方法

### exec方法
```js

exec主要用来提取捕获组，接收一个字符作为参数，如果匹配成功，返回一个匹配项信息的数组，在没有匹配到的时候返回null

数组中包含2个属性，index和input，index表示匹配字符串在文本的起始位置，input表示匹配的字符串

在非全局模式下，如果字符串中含有与模式匹配的多个子字符串，那么只会返回第一个匹配项的结果，返回的数组中下标为0的位置表示匹配到的字符串，其余位置表示匹配到的捕获组信息，exec方法，依次返回的是每一个匹配项信息的数组

// 在全局模式匹配下，找到第一个匹配项信息之后，如果继续执行，会在字符串中继续查找下一个匹配项

var str = `<div class="hello"><b>hello</b><i>regexp</i></div>`

var reg = /<(\/?)(\w+)([^>]*?)>/

var matches = reg.exec(str)

console.log(
    matches[0],
    matches[1],
    matches[2],
    matches[3],//分组的匹配结果
    matches.index,//从第几个字符开始匹配
    reg.lastIndex//reg记录最后一次匹配的位置
)

//与上一次匹配结果一致
var matches = reg.exec(str)

console.log(
    matches[0],
    matches[1],
    matches[2],
    matches[3],
    matches.index,
    reg.lastIndex
)

//加上全局模式后再次执行exec会从上次记录的位置开始匹配
var reg = /<(\/?)(\w+)([^>]*?)>/g

var matches = reg.exec(str)

console.log(
    matches[0],
    matches[1],
    matches[2],
    matches[3],
    matches.index,
    reg.lastIndex
)

var matches = reg.exec(str)

console.log(
    matches[0],
    matches[1],
    matches[2],
    matches[3],
    matches.index,
    reg.lastIndex
)



```

### test方法

```js
//检测字符串中是否有与模式匹配的字符
var pattern = /\d{2}-\d{3}/,
    str = '29-234',
    str1 = '290-3345',
    str2 = '1-33245';
pattern.test(str);  // true
pattern.test(str1); // true
pattern.test(str2); // false
```
## RegExp构造函数属性

```js
构造函数静态属性，分别有一个长属性名和一个短属性名(Opera是例外，不支持短属性名)。

长属性名	短属性名	说明
input	$_	最近一次要匹配的字符串
lastMatch	$&	最近一次的匹配项
lastParen	$+	最近一次匹配的捕获组
leftContext	$`	input字符串中lastMatch之前的文本
multiline	$*	布尔值，表示是否所有表达式都使用多行模式
rightContext	$'	input字符串中lastMatch之后的文本

var pattern = /(.)or/,
    str = 'this is a normal string or meaningful';

if (pattern.test(str)) {
    console.log(RegExp.input);          // this is a normal string or meaningful
    console.log(RegExp.lastMatch);      // nor
    console.log(RegExp.lastParen);      // n
    console.log(RegExp.leftContext);    // this is a
    console.log(RegExp.multiline);      // false
    console.log(RegExp.rightContext);   // mal string or meaningful

    console.log(RegExp['$_']);          // this is a normal string or meaningful
    console.log(RegExp['$&']);          // nor
    console.log(RegExp['$+']);          // n
    console.log(RegExp['$`']);          // this is a
    console.log(RegExp['$*']);          // false
    console.log(RegExp['$\'']);         // mal string or meaningful
}

```

## 用于模式匹配的string方法


### str.search

```js
//传入一个正则表达式，如果不是会转成正则表达式，返回匹配的位置，不支持全局g模式
var str = 'JavaScript is not java',
    pattern = /Java/i,
    pattern2 = /Java/ig;

console.log(str.search(pattern))
console.log(str.search(pattern2))
```

### str.replace

```js
//参数是一个正则表达式，如果参数不是正则表达式不会进行转换，而是进行字符串匹配
var str = 'JavaScript is not java',
    pattern = /Java/i;

console.log(str.replace(pattern, 'live'));     // liveScript is not java
console.log(str.replace('Java', 'live'));      // liveScript is n


//第二个参数是函数会拿到匹配结果
// function
var str2 = '123-456-789',
    pattern2 = /(\d+)/g;

str2.replace(pattern2, function(v) {
    // 会执行三次
    // 第一次打印123
    // 第二次打印456
    // 第三次打印789
    console.log(v);
});

/**
 * 第一个参数表示匹配的字符串
 * 第二个参数表示匹配的元组(如果没有元组,则实际上回调函数只有三个值,即此时的d值会为undefined)
 * 第三个参数表示字符串匹配的开始索引
 * 第四个参数表示匹配的字符串
 */
str2.replace(pattern2, function(a, b, c, d) {
    // 会执行三次
    // 第一次打印123, 123, 0, 123-456-789
    // 第二次打印456, 456, 4, 123-456-789
    // 第三次打印789, 789, 8, 123-456-789
    console.log(a, b, c, d);
});

// 此时没有进行元组匹配
str2.replace(/\d+/g, function(a, b, c, d) {
    // 会执行三次
    // 第一次打印123, 0, 123-456-789, undefined
    // 第二次打印456, 4, 123-456-789, undefined
    // 第三次打印789, 8, 123-456-789, undefined
    console.log(a, b, c, d);
});
```

### str.match

```js
//全局模式下返回匹配数组，反之第一个是匹配结果，后面的是捕获结果
var str3 = 'yuzhongzi_91@sina.com',
    pattern3 = /^([a-zA-Z][\w\d]+)@([a-zA-Z0-9]+)(\.([a-zA-Z]+))+$/,
    matches;

matches = str3.match(pattern3);
console.log(matches[0]);        // yuzhongzi_91@sina.com
console.log(matches[1]);        // yuzhongzi_91
console.log(matches[2]);        // sina
console.log(matches[3]);        // .com
console.log(matches[4]);        // com
console.log(matches.index);     // 0
console.log(matches.input);     // yuzhongzi_91@sina.com
```

## str.split

```js
//第一个参数是分割模版，第二个参数是长度
var str4 = 'Hope left life become a cat.',
    pat1 = /\s/,
    pat2 = ' ';

console.log(str4.split(pat1));      // [ 'Hope', 'left', 'life', 'become', 'a', 'cat.' ]
console.log(str4.split(pat2),2);   // [ 'Hope', 'left' ]
```

## 匹配

### 精准匹配

```js

字符	匹配内容
[...]	方括号内的任意字符
[^...]	不在方括号内的任意字符
·	除换行符和其他Unicode行终止符之外的任意字符
\w	任意ASCII字符组成的单词，等价于[a-zA-Z0-9_]
\W	任意不是ASCII字符组成的单词，等价于[^a-zA-Z0-9_]
\s	任何Unicode空白符
\S	任何非Unicode空白符的字符，注意w和S不同
\d	任何ASCII数字，等价于[0-9]
\D	除了ASCII数字之外的任何字符，等价于[^0-9]
\b	单词边界
\B	非单词边界
\t	水平制表符
\v	垂直制表符
\f	换页符
\r	回车
\n	换行符
\cA:\cZ	控制符，例如：\cM匹配一个Control-M
\u0000:\uFFFF	十六进制Unicode码
\x00:\xFF	十六进制ASCII码

```

```js
// [abc] 表示匹配abc其中的一个
var str = 'abc',
    pattern = /[abc]/;

console.log(str.match(pattern));    // [ 'a', index: 0, input: 'abc' ]

var str = 'abcd',
    pattern = /[^abc]/;

console.log(str.match(pattern));    // [ 'd', index: 3, input: 'abcd' ]


// · 匹配除换行以外的字符
var str = '<p>I am a cat.</p>\n<b>what about you?</b>',
    pattern = /.*/;

console.log(str.match(pattern));    // [ '<p>I am a cat.</p>', index: 0, input: '<p>I am a cat.</p>\n<b>what about you?</b>' ]

// \w 匹配包括下划线的任意单词字符
var str = 'Your are _ so cute #*& so | beatiful ',
    pattern = /\w+/g;

console.log(str.match(pattern));   // [ 'Your', 'are', '_', 'so', 'cute', 'so', 'beatiful' ]


// \W 匹配任意非单词字符
var str = 'Your are _ so cute #*& so | beatiful ',
    pattern = /\W+/g;

console.log(str.match(pattern));    // [ ' ', ' ', ' ', ' ', ' #*& ', ' | ', ' ' ]

// \s 匹配空白字符
var str = 'Your \r\n\f ful',
    pattern = /\s/g;

console.log(str.match(pattern));    // [ ' ', '\r', '\n', '\f', ' ' ]

console.log(str.match(pattern));    // [ 'Y', 'o', 'u', 'r', 'f', 'u', 'l' ]

// \d 匹配任意数字
var str = '123helloworld34javascript',
    pattern = /\d+/g;

console.log(str.match(pattern));    // [ '123', '34' ]

// \D 匹配任意非数字
var str = '123helloworld34javascript',
    pattern = /\D+/g;

console.log(str.match(pattern));    // [ 'helloworld', 'javascript' ]

// \b 匹配单词边界
var str = 'I never come back',
    pattern = /e\b/g;

if (pattern.test(str)) {
    console.log(RegExp['$&']);  // e
    console.log(RegExp['$`']);  // I never com
}

// \B 匹配非单词边界
var str = 'I never come back',
    pattern = /e\B/g;

if (pattern.test(str)) {
    console.log(RegExp['$&']);  // e
    console.log(RegExp['$`']);  // I n
}

// \n 匹配换行符
var str = 'hello \n world',
    pattern = /\n/;

console.log(str.match(pattern));  // [ '\n', index: 6, input: 'hello \n world' ]

// \u0000:\uFFFF 匹配十六进制Unicode字符
var str = 'Visit W3School. Hello World!',
    pattern = /\u0057/g;

console.log(str.match(pattern));    // [ 'W', 'W' ]

// \x00:\xFF 匹配十六进制ASCII字符
var str = 'Visit W3School. Hello World!',
    pattern = /\x57/g;

console.log(str.match(pattern));    // [ 'W', 'W' ]
```

### 转义

```js
与其他语言中的正则表达式类似，模式中使用的所有元字符都必须转义。正则表达式中的元字符包括：

( [ { \ ^ $ | ? * + . } ] )

由于RegExp构造函数的模式参数是字符串，所以在某些情况下需要双重转义。例如：

字面量模式	等价的字符串
/\[abc\]/	"\\[abc\\]"
/\w{2, 3}/	"\\w{2, 3}"

```

```js

var reg1 = /\[abc\]/,
    reg2 = new RegExp("\\[abc\\]"),
    str = '[abc]';

console.log(str.match(reg1));   // [ '[abc]', index: 0, input: '[abc]' ]
console.log(str.match(reg2));   // [ '[abc]', index: 0, input: '[abc]' ]
console.log(reg1.source);       // \[abc\]
console.log(reg2.source);       // \[abc\]

```

### 匹配开始和结尾

```js
匹配一个字符串的开始使用符号(^)，例如: /^java/表示匹配已"java"开头的字符串
匹配一个字符串的结尾使用符号($)，例如: /script$/表示匹配已"script"结尾的字符串
如果一个正则表达式中即出现了(^)又出现了($)，表示必须匹配整个候选字符串，例如：/^javaScript$/表示匹配整个"javaScript"字符串

var str = 'javaScript is fun',
    str2 = 'JavaScript',
    reg1 = /^java/,
    reg2 = /fun$/i,
    reg3 = /^javascript$/i;

console.log(reg1.test(str));    // true
console.log(reg2.test(str));    // true
console.log(reg3.test(str));    // false
console.log(reg3.test(str2));   // true
```

### 重复

```js

贪婪重复
要连续匹配四个'a'，我们可以使用/aaaa/来进行匹配，但如果匹配的不止是四个，而是十几个呢？想必是不方便的，在重复匹配的选项上，正则表达式提供了很多方式。

贪婪匹配 贪婪匹配表示尽可能多的匹配，例如/a+/表示至少匹配一个"a"字符，即表示有多少个"a"就会匹配多少个，例如：

var str = 'aaaaa',
    str1 = 'aaaaaaaaaa',
    reg = /a+/;

console.log(reg.exec(str));     // [ 'aaaaa', index: 0, input: 'aaaaa' ]
console.log(reg.exec(str1));    // [ 'aaaaaaaaaa', index: 0, input: 'aaaaaaaaaa' ]
```

```js
在贪婪重复上，正则表达式主要有以下方式：
字符	含义
{x,y}	匹配至少x次，最多y次
{x,}	匹配至少x次
{x}	匹配x次
?	匹配0次或者1次, { 0, 1 }
+	匹配至少1次, { 1, }
*	匹配0次或者多次, { 0, }

// {2,3} 匹配至少2次，最多3次
var str = '.abc.def.xyz.com',
    reg = /(.\w+){2,3}/;

console.log(str.match(reg));    // [ '.abc.def.xyz', '.xyz', index: 0, input: '.abc.def.xyz.com' ]

// {2,} 匹配至少2次
var str = '.abc.def.xyz.com',
    reg = /(.\w+){2,}/;

console.log(str.match(reg));    // [ '.abc.def.xyz.com', '.com', index: 0, input: '.abc.def.xyz.com' 

// {2} 匹配2次
var str = '.abc.def.xyz.com',
    reg = /(.\w+){2}/;

console.log(str.match(reg));    // [ '.abc.def', '.def', index: 0, input: '.abc.def.xyz.com' ]

// ? 匹配0次或者1次
var str = '.abc//.def.xyz.com',
    reg = /(.\w+\/?)?/;

console.log(str.match(reg));    // [ '.abc/', '.abc/', index: 0, input: '.abc//.def.xyz.com' ]

// * 匹配0次或者多次
var str = '.abc//.def.xyz.com',
    reg = /(.\w+\/*)*/;

console.log(str.match(reg));    // [ '.abc//.def.xyz.com', '.com', index: 0, input: '.abc//.def.xyz.com' ]

```

```js
非贪婪重复
非贪婪模式就是在贪婪模式后面加上一个"?"，表示尽尽可能少的匹配，例如：

{x,y}?, {x,}?, {x}?, ??, +? *?
```

```js
var str = 'aaaa',
    str1 = '<p>Hello</p><p>Javascript</p>',
    reg = /a+?/,
    reg2 = /(<p>[^<]*<\/p>)+/,
    reg3 = /(<p>[^<]*<\/p>)+?/;

console.log(reg.exec(str));     // [ 'a', index: 0, input: 'aaaa' ]
console.log(str1.match(reg2));  // [ '<p>Hello</p><p>Javascript</p>', '<p>Javascript</p>', index: 0, input: '<p>Hello</p><p>Javascript</p>' ]
console.log(str1.match(reg3));  // [ '<p>Hello</p>', '<p>Hello</p>', index: 0, input: '<p>Hello</p><p>Javascript</p>' ]

```

### 分组

```js
使用()表示分组，表示匹配一类字符串，例如：/(\w\s)+/表示匹配一个或者多个以字母和空格组合出现的字符串

var str = 'a b c',
    reg = /(\w\s)+/;

console.log(str.match(reg));    // [ 'a b ', 'b ', index: 0, input: 'a b c' ]
```

### 捕获组

```js
当用于模式匹配之后，匹配到的元组就被成为捕获组。捕获组对应着括号的数量，分别使用$1,$2...$99...$x表示匹配到的捕获组，另外可以使用\1,\2...\99...\x表示引用捕获组。

var str = '20170809',
    str2 = '20170808',
    reg = /(\d{4})(\d{2})(\d{2})/,
    reg2 = /(\d{4})(\d{2})\2/;

console.log(str.replace(reg, '$1-$2-$3'));  // 2017-08-09
console.log(str.replace(/a/, '$1/$2'));     // 20170809
console.log(str.replace(reg2, '$1/$2'));    // 20170809
console.log(str2.replace(reg2, '$1/$2'));   // 2017/08
可以看到第三个打印与第四个打印之间的差异，原因是因为第三个没有正则匹配项。

\x表示引用，引用的是具体的匹配字符串，也就是说上面例子中的\2引用的是第二个捕获组中的内容，其实应该对应的是"08"字符串，因此"20170808"当然与"20170809"字符串不匹配；反证可以看第四个匹配，验证了上面的结果
```

### 非捕获组

```js
若希望以()分组的元组在匹配的时候不被捕获，可以使用如下形式：

(?:str|pattern)

var str2 = '20170808',
    reg2 = /(?:\d{4})(\d{2})\1/;

console.log(str2.replace(reg2, '$1'));   // 08
```

## 或操作符(|)

```js
用(|)表示或者的关系。例如：/a|b/表示匹配字符"a"或者"b"，/(ab)+|(def)+/表示匹配一次或者多次出现的"ab"或者"def"


```

## 断言

```js
正则表达式中的断言大体分为两类，先行断言与后发断言；在每一种断言中又分为正环顾和负环顾，因此一共有四种断言形式。

先行断言
通俗的理解，先行断言就是表示在匹配的字符串后必须出现(正)或者不出现(负)什么字符串

(?=patten) 零宽正向先行断言

(?!patten) 零宽负向先行断言
```

```js
// (?=patten)零宽正向先行断言
var str = 'yuzhongzi91',
    str2 = '123abc',

    // 表示匹配以字母或者数字组成的，并且第一个字符必须为小写字母开头的字符串
    reg = /^(?=[a-z])([a-z0-9])+$/;

console.log(str.match(reg));    // [ 'yuzhongzi91', '1', index: 0, input: 'yuzhongzi91' ]
console.log(str2.match(reg));   // null

// (?!pattern)零宽负向先行断言
var str = 'bb<div>I am a div</div><p>hello</p><i>world</i><p>javascript</p>',
    // 表示匹配除了<p>标签之外的标签
    // 分析：
    // ?! 表示否定，这个可以先不用看
    // \/?p\b 表示匹配"/p"或者"p"
    // [^>]+ 表示匹配不是">"字符
    // 如果没有"?!"就表示匹配"<p>"标签
    // 而加上了"?!"，就表示否定，因此就是匹配除了<p>标签之外的标签
    reg = /<(?!\/?p\b)[^>]+>/g;

console.log(str.match(reg));    // [ '<div>', '</div>', '<i>', '</i>' ]
```

```js
后发断言(兼容性，测试目前仍不支持)
通俗的理解，后发断言就是表示在匹配的字符串前必须出现(正)或者不出现(负)什么字符串

(?<=patten) 零宽正向后发断言

(?<!patten) 零宽负向后发断言

var str = '<p>hello</p>',
    reg = /(?<=<p>).*(?=<\/p>)/;

console.log(str.match(reg));    // 兼容情况下会匹配到"hello"字符串
```

## 常用正则表达式

```js
4.1 去除首尾字符串空格(trim)
var str = ' I am a cat. ',
    reg = /^\s+|\s+$/;

console.log(str.replace(reg, ''));  // I am a cat.
4.2 匹配电话号码
var str = '0715-85624582-234',
    str2 = '027-51486325',

    // \d{3,4} 3-4位区号
    // \d{8} 8位电话号码
    // (-(\d{3,4}))? 可能存在3-4位分机号
    reg = /^(\d{3,4})-(\d{8})(?:-(\d{3,4}))?$/;

console.log(str.match(reg));    // [ '0715-85624582-234', '0715', '85624582', '234', index: 0, input: '0715-85624582-234' ]
console.log(str2.match(reg));   // [ '027-51486325', '027', '51486325', undefined, index: 0, input: '027-51486325' ]
4.3 匹配手机号码
var str = '17985642351',
    reg = /^1[3|4|5|7|8]\d{9}$/;

console.log(str.match(reg));    // [ '17985642351', index: 0, input: '17985642351' ]
4.4 匹配邮箱
var str3 = 'yuzhongzi_91.xiao@sina.com',
    pattern3 = /^(?=[a-zA-Z])([\w\d-]+)(?:\.([\w\d-]+))*?@([a-zA-Z0-9]+)(?:\.([a-zA-Z]+))+$/;

// [ 'reg.xiao91@gmail.com', 'reg', 'xiao91', 'gmail', 'com', index: 0, input: 'reg.xiao91@gmail.com' ]
console.log(str3.match(pattern3));
4.5 限制文本框只能输入数字和小数点(二位小数点)
var reg = /^\d*\.?\d{0,2}$/;

console.log(reg.test('.4'));        // true
console.log(reg.test('3'));         // true
console.log(reg.test('3.3333'));    // false
4.6 匹配中文
var str = '223我是abc一只猫234',
    reg = /[\u4E00-\u9FA5\uf900-\ufa2d]/g;

console.log(str.match(reg));    // [ '我', '是', '一', '只', '猫' ]
4.7 匹配标签中的内容
var str = '<div>java<p>hello</p>script</div>',

    // 或者为 /<div>(.*)<\/div>/ 但不能匹配'\n'
    // /<div>((?:.|\s)*)<\/div>/
    reg = /<div>[\s\S]*<\/div>/;

if (reg.test(str)) {
    console.log(RegExp.$1);     // java<p>hello</p>script
}
4.8 匹配带有属性的标签
var str = '<div class="test" id="test" data-id="2345-766-sd24">hello</div>',
    reg = /<([a-zA-Z]+)(\s*[a-zA-Z-]*?\s*=\s*".+?")*\s*>([\s\S]*?)<\/\1>/;

if (reg.test(str)) {
    console.log(RegExp.$3);     // hello
}
4.9 将数字转化为中文大写字符
var arrs = ["零","壹","贰","叁","肆","伍","陆","柒","捌","玖"],
    reg = /\d/g,
    str = '135268492580',
    res;

res = str.replace(reg, function(v) {
   return arrs[v];
});

console.log(res);   // 壹叁伍贰陆捌肆玖贰伍捌零
4.10 查找链接
var str = '<a href="http://www.rynxiao.com">rynxiao.com</a>',
    reg = /http:\/\/(?:.?\w+)+/;

console.log(str.match(reg)[0]); // http://www.rynxiao.com
5. ES6新增
构造函数可以添加第二个规则参数
// es5添加第二个参数时会报错
var regex = new RegExp(/test/, 'i');

// es6
var regex = new RegExp(/test/, 'ig');
regex.flags; // ig
添加了u修饰符，含义为"Unicode"模式，用来正确处理大于\uFFFF的Unicode字符。
// \uD83D\uDC2A会被es5认为是两个字符，加了u之后就会被认定为一个字符
/^\uD83D/u.test('\uD83D\uDC2A'); // false
/^\uD83D/.test('\uD83D\uDC2A');  // true
u字符对正则表达式行为造成的影响，具体参考阮一峰的《ECMAScript 6入门》

添加了y修饰符(粘连修饰符)，全局匹配，但必须从剩余字符串的第一个位置进行匹配。
var str = 'aaa_aa_a',
    reg1 = /a+/g,
    reg2 = /a+/y;

reg1.exec(str); // aaa
reg2.exec(str); // aaa

reg1.exec(str); // aa
// 第二次执为null，是因为剩余字符串为_aa_a，必须从头部开始匹配，因此没有匹配到规则`a+`
// 若将reg1和reg2修改为`/a+_/g`和/a+_/y，结果将相同
reg2.exec(str); // null
增加了RegExp实例属性sticky，表示是否设置了y标志
var r = /test/y;
r.sticky;   // true
增加了RegExp实例属性flags，表示正则表达式的修饰符
var r = /test/ig;
r.flags;   // gi
构造函数增加方法RegExp.escape()，表示对字符串转义，用于正则模式
RegExp.escape('The Quick Brown Fox');
// "The Quick Brown Fox"

RegExp.escape('Buy it. use it. break it. fix it.');
// "Buy it\. use it\. break it\. fix it\."

RegExp.escape('(*.*)');
// "\(\*\.\*\)"
增加修饰符s，可以使得.可以匹配任意单个字符，包括行终止符(\n,\r,U+2018,U+2029)
const re = /foo.bar/s;
// 另一种写法
// const re = new RegExp('foo.bar', 's');

re.test('foo\nbar') // true
re.dotAll // true
re.flags // 's'
支持后行断言，具体参看断言所讲

Unicode属性类

目前，有一个提案，引入了一种新的类的写法\p{...}和\P{...}，允许正则表达式匹配符合Unicode某种属性的所有字符。

// 匹配所有数字
const regex = /^\p{Number}+$/u;
regex.test('²³¹¼½¾') // true
regex.test('㉛㉜㉝') // true
regex.test('ⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩⅪⅫ') // true
```
