[TOC]
##JS_ZS_1 变量提升(难点非重点)和作用域(重点)
###1.定时器
- 定时器是异步的
- 定时器是有返回值的,是一个数字,代表他是第几个定时器(注意:第几个跟执行顺序没有关系,你写的他是第几个就是第几个)
- 清除定时器参数是一个数字,表示第几个定时器(清除定时器原理是将定时器中的函数阻止了,这个定时器本身还是在的,他的序号还在)
- 清除定时器的时候,如果你知道这个是第几个定时器那么直接可以写一个数字,但是开发中可能好多个,就容易乱套.此时一般都是用一个变量来接收这个定时器的序号,清除的时候就准确了
- 定时器容易造成累加,没有用的一定要清除掉

###2.引用数据类型
- 存储引用数据类型的过程中拿不到obj 的值,存完了才可以拿到obj的值
```
var obj={
    name:'js',
    can:(function () {
        console.log(obj);
        return obj.name;
    })(),
    do:function () {}  
};
```
- 函数可以增加自定义属性
```
function fn(){};
fn.index="哈哈";
```
- 函数的name属性
```
1.构造函数方式创建的函数name都是 'anonymous'
var f=new Function("var a=1");
console.log(f.name);//'anonymous'

2.使用bind方法得到一个新的函数
function ff(){};
var f=ff.bind();
f.name 是 bound ff

```
- 函数的length
```
形参没有默认值时候,函数的length就表示形参的长度

有默认值时候会影响
function fn(x,y=1,z){}
fn.length ==1
```
###3.JS运行机制
* 1.当浏览器(它的内核/引擎)渲染和解析JS的时候会提供一个供JS代码运行的环境,我们把这个环境称之为"全局作用域(global scope)"(全局在后台语言叫global 客户端叫window),也叫栈内存
* 2.代码自上而下执行(执行之前还有一个变量提升阶段)
	+ 基本数据类型的值会存储在当前作用域下
   var a=12;//分三步操作
     1)首先开辟一个空间存储12
     2)在当前作用域中声明一个变量a (var a)
     3)让声明的变量和存储的12进行关联(把存储的12赋值给a =>赋值操作叫定义
   注意:变量提升阶段的顺序是2-1-3
	+基本数据类型值(也叫作值类型),是按照值来操作的;把原有的值复制一份放到新的空间或者位置上,和原来的值没有关系
;
	+ 引用数据类型的值不能直接存储到当前的作用域下(因为可能存储的内容过于复杂,我们需要先开辟一个新的空间(理解为仓库),把内容存储到这个空间中)
    var obj1={n:100};
    1)首先开辟一个新的内存空间,把对象中的键值对依次存储起来(为了保证后面可以找到这个空间,此空间有一个16进制的地址)
    2)声明一个变量
    3)让变量和空间地址关联在一起(把空间地址赋值给变量)

      引用类型不是按照值来操作,它操作的是空间的引用地址:把原来空间的地址赋值给新的变量,但是原来的空间没有被克隆,还是一个空间,这样就会出现多个变量关联的是相同的 空间,相互之间就会存在影响了

`所有数据类型都是先处理值再处理变量再关联到一起,也就是说先处理等号右边的值再给变量,变量提声阶段就是先声明变量,再处理值(看值是基本的还是引用的,引用的就开辟一个堆内存),再把值赋给变量`
###3.堆内存和栈内存

`栈内存`
      本身就是一个供JS代码执行的环境
      所有的基本类型值都会直接的在栈内存中开辟一个位置进行存储

 `堆内存`
       用来存储引用类型中的信息值的
		 >- 对象存储的是键值对
		 >- 函数存储的是代码字符串
```
var obj={
	n:10,
	m:obj.n*10
};
console.log(obj.m);//=>Uncaught TypeError:Cannot read property 'n' of undefined
/*
*1.形成一个全局作用域(栈内存)
*2.代码自上而下执行
*	1.首先开辟一个新的堆内存(AAAFFF111),把键值对存储到堆内存中
*	n:10
*	m:obj.n*10 =>obj.n 此时堆内存信息还没有存储完成,空间的地址还没有给obj,此时的obj是undefined,obj.n是undefined.n 基本类型没有属性所以报错TypeError:Cannot read property 'n' of undefined
*	
*/	
```
###4.变量提声:变量的提前声明
当前作用域加载的时候会对带var和function关键字的进行声明+定义:使用var只声明不定义,function既声明又定义
  `作用域形成之后, 代码执行之前, 先将带var和function 关键字的提前声明或定义`
   - 使用var只声明不定义
   - function既声明又定义
