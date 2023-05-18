# Go语言
## 1 基础
### 1.0 快捷键
    pkgm：main包 + main主函数
    fp：fmt.Println("")
    ff：fmt.Printf("", var)
    for：for i := 0; i < count; i++ {}
    forr：for _, v := range v {}
    fmain：func main() {}
    tyi：type name interface {}
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
>在函数内部，可以使用短变量声明：

    name := "Tom"
    age := 18
>匿名变量：

    return "Tom"

    func getNameAndAge() (string,int) {
        return "Tom", 20
    }
    func main() {
        _, age := getNameAndAge() //假如name接收过来后不使用，可以用_接收
    }
>变量交换：

    a,b = b,a
### 1.3 常量
    const constName type = value    // type可以省略
    const PI float64 = 3.14
>iota:

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
#### 1.6.1 基础
	str := "world"
    str[1] = 'a'        // ERROR: 不能修改
	len(str)            // 5
>多行用``

    s := 
        `               // 键盘左上角1旁边那个按键
    line1
    line2
    `
#### 1.6.2 字符串连接

>直接相加

>strings.Join():

        name := "Tom"
        age := "18"
        s := strings.Join([]string{name, age}, "-")
        fmt.Printf("s: %v\n", s)
>buffer.WriteString():

        var buffer bytes.Buffer
        buffer.WriteString("Tom")
        buffer.WriteString(",")
        buffer.WriteString("18")
        fmt.Printf("buffer.String(): %v\n", buffer.String())
>切片：

    str[m:n]    // 左闭右开
### 1.7 数据类型转换
>Go语言数据类型转换都是显式的

    a := 5.2
    b := int(a)
### 1.8 位运算符
>按二进制位运算：& | ^(异或) &^(位清空) << >>
## 2 流程控制
### 2.1 if语句
    if condition {

    } else {

    }
### 2.2 switch语句
>选中case后只执行对应case的语句

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
### 2.3 fallthrough
>接着往下执行一个case

	switch a {
	case 90:
		fmt.Println("A")
        fallthrough     // 如果a是90，执行完该case的语句后会接着往下执行一个
	case 80:
		fmt.Println("B")
    default:
        ...
	}
>break中止
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
>注意：
>- 有可变参数和其他参数时，把可变参数放在最后
>- 可变参数只能有一个
### 3.3 值传递和引用传递
#### 3.3.1 数组的定义
    [个数]类型{数组体}
    var nums = [5]int{1, 2, 3, 4, 5}
    var nums = [...]int{1, 2, 3, 4, 5}      // 自动判断数组长度
#### 3.3.2 切片slice
>定义：

    s := []int{1,2,3}
>初始化：

    arr := [5]int{1, 2, 3, 4, 5}
    s := arr[1:3]       // 左闭右开
>添加：

    s := []int{1,2,3}
    s = append(s,1)
>删除：

	s := []int{0, 1, 2, 3, 4, 5}
	s = append(s[:2], s[3:]...)     // 0 1 3 4 5，其中...表示把 s[3:] 切片中的所有元素展开为一个个独立的元素，这样才能与 s[:2] 切片中的元素合并。如果省略 ... 操作符，则会将 s[3:] 切片作为一个整体添加到 s[:2] 切片中，这样就无法实现删除操作。
#### 3.3.3 映射map
>定义：

    var map_variable map[key_data_type]value_data_type        //定义
    var map_variable = make(map[key_data_type]value_data_type)  // 初始化
    // 或者：
    var mp = map[string]int{"s1": 1, "s2": 2}
>判断key是否存在：

    var mp = map[string]int{"s1": 1}
    v, ok := mp["s3"]
	fmt.Printf("v: %v\n", v)
	fmt.Printf("ok: %v\n", ok)  // true or false
>遍历：

    var mp = map[string]int{"s1": 1, "s2": 2}
	for k, v := range mp {
		fmt.Printf("k: %v\n", k)
		fmt.Printf("v: %v\n", v)
	}
#### 3.3.2 值传递
>基础数据类型

>数组: var nums = [5]int{1, 2, 3, 4, 5}

>结构
#### 3.3.3 引用传递
>切片：var s1 = []int{1,2,3,4,5}

>**注意**：其实go语言只有值传递，当然传入地址的值就可以通过地址操作变量本身，而对于slice，map等而言，在其定义时，本质上就是*slice和*map，因此传入值是可以达到“引用传递”的效果
### 3.4 defer关键字
>放最后执行，并且先进后出

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
>注意defer只决定执行顺序，下面函数里结果是10而非11：

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
>闭包就是一个函数和与其相关的引用环境组合成的一个整体

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
>先于main函数执行，无参数和返回值，自动执行，不需要调用，每个包可以有多个init函数

    func init() {
        
    }
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
### 4.2 结构体指针
#### 4.2.1 普通指针
	var str string = "tom"
	var p_str *string = &str
	fmt.Printf("str: %v\n", str)
	fmt.Printf("p_str: %p\n", p_str)
	fmt.Printf("*p_str: %v\n", *p_str)
