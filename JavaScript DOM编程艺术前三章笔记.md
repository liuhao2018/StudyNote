#### JavaScript DOM编程艺术前三章笔记
##### js代码书写位置：

head标签中添加script标签，并在其中书写js代码。

在单独的.js文件中书写js代码,在head标签中script标签，并使用src引入这个js文件。

最好的是把script标签放在HTML文档的最后，/body 标签之前。

#### 变量：

- 在JavaScript脚本中，如果程序员对某个变量赋值之前未声明，赋值操作将自动声明该变量。虽然JavaScript没有强制要求程序员必须提前声明变量，但是提前声明变量是一种良好的编程习惯。

- 必须明确类型声明的语言称为强类型语言。JavaScript不需要进行类型声明，因此它是一个弱类型语言。这意味着程序员可以在任何阶段改变变量的数据类型。

#### 数组：

在JavaScript中，数组可以用关键字Array声明。声明数组的同时还可以指定数组的初始元素的个数，也就是数组的长度。

`var beatles = Array(4);   //给出个数`

`var beatles = Array();     //没有给出个数` 

##### 简单方式
`var beatles = Array('John','Paul','George','Ringo');`
`var beatles = ['John','Paul','George','Ringo']`

> 数组中元素不需要定必须类型一致，可以是字符串，也可以是布尔值等等。甚至数组的元素也可以是一个数组。

```
var lennon = ['Ringo',1940,false];
var beatles = [];
beatles[0] = lennon;
```

##### 关联数组(了解)
它可以使用字符串来代替数字值。

#### 对象

##### 创建方式
```
第一种：
var lennon = Object();
lennon.name = 'John';
lennon.year = 1940;
lennon.living = false;
```

```
第二种：
var lennon = {
	name:'John',
	year: 1940,
	living:false
};
```

#### 函数

```
function multiply(num1,num2){
	var total = num * num2;
	alert(total);
}
```

```
function multiply(num1,num2){
	var total = num * num2;
	return(total);
}
```

#### 变量作用域
如果在某个函数中使用了var，那个变量就将被视为一个局部变量，它只存在于这个函数的上下文中，反之，如果没有使用var，那个变量就将被视为一个全局变量。
 
#### 对象类型

- 用户自定义对象   程序员创建自己的对象
- 内建对象 JavaScript提供的一系列预先定义好的对象
- 宿主对象 运行环境提供的，也就是浏览器

#### DOM   (Document Object Model) 它把你编写的网页文档转换为一个文档对象
- 节点的概念
	
	在DOM中，文档是由节点构成的集合
	
	节点类型：元素节点，文本节点，属性节点。
- 5个常用的DOM方法
	- getElementById 返回一个给定id属性值的元素节点对应的对象
	- getElementsByTagName 返回一个数组
	- getElementsByClassnName 返回一个数组
	- getAttribute 获取属性值
	- setAttribute 设置属性值