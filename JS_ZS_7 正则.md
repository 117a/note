[TOC]
##JS_ZS_7 正则
###1.定义/语法/作用
`定义`
自己定义的字符串模型,看目标字符串是否符合这个模型的规则
`语法`
用两个/包起来
`作用`
就是一个正确的规则,可以用这个模型验证某个字符串(或字符串里的一部分)是否和这个模型相匹配,或使用这个模型把某个字符串里和这个模型匹配的那一部分找出来
> 学习正则:就是学习如何编写一个规则
`组成`
正则是由元字符和修饰符组成
在正则当中,两个斜杠中间的是元字符,最后一个斜杠后面的是修饰符
```
var reg=/^$/g
```
###2.`创建一个正则`
>- 1.字面量方式的创建: var reg=/^\d$/
>- 2.构造函数方式创建:
 - 第一个参数是元字符,双斜杠里面的内容,写在""中但是注意原来的\写成\\, 因为本身字符串里的\有转义的意思
 - 第二个参数是修饰符:当你有一个变量时,通过构建函数的方式创建:var reg=new RegExp('\\d','g')
```
	//字面量创建正则
    var reg1=/^-?([1-9]\d+|\d)(\.\d+)?$/g;
    //构造函数方式创建正则
    //第一个参数是双斜杠里面的内容,写在""中但是注意原来的\写成\\ 因为本身字符串里的\有转义的意思
    //第二个参数是修饰符
    var reg2=new RegExp('^-?([1-9]\\d+|\\d)(\\.\\d+)?$',"g");
    console.log(reg2.test("-13.5"));
    console.dir(RegExp.$1);//RegExp对应的属性$1 第一个分组捕获的内容"13"
    console.dir(RegExp.$2);//RegExp对应的属性$1 第一个分组捕获的内容".5"
    console.dir(RegExp.$3);//RegExp对应的属性$1 第一个分组捕获的内容""
```
###3.`正则上的方法`
>- 1.test:通过创建一个正则,在当前字符串中来验证是否符合这个规则
返回值是true和false
>- 2.exec:捕获:创建一个正则,在当前的字符串中,验证是否符合这个规则
返回值符合的话返回一个数组,不符合规则返回一个null
```
var reg=/\d/;
//0-9之间的任意一个数字
var str='1234';
console.log(reg.test(str));//true
var reg1=/\d/;
var str1='a1b2c3';
console.log(reg1.exec(str));
//["1", groups:undefined, index:0, input:"1234"]
```
###4.`修饰符`
i:ignoreCase 忽略大小写
m:multiLine 多行匹配
g:global 全局匹配
###5.`元字符`
>- 定义:只要是正则两个斜杠/包起来的任意一个符号都是元字符
>- 分类:
			- 1)特殊元字符
			- 2)普通元字符
			- 3)量词元字符
####`常用量词元字符`
>- `定义`只有放在元字符后面才表示量词元字符
*:出现0个到多个
+:出现1个到多个
?:出现0个到1个[有或者没有]
{n}:出现n个
{n,}:出现n个到多个
{n,m}:出现n个到m个
```
var reg=/\d+/;//=>  0-9之间某个数字出现0到多次
```
####`常用的特殊元字符`
\d:0-9之间的任意一个数字
\D:除了0-9之间的任意一个数字
\w:字母数字下划线之间的任意一个字符[0-9a-zA-Z_]之间的任意一个数字 共63个
\W:除了字母数字下划线之间的任意一个字符
\s:匹配任意一个空白字符
\S:非空白符
\b:匹配边界,任意一个边界符号'zhufeng' 'zhu-feng'碰到-的时候左右两边也属于边界
\B:非边界
\n:任意一个换行符
\ :转义 \d 表示将普通的元字符,变成一个特殊带有意义的元字符
. :这个点不是普通的小数点,它是除了\n以外的任意一个字符
^:表示开始的字符
$:表示结束的字符
注意:如果有^和$确定且中间没有量词元字符,那么匹配的字符的length就确定了
或
x|y:表示x或者y中的任意一个字符
```
var reg=/^10|56$/;//=>|表示或者,此正则表示以10开头或者56结尾的字符串,不确定length
var reg=/^(10|56)$/;//=>此正则表示要么是10要么是56
//需求:给字符串去掉首尾空格
var reg1=/^ +| +$/;//=>匹配首尾空格的正则
```
`中括号:出现在中括号中的字符,除了- ^ ?!之外都是本身的意思`
`中括号包的字符只取其中一个字符`
`要从小到大写[0-6] [a-z],不能写[6-0]`
[xyz] :表示xyz中的任意一个字符,[?&]:或者?或者&
[^xy] :^非:除了xy当中的任意一个字符
[a-z]:范围 a-z之间的任意一个字符  [0-9a-zA-Z_]==\w
	  -:-在中括号中表示范围,如果它左边或者右边没有字符的话,代表它本身的含义
	  