```
    console.log(a);//undefined
    console.log(fn);//undefined
    //console.log(b);//报错b is not defined
    var a=1;
    console.log(window.a);//1
    b=1;// window.b=1 setTimeout=>setTimeout
    console.log(b);//1
    var fn=function () {}
```
   
####4.1声明变量var和function重名
`重名情况下,不进行重复声明,只对其赋值`
   `注意`遇到var声明关键字和function声明的关键字重名了,在变量提声阶段是一个变量,值是函数的地址值;但是代码执行时又给变量赋值的话,而且赋的值不是函数,再让变量当做函数执行的时候就会报错
```
    var f=10;
    function f() {
        console.log("A");
    }
    f();//f=10,f is not a function
    //实例2
    alert(a);//弹出function a的内容
    function a(x){
        return x*2
    }
    var a=2;
    alert(a);//2
```
实例2分析:变量提声阶段变量a声明并且被赋值,这个值是堆内存里的一个地址值,然后重名变量a又被声明了一次,浏览器在解释的时候发现已经有一个同名变量a,就不再去重复声明了,并且在声明的时候也没有被赋值(var 变量提声时只声明不赋值),所以第一次弹出的是function a的内容;后来代码执行 ,变量a被重新赋值为2,所以第二次弹出2
####4.2es6中定义的let和const没有变量提声
   ES6中let和const没有变量提声,不可以提前使用未声明的变量;但是在变量提声阶段浏览器会看一下是否有不合法和报错的情况
   >- let 和const 共同点
     - 1.不可以重复声明 否则报错Identifier 'a' has already been declared
     - 2.没有变量提声
     - 3.声明的变量不会给Window增加一个属性     
   >- const 不同点
     - 4.一旦声明必须赋值
     - 5.赋值之后不可以修改  
####4.3没有关键字的变量
如果遇到使用变量的时候发现只声明未定义是undefined,如果没有声明过就报错了
####4.4变量提声的特殊情况
   - 1.函数作为值进行定义的时候(等号的右边或回调函数(函数当做参数的时候)),函数名没有任何意义,所以不进行变量提声
```
     //注意:等号右边如果赋值一个函数,直接写一个匿名函数(没有函数名)即可,如果写了函数名也没有意义,当前函数名字仍然是等号左边的变量
    f3();    //f3 is not defined
    var fn=function f3() {};
    f3();    //f3 is not defined
    console.log(f2);    //f2 is not defined
    box.onclick=function f2() { };
    console.log(f2);    //f2 is not defined
    //当一个函数作为参数的时候函数名也没有意义
    [1,2,3].forEach(function B(item) {
        console.log(item);
    });
    //console.log(B);//B is not defined
```
   - 2.自执行函数不进行变量提声,但自执行函数执行的时候要进行变量提声
```
    //其实第一个()里面的函数定义就是一个值(函数的地址值)
    console.log(gg);//gg is not defined
    (function gg(){
        console.log("我是自执行函数");})()
```
   - 3.return右面的代码不变量提声,return下面的代码变量提声但不执行   
``` 
//实例1  
    return function f(){};
    var a=3;  
    //function f不是return后面的,不变量提声,var a变量提声
    //实例2
	    function f4(){
        console.log(a);//undefined
        f();//你好
        return 1;
        console.log(a);//20-22行都是return后面的代码不执行,本行不打印,
        var a=1;//本行不执行,只进行变量提声
        console.log(a);//本行不打印
        function f() {
            console.log("你好");
        }
    }
    f4();
 
```
   - 4.if判断:
   变量提声时:判断体中函数只声明不定义,var不变还是声明
   代码执行时:条件成立第一时间给函数定义,再执行代码
