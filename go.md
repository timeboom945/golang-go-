转义字符
\t制表符
\n换行符
\\就是\
\r回车(非换行)
fmt.PrintLn("你好和覅和\r你哈")->你哈和覅和

cd ..返回上级目录
go mod init gomod
var i int
i := 0  //声明与赋值的简写
var n1,n2,n3  int
var n1,n2,n3

utf-8编码中，英文一个字节，汉字三个字节
不可以用o或者非0的整数来替代ture或者false

go的字符串不可变
不是数组

go必须显式数据类型转换
%c(输出字符),%v(输出值),%T(输出类型),%d(十进制有符号数)
%f为十进制浮点数，默认保留小数点六位
转换溢出不会报错，按溢出处理

go数据类型转换都会用到函数

值类型：int，int8等、float、bool、struct、数组、string
引用类型：指针、slice切片、map、管道chan、interface
引用类型存储指向的是一个地址，地址指向空间的才是真正存储数据的空间
####值类型在栈内分配
####引用类型在堆上分配，就是指针的地址指向堆，堆中存储切片等引用类型
####值传递与引用传递(两种调用方式)，值类型也可以被引用传递

package的名字要和目录名字保持一致

标识符首字母小写不能被导入其他文件
标识符大写能够跨包使用

i++是原子性操作能保证数据安全

短路与&&
短路或||

运算符的优先级
最高()[]++--、->

八进制前面有个0

原码反码补码
最高位表示正负，0表示正数，1表示负数
正数的原码，反码补码都一样
负数的反码等于其符号位不变，其他位取反
负数的补码等于反码加一
0的反码补码都是0

计算机运算的时候，都是以补码的方式运行
在遇到负数时候一定要返回原码看结果

移位运算<<和>>
<<对补码符号位不变，低位补0；；>>对补码符号位不变，低位溢出，用符号位补高位

if内必须有条件表达式

switch后接表达式即可，即有返回值如b,10+20,a+20,fun()
case后的数据类型必须和switch后保持一致
case后用逗号连接多个表达式，表示或
switch也可不带表达式，当if。。else用
switch关键词fallthrough
```go
//生成一个四周都是1的3*4二维数组
var arr [3][4]int
for i:=0;i<3;i++{
	switch i {
	case 0,2:for j:=0;j<4;j++{
		arr[i][j]=1
		}
	case 1:	        arr[0]=1
			arr[3]=1
	}
}
fmt.Println(arr)
```

随机数生成
rand.Intn(100)->[1,100)
rand.Seed(time().Now().Unix())

字符串遍历

break和continue标签的运用
lable，here。。


return也是用来控制流程的
return表示跳出所在的方法或函数
```go
func main(){
    语句
        return//直接跳出func
}
```


包的作用——在不同的文件中调用函数
函数首字母大写

```go
func funcName(n nType,m mType,...)(returnValueType){
    函数语句
}
```




---
golang函数所有基本数据类型(非引用类型)都是值拷贝,引用类型其实也是值拷贝，但是拷贝的是地址
####拷贝后的值就进函数栈了
```go
func funcName(n int,m int)int{
    n++//n = n + 1
    m++//m = m + 1
    var sum int = n + m
    return sum
}


func funcName(ptr *int)int{
    *ptr ++//此处ptr是指针的拷贝
    return *ptr
}
```
go函数不支持函数重载————函数名一定不能相同
##go中函数也是一种数据类型
```go
func sum(n int,m int)int{
    return n + m
}
func main(){
    a:= sum
    //两个含义
    //a的type是sum的type----->var a func(int,int)int = sum
    //a的内容是sum
    fmt.Printf("a的数据类型是%T，sum的数据类型是%T\n",a,sum)

    res := a(10,20)//a相当于sum的别名了
    fmt.Println("res = ",res)//30
}
```
#### a:= sum的两个含义
```go
a的数据类型是func(int, int) int，sum的数据类型是func(int, int) int
res =  30
```
函数因为是数据类型也可作为形参进行传递
```go
func funcName(calledFuncName func(a int,b int)int,n int,m int)int{
    return calledFuncName(n,m)
}
func main(){
    res2:= funcName(sum,50,20)
    fmt.Println("res2的值是",res2)
}
```
go支持自定义数据类型————相当于给一个别名
```go
type newfuncName func(int,int)int
type myInt int
var n int 
var m myInt = 10
//n = m报错-->n = int(m)
```
go支持对函数返回值的命名
```go
func sumAndSub(n int,m int)(sum int,sub int){//此时编译器已经创建返回值sum和sub，为空
    //如果对返回值命名，一定要括号括起来
    sum = n+m
    sub = n-m
    return
}
```
go支持可变参数--->指的是不确定参数的数量
```go
//编写一个函数sum，可以求出1到多个int的和
func sum(n int,args...int)(Sum int){//返回值一定要括号括起来
//如果使用可变参数，则一定要放在形参列表后面
    Sum = n1
    for i:=0;i<len(args);i++{
        Sum += args[i]
    }
    return
}
func main(){
    fmt.Println(sum(10,20,-99,200,-55))
}
```

init函数
如果一个文件包含全局变量，init函数，main()函数，那么处理顺序是全局变量->init()->main()
```go

var static = test()---------------------->允许var i = 3.3,i是float64
//static := test()报错
//在func外,每个语句都必须是golang的关键字开始

func test()int{
    fmt.Println("test()...")
    return 90
}

func init(){
    fmt.Println("init()....)
}
//init函数在main()函数调用之前被调用
func main(){
    fmt.Println("main()...")
}
```

init函数执行顺序
被调用包的全局变量->被调用包的init函数->main文件的全局变量->main函数的init()函数->main()函数

###匿名函数————没有名字标识符的函数
```go
//全局匿名函数的定义
var (//全局匿名函数也是全局变量，全局变量必须以关键字开始
    fun1 = func(n int,m int)int{
        return n - m
    }
)

func main(){
    //使用匿名函数求和
    res := func (n1 int,n2 int)int{
        return n1 + n2
    }(10,20)//当调用res时直接运行
    fmt.Println("res = ",res)

    a := res(30,40)
    b := res(55,22)
    //匿名函数在main函数的多次调用--->先把匿名函数传递给一个变量，就可以通过调用变量来多次调用匿名函数

    c := fun1(20,5)
}
```
---
闭包
内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后,如果内部函数还存在(即被返回)
###闭包函数就是一个在函数中被*<u>匿名声明</u>*的函数
```go
func Add() func(int)int {
    var i int = 1
    return func(x int)int{
        i+=x
        return i
    }
    //p := func(x int) int {
	//	i += x
	//	return i
	//}
	//return p
    //在go中必须是匿名函数
    
}

func main(){
    h := Add()
    fmt.Println(h(1))
    fmt.Println(h(1))
    fmt.Println(h(1))
    fmt.Println(h(1))
}
```

闭包的对比
```go
func makeSuffix(suffix string)func (string)string{
    //suffix:=suffix
	return func(name string)string{
		if strings.HasSuffix(name,suffix){
			return name
		}else{
			return name+suffix
		}
	}
}

func main(){
	f:=makeSuffix(".jpg")
	fmt.Println(f("asefk"))
	fmt.Println(f("afff.jpg"))
	f = makeSuffix(".png")
	fmt.Println(f("asefk"))
	fmt.Println(f("afff.jpg"))
}

//对比

func suffix(suffix string,name string)string{
    if !strings.HasSuffix(suffix,name){
        return name + suffix
    }else{
        return name
    }
}

func main(){
    fmt.Println(suffix(".jpg","aeffgff"))
    fmt.Println(suffix(".jpg","aef.jpg"))
    fmt.Println(suffix(".jpg","aeasf"))
}
```

####defer
defer入栈时，同时将值拷贝入栈
注意不能再defer内赋值
```go
func test(){
    file = openfile(filename)
    defer file.close()
    //defer n := 10错误
}
```


变量作用域就近原则
####赋值语句不能在函数体外进行
####所以需要init函数初始化
```go
var name = "tom"//初始化
//name := "tom"错误，赋值语句不能在函数体外进行
func test(){
    fmt.Println(name)
}

func test2(){
    name:="jim"
    fmt.Println(name)//jim
}

func test2(){
    name = "jim"//全局变量被修改了
    fmt.Println(name)//jim
}

func test(){
    fmt.Println(name)//jim
}
```

字符串处理
strings、strconv
时间处理
time
#### 时间的类型time.Time
下面的方法只能对time.Time类型进行操作
now.Year()、int(now.Month())--->或者%d、now.Second()
格式化日期和时间
```go
now = time.Now()
fmt.Printf("当前年月日 %d %d %d %d:%d:%d\n",
now.Year(),now.Month(),now.Day(),now.Hour(),now.Minute(),now.Second())
dataStr := Sprintf("当前年月日 %d %d %d %d:%d:%d\n",
now.Year(),now.Month(),now.Day(),now.Hour(),now.Minute(),now.Second())
fmt.Printf("dataStr=%v\n",dataStr)


fmt.Printf(now.Format("2006-01-02 15:04:05"))
fmt.Println()
fmt.Printf(now.Format("2006-01-02"))
fmt.Println()
fmt.Printf(now.Format("2006/01/02"))
fmt.Println()
fmt.Printf(now.Format("02"))
fmt.Println()
//"2006-01-02 15:04:05"是固定的
```

时间常量与休眠
```go
//每隔一秒钟打印
i:=0
for{
    i++
    fmt.Println(i)
    //休眠
    //time.Sleep(time.时间常量*数量)
    time.Sleep(time.Second)
    if i == 10{
        break
    }
}
```
时间戳
```go
//时间戳
//从1970年1月1日开始
//纳秒时间戳
fmt.Printf("Unix时间戳%v,Unix纳秒时间戳%v\n",now.Unix(),now.UnixNano())
```

new来分配值类型内存
make分配引用类型内存

#### 错误处理机制
当程序遇到错误就会崩溃，停止执行
需求：在遇到错误的时候按照要求返回错误并继续执行
defer、panic、recover
函数直接调用运行
```go
func test(){
    defer func(){
        err:=recover()//在程序结束时候捕获错误
        if err != nil{//判断程序结束程序是否是因为错误结束的
            fmt.Println("err=",err)//汇报错误
        }
    }()//匿名函数直接调用运行
    num1:=10
    num2:=0
    res:=num1/num2
    fmt.Println(res)
}

func main(){
    test()
    fmt.Println("next.....")
}
```
自定义错误类型
errors.New返回error类型
panic接受错误，打印并结束程序
```go
//读取配置文件
//如果配置文件不正确则返回一个自定义错误并结束
func readConf(name string)(err error){
    if name == "config.ini"{
        //读取语句
        return nil
    }else{
        //创建并返回一个自定义错误
        return errors.New("读取文件错")
    }

func test(){
    err := readConf("config.ini")
    if err != nil{
        //如果读取文件发送了错误，则打印错误并终止程序
        panic(err)
    }
    fmt.Println("test()继续执行。。。")
}

func main(){
    test()
    fmt.Println("main的代码...")
}

```
###数组
在go中数组是值类型
数组一旦定义了长度就是固定的
#####数组第一个元素的地址就是数组的地址
```go
var hens [6]float64;
hens[0] = 3.0
....
sum := 0.0
for i:=0;i<len(hens);i++{
    sum += hens[i]
}



//数组作为形参传入函数
var arr [3]int = [3]int{1,2,3}
func test(arrName [3]int){
    arrName[1] = 10
}

func main(){
    test(arr)
    fmt.Println(arr[1])//还是2
}

//引用传入数组
func test(arrName *[3]int){//指针被拷贝传入函数
    (*arrName)[1]=10//*arrName取数组内容
    //*arrName就是数组第一项的内容
}

func main(){
    test(&arr)
    fmt.Println(arr[1])//10
}
```
数组初始化方式
var arrName [10]int = [10]int{1,2,3,4,5,6,7,8,9,0}
var arrName = [3]int{1,2,3}
var arrName = [...]int{1,2,3}
var arrName = [...]int{0:1,2:3,1:2}
arrName := [...]string{0:"aeff",1:"aedfwf"}
#### 数组for range遍历
```go
for index,value := range arrName{
    ...
}
```
#### 不同长度的数组被认为是不同的类型
```go
func funcName(n int)[10]int{
    ...
}
func funcName(text [6]string)[6]int{
    ...
}
```

随机生成五个数并反转
```go
var randomArr [5]int
rand.Seed(time.Now().Unix())
//是随机种子不同，每次随机生成的数字也不同
for i:=0;i<5;i++{
    randomArr[i] = rand.Intn(100)
}
var temp int
for j:=0;j<len(randomArr)/2;j++{
    randomArr[j],temp = randomArr[4-j],randomArr[j]
    randomArr[4-j] = temp
}
```
### 切片(动态数组)
切片是数组的引用，是引用类型
make函数
```go
var sliceName []int = make([]int,5,10)
//长度为5，默认为0，容量为10-->容量参数可选

var slice []int
fmt.Println(slice)---->[]
//构建切片
var sliCe []string = []string{"tome","sg","sdfk"}

//
var arrName [10]int = [10]int{1,2,3,4,5,6,7,8,9,0}
slice := arrName[1:3]//一个切片--->[2,3]
slicE := arrName[:]
Slice := slice[:2]


fmt.Println("slice的元素:",slice)
fmt.Println("slice的长度:",len(slice))
fmt.Println("slice的容量:",cap(slice))//切片的容量动态变化，摊销容量2倍
```
通过截取数组而成的slice的append
注意append是在数组里面改
```go
func main() {
	var arrName [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 0}
	slice := arrName[:5]
	slice = append(slice, 88)
	slice[0] = 100
	fmt.Println(arrName)
}
//[100 2 3 4 5 88 7 8 9 0]


func main() {
	var arrName [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 0}
	slice := arrName[:]//截取arrName全部
	slice = append(slice, 88)
	slice[0] = 100
	fmt.Println(arrName)//[1 2 3 4 5 6 7 8 9 0]说明一旦slice加到arrName外就解除对arrName的指向，此时改变slice并不会改变arrName的值
	fmt.Println(slice)//[100 2 3 4 5 6 7 8 9 0 88]
}
```
只有都是切片类型才可以执行拷贝操作
数组可以直接赋值，值类型
...代码
copy(slice1,slice2)--->把slice2的内容拷贝给slice1
...代码
拷贝完后slice1和slice2的数据空间是相互独立的，不会相互影响


####string也可以切片处理
string底层是byte数组
string本身也是切片
string不可变
string修改字符
```go
var stingName string = "asefsfg"
arr1 := []byte(string)//转换成byte切片
//byte按字节处理
//var stingName string = "分为fwfe俺是个"
//arr1 := []rune(string)---->rune兼容汉字，按字符处理
arr1[0] = "z"
stringName = string(arr1)


//也可以
p := stingName[0]
//此时p的类型是uint8
//byte类型实际上是uint8

```
### 冒泡排序法
```go
func BubbleSort(slice []int){
	for i:=1;i<=len(slice)-1;i++{
		for j:=1;j<=len(slice)-i;j++{
			if slice[j-1]>slice[j]{
				slice[j-1],slice[j]=slice[j],slice[j-1]
			}
		}
	}
}

```

