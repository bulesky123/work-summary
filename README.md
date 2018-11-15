##一、reduce(fn,initValue) 基本用法
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

##二、animation 基本用法
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

##三、重放攻击：重复的会话请求就是重放攻击
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

## 五、Object.assign的实现：
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

##六、浏览器判断

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

##七、数组的去重
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

##八、将重复个数排列
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






<!-- <meta http-equiv="refresh" content="1"> -->