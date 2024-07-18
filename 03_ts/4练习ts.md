1. 类型1
```typescript
// 也可以直接使用自变量进行类型声明
let a: 10;
a = 10;
// a= 11  报错

// 可以使用  |  来连接多个类型（联合类型）
let b: "male" | "female";
b = "male"
b = "female"
// b = "another"  报错

let c: string | number
c = 123;
c = '123'
// c=[]  报错

// any任意类型，相当于关闭类型检测，尽量不用
// let d; 隐式any
let d: any
d = 10
d = '123'
// any 可以复制给任意变量，不会报错，相当于关闭s的类型检测
let s: string
s = d;

// 声明变量是如果不指定类型，ts会自动判断变量类型，（隐式）

// unknown 表示未知类型的值
let e: unknown;
e = 10;
e = 'qwe';
// s = e  报错
// unknown 实际是一个类型安全的any
// unknown 类型的变量，不能直接赋值给其它变量
if (typeof e === "string") {
    s = e;
}

// 类型断言，可以用来告诉解析器变量的实际类型,
/**
 * 语法：
 * 变量 as 类型
 * <类型> 变量
 */
s = e as string;
s = <string>e;

// 函数
// void 表示空，函数里面表示没有返回值
function fn() :void{
    // return true
}
// never 表示永远不会返回结果（报错时使用）
function fn2():never{
    throw new Error('报错了')
}

export { }
```
2. 类型2
```typescript
// object 表示一个js对象,太广泛
let a: object
a = {}
a = []
a = function () { }

// {} 用来指定对象中可以包含哪些属性
// 语法：{属性名：属性值，属性名：属性值}
// 属性名？：属性值，表示这个属性是可选的
let b: { name: string, age?: number }
b = { name: "李四" }

// [propName: string] 任意字符串属性名，any 任意类型
let c: { name: string, [propName: string]: any }
c = { name: '张三', age: 18, gender: '男' }

/**
 * 设置函数结构的类型声明：
 * 语法：(形参：类型，形参：类型，...)=> 返回值
 */
let d: (a: number, b: number) => number;
d = function (n1, n2): number {
    return n1 + n2;
}

/**
 * 数组的类型声明
 * 类型[]
 * Array<类型>
 */

// string[] 字符串数组
let e: string[]
e = ['q', 'w']
// number[] Array<number> 数字数组
let f: number[]
f = [1, 2, 3, 2]
let g: Array<number>
g = [123, 123, 122]
// 
let h: {}[]
h = [{ id: 1 }, { id: 2 }, '1232']

/**
 * 元祖，固定长度的数组
 * 语法：[类型，类型] 长度为2
 */
let i: [string, string];
i = ['123', 'q']

/**
 * enum 枚举
 */
enum Gender {
    Male = 0,
    Famale = 1,
}
// let j:{name:string,gender:0|1}
// j = {name:'李四',gender:1}
let j: { name: string, gender: Gender }
j = { name: '李四', gender: Gender.Male }
console.log(j.gender === Gender.Male) // true

// & 且
let k: { name: string } & { age: number }
k = { name: '张', age: 18 }

// 类型的别名
type myType = string
let l: myType
type myType2 = 1 | 2 | 3 | 4
let m: myType2
m = 1

export { }
```
3. 编译选项
```typescript
 /*
    tsconfig.json 是ts编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译
    "include" 用来指定哪些ts文件需要被编译
      路径：** 表示任意目录 *表示任意文件
    "exclude" 不需要被编译的文件
      默认值：["node_modules", "bower_components", "jspm_packages"]
    "compilerOptions" 
      target ts编译es版本
      module 指定模块化规范
      lib 用来指定项目中使用的库
      outDir 指定编译后文件所在的，目录
      outFile 将代码合并为一个文件，所有的全局作用域中的代码会合并到同一个文件中
      allowJs 是否对js文件进行编译，默认false
      checkJs 是否检测js代码是否符合代码规范
      removeComments 是否移除注释
      noEmit 不生成编译后的文件
      noEmitOnError 有错误不生成编译后文件
      alwaysStrict 用来设置编译后文件是否使用严格模式，默认false
      noImplicitAny 不允许隐式any
      noImplicitThis 不允许不明确类型的this
      strict 所有严格检测
      strictBindCallApply 严格检查bind、call和apply的参数列表
      strictFunctionTypes 严格检查函数的类型
      strictNullChecks 严格的空值检查
  */
```
4. webpack打包
```typescript
// 引入一个包
const path = require('path');
// webpack中的所有配置信息都应该写在moudule.export中
module.exports = {
    // 指定入口文件
    entry: './src/index.ts',
    // 指定打包文件所在目录
    output: {
        // 指定打包文件目录
        path: path.resolve(__dirname, 'dist'),
        // 打包后文件的文件
        filename: "bundle.js"
    },
    
    "mode": 'development',
    // 指定webpack打包是要使用的模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // 指定要加载的规则
                test: /\.ts$/,
                // 要使用的loader
                use: 'ts-loader',
                // 要排除的文件
                exclude: /node-modules/
            }
        ]
    }
}
```
配置
```typescript
// 引入一个包
const path = require('path');
// 引入html插件
const HTMLWebpackPlugin = require('html-webpack-plugin')
// 引入clean插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

// webpack中的所有配置信息都应该写在moudule.export中
module.exports = {
    // 指定入口文件
    entry: './src/index.ts',
    // 指定打包文件所在目录
    output: {
        // 指定打包文件目录
        path: path.resolve(__dirname, 'dist'),
        // 打包后文件的文件
        filename: "bundle.js"
    },

    "mode": 'development',
    // 指定webpack打包是要使用的模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // 指定要加载的规则
                test: /\.ts$/,
                // 要使用的loader
                use: 'ts-loader',
                // 要排除的文件
                exclude: /node-modules/
            }
        ]
    },
    // 配置webpack插件
    plugins: [
        new CleanWebpackPlugin(),
        new HTMLWebpackPlugin({
            // title:"自定义title"
            // 模板
            template: './src/1.html'
        }),
    ],
    // 用来设置引用模块
    resolve:{
        extensions:['.ts','.js']
    }
}
```
package
```typescript
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack --mode development",
    "start": "webpack serve --open chrome.exe"
  },
```