### 二分查找法
```go
//传入切片，带查找数字，传入切片最左相对于初始切片的索引值
func biFind(arr []int,num int,leftIndex int)int{
	//创建对于当前传入切片的头尾index
	leftIndexToArr := 0
	rightIndexToArr := len(arr)-1

	//计算中间index
	middleIndex := (leftIndexToArr + rightIndexToArr)/2

	//比较中间index对应数
	//若中间数小，则往右跳
	if arr[middleIndex]<num{
		leftIndexToArr = middleIndex + 1

		//更新往右跳后相对于初始arr切片的最左index值
		leftIndex += middleIndex + 1
	}else if arr[middleIndex]>num{
		rightIndexToArr = middleIndex - 1
	}else{
		//返回最左值相对于初始切片arr的index值+往右跳的值
		return middleIndex + leftIndex
	}

	if leftIndexToArr > rightIndexToArr{
		return -1
	}else{
		//传入leftIndex记录传入新函数的切片的最左相对于初始arr的index
		return biFind(arr[leftIndexToArr:rightIndexToArr+1],num,leftIndex)
	}
}

```
### 二分查找法其二(更好)
```go
func biFind(arr []int,leftIndex int,rightIndex int,num int)int{
    if leftIndex > rightIndex{
        return -1
    }
    middleIndex := (leftIndex + rightIndex)/2
    if arr[middleIndex]<num{
        return biFind(arr,middleIndex + 1,rightIndex,num)
    }else if arr[middleIndex]>num{
        return biFind(arr,leftIndex,middleIndex - 1,num)
    }else{
        return middleIndex
    }
}
```
###指针可以加8，地址不能
```go
var ptrArr *[10]int
fmt.Println(*prtArr)---->(*ptrArr)[0]
fmt.Println(*(prtArr+8))---->(*ptrArr)[1]
```
####二维数组的内存
```go
var diDimensionArr [n][m]int
//二维数组的元素是数组----->diDimensionArr[0~n-1]是一个数组

//数组的地址是其首元素的地址
&diDimensionArr --> &diDimensionArr[0] --> &diDimensionArr[0][0]
&diDimensionArr[1] --> &diDimensionArr[1][0]

//创建一个二维数组类型的指针
ptr := &diDimensionArr
//ptr的类型是*[n][m]int
(*ptr)[0] --> diDimensionArr[0]
//(*ptr)[0]不等于*ptr
//*ptr--->diDimensionArr
```
二维数组遍历
```go
var arr = [2][3]int{{1,2,3},{4,5,6}}
for i,v in range arr3{
    for j,v_j in range v{
        fmt.Println("v_j\n")
    }
}
```
二维数组的函数值传递
```go
//将1~16赋值给4*4的二维数组
var arr [4][4]int
count:=1
for i:=1;;i++{
	for j:=1;;{
		arr[i-1][j-1] = count
		count++
		j++
		if j>4{
			break
		}
	}
	if i>3{
		break
	}
}
fmt.Println(arr)//[[1 2 3 4] [5 6 7 8] [9 10 11 12] [13 14 15 16]]

//交换行
arr[0],arr[3] = arr[3],arr[0]
arr[1],arr[2] = arr[2],arr[1]
fmt.Println(arr)//[[13 14 15 16] [9 10 11 12] [5 6 7 8] [1 2 3 4]]

func change_arr(arr [4][4]int){
	arr[0][0] = 999
	arr[3][3] = 999
}
fmt.Println(arr)//[[13 14 15 16] [9 10 11 12] [5 6 7 8] [1 2 3 4]]
//二维数组的函数值传递仍然是值拷贝
```
###map和字典
```go
//map的声明
var mapName map[keyType]valType

var a map[string]int

var b map[string]map[string]int---->valType是 map[string]int
//声明一个map不会分配内存空间
```
声明一个map后要make分配空间
```go
//3种声明分配空间的方式
var a map[string]int
a = make(map[string]int,10)

var a = make(map[string]string,10)
b := make(map[string]string,10)

var a = map[string]string{
    "fjio":"adfku",
    "aefk":"akfug",
}
```
### make函数--->待分配var = make(varType,size)
#### delete删除键
...代码
delete(mapName,KeyName)--->删除map内的key
map查找
```go
var lam = map[string]string{
    "a":"aff",
    "b":"adf",
}
val,ok := lam["a"]
//如果存在key，则val接受返回的value,ok接受true
```
map的遍历
for---range
```go
var lam = map[string]string{
    "a":"aff",
    "b":"adf",
}
//k---key;v---value
for k,v := range lam{
    lmt.Println("k, ",k)
    lmt.Println("v, ",v)
}
```
map是引用类型
map切片
```go
var mapSlice  = make([]map[string]string)--->make []map
mapSlice[0] = make(map[string]string)--->make map
```

```go
func modify(users map[string]map[string]string),name string{
    if users[name]!=nil{//判断是否有键值
        users[name]["pwd"]="888888"
    }else {
        users[name] = make(map[string]string,2)
        users[name]["pwd"] = "888888"
        users[name]["nickname"] = "昵称~“+name
    }
}

func main(){
    users := make(map[string]map[sting]string)
    users["smith"] = make(map[string]string,2)//map一定要make
    users["smith"]["pwd"] = "9999"

    modify(users,"smith")
    modify(users,"tom")//引用传递
}
```

## 结构体
创建一个结构体
一个结构体是一个数据类型
一个结构体变量是一个实例化的对象

```go
var i int = ...
//创建一个int数据类型的变量i
//实例化一个int类型

//定义一个结构体，创建一个新的结构体数据类型
type Cat struct{
    Name string
    Age int
    Color string
}

//实例化
//创建一个Cat结构体变量
//当实例化时，空间就已经自动分配
//所以结构体是值类型，不是动态分布的
func main(){
    var cat1 Cat
    cat1.Name = "sef"
    cat1.Age = 3
    cat1.Color = "yellow"


    car cat2 = Cat{"ddd",10,"black"}
}
```

结构体的字段可以指值类型，也可以是引用类型
字段为引用类型时必须make，否则为nil
```go
type Demo struct{
    Name string
    ptr *int
    slice []int
    map1 map[sting]string
}

func main(){
    var v Demo
    //引用类型初始都是nil
    if v.ptr == nil{
        fmt.Println("yes")
    }
    if v.slice == nil{
        fmt.Println("yes")
    }
    if v.map1 == nil{
        fmt.Println("yes")
    }

    v.slice = make([]int,10)
    ptr = new(int)//new返回的是一个指针
    map1 = make(map[string]string,10)
}
```
结构体指针和底层优化
```go
type Demo struct{
    Name string
    Age int
}


func main(){
    var v *Demo = &Demo{"mary",10}
    (*v).Name = "TOM"
    V.Name = "Sandy"
}
//go支持 结构体指针.字段
```

结构体强制类型转换
结构体的字段、字段类型、个数要完全一致 

### 方法
方和和自定义数据类型绑定
自定义数据类型都可以有方法
```go
type structName struct{
    Name string
}

//(varName structName)与函数传参一致，都是将拷贝传入
func (p structName) funcName(param int)int{
        p.Name = "www"//修改为www
        return param
}

func main(){
    var p = structName{"red"}
    param:=10
    value := p.funcName(param)

    fmt.Println(p.Name)//还是red
}
```
```go
//写在函数外面
type Integer int

func (a Integer) sUm(b Integer)Integer{
    return a + b
    //运算的类型限制
}

func (a *Integer) Change(){
    *a = *a + 1
}

func main(){

    var a Integer = 10
	var b int = 20
    fmt.Println(int(a.sUm(Integer(b))))
    a.Change()
    fmt.Println(a)
}
```
注意，方法和函数的区别
```go
type Stu struct{
    Name string
}

func (a Stu)test(){
    a.Name = "peter"
}
func (a *Stu)test2(){
    a.Name = "peter"
}


func main(){
    var c = Stu{"tom"}
    c.test()
    fmt.Println(c.Name)//tom
    (&c).test()//虽然传递的是地址但是仍然是拷贝
    fmt.Println(c.Name)//tom

    c.test2()
    fmt.Println(c.Name)//peter
    (&c).test2()
    fmt.Println(c.Name)//peter
    //到底是指针还是拷贝传入方法，取决于方法的(a Stu)还是(a *Stu)

}
```

声明结构体的指针类型
```go
type stu struct{
    Name string
    Age int
}

func main(){
    var b *stu = &stu{"tom",10}
    var c *stu = &stu{
        Name:"tom",
        Age:9,
    }
    d := &stu{"lam",22}
}
```
嵌套结构体
```go
type A struct{
    Name string
    Age int
}
type B struct{
    Name string
    Age int
    A_str A
}

func main(){
    var demo *B
    (*demo).A_str = make(A,2)

    (*demo).Name = "hhh"
    (*demo).Age = 20
    (*demo).A_str.Name = "tom"
    (*demo).A_str.Age = 22
}
```
工厂模式
如果struct类型小写，要通过工厂模式在别的包访问
```go
package model

//如果student小写，就只能在包内使用
type student struct{
	Name string
	Age int
}

//在别的包调用此函数，返回一个struct指针
func NewStudent(n string,a int)*student{
	return &student{
		Name:n,
		Age:a,
	}
}
```
```go

//如果字段小写
//则不能用
type student struct{
	name string
	age int
}


//提供的方法
//来访问不可导出的字段
func (s *student)Getname()int{
    return s.name
}

```
```go
func main(){
	// var stu = model.Student{"tome",22}

	var stu = model.NewStudent("marry",22)

	fmt.Println(*stu)
	//fmt.Println((*stu).name)----->error
	//如果字段小写，则是私有的，只能读取不能访问改写
	//通过方法来改写
	fmt.Println((*stu).Getname())

}
```
## 封装、继承和多态
### 封装
封装就是把抽象出的字段和对字段的操作封装在一起，数据被保护在内部，隐藏细节，对数据提供保护。外部必须通过被授权的方法对内部数据进行操作
go通过包实现封装
```go
//放在model包
package model

type a_S struct{//小写，其他包不能直接构造
    Name string //大写，其他包可以直接访问
    age int //小写，其他包不能直接访问
    sex byte
}


//工厂模式
func BuildStruct(s string,age int,sex byte)*a_S{
    ....
    return &a_S{
        Name:s,
        age:age,
        sex:sex,
    }
}

//Set函数
func (a *a_S)Set_age(age int){
    if ...
    a.age = age
}

//Get函数
func (a *a_S)Get_age(){
    return a.age
}


//在其他包的调用
d := model.BuildStruct(s,age,sex)
d.Set_age(age)
age = d.Get_age()
```
### 继承
继承避免代码冗余，提高代码复用性
代码的扩展性和维护性提高了

当子类继承父类时候，就继承了父类所有的字段和方法
```go
type pupil struct{
    Name string
    age int
    score float64
    grade byte
}

type under_graduate struct{
    Name string
    age int
    gpa float32
}

//构建共同的继承父类
//将共有的字段和方法封装在一起
//在子类中嵌套匿名结构体
type stu struct{
    Name string
    age int
}
type pupil struct{
    stu                //匿名结构体
    score float64
    grade byte
}
type under_graduate struct{
    stu
    gpa float32
}


//子类可以调用父类所有的方法
func (a *stu)Test(){
    fmt.Println("hello.")
}
func (a *stu)Change_age(c int){
    (*a).age = c
}


func main(){
    var t under_graduate
	// t.stu.Name = "xxx"
	// t.stu.age = 22
	// t.gpa = 6.33

	t.Name = "xxx"//会自动寻找
	t.age = 22
	t.gpa = 6.33

    //调用父类的方法
    t.stu.Test()//hello
    t.stu.Change_age(66)
    fmt.Println(t.stu.age)//66
    //或者
    // t.Test()//hello
    // t.Change_age(66)
    // fmt.Println(t.stu.age)//66
}
```
就近原则
当结构体和匿名结构体有相同的字段或者访问方法时，编译器采用就近访问原则
```go
type A struct{
    Name string
    Age int
}
type B struct{
    A
    Name string
}

func main(){
    var b B
    b.Name = "addd"//就近访问B内的Name
    //必须b.A.Name = "ddd"才能给A中的Name赋值
}
```
```go
type A struct{
    Name string
    Age int
}
type B struct{
    Name string
    gpa int
}
type C struct{
    A
    B
}
func main(){
    var c C
    c.Name = "dxx"//报错
}
```
嵌套有名结构体
组合模式
```go
type C struct{
    typeName A
    B
}
func main(){
    var c C
    c.typeName.Name = "dxx"//访问有名结构体的字段时，必须带上名字
}
```
创建嵌套结构体和工厂模式
```go
type stu struct{
    Name string
    age int
}
type pupil struct{
    stu                //匿名结构体
    score float64
    grade byte
}
type under_graduate struct{
    stu
    gpa float32
}

func BuildUnder_graduate(s string,age int,gpa float32)*under_graduate{
    return &under_graduate{
        stu{
            Name : s,
            age : age,
        },
        gpa : gpa,
    }
}

func main(){
    var student under_graduate = under_graduate{{"name",22},6.33}
    student2 := under_graduate{
        stu{
            Name : "name",
            age : 22,
        },
        gpa : 6.33
    }
}
```
继承结构体指针
```go
type C struct{
    *A//C的字段是两个指针，指针指向A和B，c.A就是A【实例化】的地址
    *B//struct内的字段就是struct的内存标识符
         A           B
    ————————————————————————
C   |0x00...   |0x00...    |
    ————————————————————————
}
func main(){
    var c C = C{&A{"name",22},&B{"name",6.33}}
    fmt.Println(*c.A)//c.A是A的地址，*c.A就是A的内容
    fmt.Println((*c.A).Name)
}
```
结构体的匿名字段是基本数据类型
```go
type A struct{
    B
    Name string
    int
}
        B(匿名)      Name           int
    ————————————————————————————————————————
A   |Name|Age|...|  string  |      int     |
    |str |int|...|          |              |
    ————————————————————————————————————————
func main(){
    var foo A
    foo.int = 20
    foo.Name = "adad"
}
```
## 接口
接口可以定义一组方法，不需要实现，并且接口不能包含任何变量
实现接口就是实现(有定义了)了接口内定义的所有方法

接口实现了程序【多态】和【高内聚低耦合】的思想
程序与程序之间运行过程和运行结果相互关联就是高耦合
我们希望程序之间是相互独立的

只要有一个变量，含有实现了接口内所有的方法，就实现了接口
golang实现接口基于方法而不是基于接口本身


