# 作用
node环境下的类型检测工具模块

# 安装
```javascript
npm install is-type-of
```

# 使用方式
```javascript
var is = require('is-type-of');

// 是否是数组
is.array([1]); // => true

//是否是基本类型
is.primitive(true); // => true
is.primitive({}); // => false

//是否是generate 函数
is.generatorFunction(function * () {}); // => true

//是否是long类型
is.long(Math.pow(2, 33)); // => true

//是否是double类型
is.double(0); // => false
```

# 模块依赖
- [core-util-is](https://github.com/isaacs/core-util-is)
- [is-stream](https://github.com/rvagg/isstream)
- [is-class](https://github.com/miguelmota/is-class)

# 主要工具API

### From [core-util-is](https://github.com/isaacs/core-util-is)

#### is.array(arr)

#### is.boolean(bool)

#### is.null(null)

#### is.nullOrUndefined(null)

#### is.number(num)

#### is.string(str)

#### is.symbol(sym)

#### is.undefined(undef)

#### is.regExp(reg)

#### is.object(obj)

#### is.date(date)

#### is.error(err)

#### is.function(fn)

#### is.primitive(prim)

#### is.buffer(buf)

### from [is-stream](https://github.com/rvagg/isstream)

#### is.stream(stream)

#### is.readableStream(readable)

#### is.writableStream(writable)

#### is.duplexStream(duplex)

### from [is-class](https://github.com/miguelmota/is-class)

#### is.class(obj)

### Extend API

#### is.finite(num)

#### is.NaN(NaN)

#### is.generator(gen)

#### is.generatorFunction(fn)

#### is.promise(fn)

#### is.int(int)

#### is.double(double)

#### is.int32(int)

#### is.long(long)

#### is.Long(Long)

  * Support [Long](https://github.com/dcodeIO/Long.js) instance.



# 实战提示1

```javascript
  use(fn) {
    assert(is.function(fn), 'app.use() requires a function');
    debug('use %s', fn._name || fn.name || '-');
    this.middleware.push(utils.middleware(fn));
    return this;
  }
```

以上代码摘自egg.js框架，即 中间件必须是一个函数

# 实战提示2

此模块虽然依赖于：
- [core-util-is](https://github.com/isaacs/core-util-is)
- [is-stream](https://github.com/rvagg/isstream)
- [is-class](https://github.com/miguelmota/is-class)

但是也扩展或重写一些方法。
而且，模块中所有方法都是通过 exports.name 直接导出的。
这样 `var is = require('is-type-of');` 后，可以通过is.name 直接使用

### 例如：

```javascript
// 源码
var isClass = require('is-class');
exports.class = isClass;
// 使用方式
var is = require('is-type-of');
class F {}
function G() {}

console.log(is.class(F)); // true
console.log(is.class(G)); // false
```

# 实战提示3, 万能的toString

我们知道jquery 源码中的type方法使用了toString 方法检测对象的类型。

```javascript
 // 部分源码，具体查看完整源码
 function type( obj ) {
	if ( obj == null ) {
		return obj + "";
	}
	// Support: Android <=2.3 only (functionish RegExp)
	return typeof obj === "object" || typeof obj === "function" ?
		class2type[ toString.call( obj ) ] || "object" :
		typeof obj;
	}
}
``` 
其实toString，除了可以检测对象的类型，还可以检测 ES6中class 的类型, 而且适用于node和browser 环境

```javascript
var objToString = Object.prototype.toString;
var fnToString = Function.prototype.toString;

function Log(str){
    console.log(str);
}
class A {}
function B() {}
var C = [2,3];
var D = new Date();


Log(objToString.call(A));
Log(fnToString.call(A));

Log(objToString.call(B));
Log(fnToString.call(B));

Log(objToString.call(C));
Log(objToString.call(D));

// 输出结果如下：
// [object Function]
// class A {}
// [object Function]
// function B() {}
// [object Array]
// [object Date]

```

# 实战提示4
ES6 ，ES7中新增了generateor 函数和 async 函数，如何检测呢? 很简单，看源码：

```javascript
exports.generatorFunction = function (obj) {
  return obj
    && obj.constructor
    && 'GeneratorFunction' === obj.constructor.name;
};

exports.asyncFunction = function (obj) {
  return obj
    && obj.constructor
    && 'AsyncFunction' === obj.constructor.name;
};
```