#### 4.2.2 结构体指针
	var p_person *Person
	p_person = &tom
	fmt.Printf("tom: %v\n", tom)
	fmt.Printf("p_person: %p\n", p_person)
	fmt.Printf("*p_person: %v\n", *p_person)

    tom.name
    p_person.name   // 与C语言不同，结构体和结构体指针都通过.访问属性
### 4.3 方法
>Go语言没有面向对象的特性，也没有类和对象的概念。但是可以使用结构体来模拟这些特性。Go的方法，是一种特殊的函数，与struct绑定，struct是该方法的接受者。方法是接受者的函数。

    type mytype struct {}
    func (recv mytype) my_method(paras) return_type {}
>相比于其他函数，方法多了一个(recv mytype)字段

    type Person struct {
        name  string
        id    int
        age   int
        email string
    }

    func (per Person) eat() {
        fmt.Printf("%v is eating\n", per.name)
    }

    func (per Person) sleep() {
        fmt.Printf("%v is sleeping\n", per.name)
    }

    func main() {
        var tom Person
        tom.age = 18
        tom.name = "Tom"
        tom.email = "xxx"
        tom.id = 101

        tom.eat()
        tom.sleep()
        fmt.Printf("tom.name: %v\n", tom.name)
    }
注意：
- receive_type不一定是sturct，也可以是slice, map, channel, func等
### 4.4 方法接收者类型
>接收者类型是结构体时，方法内不改变结构体本身；是结构体指针时，改变

    type Person struct {
        name string
    }

    func (per Person) f1() {
        per.name = "Jerry"
    }

    func (per *Person) f2() {
        per.name = "Jerry"
    }

    func main() {
        var tom Person
        tom.name = "Tom"

        var danny Person
        danny.name = "Danny"

        tom.f1()
        fmt.Printf("tom.name: %v\n", tom.name)
        danny.f2()
        fmt.Printf("danny.name: %v\n", danny.name)
    }
### 4.5 接口（“多态”）
> 下面有computer和mobile两个结构体，有USB这一个接口，接口可以将不同类型的对象视为相同类型，并且都能执行read和write这两个方法，在函数do中传入参数是USB，不需要考虑具体的数据类型，这样传入computer和mobile时，都可以调用read和write方法

>其实是在实现其他语言的多态

    type USB interface {
        read()
        write()
    }

    type Computer struct {
        name string
    }

    type Mobile struct {
        name string
    }

    func (c Computer) read() {
        fmt.Printf("%v is reading.\n", c.name)
    }

    func (c Computer) write() {
        fmt.Printf("%v is writing.\n", c.name)
    }

    func (m Mobile) read() {
        fmt.Printf("%v is reading.\n", m.name)
    }

    func (m Mobile) write() {
        fmt.Printf("%v is writing.\n", m.name)
    }

    func do(u USB) {
        u.read()
        u.write()
    }

    func main() {
        c := Computer{
            name: "Lenovo",
        }

        m := Mobile{
            name: "OPPO",
        }
        do(c)
        do(m)
    }
### 4.6 结构体嵌套（“继承”）
>很好理解
### 4.7 构造函数
>同样地，Go语言不存在构造函数的概念，但是可以用函数模拟

    type Person struct {
        name string
        age  int
    }

    func NewPerson(name string, age int) (*Person, error) {
        if name == "" {
            return nil, fmt.Errorf("name不能为空")
        }
        if age < 0 {
            return nil, fmt.Errorf("age不能小于0")
        }
        return &Person{name: name, age: age}, nil
    }

    func main() {
        per, err := NewPerson("Tom", -5)
        if err == nil {
            fmt.Printf("per: %v\n", per)
        } else {
            fmt.Printf("err: %v\n", err)
        }
    }
## 5 golang包
>云里雾里
## 6 并发编程
### 6.1 协程
>协程可以理解为轻量级线程，一个线程可以拥有多个协程，与线程相比，协程不受操作系统调度，协程调度器按照调度策略把协程调度到线程中执行，协程调度器由应用程序的runtime包提供，用户使用go关键字即可创建协程.

    type Person struct {
        name string
        age  int
    }

    func show(msg string) {
        for i := 0; i < 5; i++ {
            fmt.Println(msg)
            time.Sleep(time.Millisecond * 100)
        }
    }

    func main() {
        go show("aaaa")
        go show("bbbb")
        time.Sleep(time.Millisecond * 3000)
        fmt.Println("main end .............")
    }