```go
type USB interface{
    func1(i int)
    func2()
    func3()string
}

type A struct{
    Name string
    Age int
}

type B struct{

}

//此处是和指针绑定的
func (a *A)func1(i int){
    (*a).Age += i   
}
func (a *A)func2(){
    fmt.Println("a run")
}
func (a *A)func3()string{
    return (*a).Name
}

//接口以引用的方式传入usb至函数内
//在这里引用的是指针
func (b *B)Called_Other_struct_Method(usb USB)string{
    usb.func1(2)//将传入的实现接口的自定义类型变量来和接口方法绑定
    usb.func2()
    return usb.func3()
}

func main(){
    var a *A = &A{"name",20}
    var b B
    //方法需要传入指针，这里必须是指针传入--->a必须是指针
    p := b.Called_Other_struct_Method(a)//a run
    fmt.Println("p is ",p)//name
    fmt.Println("a.age is ",(*a).Age)//22
}
```
创建一个接口变量，指向自定义类型
```GO
type USB interface{
    func1(i int)
    func2()
    func3()string
}

type A struct{
    Name string
    Age int
}

//实现接口
func (a *A)func1(i int){
    (*a).Age += i   
}
func (a *A)func2(){
    fmt.Println("a run")
}
func (a *A)func3()string{
    return (*a).Name
}

func main(){
    var b A
    var interFace USB = &b//必须是指针
    //将接口指向一个实现了该接口的自定义类型变量
    interFace.func2()
}
```
接口的继承
```go
type t1 interface{
    test1()
}
type t2 interface{
    test2()
}
type t3 interface{
    t1
    t2
    test3()
}

type a struct{ 
}

func (oo a)test1(){
}
func (oo a)test2(){
}
func (oo a)test3(){
}

func main(){
    var oo a
    var pp t3 = oo
    pp.test1()
}
```
### 接口是引用类型
空接口没有任何方法，所有数据类型都实现了空接口

### 例子1，利用sort，实现接口排序struct切片
```go
type Hero struct{
	Name string
	Age int
	Score int
}

type HerOSlice []Hero


func (a HerOSlice)Len()int{
	return len(a)//返回切片的长度
}
func (a HerOSlice)Less(i,j int)bool{
	//在什么条件下i应该排在j前面
	if a[i].Score > a[j].Score{
		return false
	}
	if a[i].Score < a[j].Score{
		return true
	}
	if a[i].Age < a[j].Age{
		return true
	}
	if a[i].Age > a[j].Age{
		return false
	}
	if a[i].Name < a[j].Name{
		return true
	}
	return false
}
func (a HerOSlice)Swap(i,j int){
	a[i],a[j] = a[j],a[i]
}

func main(){
	var pp HerOSlice
	// pp = make([]Hero,50)
	// for i:=0;i<50;i++{
	// 	pp[i].Name = fmt.Sprintf("隋兰~%d",rand.Intn(100))
	// 	pp[i].Age = rand.Intn(50)
	// 	pp[i].Score = rand.Intn(200)
	// }

	for i:=0;i<50;i++{
		hero := Hero{
			Name : fmt.Sprintf("隋兰%d",rand.Intn(5)),
			Age : rand.Intn(5),
			Score : rand.Intn(5),
		}
		pp = append(pp,hero)
		//append开辟内存
	}


	sort.Sort(pp)
	for i:=0;i<50;i++{
		fmt.Printf("%v\t,%v\t,%v\t\n",pp[i].Name,pp[i].Age,pp[i].Score)
	}

}
```
### 列子2 猴子会飞
```go
//父类自定义类型
type Monkey struct{
    Name string
}
//父类方法
func (a Monkey)climbing(){
    fmt.Println("猴子上树")
}

//子类自定义类型
type YoungMonkey struct{
    Monkey
}
//子类特有方法
func (a YoungMonkey)Dancing(){
    fmt.Println("猴子跳舞")
}
func(a YoungMonkey)Jumping(){
    fmt.Println("猴子跳高")
}

//定义一个接口
type flying interface{
    climbing()
    Dancing()
    Jumping()
}

//定义学飞函数
func Learning_Flying(flying flying){
    fmt.Println("猴子会飞！")
}

func main(){
    var c YoungMonkey
    Learning_Flying(c)
}
```
## 多态
多态通过接口实现
变量(示例)具有多种形态。按照统一的接口调用不同的实现。此时接口变量呈现不同形态。
####多态参数
```go
//接口
type Usb interface{
    call()
    run()
}

//类型一
type camera struct{
    Name string
}

//类型二
type phone struct{
    Name string
}

//实现接口
func (a camera)call(){
    fmt.Println("camera connected")
}
func (a phone)call(){
    fmt.Println("phone connected")
}

func (a camera)run(){
    fmt.Println("camera running")
}
func (a phone)run(){
    fmt.Println("phone running")
}

//多态参数
//参数可以是实现接口的不同自定义数据类型
func Computer(usb Usb){
    usb.call()
    usb.run()
}
```
#### 多态数组
一个数组内存放不同的数据类型
利用空接口存放任意数据类型
```go
type Usb interface{
    call()
    run()
}

//类型一
type camera struct{
    Name string
}

//类型二
type phone struct{
    Name string
}

//实现接口
func (a camera)call(){
    fmt.Println("camera connected")
}
func (a phone)call(){
    fmt.Println("phone connected")
}

func (a camera)run(){
    fmt.Println("camera running")
}
func (a phone)run(){
    fmt.Println("phone running")
}

//空接口
type Null interface{}



func main(){
    var usbArr [3]Usb
    usbArr[0] = phone{"sony"}
    usbArr[1] = phone{"iphone"}
    usbArr[2] = camera{"canno"}

    //利用空接口存放任何类型
    var nullArr [3]Null
    nullArr[0] = 0
    nullArr[1] = "max"
    nullArr[2] = camera{"canno"}
    fmt.Println(nullArr)//[0 max {canno}]
   
}
```
### 类型断言
```go
type p struct{
    x int
    y int
}

type null interface{}

func main(){
    //实例化一个空接口变量
    var a null
    //或者
    //var a interface{}
    var b p = p{1,2}
    a = p
    //一个新的p类实例
    var c p


    //c = a
    //error，类型不匹配
    //a = p时，因为多态，p的身份(数据类型)被忽视了，变成了泛型(一般类型)

    c := a.(p)//类型断言
    //判断a是否是指向p类型的变量，如果是就转成p类并赋值给c，否则报错
    
    //带检查的非停止的类型断言
    c , ok := a.(p)
    if ok == true{
        ...success
    }else{
        ...wrong
    }
    .....
}
```
类型断言使用
创建一个多态数组，当检测到是phone时，不但调用call()和run()，还调用独有的fun()
```go
type Usb interface{
    call()
    run()
}

//类型一
type camera struct{
    Name string
}

//类型二
type phone struct{
    Name string
}

//实现接口
func (a camera)call(){
    fmt.Println("camera connected")
}
func (a phone)call(){
    fmt.Println("phone connected")
}

func (a camera)run(){
    fmt.Println("camera running")
}
func (a phone)run(){
    fmt.Println("phone running")
}

//phone独有的fun()
func (a phone)fun(){
    fmt.Printf("%v running\n",a.Name)
}

//方法和函数的区别
//这里函数和方法重名，但是能够跑
//go不支持函数重载
//go没有检测到绑定项时，提示这是函数，就会在函数里寻找
func run(usb Usb){
    usb.call()
    usb.run()
    if Phone,ok = usb.(phone);ok{
        Phone.fun()
    }
}

func main(){
    var Arr [3]Usb
    Arr[0] = phone{"jason"}
    Arr[1] = phone{"lava"}
    Arr[2] = camera{"scape"}
    for _, v = range Arr{
        Run(v)
    }
}

```
判断传入参数类型
```go
//定义student类型
type student struct{}
//参数是多个空接口类型
func PrintType(params ...interface{}){
    for index,v:=range params{
        //type关键字，固定写法
        //判断返回v的类型
        switch v.(type){
            case bool:
                fmt.Printf("第%d个参数是bool类型\n",index)
            case float32:
                fmt.Printf("第%d个参数是float32类型\n",index)
            case student:
                fmt.Printf("第%d个参数是student类型\n",index)
            ...
        }
    }
}
```
### 项目1 家庭收支账目
```go
func main(){

    balance := 10000.0
    condition := true
    var details string
    var choice int
    for {
        fmt.Println("--------------------家庭收支记账软件--------------------")
        fmt.Println("                      1.收支明细                       ")
        fmt.Println("                      2.登记收入                       ")
        fmt.Println("                      3.登记支出                       ")
        fmt.Println("                      4.退出软件                       ")
        fmt.Println("请选择：")

        //创建变量接受选择
        
        fmt.Scanln(&choice)

        switch choice {
            case 1:
                fmt.Println("收支         账户金额          收支金额         说    明")
                //创建记录明细字符串
                fmt.Println(details)
            case 2:
                //存储收入
                var income float64
                fmt.Println("请输入收入:")
                fmt.Scanln(&income)
                for{
                    if income>0.0{
                        break
                    }else{
                        fmt.Println("收入不能小于0，请重新输入：")
                        fmt.Scanln(&income)
                    }
                }
                balance += income

                //存储说明
                var reason string
                fmt.Println("请输入原因:")
                fmt.Scanln(&reason)

                //存储明细
                details += fmt.Sprintf("收入         %v         %v         %v\n",balance,income,reason)
            case 3:
                //存储支出
                var outcome float64
                fmt.Println("请输入支出:")
                fmt.Scanln(&outcome)
                for{
                    if outcome<=balance{
                        break
                    }else{
                        fmt.Println("支出不能大于存款，请重新输入：")
                        fmt.Scanln(&outcome)
                    }
                }
                balance -= outcome

                //存储说明
                var reason string
                fmt.Println("请输入原因:")
                fmt.Scanln(&reason)

                //存储明细
                details += fmt.Sprintf("支出         %v         %v         %v\n",balance,outcome,reason)
            case 4:
                fmt.Println("是否退出，y/n：")

                //接受选择
                yes_no := ""
                for{
                    fmt.Scanln(&yes_no)
                    if yes_no == "y"{
                        condition = false
                        break
                    }else if yes_no == "n"{
                        break
                    }else{
                        fmt.Println("输入错误请重新输入:")
                    }
                }
        


        }
        if !condition {
            break
        }

    }
}
```
### 项目2 CRM管理
文件main.go
```go
package main

import (
	"go_code/CRM/customer_management"

)

func main(){
	customer_managementor := customer_management.Factory_customer_management()
	customer_managementor.Show_Menu()
}
```
文件 customer_management
```go
package customer_management
import (
	"fmt"
	"go_code/CRM/customer"
)

type CRM_interface interface{
	Show_customer()
	Get_age()int
	Get_name()string
	Get_phone_num()string
	Get_sex()int
	Get_email()string
	Rewrite_age(int)
	Rewrite_email(string)
	Rewrite_name(string)
	Rewrite_phone_num(string)
	Rewrite_sex(int)
}

type customer_management struct{
	//存储客户信息的切片
	customer_lst []CRM_interface

	//客户的主菜单和修改菜单的选择	
	choice string

	//是否继续使用软件
	tag bool 

	//用户信息临时存储
	new_customer_name string
	new_customer_age int
	new_customer_sex int
	new_customer_phone_num string
	new_customer_email string

	//是否继续修改用户信息
	tag_modify bool
}


func (a *customer_management) Show_Menu(){
	for{
		fmt.Println("--------------------客户信息管理软件--------------------")
        fmt.Println("                      1.添加客户                      ")
        fmt.Println("                      2.修改客户                       ")
        fmt.Println("                      3.删除客户                       ")
        fmt.Println("                      4.客户列表                       ")
        fmt.Println("                      5.退出软件                       ")
        fmt.Println("请选择：")

		fmt.Scanln(&a.choice)

		switch a.choice{
			case "1":
				a.Add_customer()
			case "2":
				a.Modify_customer()
			case "3":
				a.Delete_customer()
			case "4":
				a.Show_customer_lst()
			case "5":
				a.tag = false
		}

		if a.tag == false{
			break
		}
		
	}
	
}

//工厂模式
func Factory_customer_management()* customer_management{
	//null_customer := customer.Factory_Customer_null()
	//构建一个空的接口切片
        //当struct都是私有时候用接口引入
	var numArr []CRM_interface
	//numArr = append(numArr,null_customer)
	return & customer_management{
		customer_lst : numArr,
		choice : "1",
		tag : true,
		new_customer_name : "",
		new_customer_age : 0,
		new_customer_sex : 0,
		new_customer_phone_num : "",
		new_customer_email : "",
		tag_modify : true,
	}
}

//添加一个用户至列表
func (a *customer_management)Add_customer(){


	fmt.Println("请输入需要加入客户的姓名：")
	fmt.Scanln(&a.new_customer_name)
	fmt.Println("请输入需要加入客户的年龄：")
	fmt.Scanln(&a.new_customer_age)
	fmt.Println("请输入需要加入客户的性别：(1-男性;2-女性)")
	fmt.Scanln(&a.new_customer_sex)
	fmt.Println("请输入需要加入客户的电话：")
	fmt.Scanln(&a.new_customer_phone_num)
	fmt.Println("请输入需要加入客户的邮箱：")
	fmt.Scanln(&a.new_customer_email)


	a.customer_lst = append(a.customer_lst,customer.Factory_Customer(a.new_customer_name,a.new_customer_age,
									a.new_customer_sex,a.new_customer_phone_num,a.new_customer_email))								
}


//修改列表中指定用户信息
func (a *customer_management)Modify_customer(){
	fmt.Println("请输入需要修改的客户姓名：")
	var customer_index int
	for {
		
		fmt.Scanln(&a.new_customer_name)


		customer_index = a.Find_customer(a.new_customer_name)
		if customer_index == -1{
			fmt.Println("请重新输入：")
		}else{
			break
		}
	}

	for{
		fmt.Printf("--------------------------用户%v-----------------------\n",a.new_customer_name)
		fmt.Println("                      1.修改用户年龄                       ")
		fmt.Println("                      2.修改用户性别                       ")
		fmt.Println("                      3.修改用户电话                       ")
		fmt.Println("                      4.修改用户邮箱                       ")
		fmt.Println("                      5.退出修改界面                       ")

		fmt.Println("请选择要修改的用户信息：")
		fmt.Scanln(&a.choice)

		switch a.choice{
		case "1":
			fmt.Println("请输入修改后的年龄：")
			fmt.Scanln(&a.new_customer_age)
			a.customer_lst[customer_index].Rewrite_age(a.new_customer_age)
		case "2":
			if a.customer_lst[customer_index].Get_sex() == 1{
				a.new_customer_sex = 2
			}else if a.customer_lst[customer_index].Get_sex() == 2{
				a.new_customer_sex = 1
			}
			a.customer_lst[customer_index].Rewrite_sex(a.new_customer_sex)
		case "3":
			fmt.Println("请输入修改后的电话：")
			fmt.Scanln(&a.new_customer_phone_num)
			a.customer_lst[customer_index].Rewrite_phone_num(a.new_customer_phone_num)
		case "4":
			fmt.Println("请输入修改后的邮箱：")
			fmt.Scanln(&a.new_customer_email)
			a.customer_lst[customer_index].Rewrite_email(a.new_customer_email)
		case "5":
			return
		}
	}
}


//查找列表用户
func (a *customer_management)Find_customer(customer_name string)int{
	for index, customer_in_lst := range a.customer_lst{
		if customer_in_lst.Get_name() == customer_name{
			return index
		}
	}
	fmt.Println("没有找到！")
	return -1
}

//删除用户
func (a *customer_management)Delete_customer(){
	if len(a.customer_lst) == 0{
		fmt.Println("用户列表空，请添加用户！")
		return
	}
	fmt.Println("请输入要删除的客户姓名：")

	var customer_index int
	for {
		
		fmt.Scanln(&a.new_customer_name)

		customer_index = a.Find_customer(a.new_customer_name)
		if customer_index == -1{
			fmt.Println("请重新输入：")
		}else{
			break
		}
	}
	a.customer_lst[customer_index].Show_customer()
	fmt.Println("是否确认删除？y/n")
	fmt.Scanln(&a.choice)
	if a.choice == "y"{

		//注意append只能挨个添加，必须要在末尾添加...
		a.customer_lst = append(a.customer_lst[:customer_index],a.customer_lst[customer_index + 1:]...)

	}else{
		return
	}
}

//显示用户列表
func (a *customer_management)Show_customer_lst(){
	if len(a.customer_lst) == 0{
		fmt.Println("用户列表空，请添加用户！")
		return
	}
	fmt.Println("用户姓名          用户年龄          用户性别          用户电弧          用户邮箱")
	for _ ,customer_in_lst := range a.customer_lst{
		customer_in_lst.Show_customer()
	}
}
```
文件customer
```go
package customer
import "fmt"


//customer类型的定义
type customer struct{
	name string
	age int
	sex int
	phone_num string
	email string
}

//显示用户信息的方法
func (a *customer)Show_customer(){
	var sex_str string
	if a.sex == 1{
		sex_str = "男性"
	}else{
		sex_str = "女性"
	}
	fmt.Printf("%v\t\t%v\t\t%v\t\t%v\t\t%v\n",a.name,a.age,sex_str,a.phone_num,a.email)

}

//获取和修改方法的定义
func (a *customer)Get_age()int{
	return (*a).age
}
func (a *customer)Get_name()string{
	return (*a).name
}
func (a *customer)Get_sex()int{
	return (*a).sex
}
func (a *customer)Get_phone_num()string{
	return (*a).phone_num
}
func (a *customer)Get_email()string{
	return (*a).email
}

//修改用户信息方法
func (a *customer)Rewrite_name(new_name string){
	(*a).name = new_name
}
func (a *customer)Rewrite_age(new_age int){
	(*a).age = new_age
}
func (a *customer)Rewrite_sex(new_sex int){
	(*a).sex = new_sex
}
func (a *customer)Rewrite_phone_num(new_phone_num string){
	(*a).phone_num = new_phone_num
}
func (a *customer)Rewrite_email(new_email string){
	(*a).email = new_email
}

//工厂模式
func Factory_Customer(name string,age int,sex int,phone_num string,email string)*customer{
	return &customer{
		name : name,
		age : age,
		sex : sex,
		phone_num : phone_num,
		email : email,
	}
}

//生成一个空类型
// func Factory_Customer_null()* customer{
// 	return &customer{}
// }
```
## 文件与流
流是数据源(文件磁盘)和程序(内存)之间经历的路程
os包的File结构体封装了所有的对文件的操作
os.File