[^a-z] :表示除了从a-z之间的任意一个字符
```
reg=/^[12]$/;//=>"1"或者"2"中的一个
reg=/[.]/;//=>表示.本身
reg=/[24-7]/;//=>24 25 26 27
//需求:18-47之间的一个数字
reg=/^(1[8-9]|[2-3][0-9]|4[0-7])$/
//需求:匹配一个小数
reg=/^-?\d\.\d+$/;//整数部分只有一位
////匹配一个数字 正负 小数 整数   小数的整数部分多位或一位 小数部分不出现就是整数,出现就是整数,且小数部分只能出现一次
reg1=/^-?([1-9]\d+|\d)(\.\d+)?$/;
```

():表示分组的意思
(?:):表示只匹配不捕获
####`普通元字符`
在正则当中,两个斜杠中(字面量方式的创建)除了特殊的元字和修饰符其他都是普通元字符

+ 中括号中的特殊字符都代表本身的含义
+ \d  \w \s 代表它本身的含义
+ -在中括号中代表范围的意思,如果左右两边没有代表本身的含义
+ 中括号的两位数,都是个位数[1123123122]
###6.` ()小括号分组的意义`
1.提高优先级
2.分组引用
3.分组捕获
>- \1表示引用第一个小括号的内容,字符串必须相同
>- \m没有那个分组匹配的就是\m m代表[0-9]之间的一个数

```
var reg1=/^\w(\w)\1\w$/;
console.log(reg1.test("asdf"));//false
var reg=/^([a-z])([a-z])\1\2([a-z])$/;
console.log(reg.test('abcde'));//false
console.log(reg.test('ababc'));//true
console.log(reg.test('ab123'));//false
console.log(reg.test('abdda'));//false
var reg2=/^(3)\1\5&/; //"33\5"
```
###7.需要背下来的正则
`var reg1=/^-?([1-9]\d+|\d)(\.\d+)?$/;//匹配一个数字`
`var reg2=/^ +| +$/;//=>匹配首尾空格的正则`