### 6.2 channel通信
>values <- value / <-values

    var values = make(chan int)

    func send() {
        rand.Seed(time.Now().UnixNano())
        value := rand.Intn(10)
        fmt.Printf("send: %v\n", value)
        time.Sleep(time.Second * 3)
        values <- value
    }

    func main() {
        defer close(values)
        go send()
        fmt.Println("waiting...........")
        value := <-values
        fmt.Printf("value: %v\n", value)
        fmt.Println("end.")
    }
### 6.3 WaitGroup实现同步
>调用wg.Wait()的goroutine会一直阻塞，直到WaitGroup的计数值变为0，wg.Add(x)给计数值加x，wg.Done()实质上是wg.Add(-1)即计数值减一。

    var wg sync.WaitGroup

    func show(x int) {
        defer wg.Done()
        fmt.Println(x)
    }

    func main() {
        for i := 0; i < 10; i++ {
            wg.Add(1)
            go show(i)
        }
        wg.Wait()
        fmt.Println("end.")
    }
### 6.4 runtime包
#### 6.4.1 runtime.Gosched()
>当前协程有权执行时，先让给其他协程去执行。

    func show(x int) {
        for i := 0; i < 2; i++ {
            fmt.Printf("x: %v\n", x)
        }
    }

    func main() {
        go show(10010)

        for i := 0; i < 2; i++ {
            runtime.Gosched()
            fmt.Println("main routine")
        }
    }
#### 6.4.2 runtime.Goexit()
>退出当前协程。

    func show() {
        for i := 0; i < 10; i++ {
            if i > 5 {
                runtime.Goexit()
            }
            fmt.Printf("i: %v\n", i)
        }
    }

    func main() {
        go show()

        runtime.Gosched()
        fmt.Println("main end")
    }
#### 6.4.3 runtime.GOMAXPROCS()
>runtime.GOMAXPROCS(1)没有注释掉时，a和b会交替执行，不注释时，仅使用一个CPU，不交替。

    func a() {
        for i := 0; i < 10; i++ {
            fmt.Printf("a: %v\n", i)
        }
    }

    func b() {
        for i := 0; i < 10; i++ {
            fmt.Printf("b: %v\n", i)
        }
    }

    func main() {
        fmt.Printf("runtime.NumCPU(): %v\n", runtime.NumCPU())

        // runtime.GOMAXPROCS(1)

        go a()
        go b()

        time.Sleep(time.Second * 2)
    }
#### 6.4.4 Mutex互斥锁
>有必要复习一下线程同步的知识：线程并发执行时，T1线程有指令I1，I2，T2线程有指令I3，I4，那么在实际执行时指令执行顺序可能是1234，1324，1342...即异步，同步则是让某条指令只能在某条指令之后执行，如I2必须在I4后执行，那么1234的顺序就是不可接受的。那么该如何实现同步呢？这里要再插入一下互斥资源的访问机制，再将其思想迁移过来。

>两个线程访问打印机这种互斥资源，必须在一个线程占有资源-打印数据-释放资源这一整套操作之后才能将资源给下一个线程使用，否则在并发执行过程中，两个线程各自打印自己的数据，打印出来的结果就混在一起了。互斥的实现分软件和硬件两大类。软件类下有4种，重点关注一下双标志先检查法，即在确认对方不想占有资源后，表示自己想要占有资源。当然这种方法在并发下存在问题，因为“确认对方不想占有”和“表示自己想要占有”两个指令不能保证紧接着执行而不被其他程序抢走时间片。

>那么解决方法就很明显了——原语，不执行完不能让出时间片，这就有了wait和signal（PV）信号量。P表示占有一个，V表示让出一个，信号量又分为整型信号量和记录型信号量，后者可以在资源不足时将线程阻塞，而不是一直询问（实现让权等待）。硬件实现就更简单，如关中断指令-开中断指令，在两条指令之间不接收时间片切换灯中断指令。

>回到进程同步的问题，把信号量的概念迁移过来，设置信号量S=0，I2必须在I4后执行，也就是必须让4“释放资源”后，2才可以“占有资源”，否则2就被阻塞，这样2就只能在4后执行。（顺口溜：前VP后）

>这一小节主要用到的就是类似关中断指令-开中断指令的语句。

    var wg sync.WaitGroup

    var x int = 100

    var lock sync.Mutex

    func a() {
        defer wg.Done()
        lock.Lock()
        x++
        lock.Unlock()
    }

    func b() {
        defer wg.Done()
        lock.Lock()
        x--
        lock.Unlock()
    }

    func main() {
        for i := 0; i < 100; i++ {
            wg.Add(1)
            go a()
            wg.Add(1)
            go b()
        }

        wg.Wait()
        fmt.Printf("end x: %v\n", x)
    }
>上面的代码里要注意x++不是原语，可能是先取数x，再加一，再赋值回去，这样假设协程并发执行，俩协程都取了x=100，都加一，再赋值回去，那么x是101而不是102，所以就用锁把它锁上，可以类比关中断和开中断。