重要函数和方法
os.Open
os.OpenFile
bufio.NewReader
bufio.NewWriter
reader.ReadString
writer.WriteString
writer.Flush
io.Stat
io.IsNotExist
io.Copy


####打开阅读文件
os.Open(filepath string)(file *File,err error)返回文件句柄(指针)
(file *File)Close()error
file.Close()
```go
import (
    "fmt"
    "os"
)

func main(){
    file ,err := os.Open("./.....txt")
    //将err取出就不会终止程序
    if err != nil{
        fmt.Println("the error is ",err)
    }
    //file是指针
    //此后打印的是指针
    //就是指针指向的地址
    fmt.Printf("file = %v\n",file)

    err = file.Close()
    if err != nil{
        fmt.Println("close file err = ".err)
    }
}
```
用缓冲区读取文件
包bufio
bufio.NewReader(file *File)*Reader-->可以把文件中的内容拿到缓冲区
Reader.ReadString(b byte)(string error)
默认缓冲区4096字节
```go
import (
    "fmt"  
    "os"
    "bufio"
    "io"
)
func main(){ 
    file ,err := os.Open("./.....txt")
    //将err取出就不会终止程序
    if err != nil{
        fmt.Println("the error is ",err)
    }
    defer file.Close()

    reader := bufio.NewReader(file)
    //循环读取内容
    for {
        //读到一个换行就结束
        //把内容拿到缓冲区
        //此处会将\n读取进去
        //拿取到\n结束
        str, err := reader.ReadString('\n')
        if err == io.EOF{
            break
        }
        //输出
        fmt.Print(str)
        //不是Println，因为如果这样就会有两个\n
    }
    fmt.Println("文件读取结束")
}
```
io/ioutil包一次性读取全部文件
ioutil.ReadFile(filepath string)([]byte err)
输出的是byte切片
同时ReadFile不需要显示Open和Close文件，被封装到ReadFile内部
fmt.Printf("%v",string(content))


####写入文件
os.OpenFile(namepath string,打开模式 int,perm FileMode)(file *File,err error)
bufio.NewReader(file *File)*writer将内容压到缓冲区
#####创建一个新文件，写入5句话
```go
import (
    "fmt"
    "bufio"
    "os"
)
func main(){
    //写入模式，创建文件模式
    file,err := os.OpenFile(filepath,os.O_WRONLY|os.O_CREATE,0666)
    if err != nil{
        fmt.Println("the error is ",err)
        return
    }

    defer file.Close()

    str := "hello world\n"
    //NewWriter是带缓存的
    //返回*writer
    writer := bufio.NewWriter(file)
    for i := 0;i<5;i++{
        //WriterString将str写入缓存
        writer.WriteString(str)
    }
    //将缓存的内容flush入文件
    Writer.Flush()

}
```
OpenFile模式常量
```go
import (
    "os"
)
const{
    O_RDONLY
    //会覆盖文件已有内容
    O_WRONLY
    O_RDWR
    O_CREATE
    O_APPEND
    O_TRUNC
}

```
判断文件是否存在
os.Stat(name string)(fi FileInfo,err error)
如果返回的err为nil则文件或文件夹存在
如果返回的错误类型为os.IsNotExist()判断为true，说明文件或文件夹不存在
如果返回的错误为其他类型则不确定是否存在
```go
//判断文件或文件夹或目录是否存在
func PathExists(path string)(bool error){
    _, err := os.Stat(path)
    if err == nil{
        return true,nil
    }else if os.IsNotExist(err){
        return false,nil
    }else{
        return false,err
    }
}
```
拷贝图片/电影/音乐
io.Copy(dst Writer,src Reader)(written int64,err error)
接受一个Writer类型和一个Reader类型
将src的数据拷贝到dst，直到在src上发生EOF错误，返回拷贝的字节数和错误类型
```go
import (
    "os"
    "bufio"
    "io"
)


func CopyFile(dstname string,srcname string)(written int64,err error){
    file_src,err := os.OpenFile(srcname,OS.O_RDONLY)
    //此处等价于
    //file_src,err := os.Open(srcname)
    //os.Open(srcname)等价于os.OpenFile(srcname,O_RDONLY)
    //即os.Open是os.OpenFile的只读模式
    if err != nil{
        fmt.Println("read file err = ",err)
        return
    }
    reader := bufio.NewReader(file_src)
    defer file_src.Close()

    file_dst ,err := os.OpenFile(dstname,OS.O_WRONLY|OS.O_CREATE)
    if err != nil{
        fmt.Println("CREAT/point file err = ",err)
        return
    }
    writer := bufio.NewWriter(file_dst)
    defer file_dst.Close()

    return io.Copy(writer,reader)
}
```
##### flag包命令行参数解析
```go
import (
    "fmt"
    "flag"
)

func main(){
    var user string
    var pwd string
    var host string
    var port int

    //注册os.Args[1:]
    //放到user内，u是接受标识符，传送其后面的字符串至指定地址，若无，默认值为"timeboom",其后为说明
    flag.StringVar(&user,"u","timeboom","用户名默认为timeboom")
    flag.StringVar(&pwd,"pwd","","密码默认为空")
    flag.StringVar(&host,"h","","主机默认为空")
    flag.StringVar(&port,"p","","端口号默认为空")

    //重要
    //转换
    //从os.Args[1:]中解析注册的flag，必须在所有flag都注册好而未访问前执行
    flag.Parse()
    fmt.Printf("user=%v, pwd=%v,host=%v, port=%v\n",user,pwd,host,port)
}
```

### 序列化与tag标签
json.Marshal(interface{})([]byte,error)
接受一个空接口，返回一个byte切片和错误
```go
import (
    "fmt"
    "encoding/json"
)

type AA struct{
    //tag标签
    //跨包调用必须大写
    //序列化是跨包调用
    Name string `json:"name"`
    Age int `json:"age"`
    Sex int `json:"sex"`
}

//序列化struct
func test_encoding_struct(){
    A := AA{
        Name : "sedgf",
        Age : 22,
        Sex : 2,
    }
    //将 A 序列化
    //指针传入和非指针传入都可
    data,err := json.Marshal(&A)
    if err != nil{
        fmt.Println("err is ",err)
        return
    }
    fmt.Printf("序列化后是%v",string(data))
    //需要string
}

//序列化map
func test_encoding_map(){
    var a map[string]interface{}
    a = make(map[string]interface{})
    a["make"] = "wfa"
    a["age"] = 32
    a["address"] = "wsefdf"

    data,err := json.Marshal(&a)
    if err != nil{
        fmt.Println("err is ",err)
        return
    }
    fmt.Printf("序列化后是%v",string(data))

}

//序列化切片
func test_encoding_slice(){
    var a []map[string]interface{}
    var b map[string]interface{}

    b = make(map[string]interface{})
    b["make"] = "wfa"
    b["age"] = 32
    b["address"] = "wsefdf"

    var c map[string]interface{}
    c = make(map[string]interface{})
    c["no"] = "asdf"
    c["yes"] = 3
    c["address"] = "wsdf"

    a = append(a,b,c)
    data,err := json.Marshal(&a)
    if err != nil{
        fmt.Println("err is ",err)
        return
    }
    fmt.Printf("序列化后是%v",string(data))
    
}


func main(){
    test_encoding_slice()
} 
```

### 反序列化
```go
import (
    "fmt"
    "encoding/json"
)

type AA struct{

    Name string 
    Age int 
    Sex int 

}



func UnmarshalStruct(){
    str = "{\"address\":\"wsefdf\",\"age\":32,\"make\":\"wfa\"}"
    //"里面 含有"需要转义处理

    var a AA
    //返回错误，引用改写目标对象
    //传入str的byte切片
    err := json.Unmarshal([]byte(str),&a)
    if err != nil{
        fmt.Println("err is ",err)
        return
    }
    fmt.Printf("反序列化后是%v",a)
    
}


func UnmarshalMap{
    str = "[{\"address\":\"wsefdf\",\"age\":32,\"make\":\"wfa\"},{\"address\":\"wsdf\",\"no\":\"asdf\",\"yes\":3}]"

    var b []map[string]interface{}
    //反序列化不需要make
    //Unmarshal封装了make----->类型断言，然后make
    err := json.Unmarshal([]byte(str),&b)
    if err != nil{
        fmt.Println("err is ",err)
        return
    }
    fmt.Printf("反序列化后是%v",b)
    //反序列化类型必须一致

}
```
# 单元测试
```go
func testFun1(funcOne func(int)int,param int, answer int){
    switch{
        case funcOne(param) == answer:
            fmt.Println("正确！答案是:",answer)
        default:
            fmt.Println("错误！答案不是:",answer)
    }
}


func Add(n int)int{
    sum := 0
    for i:=1;i<=n;i++{
        sum += i
    }
    return sum
}

func main(){
    testFun1(Add,10,55)
}
```
testing包，通过go test命令能够自动执行如下形式的任何函数
要编写一个新的测试套件，需要创建一个名称以 _test.go 结尾的文件，该文件包含 `TestXxx` 函数，如上所述。 将该文件放在与被测试的包相同的包中。该文件将被排除在正常的程序包之外，但在运行 “go test” 命令时将被包含
```go
func TestXxx(*testing.T)
```
```go
package test_case

func Add(n int)int{
    sum := 0
    for i:=1;i<=n;i++{
        sum += i
    }
    return sum
}

func Sub(n ,m int)int{
	return n - m
}
```
```go
package test_case

import (
	_ "fmt"
	"testing"
)


func TestAddupper(t *testing.T){
	res := Add(10)
	if res != 55{
		t.Fatalf("Add执行错误")
		//输出日志并且结束程序
	}
	t.Logf("代码执行正确")
	//输出日志
}

func TestSub(t *testing.T){
	if Sub(10,2) != 8{
		t.Fatalf("执行错误")
	}
	t.Logf("正确")
}
```
```
PS E:\go\src\go_code\test\test_case> go test -v
=== RUN   TestAddupper
    func_test.go:15: 代码执行正确
--- PASS: TestAddupper (0.00s)
=== RUN   TestSub
    func_test.go:23: 正确
--- PASS: TestSub (0.00s)
PASS
ok      go_code/test/test_case  0.070s
```
测试单个方法
go test -v -run TestSub(TestFuncName)