```
    //条件不成立也会变量提声
    console.log(fn);//undefined
    fn();//fn is not a function
    if(9==8){
        function fn() {
            alert("你好")
        }
    }
```
>- 5.连等情况下,带var关键字的只声明,`其他都相当于给window添加了一个属性`
```
	console.log(a);//undefined
	console.log(b);//b is not defined
	var a=b=c=4;
	console.log(b);//4
	console.log(c);//4
```
###5.带var和不带var的
`带var的`是变量 
`不带var`的是window的属性
在全局作用域下声明一个变量,也相当于给window全局对象设置了一个属性,变量的值就是属性值(私有作用域中声明的私有变量跟window没关系)
```
	console.log(a);//undefined
    console.log(window.a);//undefined
    console.log('a' in window);//true  =>在变量提声阶段,在全局作用域中声明了一个变量a,此时就已经把a当做属性赋值给windowL ,只不过此时还没有给A赋值,默认值undefined  
    //in:检测某个属性是否隶属于这个对象
    var a=12;//全局变量值修改,window的属性值也跟着修改
    console.log(a);//12
    console.log(window.a);//12
    a=13;
    console.log(window.a);//13
    window.a=14;
    console.log(a);//14
    //全局变量和window中的属性存在一一映射机制,一个改了另一个也跟着改
    
```
`不带var的`
本质是window下的属性
```
	console.log(a);//Uncaught ReferenceError: a is not defined
    console.log(window.a);//undefined 对象没这个属性,就是undefined
    console.log('a' in window);//false
    a=12;//不是变量的赋值,是window.a=12的缩写
    console.log(a);//12 此处a不是变量,也是window.a的简写
    console.log(window.a);//12
```
//ms题
```
 
<script>
    //if(g()&&[]==![]){这行报错g is not a function ,故此题不会有弹框
    console.log(f);//undefined
    //console.log(g);//g is not defined
    var f=function(){return true;};
    g=function () {return false;};
    //console.log(f1);//f1 is not defined
    (function f1() {
        console.log(f);//ƒ (){return true;}
        console.log(g);//undefined
        console.log(g());// g is not a function
        if(g()&&[]==![]){// g is not a function
        f=function(){return false;};
        function g(){return true;}
        //var t=1;
        }
    })();
    alert(f());
    alert(g());
</script>
```
###6.作用域和作用域链
####6.1 作用域
 - 作用域:代码执行的环境 栈内存
    1.全局作用域:打开浏览器就形成全局作用域
    2.私有作用域:一个函数执行就会形成一个私有作用
    3.块级作用域:使用{}包起来的就是块级作用域 {},for(){},if(){},while(){};注意对象里的{}不是
####6.2.私有作用域
一个函数执行会形成一个私有作用域,保护里面的`私有变量(声明过的,形参)`不受外界干扰
>- 私有变量
  - 1.在私有作用域下声明的变量
  -  2.形参
>- 函数执行的顺序
  - 1.私有作用域形成
  - 2.给形参赋值
  - 3.变量提声
  - 4.函数体中的代码执行
`在私有作用域下变量提声的时候遇到已经有的私有变量(形参)就不用再声明了`
`在私有作用域下遇到变量先看是不是自己的私有变量(声明过的,形参);如果是就用自己的,不是的话就往上一级去找`
####6.3.上一级作用域
看一个函数的上一级作用域是谁,就看这个函数所对应的地址在哪个作用域下定义的,在哪定义的作用域就是谁
```
  var a=1;
  function f1() {
    console.log(a);
  }
  function fn1() {
    var a=2;
    f1();
  }
  fn1();//1 f1执行的时候打印a,f1里没有a,往上一级作用域找,就是找f1在哪定义的,实在全局作用域里定义的,全局的a=1所以打印1
```
#### 6.4作用域链
当前作用域下,遇到变量先找该作用域的私有变量,没有就往上一级作用域找,如果还没有就再找上一级,一直找到全局Window下.还是没有就报错了,在哪里找到就用哪里的,这个查找过程就叫作用域链
```
    var a=1;
    function fn1(a) {
      var f2=fn2;
      f2();
    }
    function fn2() {
      console.log(a);
    }
    fn1(a);//1
```
####6.5.块级作用域{}
>- 如果想要{}表示对象千万不要放在行首;
var obj={name:"珠峰",age:10};
>- 1.在块级作用域中使用ES5声明的变量依然是当前作用域的,但是变量提声有点改变:var和function都是只声明不定义,进入到块级作用域中先给函数赋值,再去执行块级作用域的代码
>-2.在块级作用域中使用ES6声明的变量是私有变量
暂时性死区:在块级作用域中不可以提前使用未被定义(ES6中的let,const)的变量
>-3.块级作用域下var和function不允许重名
```
if (!("aa" in window)) {
        var aa = 1;
        function aa() {
            console.log(aa)
        }
    }
    console.log(aa);//Identifier 'aa' has already been declared
```

