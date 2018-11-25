### 一、reduce(fn,initValue) 基本用法
方法详解：接收两个参数
####
1. 第一个是一个函数fn(accumulator、currentValue、currentIndex、array)
2. 第二个initvalue是初始值
3. accuaccumulator--->累加器，即函数上一次调用的返回值。第一次的时候为 initialValue || arr[0]
4. currentValue 数组中函数正在处理的的值第一次的时候initialValue || arr[1]
5. currentIndex 数组中函数正在处理的的索引
6. array 函数调用的数组

````
场景一、实现一个sum 函数，使的输出的值为

1. sum(1,2,3).valueOf()       7
2. sum(2,3)(2).valueOf()      7
3. sum(1)(2)(3)(4).valueOf()  10
4. sum(2)(4,1)(2).valueOf()   9


function sum(...x) {
	var sum = x.reduce((a,b)=>a+b,0)
	var tmp = function(...y) {
		sum =sum+y.reduce((a,b)=>a+b,0)
		return tmp;
	};
	tmp.toString = function() {
		return sum;
	};
	return tmp;
}

console.log(sum(1,2,3).valueOf()) 
console.log(sum(2,3)(2).valueOf())
console.log(sum(1)(2)(3)(4).valueOf())
console.log(sum(2)(4,1)(2).valueOf())

````

````
排序：只用for循环

const sortArror = function(arr){
	let arror = arr || [12, 13, 65, 54, 86, 21, 37, 1, 95, 4]
	let length = arror.length;
	for(let i=0;i<length;i++){
		let tem = arror[i];
		if(arror[i]>arror[i+1]){
			arror[i]=arror[i+1];
			arror[i+1]=tem;
		}
		if(i==length-1){
			i=-1;
			length--;
		}
	}
	return arror;
}

sortArror()

````

### 二、animation 基本用法
animation: name duration timing-function delay iteration-count direction 
####
1. ------name--------------动画的名字
2. ------duration----------动画完成的时间 以秒或者毫秒
3. ------timing-function---动画的速度	曲线
4. ------delay-------------延迟多少时间开始执行动画
5. ------iteration-count --动画播放的次数
6. ------direction --------是否应该轮流反向播放动画。
####
transform：允许你将元素旋转，缩放，移动，倾斜等 
####
1. ------translate(x,y)----
2. ------translateX--------定义转换，只是用 X 轴的值。
3. ------scaleX(x)---------通过设置 X 轴的值来定义缩放转换
4. ------rotateX(angle)----定义沿着 X 轴的 3D 旋转
####
transform-Origin: 允许您更改转换元素的位置,从哪个位置开始转动
####
1. ------transform-origin: x-axis y-axis z-axis;
2. ------x-axis 定义视图被置于 X 轴的何处。可能的值：left center right length %
####
transform--style：属性指定嵌套元素是怎样在三维空间中呈现。	
####
````
.animation{
			width: 200px;
			height: 200px;
			margin: 0 auto;
			background: #090;
			-webkit-animation: rotateIn 1s .2s ease both;
			-moz-animation: rotateIn 1s .2s ease both;
}

@-webkit-keyframes rotateIn {
		0% {
			-webkit-transform-origin: center center;
			-webkit-transform: rotate(-200deg);
			opacity: 0
		}
		100% {
			-webkit-transform-origin: center center;
			-webkit-transform: rotate(0);
			opacity: 1
		}
}
	
@-moz-keyframes rotateIn {
		0% {
			-moz-transform-origin: center center;
			-moz-transform: rotate(-200deg);
			opacity: 0
		}
		100% {
			-moz-transform-origin: center center;
			-moz-transform: rotate(0);
			opacity: 1
		}
}

````