### Monster案例
```go
package monster

import (
	"fmt"
    "encoding/json"
    "os"
    "bufio"
    "io"
)


type Monster struct{
    Name string `json:"name"`
    Age int `json:"age"`
    Sex int `json:"sex"`
    Address string `json:"address"`
}


func (monster *Monster)Store(filepath string){
    //打开文件
    file , err:= os.OpenFile(filepath,os.O_WRONLY|os.O_CREATE,0666)
    if err != nil{
        fmt.Println("打开文件发生错误")
        return
    }
    defer file.Close()
    writer := bufio.NewWriter(file)

    //序列化
    data,err := json.Marshal(monster)
    if err != nil{
        fmt.Println("序列化发生错误")
        return
    }
	fmt.Println(string(data))
    writer.WriteString(string(data))
	writer.Flush()

}
func (monster *Monster)Restore(filepath string){
    //打开文件
    file , err:= os.Open(filepath)
    if err != nil{
        fmt.Println("打开文件发生错误")
        return
    }
    defer file.Close()
    Reader := bufio.NewReader(file)
    data,err := Reader.ReadString('\n')
    if err == io.EOF{
        fmt.Println("读取完毕，继续执行")
    }else if err != nil{
        fmt.Println("读取错误")
        return
    }
    err = json.Unmarshal([]byte(data),monster)
    if err != nil{
        fmt.Println("反序列化错误")
        return
    }
    fmt.Printf("反序列化后是%v",monster)

}
```
```go
package monster

import (
	"testing"
	"os"
	"bufio"
)

func TestStore(t *testing.T){

	filepath := "./monster.txt"
	Monst := Monster{
		Name : "牛魔王",
		Age : 22,
		Sex : 1,
		Address : "三里屯",
	}

	Monst.Store(filepath)
	file,err := os.Open(filepath)
	if err != nil{
		t.Fatalf("打开文件失败")
	}
	defer file.Close()
	reader := bufio.NewReader(file)
	data,erro := reader.ReadString('}')
	if erro != nil{
		t.Fatalf("读取文件失败")
	}
	str := string(data)
	if str != "{\"name\":\"牛魔王\",\"age\":22,\"sex\":1,\"address\":\"三里屯\"}"{
		t.Fatalf("序列化失败")
	}
	t.Logf("序列化成功")
}
func TestRestore(t *testing.T){
	filepath:= "./monster.txt"
	str := "{\"name\":\"牛魔王\",\"age\":22,\"sex\":1,\"address\":\"三里屯\"}"
	Monst := Monster{}

	file,err := os.OpenFile(filepath,os.O_WRONLY|os.O_CREATE,0666)
	if err != nil{
		t.Fatalf("打开文件失败")
	}
	defer file.Close()
	writer := bufio.NewWriter(file)
	writer.WriteString(str)
	writer.Flush()

	Monst.Restore(filepath)


	comp_Monster := Monster{
		Name : "牛魔王",
		Age : 22,
		Sex : 1,
		Address : "三里屯",
	}
	if Monst != comp_Monster {
		t.Fatalf("反序列化失败")
	}
	t.Logf("序列化成功")


}
```
# goroutine
### 并发
多线程程序在单核上运行，就是并发
就是线程排队
### 并行
多线程程序在多核上运行，就是并行
### 进程
就是程序在操作系统中的一次执行过程————程序加载到内存，运行起来就是进程，是系统进行资源分配和调度的基本单位
### 线程
线程是进程的一个执行实例，是程序执行的最小单位，他是比进程更小的能独立运行的基本单位
一个进程可以创建和销毁多个线程，同一个进程中的多个线程可以并发执行

##### 例子
找到1~1100000内的所有素数
进程：
```GO
var b []int
for i:=1;i<=1100000;i++{
    判断i是否是素数
        如果是：
            b = append(b,i)
}
```
线程：
有1100000个子线程排队等待运行
大并发

###协程和go主线程
###go的协程特点
#####1.独立的栈空间
#####2.共享程序堆空间
#####3.调度由用户控制
#####4.协程是轻量级的线程
在主线程中开启goroutine，该协程每隔1秒输出hello world
在主线程中也每隔一秒输出hello golang，输出十次后退出程序
主线程和goroutine同时执行
```go
pakage main
import (
    "fmt"
    "strconv"
    "time"
)

func test(){
    for i:=1;i<=10;i++{
        fmt.Println("hello world"+strconv.Itoa(i))
        time.Sleep(time.Second)
    }
}

func main(){

    //开启一个协程
    //协程是在main()中的
    //就是说，协程在主线程中
    //主线程结束就意味着main()退出
    //协程就结束
    go test()

    for i:=1;i<=10;i++{
        fmt.Println("hello golang"+strconv.Itoa(i))
        time.Sleep(time.Second)
    }
}
```
### MPG模式
M主线程
P协程的上下文
G协程
阻塞的处理
###runtime包
tuntime包提供了和go运行时环境的互操作：
协程和主线程之间的交互：
主线程运行完毕协程还未运行完毕情况的处理等
```go
import (
    "fmt"
    "runtime"
)
fun main(){
    //返回电脑的cpu个数
    cpuNUm := runtime.NumCPU()
    //设置可用CPU个数
    runtime.GOMAXPROCS(cpuNUm - 1)

}
```
计算1~200各个数的阶乘，放到map中，用goroutine完成
```go

func factor(n int,b map[int]int){
    var a int = 1
    for i:=1;i<n;i++{
        a *= i
    }
    //map本来就是引用传递
    b[n]=a
}

func main(){
    var a map[int]int
    a = make(map[int]int)
    for i:=1;i<=200;i++{

        //fatal error
        //并行写入数据
        go factor(i,a)

    }
}
```
go build -race xxxxx.go
查看竞争关系
报告同时写入的数据个数
concurrent map writes

### 全局变量互斥锁
包sync
当协程对map写入时，对map加锁
被加锁的map无法同时被其他协程写入
其余协程会进入队列等待
当协程写入完毕后，对map解锁
保证了map从头到尾只有一个协程进行写入


此时互斥锁锁的类型是map[int]int
锁是锁一整个非切片类型的
但如果是[]map[int]int
如果能保证每个协程访问不同的元素，就不需要上锁
详细见快速排序法

【全局变量】互斥锁

```GO
import (
    "sync"
    "fmt"
)

var (
    b map[int]int = make(map[int]int)
    //声明一个全局的互斥锁
    //准备互斥锁
    //lock是锁的名字
    lock sync.Mutex
    //Mutex为互斥锁类型，可以由不同的线程加锁和解锁
    //和线程的类型无关

)
//var look sync.Mutex

func factor(n int){
    var a int = 1
    for i:=1;i<n;i++{
        a *= i
    }
    lock.Lock()
    //defer解锁
    //防止异常终止
    defer lock.Unlock()
    b[n] = a
}

func main(){

    for i:=1;i<=200;i++{

        //fatal error
        //并行写入数据
        go factor(i)

    }
    //golang的互斥锁非读写锁，只要访问就会发生竞争，不只是写入才有竞争
    //读写锁区分读者和写者，而互斥锁不区分
    //写锁会阻塞其它读写锁，也就是在写入时不能读取
    //互斥锁同一时间只允许一个线程访问该对象，无论读
    //写；读写锁同一时间内只允许一个写者，
    //但是允许多个读者同时读对象
    lock.Lock()
    defer lock.Unlock()
    for i:=1;i<=200;i++{
        fmt.Printf("map[%v] = %v\n",i,b[i])
    }
}
```
如果在一个访问共享资源的操作执行完毕前，其他访问请求必须等待，理论上可以解决上述情景的资源竞争问题。这样的编码方式，我们称之为互斥访问，而实现该方式的代码段，称之为临界区。
当某个协程进入临界区后，其他所有要访问共享资源的协程，必须阻塞地等待该协程对临界区访问结束，也就是在调用unlock之后。这意味着初衷是提高性能的多协程设计，此刻“退化为”单协程，这也是上文提到 同步访问 的原因。
一旦采用了加解锁的方式来实现对临界区的互斥访问，今后所有涉及此类临界区内的共享资源的【读写】操作，都必须遵循这样的开发模式。一旦有哪怕一次疏漏，都有可能导致bug。

# 管道
channel是线程安全的，当多个协程操作同一个管道时，不会发生资源竞争问题
管道就是队列
```go
func main(){
    //声明一个管道
    //必须要make
    //引用变量
    var intchan chan int
    intchan = make(chan int,3)

    //将值放入管道
    intchan <- 10
    num := 10
    inchan <- num

    //一个空接口类型管道
    var Echan chan interface{}
    Echan = make(chan interface{},3)

    //当给管道写入数据时候
    //最多只能加到make的长度
    //否则报错死锁

    //取
    var num int
    num2 = <- intchan
    //如果取完还取
    //一样死锁

    //把管道数据扔了
    //管道不一定要给别人值
    <- intchan

}
```
以下代码出错
```go
func main(){
    var allchan chan interface{}
    allchan = make(chan interface{},10)

    cat1 := cat{Name:2,Age:22,}
    cat2 := cat{Name:4,Age:26,}

    allchan <- cat1
    allchan <- cat2
    allchan <- 10
    allchan <- "tag"

    cat11 := <- allchan
    //此处cat11是interface{}类型的
    fmt.Println(cat11.Name)

    //var cat11 cat
    //cat11 := (<- allchan).(cat)
}
```
### 管道的关闭和遍历
当管道关闭后，只能读(取出)，不能写(入)
管道未关闭时，将管道值全部取出后，若再索取值，则死锁
```go
func main(){
    intchan := make(chan int,10)
    intchan <- 100
    intchan <- 200
    close(intchan)
    //关闭管道

    //管道的遍历
    //在遍历时，管道未关闭，则会出现死锁(阻塞)
    //在遍历时，管道关闭，则正常遍历
    intchan2 := make(chan int ,100)
    for i:=1;i<101;i++{
        intchan2 <- i
    }
    //注意不能用如下方式遍历
    // for i:=0;i<len(intchan2);i++{
    //     ...
    // }

    // close(intchan2)

    lenth := len(intchan2)
    p := 0
    for i:=0;i<lenth;i++{
        p <- intchan2
        fmt.Println(p)
    }
    //此时再p = <- intchan2则死锁


    //for range遍历
    intchan3 := make(chan int ,100)
    for i:=1;i<101;i++{
        intchan3 <- 2*i
    }
    //未关闭管道后则在取出所有数据后死锁
    //如果要for-range遍历则必须关闭管道
    //不然阻塞
    close(intchan3)
    for v := range intchan3{
        fmt.Println(v)
    }
    
}
```
管道关闭后，取出所有管道的值，再取值则返回默认值和状态值false
close(chan)
v,ok := <- chan
```go
func main(){
    i := make(chan int,5)

    i <- 5
    i <- 6
    close(i)

    v ,ok := <- i
    fmt.Println(v)//5
    fmt.Println(ok)//true
    v,ok = <- i
    fmt.Println(v)//6
    fmt.Println(ok)//true
    v,ok = <- i
    //此时管道已为空
    //再次进行取值
    fmt.Println(v)//0
    fmt.Println(ok)//false



    j := make(chan string,5)

    j <- "asedfa"
    j <- "asedf"
    close(j)

    o ,nok := <- j
    fmt.Println(o)//"asedfa"
    fmt.Println(nok)//true
    o,nok = <- j
    fmt.Println(o)//"asedf"
    fmt.Println(nok)//true
    o,nok = <- j
    fmt.Println(o)//""
    fmt.Println(nok)//false

}
```
#### 与go一起的应用案例
1.开启一个协程向管道写入数据
2.开启另一个协程从管道读取数据
3.两个管道是一样的
4.【等待协程结束再结束主协程】
```go

func  Write(i chan int){
    for i:=0;i<50;i++{
        i <- i
    }
    close(i)
}

func Read(i chan int,j chan bool){
    for{
        //<- i在错误时返回两个值
        //在读取完毕后返回false
        //只有在管道关闭的时候返回false
        v,ok := <-i
        if !ok{
            break
        }
        fmt.Println("取到:",v)
    }
    //向监听管道发送信息
    j <- true
    //重要
    close(j)
}




 func main(){
    //创建读写管道
    intchan := make(chan int,50)
    //创建监听管道
    supervisechan := make(chan bool,1)


    go Write(intchan)
    go Read(intchan,supervisechan)

    for{
        //一直向监听管道读取
        //当监听管道没有值时
        //被阻塞，没有返回值
        //一直等待返回值
        //当成功读取数据后，管道被关闭且清空
        //返回false

        //对一个空管道会等待取值
        _ , ok:= <- supervisechan
        //fmt.Println(" ok is ",ok)
        if !ok {
            break
        }
    }

    //或者
    //这样就不需要Read关闭管道了
    //但是耦合性变高了
    // for {
    //     v,_ := <-supervisechan
    //     if v {
    //         break
    //     }
    // }


 }
```
 终端输出结果(加了sleep和打印后)
 ```
 写入: 0
取到: 0
取到: 1
写入: 1
写入: 2
取到: 2
写入: 3
取到: 3
写入: 4
取到: 4
写入: 5
取到: 5
写入: 6
取到: 6
写入: 7
取到: 7
写入: 8
取到: 8
写入: 9
取到: 9
ok is  true
ok is  false
 ```
练习
#### 异步
当管道上下文有写入时，即使为空，读取也会等待不会死锁
当管道上下文有读取时，即使为满，写入也会等待不会死锁
```go
type ADD_UPP struct{
    Key int
    Sum int
}

//将1~2000加入管道
func Add(indexChan chan int){
    for i:=0;i<2000;i++{
        indexChan <- i + 1
    }
    close(indexChan)
}

//从管道抽取数组并做前数字项和的累加，添加给sumChan
//并将线程结束的信息添加给【线程监控管道】
func Draw_Cal_Send(indexChan chan int,sumChan chan ADD_UPP,Thread_condition_Chan chan bool/*k map[string]int*/){
    for {
        v, ok := <- indexChan
        if !ok {
            Thread_condition_Chan <- true
            break
        }

        var chan_element ADD_UPP
        sum_upp := 0
        for i:=1;i<=v;i++{
            sum_upp += i
        }
        chan_element.Key = v
        chan_element.Sum = sum_upp
        sumChan <- chan_element


        // if len(n) == 2000{
        //     close(n)
        // }


        //不能通过外部map来记录线程关闭情况
        //可能出现线程写入map的竞争，出现死锁
        //以下是错误代码

        //管道状态记录
        // k["condition"] += 1
        // if k["condition"]==2000{
        //     close(n)
        // }
    }
}

//用来关闭sumChan管道
//接收8个线程来的状态信息
//当确认8个线程全部完成工作后关闭sumChan
func Close_sumChan(Thread_condition_Chan chan bool,sumChan chan ADD_UPP){
    //可以看到
    //当有写入时
    //从管道提取数值
    //即使管道不关闭也不会发生死锁
    //此处管道容量为1
    //提取会等待写入
    for i:=1;i<=8;i++{
        <- Thread_condition_Chan
    }
    close(Thread_condition_Chan)
    close(sumChan)
}

//提取sumChan内的信息并打印
//将提取完毕的信息返回给【提取监控管道】
//并关闭提取完成信息监控管道
func Print_chan(sumChan chan ADD_UPP,Read_condition_chan chan bool){
    for {
        v,ok:= <- sumChan
        if !ok{
            break
        }
        fmt.Printf("%v的前%v项累加和是%v\n",v.Key,v.Key,v.Sum)
    }
    Read_condition_chan <- true
    close(Read_condition_chan)
}

func main(){

    //注意，除了indexChan外都可以将管道只设为容量1
    //因为其余管道都是边写入边提取
    //正因为如此
    //在线程中不能根据管道长度来判断是否关闭管道
    Read_condition_chan := make(chan bool,1)
    indexChan := make(chan int,2000)

    //sumChan容量只有1
    //有8个线程往里面赛数据
    //但是不会死锁因为上下文有一个读取的线程
    sumChan := make(chan ADD_UPP,1)
    Thread_condition_Chan := make(chan bool,1)
    // k := make(map[string]int)
    // k["condition"] = 0


    Add(indexChan)

    //从管道取数字，将ADD_SUM类写入管道、监控线程状态和从管道提取ADD_SUM类是同步进行的
    for i:=0;i<8;i++{
        go Draw_Cal_Send(indexChan,sumChan,Thread_condition_Chan/*,k*/)
    }
    go Close_sumChan(Thread_condition_Chan,sumChan)
    go Print_chan(sumChan,Read_condition_chan)

    for {
        _ ,ok:= <- Read_condition_chan
        if !ok{
            break
        }
    }

}


//不通过ok判断也可以

// func Print_chan(sumChan chan ADD_UPP,Read_condition_chan chan bool){
//     for i:=1;i<=2000;i++{
//         v,_:= <- sumChan

//         fmt.Printf("%v的前%v项累加和是%v\n",v.Key,v.Key,v.Sum)
//     }
//     Read_condition_chan <- true
//     close(Read_condition_chan)
// }
```
声明只读或只写管道
```go
//只写
var chan1 chan <- int
chan1 = make(chan int,1)

//只读
var chan1 <- chan int
chan1 = make(chan int,1)

func test(t chan <- int,i int){
    ...
}
```
#### select
```go
intchan := make(chan int,10)
for i:=1;i<=10;i++{
    intchan <- i
}
stringchan := make(chan string,5)
for i:=1;i<=5;i++{
    intchan <- "hello"+fmt.Sprintln("%d",1)
}

//在实际开发中有可能不确定什么时候关闭管道
//select方法
label:
for {
    select{
        case v := <- intchan:
        //如果intchan没有关闭，也不会阻塞
        //而会向下进行case匹配
            fmt.Println(v)
        case v := <- stringchan:
            fmt.Println(v)
        default:
             fmt.Println("都取不到了")
             break label
    }
}
```
### 反射
json序列化
函数适配器(桥)

