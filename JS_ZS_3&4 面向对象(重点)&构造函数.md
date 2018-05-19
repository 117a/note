[TOC]
##JS_ZS_3and4 面向对象(重点)/构造函数
###1.面向对象
JS中万物皆对象
面向对象:除了面向对象还有面向过程
总结:面向类的封装/继承/多态,通过简单的实例化,调用其方法和状态
对象/类/实例
`对象:`多为一种泛指,任何东西都看成一个对象,可以通过原型链(原型指针`__proto__`)找到基类object.在实际生活中,桌子椅子都是一个对象,再学习js当中,对象数据类型也是js对象当中的一种
JS任何数据类型都有自己所属的类,但Null和undefined没有
`类:`对同种类型具有相同特征和特性的一个归纳,js当中也有相同的类的细分,比如number类 string类
`实例:`类中一个具体的细分,5是number类的一个实例
###2.单例模式
> 模块化开发:将一个完整的项目分模块,大家合作开发.例如:个人中心/首页/登录注册
> 合作开发很容易出现变量冲突,所以说一般每一个模块的属性和方法将其写在一个对象中,只需要对象名字不一样即可,这时候这个对象名就可以叫命名空间
> 每一个命名空间下的方法中的this也是当前命名空间(对象)

`单例模式`:把描述同一事物的属性和方法,放到一个命名空间下,避免全局变量的污染
>- 只用闭包保护变量是不完美的,我们需要搭配设计模式来实现全局变量不被污染
>- 设计模式:单例模式/构造函数模式/原型模式....
>- 单例模式的本质就是一个对象
```
    let personPage={name:'A',getName:function () {
        console.log(this.name);
    }};
    let indexPage={name:'B',getName:function () {}};
    let loginPage={name:'C',getName:function () {}};
```
>- 1.单例模式中的函数之间相互调用,采用this.函数()
>- 2.链式写法的原理:return this;
>- 3.单例模式中的函数中的箭头函数的this还是当前对象(例如上面案例中的定时器中的箭头函数);
`高级单例模式`:通过一个自执行函数返回一个对象,这个自执行函数执行的时候会形成一个私有作用域可以保护里面的私有变量不受全局污染
```
    let obj1=(function () {
        let name='obj1';
        function f1() {
            
        }
        function f2() {
            
        }
        return {name,f1,f2}   //对象简写,属性名属性值一样 
    })();
```
`单例模式中的链式写法`核心就是return this
```
    let obj1=(function () {
        let name='obj1';
        function f1() {
            return this
        }
        function f2() {
            return this
        }
        return {name,f1,f2}   //对象简写,属性名属性值一样
    })();
    obj1.f1().f2().name 
        
```
###3.内置类
内置类:
1.数据类型/ Set/ Map....
2.元素
3.arguments
4....
基本数据类型:Number数字类 String字符串类 Boolean布尔类 Null Undefined
引用数据类型:Object对象类(Array 数组类 RegExp 正则类 Date时间类 Object 对象) Function函数类 

`in`:可以找到该对象的私有属性和公有属性
`hasOwnProperty`:只能查找该对象上的私有属性的方法
###4.构造函数模式
JS提供了一种自己定义类的方式,通过自定义的函数来实现的,使用new关键字执行函数,这个函数就有一个新的身份,叫类,即构造函数
任何一个类都是一个函数
`构造函数模式的定义`
一个函数用new方式执行,就是构造函数
>- 里面的this就是当前实例
>- 通过this.属性=值的形式给当前实例增加私有属性
>- 默认返回this,手动的return一个基本数据类型不会影响实例this,但是return一个引用数据类型会影响实例this
>- 实例是一个对象
new有执行功能,如果函数不需要传参可以省略()
当我们new **的时候,当前的这个函数就已经和普通函数不一样了,返回的结果是当前类的一个实例,我们将这个模式叫做构建函数模式