5. 类
```typescript
// class关键字来定义一个类
/**
 * 对象主要包含两部分：
 *      属性{
 *             实例属性，实例属性需要通过创建对象来访问，
 *             使用static关键字可以定义类属性（静态属性），可以直接通过类访问
 *              readonly 只读属性，不可以修改，和static使用要放后面
 *          }
 *      方法 实例方法和静态方法，和对象相似
 */

class person {
    // 定义属性
    // (实例属性)
    readonly name: string = '张三';
    // age: number = 12

    // 在属性面前使用static关键字可以定义类属性（静态属性）
    // 不需要创建类就能使用的属性
    static age: number = 14

    // 定义方法
    static say(){
        console.log("hi")
    }
}

const per = new person()
// console.log(per.age)  // 实例属性需要通过创建对象来访问
console.log(person.age) // 静态属性可以直接通过类访问

// per.say()  // 通过对象调用
person.say() // 直接通过类访问

```
6. 构造函数
```typescript
class Dog {
    // 属性
    name:string
    age:number

    // constructor 称为构造函数
    // 构造函数会在对象创建时被调用
    constructor(name:string,age:number){
        // 在实例方法中，this就表示当前的实例
        // 在构造函数中，当前的对象就是新建的那个对象
        // 可以通过this向新建的对象中添加属性
        
        this.name = name;
        this.age = age
        console.log(this) // 相当于console.log(dog) console.log(dog2)
    }
    bark(){
        alert("wangwang!!")
    }
}

const dog = new Dog("小白",4)
const dog2 = new Dog("小嘿",2)
console.log(dog)
console.log(dog2)
```
7. 继承
```typescript
(function () {
    // 继承
    
    // 狗
    // class Dog {
    //     // 属性
    //     name: string;
    //     age: number;
    //     // 构造函数
    //     constructor(name: string, age: number) {
    //         this.name = name
    //         this.age = age
    //     }
    //     // 方法
    //     say() {
    //         console.log("wwwww!")
    //     }
    // }
    // const dog1 = new Dog("小小黑", 12)
    // console.log(dog1)
    // dog1.say()
    /**
     *  Dog extends Animal 
     *      此时Animal称为父类，Dog成为子类，
     *      继承后，子类拥有父类所有方法
     *      通过继承可以将多个类中共有的代码写在一个父类中
     *          如果子类需要添加父类中没有的属性和方法，直接添加
     *      如果子类中添加了父类相同的方法，则子类方法会覆盖父类方法
     */
    class Animal {
        // 属性
        name: string;
        age: number;
        // 构造函数
        constructor(name: string, age: number) {
            this.name = name
            this.age = age
        }
        // 方法
        say() {
            console.log("叫")
        }
    }
    // 狗
    class Dog extends Animal {
        run(){
            console.log(`${this.name}在跑`)
        }
        say(): void {
            console.log(`wwww`)
        }
    }

    // 猫
    class Cat extends Animal {

    }

    const dog1 = new Dog("小小黑", 12)
    console.log(dog1)
    dog1.say()
    dog1.run()
    const cat1 = new Cat("小小白", 12)
    console.log(cat1)
    cat1.say()

})()
```
8. super
```typescript
(function () {
    class Animal {
        // 属性
        name: string;
        age: number;
        // 构造函数
        constructor(name: string, age: number) {
            this.name = name
            this.age = age
        }
        // 方法
        say() {
            console.log("叫")
        }
    }
    // 狗
    // super 在类的方法中 super就表示当前类的父类
    class Dog extends Animal {
        sex : string
        // 如果子类中写了构造函数，在子类构造函数中必须对父类的构造函数进行调用
        // 参数也是得重写
        constructor(name: string, age: number,sex:string ) {
            super(name,age) // 父类的构造函数进行调用
            this.sex = sex
        }
        say(): void {
            super.say()  // 父类方法
        }
    }
    const dog1 = new Dog("小小黑", 12,"公")
    console.log(dog1)
    dog1.say() // 父类的say 

})()
```
9. 抽象类
```typescript
(function () {
    /**
     * 以abstract开头的类是抽象类
     *      抽象类和其他类区别不大，只是不能用来创建对象
     *      抽象类就是专门被用来继承的
     *      抽象类中可以添加抽象方法 abstract say() :void
     *          抽象方法只能定义在抽象类中，子类必须对抽象方法进行重写
     */ 
    abstract class Animal {
        name: string;
        constructor(name: string) {
            this.name = name
        }
        abstract say() :void
    }
    // 狗
    class Dog extends Animal {
        sex : string
        constructor(name: string,sex:string ) {
            super(name)
            this.sex = sex
        }
        say() {
            console.log("www")
        }
    }
    const dog1 = new Dog("小小黑","公")
    console.log(dog1)
    dog1.say() 

})()
```
10. 接口
```typescript
(function(){
    // 描述一个对象的类型
    type myType = {
        name : string,
        age :number
    }
    const obj:myType = {
        name :'sss',
        age : 12
    }
    /**
     *  接口用来定义一个类的结构
     *  用来定义一个类中应该包含哪些属性和方法
     *      同时也可以当成类型声明去使用
     *      可以重复声明，会叠加
     *  接口可以在定义类的时候限制类的结构
     *      接口中所以属性都不能有实际的值
     *      接口之定义对象的结构，而不考虑实际值
     *      接口中所有方法都是抽象方法
     *  定义类时，可以使用类去实现一个接口
     *      实现接口就是使类满足接口的要求,实现所有类和方法，还有构造函数
     * ts独有
     */
    interface myTypeface{
        name :string,
        age :number
    }

    const obj2:myTypeface = {
        name :'sss',
        age : 12
    }
    // 类和接口
    interface myInter {
        name:string ;
        say():void
    }
    class MyClass implements myInter{
        name: string;
        constructor(name:string){
            this.name = name
        }
        say(): void {
            console.log('hi')
        }
    }
})
```
11. 属性的封装
数据变安全