反射可以在运行的时候动态获取变量的各种信息，比如变量的类型和类别(kind)
如果是结构体变量，还可以获取到结构体本身的信息(字段和方法)
通过反射，可以修改变量的值，可以调用关联的方法
使用反射，包reflect

```go
//传入一个空接口
//返回一个Type类型(struct)
reflect.TypeOf(v interface{})Type

//传入一个空接口
//返回一个Value类型(struct)
reflect.ValueOf(v interface{})Value


//利用空接口增大函数的适用性范围
//但是传入的参数类型变为空接口类型
//所以利用反射将传入的空接口类型变量
//转换为反射Value变量
//然后处理变量
//再通过反射转换为空接口类型
//通过反射将空接口类型断言
//返回传入参数的类型
func A_func(i interface{}){
    i_Value := reflect.ValueOf(i)
    i_Type := reflect.TypeOf(i)

    //获取kind
    kind := i_Value.Kind()
    //或
    kind := i_Type.Kind()  


    //...处理i_Value

    i_interface := i_Value.Interface()

    return i_interface.(Original_Type)
}
//这样通过此函数可以处理所有的数据类型，并返回相应的数据类型
```
更改原始数据的值
```go
func main(){
    var str string = "tom"
    fs := reflect.ValueOf(str)
    fs.SetStirng("jack")
    fmt.Println(str)
    //报错

    //正确的
    var str string = "tom"
    //要更改原始数据的值，要引用传入
    fs := reflect.ValueOf(&str)
    //此时fs是指针，但是SetStirng只能
    //作用于Value类型的值
    //因此调用Valu.Elem()返回一个Value类型
    //这个类型是指针指向的值
    fs.Elem().SetStirng("jack")
    fmt.Println(str)
}
```
反射案例
用反射获取结构体字段、方法和标签值
```go
//获取结构体的第i个方法，返回这个方法的Value类型
func (v Value)Method(i int)Value

//使用指定的Value类型的方法
//通过[]Value类型传入该方法的参数
func (v Value)Call(in []Value)[]Value





type Monster struct{
    ...
}

func (s Monster)Print(){
    ...
}
func (s Monster)GetSum(n int,m int){
    ...
}
func func (s Monster)Set(name string,age int,score float64,sex int){
    ...
}


func main(){
    var a Monster{
        ...
    }
    testMonster(a)
}
//
//
func TestMonster(i interface{}){
    //获取reflect.Type类型
    typ := reflect.TypeOf(i)
    //获取reflect.Value类型
    val := reflect.ValueOf(i)
    //获取类别
    kd := val.Kind()
    //判断是否是结构体
    //reflect.Struct是在reflect包下定义的常量
    if kd != reflect.Struct{
        fmt.Println("不是结构体，不玩了。")
        return
    }
    //获取该结构体有几个字段
    numfield := val.Numfield()
    //遍历字段
    //Value.Field(i int)Value
    for i:=1;i<=numfield;i++{
        fmt.Pirintf("Field %d:值为=%v\n",i,val.Field(i))
        //获取标签
        //Type.Field(i int)StructField
        //StructField是个结构体
        //Type StructField struct{
        //     ...
        //     Type : Type  //字段类型
        //     Tag : StructTag
        //     ...
        // }

        //func (tag StructTag)Get(key string)string
        tagval := typ.Field(i).Tag.Get("json")
        //如果有标签显示标签
        if tagval != ""{
            fmt.Println("Field %d: tag为=%v\n",i,tagval)
        }
    }

    //获取有多少个方法
    num_of_methed := val.NumMethod()
    //调用方法
    //调用第几个方法和排序无关
    //按照方法名的ASCII码排序
    //调用方法序列的第2个方法
    val.Method(1).Call(nil)

    //声明一个refect.Value类型的切片存储函数参数
    var params []reflect.Value
    params = append(params,reflect.ValueOf(10))
    params = append(params,reflect.ValueOf(44))
    //调用第一个函数
    //res同样是[]Value
    res := val.Method(0).Call(params)
    fmt.Println("res=",res[0].Int())
}
```
定义一个适配器函数，统一处理所有接口
```go
//查看一个没有返回值的函数被Call调用后返回什么
func Test(n int){
    fmt.Println("n is ",n)
}
func Test2(m int)int{
    return m
}


func Bridge(acc interface{},args...interface{}){
    lenth := len(args)
    //注意make的用法
    //mapslice := make([]map[int]string,n)
    //make(Type,int)Type
    //要注意这里Type是引用类型
    //内建函数make分配并初始化一个类型为切片、映射、或通道的对象。
    //其第一个实参为类型，而非值。make的返回类型与其参数相同，而非指向它的指针。
    //func new(Type) *Type
    //内建函数new分配内存。其第一个实参为类型，而非值。
    //其返回值为指向该类型的新分配的零值的指针。
    params := make([]reflect.Value,lenth)
    for i:=0;i<lenth;i++{
        params[i] = reflect.ValueOf(args[i])
    }
    function := reflect.ValueOf(acc)
    res := function.Call(params)
    fmt.Println("res is ",res)
}

func main(){
    Bridge(Test,2)
    Bridge(Test2,6)
}
```
```go
n is  2
//没有返回值的函数返回空切片
res is  []
res is  [<int Value>]
```
操作改写任意结构体
```go
//注意以引用方式传入
//此处acc是指针
func ReWrite_Struct(acc interface{}){
    ptr_Value := reflect.Value(acc)
    ValuE := ptr_Value.Elem()

    //转到对应键值的内容上
    Key_Content := ValuE.FieldByName(ValuE.Field(1).String())
    Key_Content_type := Key.Content.Kind()

    //修改字段
    Key_Text.SetString("xxx")

}
```
用反射创建结构体
func New(typ Type) Value
要注意这里的Value是指针
Value.Elem()
# TCP
客户端
n,err := conn.Write([]byte(line))
n是发送字节的个数
```go
package main

import (
	"fmt"
	"net"
	"bufio"
	"os"
)


func main(){
	//连接此ip的8888端口
	//conn就是线
	conn ,err := net.Dial("tcp","192.168.1.4:8888")
	if err != nil{
		fmt.Println("client err = ",err)
		return
	}
	fmt.Println("conn 连接成功 = ",conn)



	//创建一个reader，从标准输入流中读取信息
	reader := bufio.NewReader(os.Stdin)

	for {

		//把信息拿到缓存
		line,err := reader.ReadString('\n')
		if err != nil{
			fmt.Println("reading err = ",err)
			return
		}


		//准备将信息发给端口
		//n是字节数
		//传入的是byte切片

		//去除换行符
		//!!!!!!"\r\n"
		//\n是自动添加的
		//line = strings.Trim(line,"\r\n")
		if line == "exit\r\n"{
			fmt.Println("客户端主动退出")
			return
		}
		n,err := conn.Write([]byte(line))
		if err != nil {
			fmt.Println("conn.Write err = ",err)
			break
		}
		fmt.Printf("客户端发送了%v字节\n",n)
	}


}
```
服务器端
n,err := conn.Read(buf []byte)
```go
package main

import (
	"fmt"
	"net"
)


//开协程接受客户端消息
func process(conn net.Conn){
	defer conn.Close()
        //关闭对客户端ip端口的连接
	//及时关闭线程

	for {
		//1.等待客户通过conn发送信息 
		//2.如果客户端没有Write则阻塞(可能timeout)
		buf := make([]byte,1024)

		fmt.Printf("等待客户端%v发送信息。。。\n",conn.RemoteAddr().String())

		//只有当用户连接时才会阻塞等候消息
		//当用户关闭连接时候，
		n,err := conn.Read(buf)
		if err != nil{
			fmt.Println("服务器Read err =  ",err)
			return
		}
		//3.显示客户端发送的内容到服务器终端
		//没有ln，因为ReadString('\n')
		//将传送来的[]byte强制转换为string输出到终端
		//只读取传送来的信息，n是传送受到的byte个数
		fmt.Print(string(buf[:n]))
	}
}




func main(){
	fmt.Println("服务器开始监听...")
	//tcp表示使用网络协议是tcp
	//0.0.0.0:8888表示监听的端口是8888
	listen,err := net.Listen("tcp","0.0.0.0:8888")
	if err != nil{
		fmt.Println("listen err = ",err)
		return
	}
	defer listen.Close()

	//循环等待
	for {
		//等待客户端来连接
		conn,err := listen.Accept()
		if err != nil{
			fmt.Println("Accept() err = ",err)
		}else{
			fmt.Printf("conn is %v,客户端ip是%v\n",conn,conn.RemoteAddr().String())
		}


		go process(conn)
	}
}
```
## 海量用户即时通讯系统
1.用户注册
2.用户登录
3.显示用户在线信息
4.用户广播(群聊)
5.点对点聊天
6.离线留言
#### Redis和在go中使用Redis
远程字典服务器
15万qps，非常适合做缓存
缓存型内存型数据库
高性能的Key/Valu分布式的内存数据库，(not only)NoSQL数据库

Redis支持的数据类型
字符串、哈希、list、Set、有序集合
```redis
set key1 hello

get key1
"hello"
//切换数据库
select 1

select 0
//返回数据个数
dbsize
<integer> 1

//清空当前数据库
flushdb

//删除键值
del key1

//设定数据存储时间
setex key time content

setex key1 10 srgaff

//输入多个数据
mset key1 aeff key2 aefaf key3 pppp

mget key1 key2

//追加
append key1 123
"aeff123"

//查看所有键
keys *

//简单正则
mset a1 1 a2 2 a3 3
keys a*

//给已有键值设置时间
expire a1 100

//查看过期时间
ttl a1

```
```
//哈希——键值对的集合
hset user1 name smith

//type 返回键类型
type user1
hash

hset user1 age 30

hset user1 job engineer

//
hgetall user1

//获取哈希数据
hget user1 name

hget user1 age
"30"//都是字符串

hget user1 job

//删除哈希字段
hdel user1 name

//一次性输入
hmset user3 name jerry age 110 job java_coder

hmget user3 name age job

//看哈希表中给定域是否存在
hexists user3 name

//统计长度
hlen user3

//获得hash键值字段
hkeys user3

//获得hash键值字段值
hval user3
```
```
//列表_从左边往里推
lpush listName beijing shanghai guangzhou

//从0取到-1
lrange city 0 -1

//list操作
lpush\rpush\lpop(从左边取出并移走)\rpop\lrange\del(删除整个list)\llen\lrem(remove)

//删除
//lrem lstName count content
//删除多少个content
//count>0从左往右删除
//count = 0删除全部
lpush lstN a1 a2 a3 a2 a3 a1 a6 a2 a3
lrem lstN 0 a3
```
```
//Set的存储，元素无序，元素的值不能重复
//比如不能sadd setName a@mail a@mail
//ascii 排序
//动作
sadd\smembers(查看集合中所有元素)\sismember(判断是否有这个元素)\srem(remove一个元素)

//有序集合
//添加权重
//权重越小优先级越高
zadd test 2 a1 6 a2 4 a3
zrange test 0 -1
1) "a1"
2) "a3"
3) "a2"

//访问权重之间的数据
zrangebyscore test 2 4
1) "a1"
2) "a3"

//获取权重
zscore test a2
"6"

//
zrem/zrange/
```

### golang操作redis库
golang添加和获取key-value
引包
```go
import (
    "fmt"
    "github.com/garyburd/redigo/redis"
)
```
```go
func main(){
    //通过go连接到redis
    //1.本地连接到本地redis
    //redis.Dial(net,address:port)conn,err
    conn,err := redis.Dial("tcp",":6379")
    if err != nil{
        fmt.Println("连接redis错误，err = ",err)
        return
    }
    //关闭
    defer conn.Close()


    fmt.Println("连接成功，conn = ",conn)

    //通过go写入key-value string
    _, err = conn.Do("Set","name","tom")
        if err != nil{
        fmt.Println("set err = ",err)
        return
    }

    //读取数据
    //conn.Do(command,command_args...interface{})interface{},err
    //返回的是空接口
    //不能类型断言
    //receiptor = receiptor.(string)
    //redis.String(interface{},err)string err
    //redis.Strings(interface{},err)[]string err
    receiptor, err := redis.String(conn.Do("Get","name"))
    if err != nil{
        fmt.Println("set err = ",err)
        return
    }
    fmt.Println("receiptor is ",receiptor)



    //操作哈希并取值
    _,err = redis.String(conn.Do("HMSet","Hash1","name","tompson",
                                                "Age","20","Hobby","skiing"))
    if err != nil{
        fmt.Println("Hash set err = ",err)
        return
    }

    //String将一个空接口类型转换为[]string
    receiptor_hash,err := redis.Strings(conn.Do("HMGet","Hash1","name","Age","Hobby"))
    if err != nil{
        fmt.Println("Hash set err = ",err)
        return
    }
    fmt.Println("receiptor_hash is ",receiptor_hash)
    //[tompson 20 skiing]



}
```

