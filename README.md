# Js-colsure

##### JS闭包

闭包，官方对闭包的解释是：

`一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数）`

直白解释：

` 闭包指的是：能够访问另一个函数作用域的变量的函数`

`闭包就是一个函数，这个函数能够访问其他函数的作用域中的变量`

特点：

- 作为一个函数变量的一个引用，当函数返回时，其处于激活状态。
- 一个闭包就是当一个函数返回时，一个没有释放资源的栈区

代码示例：

```javascript
function outer(){
    var a = '变量1';
    var inner = function(){
        console.log(a);
    }
    // inner 就是一个闭包函数，因为它能够访问到outer函数的作用域
    return inner;
}
```

简单闭包

```javascript
function A(){
    function B(){
        console.log('Hello,Closure!');
    }
    return B；
}
var C =A();

上面就是最简单的闭包
翻译代码：
	定义普通函数A
    在A中定义普通函数B
    在A中返回B
    执行A，并把A返回结果赋值给变量C
    执行C
--总结
--函数A的内部函数B被函数A外的一个变量C引用
当一个内部函数被其他外部函数之外的变量引用时，就形成了一个闭包。

```

###### 闭包的用途

`当我们需要在模块中定义一些变量，并希望这些变量一直保存在内存中但又不会‘污染’全局变量时，就可以用闭包来定义这个模块`

```javascript
简单了解 JavaScript 中的GC机制：
在JavaScript中，如果一个对象不再被引用，那么这个对象就会被GC回收，
否则这个对象一直保存在内存中。
--例子
function A(){
    var count = 1;
    function B(){
        console.log('我运行了' + count + '次');
    }
    return B;
}

var C = A();
C();// 我运行了1次
C();// 我运行了2次
```

count 是函数A中的一个变量，它的值在函数B中被改变，函数B每执行一次，count的值就在原来的基础上累加1,。因此，函数A中的count变量会一直保存在内存中。



##### 闭包的高级写法

```javascript
(function(document){
    var viewport;
    var obj = {
        init:function(id){
            viewport = document.querySelector('#' + id);
        },
        addChild:function(child){
            viewport.appendChild(child);
        },
        reoverChild:function(child){
            viewport.removerChild(child);
        }
    }
    window.jView = obj;
})(document);
```

上面的代码解释

```javascript
var f = function(document){
    var viewport ;
    var obj = {
        init:function(id){
            viewport = document.querySelector('#' + id);
        },
        addChild:function(child){
            viewport.appendChild(child);
        },
        removeChild:function(child){
            viewport.removeChild(child);
        }
    }
    window.jView = obj;
}

f(document);
```

`window.jView = obj;`

```html
obj 是在函数f中定义的一个对象，
这个对象中定义了一系列方法，执行 window.jView = obj 就是在 window 全局对象定义了一个变量 jView，
并将这个变量指向 obj 对象，
即 全局变量 jView引用了 obj，而 obj对象中的函数又引用了函数f 中的变量 viewport，因此函数 f中的viewport 不会被GC回收，viewport会一直保存到内存中。所以形成闭包的条件。
```


