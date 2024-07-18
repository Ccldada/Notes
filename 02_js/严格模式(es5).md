# 为什么要使用严格模式

1. 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为。
2. 消除代码运行的一些不安全之处，保证代码运行的安全；
3. 提高编译器效率，增加运行速度；
4. 为未来新版本的Javascript做好铺垫。
"严格模式"体现了Javascript更合理、更安全、更严谨的发展方向。另一方面，同样的代码，在"严格模式"中，可能会有不一样的运行结果。一些在"正常模式"下可以运行的语句，在"严格模式"下将不能运行。掌握这些内容，有助于更细致深入地理解Javascript，让你变成一个更好的程序员。

# 严格模式声明

严格模式通过在脚本或函数的头部添加 use strict 表达式来声明。比如：

```javascript
<body>
    <script>
        "use strict"
        //全局严格模式

        function demo(){
            "use strict"
            //函数严格模式
        }
    </script>
</body>
```
# 严格模式的限制

在严格模式下，更多非严格模式下的js使用方式将得到限制。

## 不允许使用未声明的变量

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        a = 100; //报错

        function demo(){
            "use strict"
            //函数严格模式
            b = 200; //报错
        }
        demo()
    </script>
</body>
```
## 不允许删除变量或者对象

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var a = 100;
        delete a; //报错
    </script>
</body>
```

delete操作符用于删除对象中的某一个键值对的引用

## 不允许删除函数

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var fun = function(){

        }
        delete fun
    </script>
</body>
```

## 不允形参重名

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var fun = function(x,x){

        }
        fun(1,2)
    </script>
</body>
```

## 不允许使用八进制

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var a = 010;
    </script>
</body>
```
## 不允许对只读属性赋值

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        const obj = {};
        Object.defineProperty(obj, 'age', {
            value: 50,
            writable: false
        });
        obj.age = 100;
        console.log(obj)
    </script>
</body>
```

Object.defineProperty方法用于给一个对象定义一个新属性，或者修改一个现有属性，可以设置只读属性。

## 不允许对一个使用get方法读取的属性进行赋值

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        const obj = {
            name : 'edward',
            get objName(){
                return this.name;
            }
        };
        obj.objName = 'eric';
        console.log(obj)
    </script>
</body>
```

## 不允许删除一个不允许删除的属性

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        delete Object.prototype;
    </script>
</body>
```

## 变量名不能使用eval字符串

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var eval = 100;
    </script>
</body>
```

## 变量名不能使用arguments字符串

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var arguments = 100;
    </script>
</body>
```

## 由于一些安全原因，在作用域 eval() 创建的变量不能被调用

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        eval("var a = 100");
        console.log(a)
    </script>
</body>
```

## 禁止this关键字指向全局对象

```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var fun = function(){
            console.log(this)
        }
        fun()
    </script>
</body>
```

# 保留关键字

为了向将来Javascript的新版本过渡，严格模式新增了一些保留关键字

* implements
* interface
* let
* package
* private
* protected
* public
* static
* yield
* class
```xml
<body>
    <script>
        "use strict"
        //全局严格模式
        var yield = 100; //报错
    </script>
</body>
```