###8.正则捕获
捕获:正则的捕获是使用exec来捕获的,捕获之前要进行验证,验证成功捕获出符合规则的字符串,验证不成功,返回null
`返回的数组:`
1.大正则捕获的内容
2.分组捕获的内容
3.index:字符串当中开始捕获的索引
4.input:原字符串
`特点`
>- 懒惰性:一次捕获一个;解决懒惰性加全局修饰符g,进行多次捕获
>- 贪婪性:一次捕获符合要求最长的,主要是因为量词元字符的存在,解决贪婪性在量词元字符后面加?,就是解决量词的取值
>
`注意`在正则当中加的小括号,不一定是为了分组捕获,也有可能是为了提高优先级或者分组引用.如果只是为了加一个括号,我们需要添加一个?:只匹配不捕获
```
var reg4=/\w(\d+)/;
    var str2="huxue1107aa1010";
    console.log(reg4.exec(str2));//["e1107", "1107", index: 4, input: "huxue1107aa1010", groups: undefined]
    var reg=/\w/g;
    var str="asd";
    console.log(reg.exec(str));
    console.log(reg.exec(str));
    console.log(reg.exec(str));
    console.log(reg.exec(str));
   //将所有内容都捕获出来
    function getAll() {
        let arr = [];
        let r = reg.exec(str);
        while (r) {
            arr.push(r[0]);
            r = reg.exec(str);
        }
        return console.log(arr);
    }
    getAll();
    
    //贪婪性
    
    var reg5=/\d+/;
    str3="china2018";
    console.log(reg5.exec(str3));//["2018", index: 5, input: "china2018", groups: undefined]
    //解决贪婪性在量词元字符后面加?,就是解决量词的取值
    var reg6=/\d+?/;
    console.log(reg6.exec(str3));//["2", index: 5, input: "china2018", groups: undefined]
    //在量词元字符后面加?,就是解决量词的取值
    var reg7=/\d{2,4}/;
    console.log(reg7.exec(str3));//["2018", index: 5, input: "china2018", groups: undefined]
    var reg8=/\d{2,4}?/;
    console.log(reg8.exec(str3));//["20", index: 5, input: "china2018", groups: undefined]
```
`字符串中的捕获方法match`
>- 不加g: 得到的结果跟exec一样
>- 加g:一次性捕获所有符合规则,没有index input groups属性
>- 分组情况加g:但是不会捕获分组的内容
`捕获分组的问题`
exec的返回值就是多几项,有几个分组就多几项
索引0:大正则捕获的内容
索引1:第一个分组捕获的内容
索引2:第二个分组捕获的内容
...
index:大正则捕获的内容的首字符索引
input:
`如果想让分组的内容不被捕获出来加上?:`
var reg4=/?:[a-z]+(\d+)/g;
```
	var str="hux1989kh"
    var reg1=/\d/;
    console.log(str.match(reg1));
    //["1", index: 3, input: "hux1989kh", groups: undefined]
    var reg2=/\d/g;
    console.log(str.match(reg2));// ["1", "9", "8", "9"]
    //分组
    var reg3=/[a-z]+(\d+)/;
    var str1="h5+css3+es6";
    console.log(str1.match(reg3));//["h5", "5", index: 0, input: "h5+css3+es6", groups: undefined]
    var reg4=/[a-z]+(\d+)/g;
    console.log(str1.match(reg4));//["h5", "css3", "es6"]
```
综合
```
//获取每门课程后面的价格
	var str2="h5:2000,css3:500,js:9800";
    function getStr(str,s) {
        let reg=new RegExp(str+":(\\d+)");
        return console.log(reg.exec(s)[1]);
    }
    getStr("h5",str2);//2000
    getStr("css3",str2);//500
```

###字符串中的方法处理正则
`split` 按照指定字符拆分为数组

`search`找到符合要求的索引位置
`match`
`replace(a,b)`
	 - 如果想一次全部替换,第一个参数写一个正则并且加上g
```
	//split
	var str="1+2*3-4/5";
	//需求:拆分为["1","2","3","4","5"]
	console.log(str.split(/[+*/-]/g));
    //search 找到符合要求的索引位置
    console.log(str.search("1"));//0
    console.log(str.search(/\d/g));//0
    //match
    //replace(a,b)
    var str1="1+2+3+4";
    console.log(str1.replace(/\+/g, "*"));//1*2*3*4
var str2="h5:2000,css3:500";

    //需求:"H5:￥2000，CSS3:￥500"
    reg2=/(\w+):(\d+)/g;
    var str3 =null;
    str3=str2.replace(reg2,function () {
        //捕获多少次就执行多少次
        //每一次执行函数都默认传参数
        // arguments[0]:大正则捕获的内容
        // arguments[1]:第一个分组捕获的内容
        // arguments[2]:第二个分组捕获的内容
        //倒数第二项:大正则捕获的内容首字符的索引
        //最后一下:原字符串
        console.log(arguments);
        //第一次 Arguments(5) ["h5:2000", "h5", "2000", 0, "h5:2000,css3:500"]
        //第二次 Arguments(5) ["css3:500", "css3", "500", 8, "h5:2000,css3:500"]
        return arguments[1].toUpperCase()+":￥"+arguments[2];
        ////把return的值去替换掉大正则捕获的内容
    });
    console.log(str3);//H5:￥2000,CSS3:￥500
//其他方式,常用下面这种写法
	var str4=null;
    str4=str2.replace(reg2,(...arg)=>arg[1].toUpperCase()+":￥"+arg[2]);
    console.log(str4);//H5:￥2000,CSS3:￥500
//不常用写法
	var str5=null;
    str5=str2.replace(reg2,"$1:￥$2");
    console.log(str5);//h5:￥2000,css3:￥500
```