```typescript
(function(){
    /**
     * 属性是在对象中设置的，属性可以任意的修改
     *      会导致数据变得不安全
     * 
     */

    /**
     *  ts可以在属性前添加属性的修饰符
     *      public 修饰的属性可以在任意位置访问修改 默认值 包括子类
     *      private 私有属性， 私有属性只能在类内部访问（修改） 需要配置设置   "noEmitOnError": true
     *          通过在类中添加方法是的私有属性可以被外部访问，子类不能访问
     *      protected 受保护的属性，只能在当前类和子类中访问（修改）
     */

    /**
     *  getter 方法用来读取属性
     *  setter 方法用来设置属性
     *      被称为属性的存取器
     */
    class Person{
        private _name:string;
        private _age:number;

        constructor(name:string,age :number){
            this._name = name;
            this._age = age
        }
        // 定义方法
        getName(){
            return this._name
        }
        setName(value:string){
            // 可以设置是否能修改
            this._name = value
        }
        // ts设置getter方法的方式
        get name(){
            return this._name
        }
        set name(a:string){
            this._name=a
        }
    }
    const per = new Person("张三",8)
    per.getName()
    // ts get set 使用
    per.name
    per.name='lisi'
    console.log(per)

    // 另一种写法
    // class C {
    //     name :string
    //     constructor(name:string){
    //         this.name = name
    //     }
    // }
    class C {
        constructor(public name:string){

        }
    }
    const c = new C('lisi')
})
```
12. 泛型
```typescript
/**
 * 在定义函数或者类时，如果遇到类型不明确就可以使用泛型
 * T 任意类型
 * 泛型可以同时指定多个
 * T extends Inter 表示泛型T必须是 Inter 实现类
 */
// T 泛型，函数调用才确定类型
function fn<T>(name:T):T{
    return name
}
// 可以直接调用含有泛型的函数，返回就是string
// 不指定泛型，ts自动对类型进行推断
fn('lisi')
// 可以手动指定类型
fn<number>(112)

function fn2<T,K>(name:T,age:K):T{
    console.log(age)
    return name
}
fn2('llisi',123)
fn2<string,number>('llisi',123)

// 限制泛型的种类
interface Inter{
    length:number
}
// a的输入得有长度
function fn3<T extends Inter>(a:T):number{
    return a.length
}
fn3('qwe')
// fn3(12) 报错

// 类
class MyEn<T>{
    name :T
    constructor(name:T){
        this.name = name
    }
}
const mc = new MyEn(123)
const mc2 = new MyEn<string>('lisi')
```













.

