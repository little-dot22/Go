# go
## 1 基础
### 1.0 快捷键
    pkgm：main包 + main主函数
    fp：fmt.Println("")
    ff：fmt.Printf("", var)
    for：for i := 0; i < count; i++ {}
    forr：for _, v := range v {}
    fmain：func main() {}
### 1.1 命名规范
- 对外暴露的名字以大写字母开头
- package名称和目录一致，包名应为小写单词，无下划线
- 文件名称小写，下划线分割
- 结构体、变量驼峰命名法
- 常量均需大写，下划线分割
### 1.2 变量定义
    var name string
    var age int
    var (
        name string
        age int
    )

    var name string = "Tom"
    var age int = 18
在函数内部，可以使用短变量声明：

    name := "Tom"
    age := 18
匿名变量：

    return "Tom"

    func getNameAndAge() (string,int) {
        return "Tom", 20
    }
    func main() {
        _, age := getNameAndAge() //假如name接收过来后不使用，可以用_接收
    }
变量交换：

    a,b = b,a
### 1.3 常量
    const constName type = value    // type可以省略
    const PI float64 = 3.14
iota:

    const (
        a1 = iota
        _           // 跳过其他赋值也是跳过
        a2 = iota
    )
    fmt.Printf("name: %v\n", a1)    // 0
	fmt.Printf("name: %v", a2)      // 1
### 1.4 布尔类型
### 1.5 数字类型
- int和uint在64位OS上均64bits，32则32
- 无float，double，只有float32和float64
- 还可以使用int8，int16，int32，int64，uint和float同理

    unsafe.Sizeof(var)
    math.MaxInt8
- 输出：%b %o %d %x %f %T %p，二，八，十，十六，浮点数，类型，指针（内存）
### 1.6 字符串
	str := "world"
    str[1] = 'a'        // ERROR: 不能修改
	len(str)            // 5
多行用``

    s := 
        `               // 键盘左上角1旁边那个按键
    line1
    line2
    `
字符串连接：
- 直接相加
- strings.Join():

        name := "Tom"
        age := "18"
        s := strings.Join([]string{name, age}, "-")
        fmt.Printf("s: %v\n", s)
- buffer.WriteString():

        var buffer bytes.Buffer
        buffer.WriteString("Tom")
        buffer.WriteString(",")
        buffer.WriteString("18")
        fmt.Printf("buffer.String(): %v\n", buffer.String())
切片：

    str[m:n]    // 左闭右开
### 1.7 数据类型转换
Go语言数据类型转换都是显式的
    a := 5.2
    b := int(a)
### 1.8 位运算符
按二进制位运算：& | ^(异或) &^(位清空) << >>
## 2 流程控制
### 2.1 if语句
    if condition {

    } else {

    }
### 2.2 switch语句
选中case后只执行对应case的语句

	switch a {
	case 90:
		fmt.Println("A")
	case 80:
		fmt.Println("B")
    default:
        ...
	}

    switch {        // 什么都不传，默认布尔值true
    case true:
        ...
    case false:
        ...
    default:
        ...
    }
### 2.3 fallthrough接着往下执行一个case
	switch a {
	case 90:
		fmt.Println("A")
        fallthrough     // 如果a是90，执行完该case的语句后会接着往下执行一个
	case 80:
		fmt.Println("B")
    default:
        ...
	}
break中止
### 2.4 for循环
    str := "world"
	for i := 0; i < len(str); i++ {
		fmt.Println(str[i])         // world
	}

    str := "world"
	for i, v := range str {
		fmt.Printf("v: %c\n", i)    // 01234
		fmt.Printf("v: %c\n", v)    // world
	}
### 2.5 break continue
## 3 函数
### 3.1 定义
    func function_name([parameter list]) [return_types] {
        函数体
    }

    func add(a int, b int) int {
	    c := a + b
	    return c
    }
### 3.2 可变参数
    func getSum(nums ...int) int {          // 传入了一个不定长的数组nums
        sum := 0
        for i := 0; i < len(nums); i++ {
            sum += nums[i]
        }
        return sum
    }
