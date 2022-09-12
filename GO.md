## Golang

## 数据类型

### 基础数据类型

```go
bool		 1字节		默认值：false
byte		 1字节		默认值：unint8，取值范围[0,255]
rune		 4字节		unicode ponit code int32
int，uint     4或8字节   取决于操作系统，32或64
int8,uint8	  1字节	 -128~127，0~255
int16，uint16 2字节	
float32
float64
complex64				  对应的实部或者虚部：float64
complex128				  对应的实部或者虚部：float128
uintptr
```

### 复合数据类型

```go
array		默认值：无		值类型
struct		默认值：无		值类型
string		默认值：""		 utf-8字符串
slice		默认值：nil		 引用类型
map			默认值：nil		 引用类型
channel		默认值：nil		 引用类型
interface	默认值：nil		  接口
function	默认值：nil		  函数
```

### 指针

```go
指针方式
a := 123
var poniter unsafe.Pointer = unsafe.Pointer(&a)
var p  uintptr = uintptr(poniter)
var ptr *int = &a
```

### 自定义类型

#### 类型别名

```go
type myint = int8
type myfloat = float32
```

#### 自定义类型

```go
type ms[string]string
type add func(a,b int)int
type user struct {name:int;age:int}
```

#### 结构体

```go
type User struct{
    Name string
    Age int
}
func (ms User) hello(){
    fmt.Printf("my name is %s/n",ms.Name)
}
//赋值
var u User
u = User{
	"tom",
     18
}
u.hello()
```

### 字符串

```go
s :="my name " //字符串可以包含Unicode字符
s1 :="my name \\" //包含转义字符
s2 :=`name    hello`//原封不动输出，包括空白符合换行符
```

###### 格式化输出

```go
 		   %v,原样输出
            %T，打印类型
            %t,bool类型
            %s，字符串
            %f，浮点
            %d，10进制的整数
            %b，2进制的整数
            %o，8进制
            %x，%X，16进制
                %x：0-9，a-f
                %X：0-9，A-F
            %c，打印字符
            %p，打印地址
```



#### 常用API

```go
len(str)							求长度
strings.Split						分割
strings.Contains					判断是否包含
strings.HasPrefix,strings,HasSuffix   前缀后缀判断
strings.Index(),strings.LastIndex     寻找子字符串出现的位置
//拼接
func fmt.Sprintf(format string ,a ...interface{}) string
//参数一：返回拼接后的字符串
func strings.Join(elems []string,sep string) string
//参数二：字符串数组，中间的连接符
//当有大量字符串进行拼接的时候，使用builders
```

```go
string每个元素叫做字符，字符有两种
	byte：1字节，代表ASCILL码的一个字符
	rune：4个字节，一个代表一个UTF-8字符，一个汉字代表一个rune
string底层是byte数组，string的长度就是byte数组的长度
string可以转换成byte[]，或者rune[]类型
string是常量，不可被修改
//
func main() {
	s := "hahaha 123 www"
	arr := []byte(s)
	for _, ele := range arr {
		fmt.Printf("%d", ele)
	}
	fmt.Print("arr len %d,s len %d\n", len(arr), len(s))
}
//
func main() {
	s := "1一"
    //字符串长度
    one :=len([]rune(s))
    //[49,228,184,128]
    two :=len([]byte(s))
}
```

###### split

```go
func str_spl(){
   	s := "hello go"
	arr := strings.Split(s, "o")
	fmt.Println(arr)
}
```

###### Contains

```go
func str_spl(){
   	s := "hello go"
	arr := strings.Contains(s, "o")
	fmt.Println(arr)
}
```

###### HasPrefix,HasSuffix

```go
func str_spl() {
	s := "hello go"
	arr := strings.HasPrefix(s, "e")
	fmt.Println(arr)
}
func str_spl() {
	s := "hello go"
	arr := strings.HasSuffix(s, "o")
	fmt.Println(arr)
}
```

###### Index

```go
func str_spl() {
	s := "hello go"
	arr := strings.Index(s, "o")
	fmt.Println(arr)
}
```

###### 字符串拼接

```go
func add() {
	s := "hello"
	t := "ni"
	p := "hao"
	sub1 := s + "" + t + "" + p
	sub2 := fmt.Sprintf("%s %s %s", s, t, p)
	sub3 := strings.Join([]string{s, t, p}, "")
	sub4 := strings.Builder{}
	sub4.WriteString(s)
	sub4.WriteString("")
	sub4.WriteString(t)
	sub4.WriteString("")
	sub4.WriteString(p)
	sub5 := sub4.String()
	fmt.Println(sub1)
	fmt.Println(sub2)
	fmt.Println(sub3)
	fmt.Println(sub5)

}
```

###### 强制类型转换

```go
byte和int可以相互转换
float和int可以转换，小数位会丢失
bool和int不能相互转换
不同长度的int或float转换
string可以转换成[]byte或[]rune类型
低精度向高精度转换可以，高向低转换丢失位数
无符号向有符号转换，最高位是符号位
```

###### 丢失例子

```go
func test() {
	var ua uint64 = math.MaxUint64
	fmt.Printf("ua=%d %b\n", ua, ua)
	ue := uint8(ua)
	fmt.Printf("ue=%d %b\n", ue, ue)
	ub := uint64((ue))
	fmt.Printf("ub=%d %b\n", ub, ub)
}
//ua=18446744073709551615 1111111111111111111111111111111111111111111111111111111111111111
//ue=255 11111111
//ub=255 11111111
//发生丢失
```