### 连接池
```go
//定义一个连接池指针
//因为要对连接池引用传入
var pool *redis.Pool


func init(){
//连接池的参数
    pool = &redis.Pool{

        //最大空闲链接数
        //保证池中最少有多少个活跃的链接
        //当超过这个数目时，就不能直接连接了，要先dial了
        MaxIdle : 8,

        //表示和数据库的最大连接数，0表示没有限制
        //不限制客户端对服务器的连接数目
        MaxActive : 0,

        //最大空闲时间
        //当一个连接关闭超过xxx秒没有再次活跃时，
        //并且当前连接池中有超过8个空闲连接，
        //关闭该连接
        IdleTimeout : 100,

        //初始化链接的代码，链接哪个ip的redis
        Dial : func()(redis.Conn,error){
            return redis.Dial("tcp","localhost:6379")
        },
    }
}
//从连接池中取出一个链接
//引用地
c := pool.Get()

//关闭连接池
//一旦关闭连接池就不能从池中取链接了
//pool.Close()

func main(){
    conn := pool.Get()
    defer conn.Close()

    _,err := conn.Do("Set","name","tome")
    if err != nil{
        fmt.Println("set err = ",err)
        return
    }


    receiprot,err := redis.String(conn.Do("Get","name"))
    if err != nil{
        fmt.Println("Get err = ",err)
        return
    }
    fmt.Println("receiptor is ",receiptor)


    pool.Close()
}
```
重要redis函数和方法
```go
redis.Dial(net,port)conn,err
//不执行，将命令刷新到缓冲区
conn.Send(command,args..interface{})error
//将缓冲区推到redis
conn.Flush()error
//接受返回值
//如果没有可以不写
conn.Receive()(interface{},error)


func main(){
    conn,_:=redis.Dial("tcp",":6379")
    defer conn.Close()

    conn.Send("Set","name","tom")
    conn.Send("Get","name")

    conn.Flush()

    re,err := conn.Receive()
    fmt.Printlb(re)
}


conn.Do(command,args...interface{})interface{},error
redis.String(interface{},error)(string,error)
redis.Strings(interface{},error)[]string{},error
//for-range遍历

//重要
//能够分别处理不同类型的返回值
//Values将interface{}转成[]interface{}
redis.Values(interface{},error)[]interface{},error
//扫描传入空接口接片，一次按照args...的类型声明赋值给args...
redis.Scan([]interface{},args...interface{})[]interface{},error

//Values和Scan结合处理不同类型的返回值
func main(){
    ...
    re,err := redis.Values(conn.Do("MGet","KeyName1","KeyName2","KeyName3"))
    var KeyName1 string
    var KeyName2 int
    var KeyName3 string
    redis.Scan(re,&KeyName1,&KeyName2,&KeyName3)
    ...
}

//输出显示中文
redis-cli.exe --raw




//序列化和反序列化
//将自定义数据类型放入redis
re,_ := redis.Bytes(interface{},error)[]byte,error
var Type TypeName
err := json.Unmarshal(re,&Type)






type Monster struct{
    Name string
    Age int
    Address []map[string]map[int]string
}


func main(){
    conn,err := redis.Dial("tcp",":6379")
    if err != nil{
        fmt.Println("dial err = ",err)
        return
    }
    defer conn.Close()
    //开始定义一个切片对象
    arr_map := make([]map[string]map[int]string,2)


    arr_map_0 := make(map[string]map[int]string,2)

    arr_map_0_0 := make(map[int]string,2)
    arr_map_0_0[1] = "pig"
    arr_map_0_0[2] = "sheep"
    arr_map_0["animal"] = arr_map_0_0

    arr_map_0_1 := make(map[int]string,2)
    arr_map_0_1[1] = "woman"
    arr_map_0_1[2] = "man"
    arr_map_0["human"] = arr_map_0_1


    arr_map_1 := make(map[string]map[int]string,2)

    arr_map_1_0 := make(map[int]string,2)
    arr_map_1_0[1] = "circle"
    arr_map_1_0[2] = "triangle"
    arr_map_1["shape"] = arr_map_1_0

    arr_map_1_1 := make(map[int]string,2)
    arr_map_1_1[1] = "correct"
    arr_map_1_1[2] = "wrong"
    arr_map_1["condition"] = arr_map_1_1


    arr_map[0] = arr_map_0
    arr_map[1] = arr_map_1
    //结束定义一个切片对象

    Monster1  := Monster{
    Name : "Pco",
    Age : 22,
    Address : arr_map,
    }

    //序列化Monster1
    data,err := json.Marshal(Monster1)
    data_string := string(data)
    fmt.Println(data_string)
    //存到redis
    _,err = conn.Do("Set","Monster1",data_string)
    if err != nil{
        fmt.Println("set err = ",err)
        return
    }
    //从redis内取出
    re,err := redis.Bytes(conn.Do("Get","Monster1"))
    if err != nil{
        fmt.Println("get err = ",err)
        return
    }

    //反序列化
    var shell Monster
    err = json.Unmarshal(re,&shell)
    if err != nil{
        fmt.Println("unmarshal err = ",err)
        return
    }

    fmt.Println(shell)
    //{Pco 22 [map[animal:map[1:pig 2:sheep] human:map[1:woman 2:man]] 
    //map[condition:map[1:correct 2:wrong] shape:map[1:circle 2:triangle]]]}
}
```
## server
```go
//分两次分别读取数据长度和数据
//1.函数返回值此时已经被定义，可以被直接调用、
//2.不需要显示return value，只需要在函数内出现即可
//3.var b [10]byte    c := b[:]    更改c也会更改b
//4.当c的len比b高了会另外复制一份，不会更改b

func readpkg(conn net.Conn)(mess Common.Message,err error){
	buf := make([]byte,4096)
	n,err := conn.Read(buf[:4])
	if n != 4 || err != nil{
		fmt.Println("conn.Read err =",err)
		return
	}

	var pkglen uint32
	pkglen = binary.BigEndian.Uint32(buf[:])

	//分两次读取，也就有两次发送
	n,err := conn.Read(buf[:pkglen]) 
	if n != int(pkglen) || err != nil{
		fmt.Println("conn.Read err = ",err)
		return
	}
	//此处mes在返回值时已经被定义
	err = json.Unmarshal(buf[:pkglen],&mes)
	if err != nil{
		fmt.Println("unmarshal err = ",err)
		return
	}

}
```
# 数据结构
### 稀疏数组
```go
type sparse struct{
	Row int
	Col int
	Val int
}



func main(){
	var chess_map [11][11]int
	chess_map[1][2] = 1
	chess_map[2][3] = 2



	var sparse_array []sparse
	var total_col int = 0
	total_row := 0
	for i,v := range chess_map{
		_col := 0
		for j,val := range v{
			if val != 0{
			valnode := sparse{
				Row : i,
				Col : j,
				Val : val,
			}
			_col += 1
			sparse_array = append(sparse_array,valnode)
			}
		}
		total_col = _col
		total_row += 1
	}

	status_node := sparse{
		Row : total_row,
		Col : total_col,
		Val : 0,
	}
	sparse_array = append(sparse_array,status_node)
	//json保存



	//恢复数据

	var chess_map2 [][]int
	for i,v := range sparse_array{
		if i == len(sparse_array)-1{
		break
		}
		chess_map2[v.Row][v.Col] = v.Val
	}

}
```
### 环形队列
取模
```go
import (
	"errors"

)

type Ring_Queue struct{
    Maxsize int
    End int //尾(最后取出)----指向最后一个元素的后一个元素
    Top int //头(最先取出)----包括第一个元素
    Arr [3]int
}


func (q *Ring_Queue)AddRing_queue(a int)(err error){
    if q.End - q.Top == q.Maxsize{
    	return errors.New("Queue Oversize!")
    }   
    q.Arr[q.End % q.Maxsize] = a
    q.End += 1
    return
}
func (q *Ring_Queue)TakeRing_queue()(p int,err error){
    if q.Top == q.End{
    	err = errors.New("队列为空")
    	return
    }
    p = q.Arr[q.Top % q.Maxsize]
    q.Top ++ 
    return
}
//显示有多少个元素
func (q *Ring_Queue)NUm()int{
	return q.End - q.Top
}
```
### 单向链表
```go

import (
	"fmt"
)

//定义一个hero节点
type Hero struct{
	No int
	Name string
	Nickname string
	Next *Hero
}

//编写第一种插入方式
func InsertHero(head *Hero,newHero *Hero){
	//先找到链表最后的节点
	//创建一个辅助节点
	temp := head
	for{
		if temp.Next == nil{
			break
		}
		temp = temp.Next
	}
	//将节点插入最后
	temp.Next = newHero
}
func InsertHero2(head *Hero,newHero *Hero){
	//先找到链表最后的节点
	//创建一个辅助节点
	temp := head
	flag := true
    for{
        //如果temp的Next就是nil。那就直接插到后面
        if temp.Next == nil{
        	temp.Next = newHero
        	break
        //如果后面较大，那么插到他的前面
        }else if temp.Next.No > newHero.No{
        	newHero.Next = temp.Next
        	temp.Next = newHero
        	break
        //如果和后者相等，就拒绝插入
        }else if temp.Next.No == newHero.No{
        	flag = false
        	break
        //不然就将temp向后跳一位
        }else{
        	temp = temp.Next
        }

    }
	//将节点插入最后
    if !flag {
    	fmt.Println("有相同的节点了，不能插入！")
    }

}
//显示链表的所有信息
func ListHero(head *Hero){
	temp := head
    for {
        if temp.Next == nil{
        	fmt.Println("最后一个节点")
        	break
        }
        fmt.Printf("[%d,%s,%s]\n",temp.Next.No,
                        temp.Next.Name,temp.Next.Nickname)       
        temp = temp.Next
    }
}
//删除链表
//删除链表的节点
//特点
//单项列表无法自我删除
//即temp不能跑到所要查找到的节点上，
//只能在之前的节点才能删除后面的节点
//没有空指针，所以你不能让指针指向空
func DelHero(head *Hero,id int){
    temp := head
    tag := false
    for {
        if temp.No == id {
            temp.Next = nil
            tag = true
            break
        }else if temp.Next.No == id{
            temp.Next = temp.Next.Next
            tag = true
            break
        }else{
            temp = temp.Next
        }
    }
    if !tag {
        fmt.Println("没有找到")
    }
}


func main(){
	//创建头节点
    hero2 := &Hero{
        No : 5,
        Name : "鲁智深",
        Nickname : "笑面佛",
    }
    hero1 := &Hero{
        No : 1,
        Name : "松江",
        Nickname : "及时雨",
    }
	head := &Hero{
    }
    hero3 := &Hero{
        No : 3,
        Name : "松江",
        Nickname : "及时雨",
    }

	//创建一个新的Hero


    InsertHero2(head,hero2)
    InsertHero2(head,hero1)
    InsertHero2(head,hero3)

    ListHero(head)
}
```
### 双向列表
```go
type Double_Hero struct{
    No int
    Name string
    Nickname string
    Next *Double_Hero
    Pre *Double_Hero
}



//插入到最后
func InsertHero_Next(head *Double_Hero,newHero *Double_Hero){
    //先找到链表最后的节点
    //创建一个辅助节点
    temp := head
    for{
        if temp.Next == nil{
        	break
        }
        temp = temp.Next
    }
    //将节点插入最后
    temp.Next = newHero
    //重要
    //别忘了
    newHero.Pre = temp
	
}

//插入到最前
func InsertHero_Pre(head *Double_Hero,newHero *Double_Hero){
    //先找到链表最后的节点
    //创建一个辅助节点
    temp := head
    for{
        if temp.Pre == nil{
        	break
        }
        temp = temp.Pre
    }
    //将节点插入最后
    temp.Pre = newHero
    newHero.Next = temp
	
}

//顺序显示链表的所有信息
func ListHero_sequence(head *Hero){
    temp := head
    for {   
        fmt.Printf("[%d,%s,%s]\n",temp.Next.No,
        				temp.Next.Name,temp.Next.Nickname)  
        if temp.Next == nil{
        	fmt.Println("最后一个节点")
        	break
        }
        temp = temp.Next
    }
}

//逆序显示链表所有信息
func ListHero_inverted(tail *Double_Hero){
    temp := tail
    for {
        fmt.Printf("[%d,%s,%s]\n",temp.Pre.No,
        				temp.Pre.Name,temp.Pre.Nickname)
        if temp.Pre == nil{
        	fmt.Println("第一个节点")
        	break
        }
        temp = temp.Pre
    }
}

//顺序插入
func InsertDoubleHero_sequence(head *Double_Hero,new *Double_Hero){
    temp := head
    for {
        if temp.Next == nil{
        	temp.Next = new
        	new.Pre = temp
        	break
        }else if temp.Next.No > new.No{
        	new.Next = temp.Next
        	temp.Next.Pre = new 
        	new.Pre = temp
        	temp.Next = new
        	break
        }else if temp.Next.No == new.No{
        	fmt.Println("已存在相同编号的节点")
        	break
        }else{
        	temp = temp.Next
        }
    }
}

//逆序插入
func InsertDoubleHero_inverted(tail *Double_Hero,new *Double_Hero){
    temp := tail
    for {
        if temp.Pre == nil{
        	temp.Pre = new
        	new.Next = temp
        	break
        }else if temp.Pre.No < new.No {
        	new.Pre = temp.Pre
        	temp.Next.Pre = new 
        	temp.Pre = new
        	new.Next = temp
        	break
        }else if temp.Pre.No == new.No{
        	fmt.Println("已经存在相同编号的节点了")
        	break
        }else{
        	temp = temp.Pre
        }
    }
}

//双向链表删除节点
func DelDouble_Hero(head *Double_Hero,d int){
    temp := head
    tag := false
    for {
        if temp.Pre == nil && temp.No == d{
        	temp.Next = nil
        	tag = true
        	break
        }else if temp.Next == nil && temp.No == d{
        	temp.Pre = nil
        	tag = true
        	break
        }else if temp.No == d{
        	temp.Next.Pre = temp.Pre
        	temp.Pre.Next = temp.Next
        	tag = true
        	break 
        }else {
        	temp = temp.Next
        }
    }
    if tag == false{
    	fmt.Println("没有找到！")
    }
}
```
### 环形链表
node1---->node2---->node1
```go
func Del_cycleNode(head *Cycle,id int){
    temp := head

    //如果是空链表
    if temp.Next == nil{
        fmt.Println("空链表，不能删除")
        return
    }
    //单个节点的环形链表
    //断开链表
    if temp.Next == head{
        temp.Next = nil
        return
    }
    //两个及以上节点
    tag := false
    for {
        if temp.Next.No == id{
            temp.Next = temp.Next.Next
            tag = true
            break
        }
    }
    if tag == false{
        fmt.Println("没有找到")
    }
}
```
### 排序
#### 选择排序
```go
func FindMax(arr []int){
    //找出最大值
    //将其与第一个元素的位置呼唤

    max := arr[0]
    index := 0
    for i:=1;i<len(arr);i++{
        if max < arr[i]{
            max = arr[i]
            index = i
        }
    }

    //交换
    if index != 0{
        arr[0],arr[index] = arr[index],arr[0]
    }
}

func SelectSort(arr *[5]int){
    for i:=0;i<len(arr)-1;i++{
        FindMax(arr[i:])
    }
}





func main(){
    ar := [5]int{10,34,19,100,80}
    SelectSort(&ar)
    fmt.Println(ar)
}
```
#### 插入排序
二分查找插入排序
```go
func BiFind(arr []int,left_index int,right_index int,a int)(int){
    //如果从小到大排序
    //此时arr[left_index]比a大
    //arr[right_index]比a小
    //从大到小排序则相反
    //所以无论如何a应该插在两者之间
    if left_index > right_index{
        return right_index + 1
    }

    mid_index := (left_index + right_index) / 2
    if arr[mid_index]>a{
        left_index = mid_index + 1
        return BiFind(arr,left_index,right_index,a)
    }else if arr[mid_index]<a{
        right_index = mid_index - 1
        return BiFind(arr,left_index,right_index,a)
    }else{
        //相等条件下插mid_index上
        //考虑到如果a是最后一项
        return mid_index 
    }
}
func Insert(arr []int,a int){
    //从大到小排序
    arr = append(arr,a)
    end_index := len(arr)-1
    //找到插入的索引值
    insert_index := BiFind(arr,0,end_index,a)
    //索引值后的值向后位移
    for {
        if end_index == insert_index{
            break
        }
        arr[end_index] = arr[end_index - 1]
        end_index --
    }
    //在索引值位置插入值
    arr[insert_index] = a
}

func InserSort(arr *[5]int){
    ar := arr[:]
    for i,v := range arr{
        Insert(ar[:i],v)
    }
}

func main(){
    arr := [5]int{23,0,12,56,34}
    InserSort(&arr)
}
```
传统插入排序
```go
func InserSort(arr *[5]int){
    //完成第一次，给第二个元素找到合适的位置插入
    //待插入的值
    insertVal := arr[1]
    //待插值前一项的index
    insertIndex := 1 - 1
    
    //将insertIndex变成待插位置的前一项index
    for insertIndex >= 0{
        if arr[insertIndex] < insertVal{
            arr[insertIndex + 1] = arr[insertIndex]
            insertIndex --
        }
    }
    //插入
    arr[insertIndex + 1] = insertVal
}
```
```go
func InserSort(arr *[5]int){
    for i:=1;i<len(arr);i++{
        insertVal := arr[i]
        //待插值前一项的index
        insertIndex := i - 1
        
        //将insertIndex变成待插位置的前一项index
        for insertIndex >= 0{
            if arr[insertIndex] < insertVal{
                arr[insertIndex + 1] = arr[insertIndex]
            }
            insertIndex --
        }
        //插入
        arr[insertIndex + 1] = insertVal
    }
}
```
#### 慢速排序
 ```go
 //从大到小
 func QuickSort(left_index int,right_index int,Array *[5]int){
    left_index_next := left_index
    right_index_next := right_index
    //首先取中间比较基准数
    mid_index := (left_index + right_index) / 2
    base_val := Array[mid_index]

    //创建变量index，记录基准数移动过后的新索引
    index := mid_index

    //创建一个循环
    //把基准数左边比基准数小的数字扔到基准数右边
    for {
        //退出循环条件
        if left_index == index{
            break
        }
        if Array[left_index] < base_val{
            temp := Array[left_index]
            for i:=left_index;i<index;i++{
                Array[i] = Array[i + 1]
            }
            Array[index] = temp
            index --
        }else if Array[left_index] >= base_val{
            left_index ++
        }
    }

    //创建一个循环
    //把基准数右边比基准数大的数字扔到基准数左边
    for{
        //退出条件
        if right_index == index{
            break
        }
        if Array[right_index] > base_val{
            temp := Array[right_index]
            for i:=right_index;i>index;i--{
                Array[i] = Array[i - 1]
            }
            Array[index] = temp
            index ++
        }else if Array[right_index] <= base_val{
            right_index --
        }

    }
    //对基准数左边和右边的子Array再次进行快速排序
    if left_index_next <= index - 1{
        QuickSort(left_index_next,index - 1,Array)
    }
    if index + 1 > right_index_next{
        QuickSort(index + 1,right_index_next,Array)
    }
 }
 ```
 #### 快速排序法
 ```go
 //快速排序
 //以最后一个属作为pivot
 //从小到大
 func QuickSort(left int,right int,array []int){
    l := left
    r := right
    //把pivot设为数组最后一个数
    pivot := array[r]

    //一个循环
    //循环的作用是，
    //通过左右两个指针交替不断交换pivot
    //把比pivot大的数往后赛
    //把比pivot小的数往前赛
    //当l == r时候就是pivot的位置
    //退出循环
    //-[+]+--+-+-+口
    //-口+--+-+[-]++
    //--[+]--+-+口++
    //--口--+[-]++++
    //-----[+]口++++
    //-----口+++++

    for ; l != r ;{
        //左指针找到下一个比pivot大的值
        for ;array[l] <  pivot; {
            l++
        }
        //右指针找到下一个比pivot小的值
        //在第一次的循环中，右指针停留在pivot上
        for ;array[r] > pivot; {
            r--
        }
        //两个寻找循环都不能加上等号
        //因为要让指针停留在pivot上
        //从而让pivot进行位置交换
        //否则pivot会进入错误的位置

        //交换
        array[r],array[l] = array[l],array[r]

        //解决一个问题
        //就是当左右指针同时指向相同的值=pivot时
        //并且两者指针不相同时
        //随机让其中一个移动一步
        if array[l] == array[r] && l != r{
            l++
        }
        //这样一意味着，等于pivot的值在
        //pivot前面和后面的哪个位置都没有问题
    }

    //执行完循环，pivot或其相等的值到了正确的位置上了
    //进行递归操作
    if left <= l-1{
        //此处不需要上锁
        //因为处理的是切片
        //只要保证每个协程处理切片的元素没有交叉
       go QuickSort(left,l-1,array)
    }
    if right >= r + 1{
       go QuickSort(r+1,right,array)
    }
 }

 ```
 ```go
//从小到大
//取array的中间为pivot
 func QuickSort(left int,right int,array []int){
    l := left
    r := right

    mid := (left + right) / 2
    pivot := array[mid]

    //for循环的目的是将小的放到右边
    //大的放到左边
    for; l != r ;{

        for;array[l] < pivot;{
            l++
        }

        for;array[r] > pivot;{
            r--
        }
        //需要支出的是
        //这两个循环也不能等号
        //原因是当指针指向pivot时候
        //就就入了以array最后一个元素为pivot的场景

        //****
        //****
        //所以实际上选哪个值做pivot都可以

        array[l],array[r] = array[r],array[l]

        if array[l] == array[r] && l != r{
            l++
        }
    }

    if left <= l-1{
        QuickSort(left,l-1,array)
    }
    if right >= r + 1{
        QuickSort(r+1,right,array)
    }
}
```
### 栈
```go
type stack struct{
    //表示栈的容量
    Max int

    //栈顶
    Top int

    Arr [5]int
}

func (this *stack)Push(val int)(err error){
    if this.Top == Max - 1{
        return errors.New("栈已满")
    }

    this.Top ++ 
    this.Arr[this.Top] = val
    return
}
func (this *stack)Show_stack(){
    if this.Top == -1{
        fmt.Println("栈空")
        return
    }
    for i:=this.Top;i>=0;i--{
        fmt.Printf("arr[%d]=%d\n",i,this.Arr[i])
    }

}
func (this *stack)Pop()(val int){
    if this.Top == -1{
        fmt.Println("栈空")
        return
    }
    val = this.Arr[this.Top]
    this.Top --
    return
}


func main(){
    sta := &stack{
        Max : 5,
        Top : -1,
    }
}
```
### 计算栈与计算过程
遇到优先级高的运算先处理，然后往右走，最后往左走
如：3 + 2 * 6 -  2
1. 计算 2 * 6 = 12
2. 计算 12 - 2 = 10
3. 计算 10 + 3 = 13
   