### 三、重放攻击：重复的会话请求就是重放攻击
####
解决方案：
####
1. 加时间戳
2. 在客户端和服务端通讯时，先定义一个初始序号，每次递增。这样，服务端就可以知道是否是重复发送的请求。
3. Https防重放攻击
4. 客户端请求服务器时，服务器会首先生成一个随机数，然后返回给客户端，客户端带上这个随机数，访问服务器，服务器比对客户端的这个参数，若相同，说明正确，不是重放攻击。这种方式下，客户端每次请求时，服务端都会先生成一个挑战码，客户端带上应答码访问，服务端进行比对，若挑战码和应答码不对应，视为重放攻击。
####
## 四、ES5与ES6 继承的不同点
````
	（1）、ES5写法
	//定义一个func

	function parent(a,b){
		this.a=a;
		this.b=b;
	}

	//func 的prototype上面加say 方法

	parent.prototype.say=function(){
		console.log(1)
	}

	//定义一个func sun

	function sun (c) {
		this.c=c; 
	}
	parent.call(sun)//sun 继承parent的属性
	sun.prototype=new parent(11,2)//sun继承parent的原型上方法
	sun.prototype.sayChild = function(){//sun 自己身上的方法
		console.log(this.a,this.b,this.c)
	}
	var childChild2 = new sun(23);
	childChild2.say();
	childChild2.sayChild();
	console.log(childChild2)


	(2)、ES6写法

	class parents{
		constructor(a,b){
			this.a=a;
			this.b=b;
		}
		say(){
			alert(1);
		}
	}
	class child extends parents{
		constructor(a,b,c){
			super(a,b);
			this.c=c;
		}
		childSay(){
			console.log(this.b)
			this.say()
		}
	}
	const childChild = new child(12,33,44);

	// childChild 上面含有a b c 属性，say childSay方法

````

### 五、Object.assign的实现：
````
如果目标对象中的属性具有相同的键，则属性将被源中的属性覆盖。后来的源的属性将类似地覆盖早先的属性。

if (!Object.assign) {
    // 定义assign方法
  Object.defineProperty(Object, 'assign', {
    enumerable: false,
    configurable: true,
    writable: true,
    value: function(target) { // assign方法的第一个参数
      'use strict';
      // 第一个参数为空，则抛错
      if (target === undefined || target === null) {
        throw new TypeError('Cannot convert first argument to object');
      }

      var to = Object(target);
      // 遍历剩余所有参数
      for (var i = 1; i < arguments.length; i++) {
        var nextSource = arguments[i];
        // 参数为空，则跳过，继续下一个
        if (nextSource === undefined || nextSource === null) {
          continue;
        }
        nextSource = Object(nextSource);

        // 获取改参数的所有key值，并遍历
        var keysArray = Object.keys(nextSource);
        for (var nextIndex = 0, len = keysArray.length; nextIndex < len; nextIndex++) {
          var nextKey = keysArray[nextIndex];
          var desc = Object.getOwnPropertyDescriptor(nextSource, nextKey);
          // 如果不为空且可枚举，则直接浅拷贝赋值
          if (desc !== undefined && desc.enumerable) {
            to[nextKey] = nextSource[nextKey];
          }
        }
      }
      return to;
    }
  });
}

````

### 六、浏览器判断

````
var Browser = /** @class */ (function() {
	function Browser() {}
	/**
	 * 获取浏览器数据
	 */
	Browser.getBrowser = function() {
		var UA = navigator.userAgent || ''
		var isAndroid = (function() {
			return UA.match(/Android/i) ? true : false
		})()
		var isQQ = (function() {
			return /(iPad|iPhone|iPod).*? (IPad)?QQ\/([\d\.]+)/.test(UA) || /\bV1_AND_SQI?_([\d\.]+)(.*? QQ\/([\d\.]+))?/.test(UA)
		})()
		var isIOS = (function() {
			return UA.match(/iPhone|iPad|iPod/i) ? true : false
		})()
		var isSafari = (function() {
			return /iPhone|iPad|iPod\/([\w.]+).*(safari).*/i.test(UA)
		})()
		var isWx = (function() {
			return UA.match(/micromessenger/i) ? true : false
		})()
		var isWb = (function() {
			return UA.match(/weibo/i) ? true : false
		})()
		var isAndroidChrome = (function() {
			return(UA.match(/Chrome\/([\d.]+)/) || UA.match(/CriOS\/([\d.]+)/)) && isAndroid && !isQQ
		})()
		var isQZ = (function() {
			return UA.indexOf('Qzone/') !== -1
		})()
		var browser = {
			isAndroid: isAndroid,
			isIOS: isIOS,
			isSafari: isSafari,
			isQQ: isQQ,
			isWb: isWb,
			isWx: isWx,
			isQZ: isQZ,
			isAndroidChrome: isAndroidChrome
		}
		return browser
	}
	return Browser
})()

