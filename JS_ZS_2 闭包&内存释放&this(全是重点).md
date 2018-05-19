[TOC]
##JS_ZS_2 闭包&内存释放&this(全是重点)
###1.堆内存和栈内存
>- 1.栈内存:俗称作用域,全局/私有(浏览器加载的时候形成全局作用域,函数执行形成私有作用域)
+为Js代码提供执行的环境
+基本数据类型值是直接存放在栈内存中的
>- 2.堆内存:存储引用数据类型的值:
+对象:存的是键值对
+函数:存的是代码字符串
在项目中,占用的内存越少性能越好,需要把一些没用的内存处理掉
`堆内存的释放`
只能手动将其释放,以后在项目中如果后面不再使用的引用数据类型地址 应该手动清空,赋值为null;
var obj={};当前对象对应的堆内存被变量obj占用着呢,堆内存无法销毁,
obj=null;null空对象指针(不指向任何的堆内存),此时上一次的堆内存就没有被占用了,谷歌浏览器会在空闲时间把没有被占用的堆内存自动释放自动释放(销毁/回收),其他浏览器不一样
`栈内存的释放`
1.释放全局作用域:加载页面形成,关闭页面销毁
2.释放私有作用域:函数执行形成栈内存,函数执行结束,浏览器空闲的时候会将其释放,有时候执行完成,栈内存不能被释放,如下情况:
`不销毁的私有作用域`:一个作用域执行结束后,作用域中有引用数据类型的值被外界(栈内存以外的变量或事件)占用了,此时这个作用域就不销毁,里面的变量也不会销毁
如果一个作用域执行之后返回一个新的函数地址并且被外界占用了,此时这个作用域就不销毁
###2.循环绑定事件var let 问题
`闭包`
函数执行的时候形成一个私有作用域,保护里面的私有变量不受外界干扰,这种保护机制称为闭包
保存机制:函数执行结束后,里面的引用数据类型被外面的(变量或事件)所占用,形成了一个不销毁的私有作用域,里面的私有变量不会被销毁了

###3.函数问题
>- 函数有length属性,没有默认值时length就是函数形参的个数
>-函数形参也可以有默认值,只有不给形参传实参的时候才会走默认值;
```
   function f1(x,y=100){
        x=10;
        console.log((x + y));
    }
    f1();//10+100=110
    f1(1,2);//10+2=12
```
>- 一旦形参有默认值,length就不再表示形参的个数
```
    function f2(x1=1,x2=2,x3){}
    console.log(f2.length);//0
```
```
    var x=100;
    var s="hh";
    function f3(x=1,y=x,z=10,m=s) {
      console.log(y);//1
      var s="HH";
      console.log(m);//"hh"
    }
    f3();
    /*函数执行步骤:
     *1.形成私有作用域
     *2.形参赋值 x=1,y=1,z=10,m=s 此时他还没有私有变量s往上找 全局的s="hh"
     *3.变量提声
     *4.代码执行
     */
     
    function f4(y1,y2,y3=m,m=10) {
        //形参赋值:y1=undefined,y2=undefined,y3=m 往前看自己有吗?没有上一级有吗?没有 全局中都没有m此时就报错 m is not defined
    }
    f4()
```
###4.箭头函数
正常函数:
function 函数名(形参) {函数体}
`箭头函数:`
>- (形参)=>{函数体}
>- 箭头函数都是匿名函数
>- 简写方式
	- 如果形参只有一个,可以省略小括号(),如果一个形参都没有,不可省略小括号()
	- 如果函数体只有一条return语句,可以省略大括号{}和return
```
    let fn=(x)=>{
        x++;
        console.log(x);
    };
    fn(10);
    
    let f1=x=>typeof x;
    function f(x){
        return typeof x;
    }//这两个函数定义是等价的
    
    let ary=[1,2,3,4,5];
    ary.forEach((item,index)=>{
        console.log(item);});
    //将所有的奇数获取处理组成新的数组
    console.log(ary.filter(item=>item % 2));
```
###5.自定义属性
###6.this
`this`是执行主体
	>- this是个对象,不可以赋值;
	>- `在全局中this就是window,`
	>- `私有作用域函数下的this看函数执行的时候前面有没有点,没有点就是window,有点的话点前面是谁this就是谁`
```
	console.log(this);//Window
    var obj={
        fn:function () {
            console.log(this);
        }
    }
    obj.fn();//this是obj
    var fn=obj.fn;
    fn();//this是window
//练习题
	var a=10;
    function fn1(){
        this.a++;
    }
    var obj1={fn1:fn1,a:100};
    fn1();//window.a=11
    obj1.fn1();//obj1.a=101
    console.log(this.a);//11
    console.log(obj1.a);//101
```

###7.this的特殊情况
>- 1.自执行函数中的this永远都是window
>- 2.给元素绑定事件中给谁绑定的this就是谁
>- 3.函数当做参数的时候,函数中的this都是window
```
    var ary=[1,2,3];
    ary.forEach(function (item) {
        console.log(this);//window
    });
    ary.forEach(function (item) {
        console.log(this);//ary[1,2,3]
    },(ary));
    //数组中的遍历方法第二个参数就是用来改变第一个参数函数中的this
```
>- 4.箭头函数中没有this指向,如果出现this指的是上一级的this
>- 5.构造函数中的this就是当前实例
```
    let fn2=()=>{
        console.log(this);
    };
    var obj3={
        fn2:fn2
    };
    obj3.fn2();//window
```
###8.对象扩展
>- 对象的属性名和变量名重名了可以直接简写
```
    function fn1() {}
    let name="xue";
    let obj={
        fn1,//fn1:fn1
        name//name:"xue"
    };
```
>- 对象的解构赋值
`数组解构赋值`
 let [x,y]=[1,2];
 `对象的解构赋值`:左侧一定要写let 不写就成了{}在行首,成了块级作用域
```
let {name:n,age:b}={name:"xue",age:10};
//这里的变量是n和b,n的值就是name的属性值"xue",同样b=10
//变量名和属性名相同了可以简写
let {age}={age:100};
//age变量 对应右边属性名是age的值
    var aa="AA";
    var obj1={a:1,aa,bb:"BB"};
    var {aa:A,bb,a,aa}=obj1;//{aa:A,bb,a,aa}={a:1,aa:"AA",bb:"BB"}
    //上面一共有4个变量 A="AA" bb="BB" a=1 aa="AA"
```
>- 对象的扩展运算符
`数组的扩展运算符`
```
	let ary1=[1,2];
    let ary2=[10,20];
    console.log([...ary1, ...ary2]);//[1,2,10,20]
```
`对象的扩展运算符`ES7中提供的
```
	let o1={name:"xue"};
    let o2={age:10};
    let o={...o1,...o2};
    console.log(o);//{name:"xue",age:10}
```