`构造函数的特征:`
>- 1.首字母大写
>- 2.用new的方式执行
`构造函数的执行顺序`
>- 1.形成一个私有作用域
>- 2.形参赋值
>- 3.变量提声
>- 4.浏览器会为其创建一个对象,将其对象指针指向this
>- 5.代码从上到下执行 有return就返回没有就undefined
>- 6.默认返回this
`用构造函数模式创建的基本数据类型,其实是对象中的一个实例;
用构造函数模式创建的引用数据类型,还是引用数据类型,但是语法会有不同`
	- 构造函数执行是通过new关键字来执行,构造函数后面的括号是用来给实例传参的1
	- 每一个实例都是对象数据类型
	- 构造函数中的this都是当前的实例,构造函数中的属性都是当前实例的私有属性
	- 构造函数执行时,
		+如果没有return同样会返回this;
		+如果有return,
		return的是基本数据类型,那实例不会发生任何改变;
	return的是引用数据类型,那return的是什么,实例就是什么
```
    function fn() {
        this.name='test'
        //return 10;
        return [];    }
    let f1=new fn;//f1是fn的一个实例
    //{name:'test'}
    console.log(f1);//return10基本数据类型返回的是fn return[]返回是[]
```

###5.instanceof
`instanceof定义`检测当前对象是否是类的一个实例,只能检测引用数据类型,检测typeof检测不到的数组和正则
类都是函数,函数就是function类的实例=>类都是function类的实例
object基类 任何类都是object类的实例

```
	[] instanceof Array;//true
	/$/ instanceof RegExp;//true
	/$/ instanceof Array;//false
	Object instanceof Function;//true
	Function instanceof Object;//true
	Function.__proto__ == Function.prototype;//true
	Function.__proto__ == Object.prototype;//false
	Object.__proto__ == Function.prototype;//true
	Array.__proto__ == Function.prototype;//true
```
###6.函数的多种类型
1.函数数据类型的多种形态:
+ 普通函数
+ 类(自定义类构造的+内置类)
+ 函数类的一个实例
2.对象数据类型的多种形态:
+ 数组
+ 正则
+ 普通对象
+ Math
+ prototype
+ `__proto__`
+ ...
###7.原型模式
`类都是函数`
>- 1.每一个函数或者说任何类,都天生自带一个属性,叫prototype,中文名原型,它是一个对象,是引用数据类型,它是当前类的实例的公有方法
>- 2.Prototype(浏览器会默认给它开辟一个堆内存),天生自带一个属性,叫constructor,它指向当前函数类本身
>- 3.每一个对象数据类型(每一个实例/原型),自带一个`__proto__`,它指向当前所属类的原型
`原型上存储的是公有的属性和方法`

+函数既有原型prototype也有`__proto__`
+基类object的prototype没有`__proto__`,因为会指会它自己的原型死循环了console.log(object.prototype.__proto__) =>null

+如果给原型重新赋值的新的地址,造成constructor丢失,可以手动的加上原来的constuctor

+`用constructor的name判断数据类型`,但null和undefined不可以,它俩没有任何属性
true.constructor.name =>Boolean
[1,2].constructor.name =>Array

+1.类是函数,函数就是Function类的实例 类都是Function类的实例
+2.Object是基类 任何类都是Object类的实例
+Object是一个类就是一个函数 ,函数是Function类的实例,所以Object就是Function类的实例
+Object.__proto__指向所属类的原型,所属的类是Function,原型是Function.prototype


###8.原型链上的this
原型链:当一个对象获取属性的时候,先找自己身上的私有属性有没有,如果有直接调用,如果没有就通过`__proto__`找到所属类的原型上看公有属性有没有,没有继续通过`__proto__`往上一级找,一直找到基类object的原型,如果还没有直接报undefined,这个查找过程就是原型链
###9.arguments
形参的集合,又是一个类数组,不可以调用数组的方法
一个实例可以用一些属性和方法,是因为它的类身上有这些属性和方法可以让其调用.如果没有这些方法,就么有办法调用比如arguments