````

### 七、数组的去重
```
// 例如：[1，2，4，4，3，3，1，5，3]
// 输出：[1，3，4]
let arr = [1, 2, 4, 4, 3, 3, 1, 5, 3];

// 方法一
function repeat1(arr){
	var result = [], map = {};
	arr.map(function(num){
	if(map[num] === 1) result.push(num); // 等于1说明之前出现过一次 这次重复出现了
		map[num] = (map[num] || 0) + 1; // 微妙之处 开始第一次出现无值 记为 0 + 1 = 1 下一次从1开始累加
	});
	return result;
}
console.log(repeat1(arr));

// 方法二

function repeat(arr) {
    let result = arr.filter((x, i, self) => {
        return self.indexOf(x) === i && self.lastIndexOf(x) !== i
    }); // 
    return result;
}
console.log(repeat(arr));

```

### 八、将重复个数排列
```
// 如果次数相同 则按照值排序 比如  2, 2, 2和 1, 1, 1  应排序为 [1, 1, 1, 2, 2, 2]

// 比如 [1,2,1,2,1,3,4,5,4,5,5,2,2] => [3, 4, 4, 1, 1, 1, 5, 5, 5, 2, 2, 2, 2]

let arr = [9, 7, 7, 1, 2, 1, 2, 1, 3, 4, 5, 4, 5, 5, 2, 2];
function sortArray(arr) {
    let obj = {};
    let newArr = [];
    for(let i = 0; i < arr.length; i++) {
      let cur = arr[i];
      if(obj[cur]){
        obj[cur].push(cur);
        continue;
      }
      obj[cur] = [cur];
    }
    for(let k in obj) {
      if(obj.hasOwnProperty(k)) {
        newArr.push(obj[k])
      }
    }
    newArr.sort((a, b) => {
      if(a.length === b.length){
        return a[0] - b[0];
      }
        return a.length - b.length;
    });
    newArr = newArr.reduce((prev, cur) => prev.concat(cur));
    return newArr;
  }
  console.log(sortArray(arr)); 

  // [ 3, 9, 4, 4, 7, 7, 1, 1, 1, 5, 5, 5, 2, 2, 2, 2 ]

```

### 九、获取对象里面的值:当一个对象你获取它不存在的key时，会报错，写一个获取key不存在是返回undefined
#### 例如： 
```
var data = { a: { b: { c: 'ScriptOJ' } } }
data.a.b.c => scriptoj
data.a.b.c.d // => 报错，代码不执行
data.a.b.c.e.f.g // => 报错，代码不执行

const safeGet = (o,path)=>{
	try{
		return path.split('.').reduce((o,k)=>o[k],o)
	}catch(e){
	return void 1;
	}
}
console.log(safeGet(data, 'a.b.c')) // => scriptoj
console.log(safeGet(data, 'a.b.c.d')) // => 返回 undefined
console.log(safeGet(data, 'a.b.c.d.e.f.g')) // => 返回 undefined
```

### 十、深拷贝
```
const deepCopy =(p,c){
	var c = c || {};

	for(var i in p){
		if(typeof p[i] === 'object'){
			c[i] = (p[i].constructor===Array)? [] : {};
			deepCopy(p[i],c[i])
		}else{
			c[i]=p[i]
		}
	}

	return c;
}

```

### 十一、手动触发一个dom事件，需要3步，document.createEvent