注意：
- 有可变参数和其他参数时，把可变参数放在最后
- 可变参数只能有一个
### 3.3 值传递和引用传递
#### 3.3.1 数组的定义
    [个数]类型{数组体}
    var nums = [5]int{1, 2, 3, 4, 5}
    var nums = [...]int{1, 2, 3, 4, 5}      // 自动判断数组长度
#### 3.3.2 切片slice
定义：

    s := []int{1,2,3}
初始化：

    arr := [5]int{1, 2, 3, 4, 5}
    s := arr[1:3]       // 左闭右开
添加：

    s := []int{1,2,3}
    s = append(s,1)
删除：

	s := []int{0, 1, 2, 3, 4, 5}
	s = append(s[:2], s[3:]...)     // 0 1 3 4 5，其中...表示把 s[3:] 切片中的所有元素展开为一个个独立的元素，这样才能与 s[:2] 切片中的元素合并。如果省略 ... 操作符，则会将 s[3:] 切片作为一个整体添加到 s[:2] 切片中，这样就无法实现删除操作。
#### 3.3.3 映射map
定义：

    var map_variable map[key_data_type]value_data_type        //定义
    var map_variable = make(map[key_data_type]value_data_type)  // 初始化
    // 或者：
    var mp = map[string]int{"s1": 1, "s2": 2}
判断key是否存在：

    var mp = map[string]int{"s1": 1}
    v, ok := mp["s3"]
	fmt.Printf("v: %v\n", v)
	fmt.Printf("ok: %v\n", ok)  // true or false
遍历：

    var mp = map[string]int{"s1": 1, "s2": 2}
	for k, v := range mp {
		fmt.Printf("k: %v\n", k)
		fmt.Printf("v: %v\n", v)
	}
#### 3.3.2 值传递
- 基础数据类型
- 数组: var nums = [5]int{1, 2, 3, 4, 5}
- 结构
#### 3.3.3 引用传递
- 切片：var s1 = []int{1,2,3,4,5}
- **注意**：其实go语言只有值传递，当然传入地址的值就可以通过地址操作变量本身，而对于slice，map等而言，在其定义时，本质上就是*slice和*map，因此传入值是可以达到“引用传递”的效果
### 3.4 defer关键字
放最后执行，并且先进后出

    func main() {
        f("1")
        f("2")
        defer f("3")
        defer f("4")
        f("5")
    }                   // 1 2 5 4 3

    func f(s string) {
        fmt.Printf("s: %v\n", s)
    }
注意defer只决定执行顺序，下面函数里结果是10而非11：

    func main() {
        a := 10
        fmt.Printf("a: %v\n", a) // 10
        defer f(a)
        a++
        fmt.Printf("end a: %v\n", a) // 11
    }

    func f(a int) {
        fmt.Printf("函数里的a: %v\n", a) // 10
    }
### 3.5 闭包
闭包就是一个函数和与其相关的引用环境组合成的一个整体

    func test() func(a int) int {
        x := 10
        return func(a int) int {
            x += a
            return x
        }
    }

    func main() {
        f := test()
        fmt.Printf("f(1): %v\n", f(1))  // 11
        fmt.Printf("f(2): %v\n", f(2))  // 13
        fmt.Printf("f(3): %v\n", f(3))  // 16
    }
    以下代码构成了闭包：
    x := 10
    return func(a int) int {
        x += a
        return x
    }
### 3.6 init函数
    func init() {
        
    }
先于main函数执行，无参数和返回值，自动执行，不需要调用，每个包可以有多个init函数
## 4 结构体
### 4.1 结构体的定义
    type Person struct {
        name  string
        id    int
        age   int
        email string
    }

    func main() {
        var tom Person
        tom.age = 18
        tom.name = "Tom"
        tom.email = "xxx"
        tom.id = 101
        fmt.Printf("tom: %v\n", tom)
    }