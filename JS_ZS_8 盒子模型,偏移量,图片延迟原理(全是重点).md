[TOC]
##JS_ZS_8 盒子模型,偏移量,图片延迟原理(全是重点)
###css盒子模型
1.标准盒模型:盒子自身的宽高就是它的width和height,有border 有padding有margin
2.怪异盒模型(了解):box-sizing, boder-box ,inherit, content-box
###JS盒子模型
JS提供了一系列获取css盒子模型的方法和属性
`13个属性的特点`分3个系列
	- 特点1:数字而且是在整数浏览器默认帮你四舍五入了
	- 特点2:没有单位
	- 特点3:只能获取不能修改 (特例scrollLeft和scrollTop是可以修改的)
`height:`
	- 只要是在样式中设置了height,不管内容有没有溢出,height都是设置的样式值
	- 没有设置height,内容的真实高度就是他的height
`css里设置的width:100px和height:100px只是内容的宽和高`
`3个系列`
>- 1.client系列
	- clientWidth=当前元素的可视区域的宽度,不包括滚动条和边线width+左右padding
	- clientHeight=当前元素的可视区域的高度,不包括滚动条和边线height+上下padding
	- clienLeft=当前元素左边框的宽度
	- clientTop=当前元素上边框的宽度
>- 2.offset系列
	 - offsetWidth=当前元素的clientWidth+外边框border的距离(左右)
	 - offsetHeight=当前元素的clientHeight+外边框border的距离(上下)
	 - offsetLeft:当前元素的外边距到其父级参照的内边距的左偏移量
	 - offsetTop:当前元素的外边距距离父级参照的内边距的上偏移量
	 - offsetParent:父级参照物,从当前元素的父级开始找找到一个定位元素就是他的父级参照,如果一直找到body没有发现定位元素那父级参照物就是body
>- 3.scroll系列
	- scrollWidth:如果内容没有溢出跟clientWidth一样,如果有溢出表示真实内容的宽度+左padding
	- scrollHeight:如果内容没有溢出跟clientHeight一样,如果有溢出表示真实内容的宽度+上padding  
	- scrollLeft:滚动条向左滚动的距离
	- scrollTop:滚动条向上滚动的距离
	+scrollTop最小值:0
	+scrollTop最大值:整个网页的高-一屏的高scrollHeight-clientHeight
	如果设置的值大于scrollTop的最大值就默认等于最大值,小于最小默认等于最小值
`如果加上overflow:hidden;之后,scrollHeight就变成真实内容的高+上下padding,所以就不常用`
###浏览器的盒子模型
>- 一屏的宽和高 
	- document.documentElement.clientWidth 或者document.body.clientWidth(其他浏览器)
	- document.documentElement.clientHeight 或者document.body.clientHeight(其他浏览器)
>- 整个网页的宽和高
	- document.documentElement.scrollWidth 或者document.body.scrollWidth(其他浏览器)
	- document.documentElement.scrollHeight 或者document.body.scrollHeight(其他浏览器)
###offsetParent 父级参照物详解
>- 从当前元素的父级开始找,找到一个定位元素就是他的父级参照,如果找到body没有找到定位元素,那它的父级参照物就是body
>- 当前元素的外边框距离父级参照物的内边框的距离
>- 为了方便以后运用偏移量,一般都会找同一个元素作为参照物,使用body作父级参照物计算偏移量
>- `注意`在ie8浏览器中这个偏移量包括父级参照物的边框
###延迟加载(又名懒加载)
项目中为了保证页面的打开速度防止加载过慢网页崩溃,尤其是图片大小比较大就会容易加载过慢,一般会做些延迟加载的处理,在性能优化方面会经常用到,主要用于pc大量图片网站,以及移动端限制流量
1.首屏的图片一般一开始的时候给他一张占位图(背景图),背景图片都处理的比较小,等页面其他内容加载完成之后再去加载真实图片,一般给一个延迟时间
2.其他屏首次加载的时候不加载图片等这张图片完全露出来之后再去加载
只要是图片img的src属性加上地址了就会自动加载,所以写html结构的时候就不要给src赋值,一般我们将真实图片的地址值给他存在自定义属性上,等到让图片加载的时候再从自定义属性中取出地址值,赋给src.
注意赋值之前需要检验地址是否正确,方法:动态创建一个img当做替身(不显示在页面上),将这个地址值赋值给这个替身的src看看它能不能加载成功,如果成功再去将其赋值给页面上的img