```
//创建事件

var enent = document.createEvent('Event');


//自定义一个事件 例如：sayByby


event.initEvent('sayByby',true,true)


//监听事件


ele = document.getElementsByClassName('ele')

ele.addEventListener('sayByby',function(){
		alert('satbyby');
	},false)


// 触发对象可以是任何元素或其他事件目标


elem.dispatchEvent(event);
```

### 十二、查找

```
方法一、二分法

function binarySearch(arr,key){

	var low = 0;

	var len = arr.length-1;
	
	while(low<=len){

		var mid = parseInt((low+len)/2);

		if(key==arr[mid]){

			return mid

		}else if(key>arr[mid]){

			low = mid + 1;

		}else if(key<arr[mid]){

			len = mid -1;

		}else{

			return -1
		}
	}
}

var arr=[1,2,3,4,5,6,7,8,9,10,11,23,44,86];

var result=binarySearch(arr,10);

alert(result); // 9 返回目标元素的索引值



方法二、递归算法


function binary_search(arr,low,high,key){

  if(low>high){

    return -1;

  }
  var mid=parseInt((high+low)/2);

  if(arr[mid]==key){

    return mid;

  }else if(arr[mid]>key){

    high=mid-1;

    return binary_search(arr,low,high,key);

  }else if(arr[mid]<key){

    low=mid+1;

    return binary_search(arr,low,high,key);

  }
};

var arr=[1,2,3,4,5,6,7,8,9,10,11,23,44,86];

var result=binary_search(arr,0,13,10);

alert(result); // 9 返回目标元素的索引值


三、冒泡排序:

function bubbleSort(arr) {

    var len = arr.length;

    for (var i = 0; i < len - 1; i++) {

        for (var j = i+1; j < len -1; j++) {

            arr[i] > arr[j] ? [arr[i],arr[j]]= [arr[j],arr[i]]:null;
        
        }
    }

    return arr;
}

bubbleSort([1,2,4,7,2,8,0,9,11,32,17])

console.log(bubbleSort([1,2,4,7,2,8,0,9,11,32,17]))

```

### 十三、setTimeout(0)

######
```
js 是单线程：

在js中分为:主线程--->从上到下执行，

异步队列（工作线程）--->当主线程执行完后，才执行。

setTimeout(0)--->在主线程执行完后，第一个执行的异步队列

```


