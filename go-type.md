## 类型
* 布尔类型 bool  长度：1字节 取值范围：true，false
* 整形 int/uint 根据运行平台可能为32或者64
* 8为整形 int8/uint8 长度：1字节 取值范围：-128～127 / 0~255
* 字节 byte （uint8的别名）
* 16位整形：int16/uint16 长度：2字节 取值范围：
* 32位整形：int32（rune）/uint32 长度：4字节 取值范围：
* 64位整形：int64/uint64 长度：8字节 取值范围:
* 浮点型：float32/float64 长度：4字节/8字节 小数位：精确到7/15小数位
* 复数：complex64/complex128 长度：8字节/16字节
* 足够保存指针的32位或64位整数型：unitptr

* 其他类型：<br/>
    -array，struct，string
* 引用类型：<br/>
    -slice，map，chan
* 接口类型：interface
* 函数类型：func

## 类型零值
* 零值并不等于空值，而是当变量被声明为某种类型后的默认值.
```js
整数类型： 0
boo： fals
string: ""
引用类型： nil

如：
var a int
fmt.Println(a) 输出：0

var b bool
fmt.Println(b) 输出：false

var c string
fmt.Println(c) 输出：“”

var slice []int
fmt.Println(slice) 输出：[]
fmt.Println(slice == nil) 输出：true
```
## 变量的声明和赋值
```js
var a int //变量的声明
a = 100 //变量的赋值

//变量声明同时赋值
var b int = 200

//由系统自动判断类型
var c = 300

//自动声明变量，并且自动推断类型，当该变量已经声明，就不能使该方式
d := 50
```
## 多变量的声明与赋值
* 全局变量的声明可使用var()的方式进行简写, 局部变量不可使用var()方式
```js
var (
	//使用常规方式
	aaa = "hello"
    //使用并行方式以及类型推断
    x, y = 1, 2
)
```
* 并行变量声明
```js
//多个变量的声明
var a, b, c, d, e int
//多个变量的赋值
a, b, c, d, e = 1, 2, 3, 4, 5
//多个变量声明同时赋值
var a, b, c int = 1, 2, 3
//多个变量的声明与赋值简写法
a, b, c := 1, 2, 3
```
## 变量类型的转换
* go中不存在隐式转换，所有类型转换必须显示声明
* 类型转换只能发生在相互兼容的类型之间
```
//将变量b转换为int类型，并且赋值给变量a
int a = int(b)

//在互相兼容的两个类型之间转换
var float32 f = 100.1
b := int(f)

//以下为错误的方式，两个变量类型不兼容
var bool boo = true
coo := int(boo)
```