数栈与符号栈实现计算过程
```go
//判断字符是否是运算符
func (this *stack)isOper(val int)bool{
    if val == 42 || val == 43 || val == 45 || val == 47 {
        return true
    }else {
        return false
    }
}

//运算方法
func (this *stack)Cal(num1 int,num2 int,oper int)int{
    res := 0
    switch oper {
        case 42 :
            res = num1 * num2
        case 43 :
            res = num2 + num1
        case 45 :
            res = num2 - num1
        case 47 :
            res = num2 / num1
        default:
            fmt.Println("运算符错误")
    }
    return res
}

//运算符优先级
//定义*和/优先级为1；+和-优先级为0
func (this *stack)Priority(oper int)int{
    res := 0
    if oper == 42 || oper == 47{
        res = 1
    }else if oper == 43 || oper == 45{
        res = 0
    }
    return res
}

func main(){
    munStack := &stack{
        Max : 20,
        Top : -1;
    }
    operStack := &stack{
        Max : 20,
        Top : -1,
    }


    exp := "3+2*6-2"
    index := 0


    //处理多位数
    keep := ""

    for {
        ch := exp[index:index + 1]
        //此处类型是[]uint8
        temp := int([]byte(ch)[0])
        //或者temp := int(exp[index])
        //或者temp := int(ch[0])
        
        if operStack.isOper(temp){
            //说明是运算符

            //******************************************************
            if operStack.Top == -1{
                soperStack.Push(temp)
            }else{
                //判断运算符栈非空
                //判断运算符优先级
                //如果栈顶部的运算符优先级更高或相等
                //取出数栈头两个数
                //取出符号栈的运算符号
                //将运算得到的值重新压入数栈
                //再将新运算符压入符号栈
                if operStack.Priority(operStack.Arr[operStack.Top]) >= operStack.Priority(temp){
                    num1 := numStack.Pop()
                    num2 := numStack.Pop()
                    Oper := operStack.Pop()
                    resp := operStack.Cal(num1,num2,Oper)

                    numStack.Push(resp)

                    //将新的运算符压入符号栈
                    operStack.Push(temp)
                }
            }
            //******************************************************

        }else{
            //说明是数
            //此时temp是数字对应的ASCII码
            //要转换成数字

            //处理多位数
            //------------------------------------------------------
            if (/*下一位对应的ASCII码是数子则进行string拼接*/){

            }else{
                val,_ := int(strconv.ParseInt(temp,10,64))
                numStack.Push(val)
            }
            //------------------------------------------------------
        }

        //继续扫描
        if index + 1 == len(exp){
            break
        }
        index ++
    }

    //数栈和运算符栈压入完毕
    //开始从右向左计算
    for {

        if operStack.Top == -1{
            //运算结束时运算符栈应当为空
            //数字栈应当含有最终结果
            break
        }

        num1 = numStack.Pop()
        num2 = numStack.Pop()
        Oper = operStack.Pop()
        resp = operStack.Cal(num1,num2,Oper)
        //将计算的结果重新入栈
        numStack.Push(resp)
    }


    //输出
    RESP := numStack.Pop()
    fmt.Println("最终结果是：",RESP)
}
```
### 递归
```go
func test(n int){
    if n > 2{
        n --
        test(n)
    }
    fmt.Println("n is ",n)
}

func main(){
    test(10)
}
```
#### 迷宫回溯
```go

//核心思想
//如果从[i][j]点出发不能到达目的地
//就将该地设为3
//否则设为2
//并且是策略强相关的
func Setway(mymap *[8][7]int,i int,j int)bool{

    if mymap[6][5] == 2{
        return true
    }else{
        if mymap[i][j] == 0{
            //假设可以通
            mymap[i][j] = 2
            //策略
            //先上下
            //后左右
            if Setway(mymap,i-1,j){
                return true
            }else if Setway(mymap,i+1,j){
                return true
            }else if Setway(mymap,i,j+1){
                return true
            }else if Setway(mymap,i,j - 1){
                return true
            }else{
                //死路
                mymap[i][j] = 3
                return false
            }
        }else {
            return false
        }
    }
}


// 
func main(){
    //如果元素的值为1表示墙
    //如果元素的值为0表示没有走过
    //如果元素的值为2表示走过
    //如果元素的值为3表示曾经走过但走不通
    var mymap [8][7]int

    //设置周围墙
    for i:=0;i<7;i++{
        mymap[0][i] = 1
        mymap[7][i] = 1
        mymap[i][0] = 1
        mymap[i][6] = 1
    }
    mymap[7][4] = 1
    mymap[6][4] = 1
    mymap[5][4] = 1

    Bool := Setway(&mymap,1,1)
    fmt.Println(Bool)
    for _,v := range mymap{
        fmt.Println(v)
    }
}
```
```go
[1 1 1 1 1 1 1]
[1 2 2 2 2 2 1]
[1 2 2 2 2 2 1]
[1 1 1 2 2 2 1]
[1 0 0 2 2 2 1]
[1 0 0 2 2 2 1]
[1 0 0 2 2 2 1]
[1 1 1 1 1 1 1]
```
### 哈希
哈希表crud操作
1.不适用数据库
2.快
3.保证雇员id从高到底
4.取模的用处
5.列表的协程性
```go
type Emp struct{
    Id int
    Name string
    Next *Emp
}

//不带表头，第一个节点就存放雇员
type EmpLink struct{
    Head *Emp
}

//在散列的链表中添加方法
func (this *EmpLink)Insert(emp *Emp){
    temp := this.Head
    for {
        if temp == nil{
            temp = emp
            this.Head = temp
            break
        }else if temp.Next != nil{
            if temp.Next.Id > emp.Id{
                emp.Next = temp.Next
                temp.Next = emp
                break
            } 
        }else if temp.Next == nil{
            temp.Next = emp
            break
        }else{
            temp = temp.Next
        }
    }
}

//hashtable是链表数组
type Hashtable struct{
    LinkArr [7]EmpLink
}

//给hashtable编写insert雇员的方法
func (this *Hashtable)Insert(emp *Emp){
    //使用散列函数(即格局雇员的id或者其他形成散列映射分开放)
    linkNo := this.HashFun(emp.Id)
    this.LinkArr[linkNo].Insert(emp *Emp)
}
//编写一个散列方法
func (this *Hashtable)HashFun(id int)int{
    return id % 7
}
```