### 十四、深拷贝
######
```
方法一、

function deepClone(obj){
    var cloneObj = Array.isArray(obj) ? [] : {};
    if(obj && typeof obj ==='object'){
        for(var i in obj){
            if(obj.hasOwnProperty(i)){
                if(obj[i]&&typeof obj[i] === 'object'){
                cloneObj[i] = deepClone(obj[i]);
            }else{
                cloneObj[i] = obj[i];
                }
            }  
        }
    }
    return cloneObj;
}

let a={arr:[1,2,3,4],name:'zz',child:{'name':'zz',age:22}},
    b=deepClone(a);

//console.log(b)


方法二、

function deepCopy(obj){
	var cloneObj = JSON.stringify(obj);
	return JSON.parse(cloneObj);
}

let a={arr:[1,2,3,4],name:'zz',child:{'name':'zz',age:22}},
    b=deepClone(a);

//console.log(b)

注意：JSON.stringify() 之坑

坑一、1、如果obj里面有时间对象，则JSON.stringify后再JSON.parse的结果，时间将只是字符串的形式。而不是时间对象；

example 1:

var a = { name:'zz',data:[new Date(1536627600000),new Date(1540047600000)]}

var b = JSON.parse(JSON.stringify(a));

console.log(a,b);

console.log(typeof a.data[0],typeof b.data[0]); //object  string


坑二、如果obj里有RegExp、Error对象，则序列化的结果将只得到空对象

example 2：

var a = {name:'zz',reg:new RegExp('\\w+'),err:new Error()}

var b = JSON.parse(JSON.stringify(a));

console.log(a,b);

console.log(a.reg,b.reg); // /\w+/ {}

console.log(a.err,b.err);// error对象  {}

坑三、如果obj里有函数，undefined，则序列化的结果会把函数或 undefined丢失

example 3：

var a = {name:'zz',sayName:function(){alert(123)}}

var b = JSON.parse(JSON.stringify(a));

console.log(a,b); //{name:'zz',sayName:function(){alert(123)}} {name:'zz'}

坑四、如果obj里有NaN、Infinity和-Infinity，则序列化的结果会变成null

example 4：

var a = {name:'zz',num1:NaN,num2:Infinity}

var b = JSON.parse(JSON.stringify(a));

console.log(a,b); //{name:'zz',num1:NaN,num2:Infinity} {name:'zz',num1:null,num2:null}

坑五、JSON.stringify()只能序列化对象的可枚举的自有属性，例如 如果obj中的对象是有构造函数生成的，  则使用JSON.parse(JSON.stringify(obj))深拷贝后，会丢弃对象的constructor

example 4：

function Person(name) {        
    this.name = name;        
    console.log(name)
}     
const test = new Person('liai');  

const a = { 
    name: 'a', 
    date: test,
};      
const b = JSON.parse(JSON.stringify(a));

console.log(a,b) 

坑六、如果对象中存在循环引用的情况也无法正确实现深拷贝
```
### 十五、POST与GET区别以及HTTP状态码详解
```
GET 请求在浏览器回退时是无害的，而POST会再次提交请求

GET 请求产生的URL地址是可以被收藏的，而POST不会

GET 请求会被浏览器主动缓存，而POST不会，除非手动设置

GET 请求只能进行URL编码，而POST支持多种编码方式

GET 请求参数会被完整保留在浏览器的历史记录里面，而POST中的参数不会被保留

GET 请求在URL总传送的参数是有长度限制的，而POST没有长度限制

GET 请求比POST更不安全，因为参数直接暴露在URL上，所以不能用来传敏感信息

GET 请求参数的数据类型只能是ASCII字符，而POST没有限制

GET 请求的参数通过URL传递，而POST放在Request body中


HTTP状态码详解：

1XX：指示信息---- 表示请求已经接受，继续处理

2XX：成功-------- 表示请求已被成功接收 200 OK  客户端请求成功

206  Partial Content ：客户发送一个带有Range头的GET请求，服务器完成了它，播放视频和音频

3XX：重定向------ 要完成请求必须进行更进一步的操作 

301  Move Permanently ： 所请求的页面已经转移到新的URL

302  Found ： 所请求的页面临时转移到新的URL

304  Not Modified ： 客户端有缓冲的文档并发出一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用

4XX：客户端错误--- 请求的语法错误或者请求无法实现 

400  Bad Request ： 客户端请求有语法错误，不能被服务端所理解

401  Unauthorized： 请求未经授权，这个状态码必须和WWW-Authenticate 报头域一起使用

403  Forbidden ： 对被请求的页面访问被禁止

404  Not Found ： 请求资源不存在

5XX：服务端错误---- 服务端未能实现合法的请求

500  Internal Server Error ： 服务端发生不可预期的错误原来缓冲的文档还能使用

503  Server Unavailable ： 请求完成，服务端临时过载或宕机，一段时间后恢复正常使用 



301适合永久重定向

　　301比较常用的场景是使用域名跳转。
       比如，我们访问 http://www.baidu.com 会跳转到 https://www.baidu.com，
        发送请求之后，就会返回301状态码，
       然后返回一个location，提示新的地址，浏览器就会拿着这个新的地址去访问。 
　　注意： 301请求是可以缓存的， 即通过看status code，可以发现后面写着from cache。
　    或者你把你的网页的名称从php修改为了html，这个过程中，也会发生永久重定向。

302用来做临时跳转
　　比如未登陆的用户访问用户中心重定向到登录页面。
　　访问404页面会重新定向到首页。


301—永久移动。被请求的资源已被永久移动位置； 【永久重定向】

302—请求的资源现在临时从不同的 URI 响应请求；  【临时重定向】

305—使用代理。被请求的资源必须通过指定的代理才能被访问； 

307—临时跳转。被请求的资源在临时从不同的URL响应请求； 

400—错误请求； 

402—需要付款。该状态码是为了将来可能的需求而预留的，用于一些数字货币或者是微支付； 

403—禁止访问。服务器已经理解请求，但是拒绝执行它； 

404—找不到对象。请求失败，资源不存在； 

406—不可接受的。请求的资源的内容特性无法满足请求头中的条件，因而无法生成响应实体； 

408—请求超时； 

409—冲突。由于和被请求的资源的当前状态之间存在冲突，请求无法完成； 

410—遗失的。被请求的资源在服务器上已经不再可用，而且没有任何已知的转发地址； 

413—响应实体太大。服务器拒绝处理当前请求，请求超过服务器所能处理和允许的最大值。 

417—期望失败。在请求头 Expect 中指定的预期内容无法被服务器满足； 

418—我是一个茶壶。超文本咖啡罐控制协议，但是并没有被实际的HTTP服务器实现； 

420—方法失效。 

422—不可处理的实体。请求格式正确，但是由于含有语义错误，无法响应； 

500—服务器内部错误。服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理；

```
### 十六、移动端touch事件(区分webkit 和 winphone)
#### 当用户手指放在移动设备在屏幕上滑动会触发的touch事件,以下支持webkit
```

touchstart——当手指触碰屏幕时候发生。不管当前有多少只手指

touchmove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用event的preventDefault()可以阻止默认情况的发生：阻止页面滚动

touchend——当手指离开屏幕时触发

touchcancel——系统停止跟踪触摸时候会触发。例如在触摸过程中突然页面alert()一个提示框，此时会触发该事件，这个事件比较少用TouchEvent

touches：屏幕上所有手指的信息

targetTouches：手指在目标区域的手指信息

changedTouches：最近一次触发该事件的手指信息

touchend时，touches与targetTouches信息会被删除，changedTouches保存的最后一次的信息，最好用于计算手指信息参数信息(changedTouches[0])

clientX、clientY在显示区的坐标

target：当前元素

```
### 十七、Object对象
```
Object.preventExtensions(obj)  让一个对象变的不可扩展，也就是永远不能再添加新的属性。

Object.isExtensible(obj) 判断一个对象是否是可扩展的

Object.seal(obj)让一个对象密封(只能读写 不能新增)

Object.isSealed(obj)判断一个对象是否密封

Object.isFrozen(arr)  让一个对象被冻结(只能读)

Object.isFrozen(obj)：判断一个对象是否被冻结

Object.keys(obj) 返回一个由给定对象的所有可枚举自身属性的属性名组成的数组

Object.getOwnPropertyNames(obj)
：返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性）组成的数组

Object.is(value1, value2)：判断两个值是否是同一个值,Object.is它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

Object.create(proto [, propertiesObject ]) 是E5中提出的一种新的对象创建方式，第一个参数是要继承的原型，如果不是一个子函数，可以传一个null，第二个参数是对象的属性描述符，这个参数是可选的。

Object.assign 把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。【浅复制】

//var copy = Object.assign({}, obj);

Object.defineProperty() 定义单个对象属性或方法(可以设置读写可枚举)

Object.defineProperties() 定义多个对象属性或方法(可以设置读写可枚举)

Object.assign() //浅拷贝，类似{...obj1,...obj2} 都是浅拷贝

Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）

var target = { a: 1 };

var source1 = { b: 2 };

var source2 = { c: 3 };

Object.assign(target, source1, source2);

target // {a:1, b:2, c:3}

//如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

var obj1 = {a: {b: 1}};

var obj2 = Object.assign({}, obj1);

obj1.a.b = 2;

obj2.a.b // 2

对于这种嵌套的对象，一旦遇到同名属性，Object.assign的处理方法是替换，而不是添加。

var target = { a: { b: 'c', d: 'e' } }

var source = { a: { b: 'hello' } }

Object.assign(target, source)

// { a: { b: 'hello' } }

```










<!-- <meta http-equiv="refresh" content="1"> -->