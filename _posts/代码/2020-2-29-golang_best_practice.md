# Golang
## 环境 

## 语言基础

### 赋值

var a string // 默认为 nil ，其他类似都会有默认值

var a = "hello" //  自动推导为 string

a:= "hello"     //  自动推导为 string。 a 不能在之前声明过。只能在函数体里出现。

a := new(int) //这时候a是一个*int指针变量

### 切片

s = append(s,"abc")   要注意，返回值才是 append 值的 s

```go
fmt.Println("数组***********************************")
var arr1 [3]int = [3]int{1, 2, 3}
var arr2 [3]int = arr1  // 这里是复制数据
fmt.Println(arr1, arr2)
arr2[0] = 10002
fmt.Println(arr1, arr2)
//区别 1： 切片可 append，可变，
fmt.Println("切片***********************************")
var slice1 []int = []int{1, 2, 3}
var slice2 []int = slice1  //区别 2： 这里是地址给到 slice2 ，
fmt.Println(slice1, slice2)
slice2[0] = 10002
fmt.Println(slice1, slice2)
```

### strings

```go
package main

import (
    "fmt"
    s "strings"
)

var p = fmt.Println

func main() {

    p("Contains:  ", s.Contains("test", "es"))
    p("Count:     ", s.Count("test", "t"))
    p("HasPrefix: ", s.HasPrefix("test", "te"))
    p("HasSuffix: ", s.HasSuffix("test", "st"))
    p("Index:     ", s.Index("test", "e"))
    p("Join:      ", s.Join([]string{"a", "b"}, "-"))
    p("Repeat:    ", s.Repeat("a", 5))
    p("Replace:   ", s.Replace("foo", "o", "0", -1))
    p("Replace:   ", s.Replace("foo", "o", "0", 1))
    p("Split:     ", s.Split("a-b-c-d-e", "-"))
    p("ToLower:   ", s.ToLower("TEST"))
    p("ToUpper:   ", s.ToUpper("test"))
    p()

    p("Len: ", len("hello"))
    p("Char:", "hello"[1])
}
```



### 枚举

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

```go
package main

import "fmt"

func main() {
	const (
		a = iota //0
		b        //1
		c        //2
		d = "ha" //独立值，iota += 1
		e        //"ha"   iota += 1
		f = 100  //iota +=1
		g        //100  iota +=1
		g2
		h = iota //7,恢复计数
		i        //8
	)
	fmt.Println(a, b, c, d, e, f, g, g2, h, i)
}


out-----------------
0 1 2 ha ha 100 100 100 8 9
```





```go
package main

import "fmt"
const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}

out ------------------
i= 1
j= 6
k= 12
l= 24
```

### BitSet
原生不支持
https://segmentfault.com/a/1190000019937023

``` bash
go get "github.com/yourbasic/bit"
```


### Set

golang 原生不支持 set

https://segmentfault.com/a/1190000019937023

可以用map 实现  github.com/deckarep/golang-set
自己写也简单

```go
package main

import "fmt"

func main() {
  type void struct{}  // 使 map 的 value 不占空间， unsafe.Size(struct{}{}) == 0

	var member void
	set := make(map[string]void)
	set["Foo"] = member
	for k := range set {
		fmt.Println(k)
	}
	size := len(set)
	fmt.Println("set size", size)
	delete(set, "Foo")

}
```



### select

随机选选一个 chan，如果有值，则运行， 如果没有，执行 default，如果没 default，阻塞。直到某个 chan 有值。

```go
// Package main provides ...
package main

import "fmt"

func main() {
	chan1 := make(chan string)

	for {
		select {
		case v1 := <-chan1:
			fmt.Println(v1)
		default:
			fmt.Println("default")
		}
	}

}

```




### 函数

要 export 的就大写，否则小写



#### 默认返回

即使不写 return r, r 也会默认返回

```go
func foo(age int, name string) (r string) {
    r = fmt.Sprintf("myname is %s , age %d", name, age)
    return 
}
```



#### 可变参

```go
package main

import "fmt"

func sum(nums ...int) {
    fmt.Print(nums, " ")
    total := 0
    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
    fmt.Println(nums[0])


}

func main() {

    sum(1, 2)
    sum(1, 2, 3)

    nums := []int{1, 2, 3, 4}
    sum(nums...)
}
```



### struct 
```
struct定义的三种形式：
a: 　　var stu Student
b:　　var stu *Student = new(Student)
c:　　var stu *Student = &Student{}
(1)其中b和c返回的都是指向结构体的指针，访问形式如下
stu.Name、stu.Age和stu.Score 或者(*stu).Name、(*stu).Age、(*stu).Scroe
```





#### 继承

```go
type Car struct {
    weight int
    name   string
}

func (p *Car) Run() {
    fmt.Println("running")
}

type Bike struct {
    Car
    lunzi int
}
type Train struct {
    Car
}

```


### 接口

go 里的接口不用实现， 只要函数名与参数名一样即可自动调用

```go
package main

type MyInterface interface {
	Print()
}

func TestFunc(x MyInterface) {
	x.Print()
}

type MyStruct struct {
}

func (me MyStruct) Print() {
	println("hello,world")

}

func main() {
	var me MyStruct
	TestFunc(me)
p}

```



#### 泛型

go 并不支持泛型，但可以通过 interface 实现

```go
package main
    type Interface interface {
        Len() int
        Less(i, j int) bool
        Swap(i, j int)
    }

    
    func Sort(data Interface) {

        n := data.Len()
        maxDepth := 0
        for i := n; i > 0; i++ {
            maxDepth++
        }
        maxDepth *= 2
        quickSort(data, 0, n, maxDepth)
    }
    

```



另一个例子

```go
// Package main provides ...
package main

import (
	"fmt"
	"sort"
)

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%s: %d", p.Name, p.Age)
}

// ByAge implements sort.Interface for []Person based on
// the Age field.
type ByAge []Person //自定义

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

func main() {
	people := []Person{
		{"Bob", 31},
		{"John", 42},
		{"Michael", 17},
		{"Jenny", 26},
	}

	fmt.Println(people)
	sort.Sort(ByAge(people))
	fmt.Println(people)
}

```



### 反射

reflect提供了两种类型来进行访问接口变量的内容：

类型	                作用
reflect.ValueOf()	   获取输入参数接口中的数据的值，如果为空则返回0  注意是0
reflect.TypeOf()	    动态获取输入参数接口中的值的类型，如果为空则返回nil  注意是nil

#### 小例子 

``` go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var name string = "咖啡色的羊驼"

    // TypeOf会返回目标数据的类型，比如int/float/struct/指针等
    reflectType := reflect.TypeOf(name)

    // valueOf返回目标数据的的值，比如上文的"咖啡色的羊驼"
    reflectValue := reflect.ValueOf(name)

    fmt.Println("type: ", reflectType)
    fmt.Println("value: ", reflectValue)
} 

out -------------
type:  string
value:  咖啡色的羊驼
```



1.使用反射时需要先确定要操作的值是否是期望的类型，是否是可以进行“赋值”操作的，否则reflect包将会毫不留情的产生一个panic。

2.反射主要与Golang的interface类型相关，只有interface类型才有反射一说。如果有兴趣可以看一下TypeOf和ValueOf，会发现其实传入参数的时候已经被转为接口类型了。

```go
// 以下为截取的源代码
func TypeOf(i interface{}) Type {
    eface := *(*emptyInterface)(unsafe.Pointer(&i))
    return toType(eface.typ)
}

func ValueOf(i interface{}) Value {
    if i == nil {
        return Value{}
    }
    escapes(i)

    return unpackEface(i)
}
```



#### struct 的反射

```go
package main

import (
    "fmt"
    "reflect"
)

type Student struct {
    Id   int
    Name string
}

func (s Student) Hello(){
    fmt.Println("我是一个学生")
}

func main() {
    s := Student{Id: 1, Name: "咖啡色的羊驼"}

    // 获取目标对象
    t := reflect.TypeOf(s)
    // .Name()可以获取去这个类型的名称
    fmt.Println("这个类型的名称是:", t.Name())

    // 获取目标对象的值类型
    v := reflect.ValueOf(s)
    // .NumField()来获取其包含的字段的总数
    for i := 0; i < t.NumField(); i++ {
        // 从0开始获取Student所包含的key
        key := t.Field(i)

        // 通过interface方法来获取key所对应的值
        value := v.Field(i).Interface()

        fmt.Printf("第%d个字段是：%s:%v = %v \n", i+1, key.Name, key.Type, value)
    }

    // 通过.NumMethod()来获取Student里头的方法
    for i:=0;i<t.NumMethod(); i++ {
        m := t.Method(i)
        fmt.Printf("第%d个方法是：%s:%v\n", i+1, m.Name, m.Type)
    }
} 

out --------------
这个类型的名称是: Student
第1个字段是：Id:int = 1 
第2个字段是：Name:string = 咖啡色的羊驼 
第1个方法是：Hello:func(main.Student)
```

#### 匿名或嵌入字段的反射

```go
package main

import (
    "reflect"
    "fmt"
)

type Student struct {
    Id   int
    Name string
}

type People struct {
    Student // 匿名字段
}

func main() {
    p := People{Student{Id: 1, Name: "咖啡色的羊驼"}}

    t := reflect.TypeOf(p)
    // 这里需要加一个#号，可以把struct的详情都给打印出来
    // 会发现有Anonymous:true，说明是匿名字段
    fmt.Printf("%#v\n", t.Field(0))

    // 取出这个学生的名字的详情打印出来
    fmt.Printf("%#v\n", t.FieldByIndex([]int{0, 1}))

    // 获取匿名字段的值的详情
    v := reflect.ValueOf(p)
    fmt.Printf("%#v\n", v.Field(0))
}

out-------------------
reflect.StructField{Name:"Student", PkgPath:"", Type:(*reflect.rtype)(0x10aade0), Tag:"", Offset:0x0, Index:[]int{0}, Anonymous:true}

reflect.StructField{Name:"Name", PkgPath:"", Type:(*reflect.rtype)(0x109f4e0), Tag:"", Offset:0x8, Index:[]int{1}, Anonymous:false}

main.Student{Id:1, Name:"咖啡色的羊驼"} 
```



#### 判断传入的类型是否是我们想要的类型

```go
package main

import (
    "reflect"
    "fmt"
)

type Student struct {
    Id   int
    Name string
}

func main() {
    s := Student{Id: 1, Name: "咖啡色的羊驼"}
    t := reflect.TypeOf(s)

    // 通过.Kind()来判断对比的值是否是struct类型
    if k := t.Kind(); k == reflect.Struct {
        fmt.Println("bingo")
    }

    num := 1;
    numType := reflect.TypeOf(num)
    if k := numType.Kind(); k == reflect.Int {
        fmt.Println("bingo")
    }
}

out:
bingo
bingo

```



#### 通过反射修改内容

```go
package main

import (
    "reflect"
    "fmt"
)

type Student struct {
    Id   int
    Name string
}

func main() {
    s := &Student{Id: 1, Name: "咖啡色的羊驼"}

    v := reflect.ValueOf(s)

    // 修改值必须是指针类型否则不可行
    if v.Kind() != reflect.Ptr {
        fmt.Println("不是指针类型，没法进行修改操作")
        return
    }

    // 获取指针所指向的元素
    v = v.Elem()

    // 获取目标key的Value的封装
    name := v.FieldByName("Name")

    if name.Kind() == reflect.String {
        name.SetString("小学生")
    }

    fmt.Printf("%#v \n", *s)


    // 如果是整型的话
    test := 888
    testV := reflect.ValueOf(&test)
    testV.Elem().SetInt(666)
    fmt.Println(test)
}

out --------------------
main.Student{Id:1, Name:"小学生"} 
666
```



#### 通过反射调用方法

```go
package main

import (
    "fmt"
    "reflect"
)

type Student struct {
    Id   int
    Name string
}

func (s Student) EchoName(name string){
    fmt.Println("我的名字是：", name)
}

func main() {
    s := Student{Id: 1, Name: "咖啡色的羊驼"}

    v := reflect.ValueOf(s)

    // 获取方法控制权
    // 官方解释：返回v的名为name的方法的已绑定（到v的持有值的）状态的函数形式的Value封装
    mv := v.MethodByName("EchoName")
    // 拼凑参数
    args := []reflect.Value{reflect.ValueOf("咖啡色的羊驼")}

    // 调用函数
    mv.Call(args)
}

out -------------------
我的名字是： 咖啡色的羊驼
```



### 协程



用关键 `go`创建协程`goroutine`

在`go`关键字后指定函数，将会开启一个协程运行该函数。

```go
package main

import (
    "fmt"
    "time"
)

func foo() {
    for i := 0; i < 5; i++ {
        fmt.Println("loop in foo:", i)
        time.Sleep(1 * time.Second)
    }
}

func main() {
    go foo()

    for i := 0; i < 5; i++ {
        fmt.Println("loop in main:", i)
        time.Sleep(1 * time.Second)
    }
    time.Sleep(6 * time.Second)
}
```



###  channel 

https://www.runoob.com/go/go-concurrent.html

ch := make(chan int) 

默认不带缓冲区，`ch <- 1` 发送后就阻塞，除非有人接收。（将 1 扔进 ch），一般这个只会写在协程里，而不是 main 里，不然 main 就阻塞了。

ch := make(chan int,2) 带 2 个值的缓冲， 除非缓冲已满， 不然不会阻塞

```go
package main

import "fmt"

func main() {
    // 这里我们定义了一个可以存储整数类型的带缓冲通道
        // 缓冲区大小为2
        ch := make(chan int, 2)

        // 因为 ch 是带缓冲的通道，我们可以同时发送两个数据
        // 而不用立刻需要去同步读取数据
        ch <- 1
        ch <- 2

        // 获取这两个数据
        fmt.Println(<-ch)
        fmt.Println(<-ch)
} 
```



在 for 循环里使用

```go
package main

import (
	"fmt"
)

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	// range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个
	// 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据
	// 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不
	// 会结束，从而在接收第 11 个数据的时候就阻塞了。
	for i := range c {
			fmt.Println(i)
	}
}

```



channel  可以有两个返回值， more 代表 channel 已关，且所有缓冲的值已经读完

```go
package main

import "fmt"

func main() {
    jobs := make(chan int, 5)
    done := make(chan bool)

    go func() {
        for {
            j, more := <-jobs
            if more {
                fmt.Println("received job", j)
            } else {
                fmt.Println("received all jobs")
                done <- true
                return
            }
        }
    }()

    for j := 1; j <= 3; j++ {
        jobs <- j
        fmt.Println("sent job", j)
    }
    close(jobs)
    fmt.Println("sent all jobs")

    <-done
}

 

out -------------
sent job 1
received job 1
sent job 2
received job 2
sent job 3
received job 3
sent all jobs
received all jobs
```

#### close

1. 使用内置函数close进行关闭，chan关闭之后，for range遍历chan中已经存在的元素后结束
2. 使用内置函数close进行关闭，chan关闭之后，没有使用for range的写法需要使用，`v, ok := <-` ch进行判断chan是否关



```go
package main

import (
    "fmt"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "started  job", j)
        time.Sleep(time.Second)
        fmt.Println("worker", id, "finished job", j)
        results <- j * 2
    }
}

func main() {

    const numJobs = 5
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= numJobs; a++ {
        <-results
    }
}
```



### 单例

http://marcio.io/2015/07/singleton-pattern-in-go/

用这个版本，最完美

```go
// Package main provides ...
package main

import (
	"sync"
)

type singleton struct{}

func (m *singleton) sayHello(msg string) {
	println("hello " + msg)

}

var instance *singleton
var once sync.Once

func GetInstance() *singleton {
	once.Do(func() {
		instance = &singleton{}
	})
	return instance
}
func main() {
	g := GetInstance()
	g.sayHello("zk")
}

```



另一个线程不安全的版本，学习用，注意 if 判断非线程安全

```go
func GetInstance() *singleton {
    if instance == nil {     // <-- Not yet perfect. since it's not fully atomic
        mu.Lock()
        defer mu.Unlock()

        if instance == nil {
            instance = &singleton{}
        }
    }
    return instance
}
```



### 可见性

https://stackoverflow.com/questions/18491032/does-go-support-volatile-non-volatile-variables

在 java 里有 volatile 保证可见性， golang 呢？

golang 里一样有可性问题，但 golang 没有 volatile 关键字。

使用锁，或者 atomic 包解决。

## 运行

main 方法必须要在 package main 里。不然不会执行。

```go
package main  //必须要写

import "fmt"

func main() {
	test()
}

func test()  {
	defer func() {
		if e := recover(); e != nil { // recover 会接收 panic 传过来的值
			fmt.Println(e,"Wrong!")     // 打印 panic wrong
		}
	}()
	panic("panic")
}
```







## 包/库管理



### 下载

go get 感觉上被墙了。

go get 内部调用的就是 git hg, svn 命令。

如果是引用的 github ，肯定就是 git 命令。

直接 

```bash
export HTTP_PROXY=http://127.0.0.1:19090  HTTPS_PROXY=http://127.0.0.1:19090
```

再执行的话，就会走代理。

取消代理

```bash
unset HTTP_PROXY
unset HTTPS_PROXY
```

如果还不行，试试

git config –global https.proxy [http://127.0.0.1:8787](http://127.0.0.1:8787/)
git config –global https.proxy [https://127.0.0.1:8787](https://127.0.0.1:8787/)
如果要取消代理设置，则执行

git config –global –unset http.proxy
git config –global –unset https.proxy
查看已经设置的值：git config http.proxy



### 查找位置

GO 首先有 \$GOROOT/src 下找包（一般都是 golang 带的系统包），如果没有，则去 \$GOPATH/src 下找，也就是你自己下载的包



### 制作


https://golang.org/doc/code.html
- 文件名/文件夹名 与包名无关。 只要声明了 package xxx， 哪在 xxx 包下，的所有*文件的大写内容*都可以这样引用 xxx.bbb。
	
- 同一个包的文件应该放在同一个文件夹里, main 包也一样，最好的方式就是在项目下只有 main 一个文件，然后其他的以文件夹的形式以包存在，
- 如果 main 包与其他包共存，测试会出问题，但执行没有问题，

- 包有两种，main包，其他包。
  - main 包里有 main 方法，一个 golang 项目只能有一个 main 入口。但可以有多个包含 package main 的文件！ 打包后会生成一个可执行文件。

  - 其他包，打包后生成链接文件



同一个包作用域下，变量不可以重复。连私有的也不能重复。

举例

$GOPATH/src/mypackage/hello.go

```go
package mypackage 
const HELLO =  "hello,world" // 如果要导出，变量一定要开头大写
```



\$GOPATH/src/caller/main.go

```go
package main
import "mypackage"
func main(){
  fmt.Println(HELLO)
}
```



*bin 的名字与main 的包名一致*

```bash
>go install 
>caller
>"hello,world"
```



#### 嵌套包

形如 import "xxx/xxx" 这种形式

![图片 7](.././assets/2cc534cf-9eb0-42fa-9639-e4ad06a1a10d.png)

![图片 8](.././assets/15460e3c-da83-4025-bd5c-62b3e55f4479.png)

### 引用



导入包指定的是包的路径，包名默认为路径中的最后部分

```
import "net/url" //导入url包
```

如果用一个名字导入多个包，有时可能重名，可以使用别名

```go
import "greet/greet"
import "greet"  // 出错
```



为导入的包设置别名, 直接在导入包时，直接在报名前面添加别名，用空格隔开

```
import "greet/greet"

```

 

- _  引用, *仅*执行 init 函数, 而其他的函数的调用就会出错.（注意 init函数只要是引入都会执行）

  还有一个用途是，当你不使用这个包时，你可以临时用_， 当注解掉。。。但是还是会执行 init。总之不会报错了

  main.go

  ``` go
  package main
  
  import _ "./hello"
  
  func main() {
      hello.Print() 
      //编译报错：./main.go:6:5: undefined: hello
  }
  ```

  hello.go

  ``` go
  package hello
  
  import "fmt"
  
  func init() {
      fmt.Println("imp-init() come here.")
  }
  
  func Print() {
      fmt.Println("Hello!")
  }
  ```

  

  - . 引用, 不推荐。容易冲突。

  ```go
  package main
   
  import . "fmt"
   
  func main() {
  	Println("Hello World!")
  }
   
  // 输出结果：
  Hellow World!
  ```

  

- 别名 引用

  ```go
  package main
   
  import f "fmt"
   
  func main() {
  	f.Println("Hello World!")
  }
   
  //输出结果：
  Hello World!
  ```

  

### 引用某版本包

使用 go mod 管理，详见 [工程组织：Src 章节](#Src)





## 异常处理

Golang提供两种错误处理方式

1. 函数返回`error`类型对象判断错误
2. `panic`异常



`error`是一个内置的接口类型

```
type error interface {
    Error() string
}
```

### error  处理

通常，使用`error`异常处理类似这样：

```go
package main

import "fmt"

func foo(i int, j int) (r int, err error) {
    if j == 0 {
        err = fmt.Errorf("参数2不能为 %d", j) //给err变量赋值一个error对象
        return //返回r和err，因为定义了返回值变量名，所以不需要在这里写返回变量
    }

    return i / j, err //如果没有赋值error给err变量，err是nil
}

func main() {
    //传递add函数和两个数字，计算相加结果
    n, err := foo(100, 0)
    if err != nil { //判断返回的err变量是否为nil，如果不是，说明函数调用出错，打印错误内容
        println(err.Error())
    } else {
        println(n)
    }
}
```







异常处理的作用域（场景）：

1. 空指针引用

2. 下标越界

3. 除数为0

4. 不应该出现的分支，比如default

5. 输入不应该引起函数错误



#### 姿势一：失败的原因只有一个时，不使用error

```go
// 错误姿势
func (self *AgentContext) CheckHostType(host_type string) error {
    switch host_type {
    case "virtual_machine":
        return nil
    case "bare_metal":
        return nil
    }
    return errors.New("CheckHostType ERROR:" + host_type)
} 
// 正确姿势！

func (self *AgentContext) IsValidHostType(hostType string) bool {
    return hostType == "virtual_machine" || hostType == "bare_metal"
}
```

说明：大多数情况，导致失败的原因不止一种，尤其是对I/O操作而言，用户需要了解更多的错误信息，这时的返回值类型不再是简单的bool，而是error。

#### 姿势二：没有失败时，不使用error

```go
// 错误姿势
func (self *CniParam) setTenantId() error {
    self.TenantId = self.PodNs
    return nil
}

// 正确姿势
func (self *CniParam) setTenantId() {
    self.TenantId = self.PodNs
}
```

#### 姿势三：error应放在返回值类型列表的最后

```go
resp, err := http.Get(url)
if err != nil {
    return nill, err
}

// bool 也一样
value, ok := cache.Lookup(key) 
if !ok {
    // ...cache[key] does not exist… 
}
```

#### 姿势四：错误值统一定义，而不是跟着感觉走

定义好统一的错误文件

```go
var ERR_EOF = errors.New("EOF")
var ERR_CLOSED_PIPE = errors.New("io: read/write on closed pipe")
var ERR_NO_PROGRESS = errors.New("multiple Read calls return no data or error")
var ERR_SHORT_BUFFER = errors.New("short buffer")
var ERR_SHORT_WRITE = errors.New("short write")
var ERR_UNEXPECTED_EOF = errors.New("unexpected EOF")
 
```

#### 姿势五：错误逐层传递时，层层都加日志

根据笔者经验，层层都加日志非常方便故障定位。

说明：至于通过测试来发现故障，而不是日志，目前很多团队还很难做到。如果你或你的团队能做到，那么请忽略这个姿势:)

#### 姿势六：错误处理使用defer

```go
//错误姿势
func deferDemo() error {
    err := createResource1()
    if err != nil {
        return ERR_CREATE_RESOURCE1_FAILED
    }
    err = createResource2()
    if err != nil {
        destroyResource1()
        return ERR_CREATE_RESOURCE2_FAILED
    }
 
    err = createResource3()
    if err != nil {
        destroyResource1()
        destroyResource2()
        return ERR_CREATE_RESOURCE3_FAILED
    }
 
    err = createResource4()
    if err != nil {
        destroyResource1()
        destroyResource2()
        destroyResource3()
        return ERR_CREATE_RESOURCE4_FAILED
    }
    return nil
}
```



```go
// 正确姿势 
func deferDemo() error {
    err := createResource1()
    if err != nil {
        return ERR_CREATE_RESOURCE1_FAILED
    }
    defer func() {
        if err != nil {
            destroyResource1()
        }
    }()
    err = createResource2()
    if err != nil {
        return ERR_CREATE_RESOURCE2_FAILED
    }
    defer func() {
        if err != nil {
            destroyResource2()
        }
    }()
 
    err = createResource3()
    if err != nil {
        return ERR_CREATE_RESOURCE3_FAILED
    }
    defer func() {
        if err != nil {
            destroyResource3()
        }
    }()
 
    err = createResource4()
    if err != nil {
        return ERR_CREATE_RESOURCE4_FAILED
    }
    return nil
}
```

#### 姿势七：当尝试几次可以避免失败时，不要立即返回错误

#### 姿势八：当上层函数不关心错误时，建议不返回error

对于一些资源清理相关的函数（destroy/delete/clear），如果子函数出错，打印日志即可，而无需将错误进一步反馈到上层函数，因为一般情况下，上层函数是不关心执行结果的，或者即使关心也无能为力，于是我们建议将相关函数设计为不返回error。

#### 姿势九：当发生错误时，不忽略有用的返回值

通常，当函数返回non-nil的error时，其他的返回值是未定义的(undefined)，这些未定义的返回值应该被忽略。然而，有少部分函数在发生错误时，仍然会返回一些有用的返回值。比如，当读取文件发生错误时，Read函数会返回可以读取的字节数以及错误信息。对于这种情况，应该将读取到的字符串和错误信息一起打印出来。

#### panic

```go
package main

import (
	"fmt"
)

func sayName(name *string) {
	defer fmt.Println("3")
	if name == nil {
		panic("4")

	}
	fmt.Printf("%s \n", *name)
	fmt.Println("5")
}

func main() {
	defer fmt.Println("1")
	sayName(nil)
	fmt.Println("2")
}

```



out

```text
3
1
panic: 4

goroutine 1 [running]:
main.sayName(0x0)
	/Users/zk/git/goprj/src/test3/main.go:10 +0x1f3
main.main()
	/Users/zk/git/goprj/src/test3/main.go:19 +0xb1
exit status 2

zk@zk-3 : ~/git/goprj/src/test3
```



#### recover

让 panic 别慌，扔掉出错的函数调用栈，继续前进，这也代表着堆栈信息丢失了。可以通过  debug.PrintStack() 找回来

```go
package main

import (
	"fmt"
)

func sayName(name *string) {
	defer func() {
		if err := recover(); err != nil {

			fmt.Println("3")
      //debug.PrintStack()
		}
	}()
	if name == nil {
		panic("4")

	}
	fmt.Printf("%s \n", *name)
	fmt.Println("5")
}

func main() {
	defer fmt.Println("1")
	sayName(nil)
	fmt.Println("2")
}

```



out

```text
3 
2
1
```



## logger 技巧





## 工程组织

use go moudle.



### Src 

详见 https://xuanwo.io/2019/05/27/go-modules/

https://www.callicoder.com/golang-packages/

```bash
# 创建文件夹
mkdir rabbit
# 初始化包管理工具，并指定好 git 地址
go mod init github.com/zk4/rabbit
# build project will automatically import package 
go build 

go download    #download  modules to  local cache

edit go.mod from tools or scripts
go mod edit -require="github.com/chromedp/chromedp@v0.1.0"


go graph       #print module requirement graph
go init        #initialize new module in current directory
go tidy        #add missing and remove unused modules
go vendor      #make vendored copy of dependencies
# use go build -mod=vendor  to use vender	package only
 
go verify      #verify dependencies have expected content
go why         #explain why packages or modules are needed
```


if download failed because of GFW, two methods solove this 
- use bash proxy 
- use alias in  go.mod 
``` go
replace (
	golang.org/x/crypto => github.com/golang/crypto latest
	golang.org/x/sys => github.com/golang/sys latest
)
	
```
after download , `latest` will be changed to the real git SHA version .

and the pacakge located in $GOPATH/pkg/mod/cache/download/

![Image](.././assets/e99ab88a-7c95-43d8-a5e4-46601fc53e47.png)


开始 main.go 

如果有包引入，直接引入，即使没有在本地， 然后运行go run main.go 会自动下载。当然，下不下来就另说了。。

创建 main.go

```go
package main

import (
	v24 "github.com/google/go-github/v24/github"
	v25 "github.com/google/go-github/v25/github"
)

var (
	_ = v24.Tag{}
	_ = v25.Tag{}
)

func main() {
	return
}

```





### Test  

https://medium.com/@sebdah/go-best-practices-testing-3448165a0e18

自动生成测试文件

https://github.com/cweill/gotests 


1. 文件名必须以xx_test.go命名
2. 方法必须是Test[^a-z]开头
3. 方法参数必须 t *testing.T
4. 使用go test执行单元测试



 



在需要测试的同级文件，比如 abc.go 下，创建 abc_test.go

测试函数都形如此

```go
// 单例测试
func TestCheckFile(t *testing.T)

//压力测试
func BenchmarkXXX(b *testing.B) { ... }

```



```bash
go test -v ./... # 运行包下所有测试，-v 输出所有正确与错误。

go test -v -test.bench=".*" # 执行当前目录下所有压力测试 (不递归)
go test -v ./... -test.bench=".*" # 执行包下所有压力测试
go test -v ./... -test.benchtime 100s  -test.bench=".*" # 跑 100 秒，看能执行多少次
go test -v ./... -test.count=5  -test.bench=".*" # 执行 5 次


go test -test.bench="test_name_regex" # 执行某个压力测试


```





```go
package image

import (
    "fmt"
    "mime/multipart"
    "testing"
)

func TestCheckFile(t *testing.T) {
    f := multipart.FileHeader{}
    f.Filename = "1.jpg.go"
    filename, err := CheckFile(&f)
    if !err {
        fmt.Println("文件类型错误")
    } else {
        fmt.Println("new file name is ", filename)
    }
}
```



运行 
```bash
	go test -v ./...
```


###  安装 go 程序

如果有 package main 的包，并且有 main 入口。执行 go install 则会将你的项目打包成一个执行文件安装到 `$GOPATH/bin`下。如果没有 package main， 则会将你项目打包成一个动态链接文件，比如.a（各平台不一样吧），放到 `$GOPATH/pkg` 下。



#### 安装到 GOPATH/bin

```go
# 下载某个包到  $GOPATH/src
go get github.com/d4l3k/go-pry
# 安装某个包到  $GOPATH/bin 下。
go install -i github.com/d4l3k/go-pry
```



### 安装到当前文件夹

```bash
go build 
```

## go 工具链

### goimports

自动引用使用的包，并格式化。集成 vim 很舒服。

![img](.././assets/79ff95ab-8013-482f-a58e-b7a60df85c50.jpg)

### 查看当前包依赖

```bash
# 查看依赖
go list -m all

#  查看依赖图 rg text 是搜索 text 这个关键字
go mod graph | rg text
# 如果输出： golang.org/x/net@v0.0.0-20190311183353-d8887717615a golang.org/x/text@v0.3.0
# 说明 net 包依赖后面的 text@0.3.0
```
### 



### 升级工程里的包

```bash
# 列出可升级的包
go list -u -m all

# 升级所有依赖到最版本
go get -u	

# 升级所有的依赖到了新的修订版本	
go get -u=patch	

# 清理未使用/生效的依赖
go mod 
```



### 竞态检测

https://segmentfault.com/a/1190000020107431

是 go 提供的一个技巧来检测竞态的，

先看个问题，下面代码有问题么？

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	start := time.Now()
	var t *time.Timer
	t = time.AfterFunc(randomDuration(), func() {
		fmt.Println(time.Now().Sub(start))
		t.Reset(randomDuration())
	})

	time.Sleep(5 * time.Second)
}

func randomDuration() time.Duration {
	return time.Duration(rand.Int63n(1e9))
}

```

看不出来？用 `go run -race main.go`跑一下。

```text
951.859272ms
==================
WARNING: DATA RACE
Read at 0x00c000010018 by goroutine 8:
  main.main.func1()
      /Users/zk/git/goprj/src/callpackage/caller.go:14 +0x121

Previous write at 0x00c000010018 by main goroutine:
  main.main()
      /Users/zk/git/goprj/src/callpackage/caller.go:12 +0x18d

Goroutine 8 (running) created at:
  time.goFunc()
      /usr/local/Cellar/go/1.13.1/libexec/src/time/sleep.go:168 +0x51
==================
1.039721505s
1.709258657s
1.944997417s
```

第 14 行，与第 12 行的 t 未同步，如果randomDuration()时间非常短， 运行是可能报错的， t 为 nil。

解决方案：

```go
func main() {
    start := time.Now()
    reset := make(chan bool)
    var t *time.Timer
    t = time.AfterFunc(randomDuration(), func() {
        fmt.Println(time.Now().Sub(start))
        reset <- true
    })
    for time.Since(start) < 5*time.Second {
        <-reset
        t.Reset(randomDuration())
    }
}
```



别一个解决方案，慢点

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    start := time.Now()
    var f func()
    f = func() {
        fmt.Println(time.Now().Sub(start))
        time.AfterFunc(time.Duration(rand.Int63n(1e9)), f)
    }
    time.AfterFunc(time.Duration(rand.Int63n(1e9)), f)
    time.Sleep(5 * time.Second)
}
```





### benchmark

 

会 checkout 你的各个版本，做 test ，画出性能图，非常有用

https://github.com/divan/gobenchui

- commit n 次

- 写好 Test


```bash
gobenchui -last 4 github.com/jackpal/bencode-go
```




## others
### args  expansion 
```go
package main

import "fmt"

func main() {
	s := []string{"a", "b"}
	s2 := []string{"a2", "b2"}

	s3 := append(s, s2...)
	fmt.Println(s3)

}
	
```
### about GO111MODULE
https://dev.to/maelvls/why-is-go111module-everywhere-and-everything-about-go-modules-24k
### vim 集成

vim-go
Sometimes it does not work correctly. try 

:GoUpdateBinaries  in Vim

coc

#### 调试

delve
### 变量初始化

在函数中，声明有顺序，

```go
// Package main provides ...
package main

func main() {
	var a int = b  // error  b not defined
	var b int = c  // error  c not defined
	var c int = 2
}

```

但如果将上面代码挪到 main 外面，则可以通过

```go
package main

var a int = b
var b int = c
var c int = 2

func main() {
	println(a, b, c)
}

```
### 什么情况下 defer 不会生效
``` go
package main

import (
    "fmt"
    "os"
)

func main() {
    // ! 不会打印
    defer fmt.Println("!")
    // 因为没有正常退出
    os.Exit(3)
}

```


### init  函数

任何包在引入时，会执行 init函数， 多个包的 init 函数执行顺序看上去像是按 a-z 的顺序来的。

基本用途就是，比如，初始化数组。因为 for 不在能包级别使用，只能在函数里。



*每个包在项目里只会初始化一次！*



### make 与 new 区别

new 返回指针

make 返回对象，make 多少，扔回多少





## Case
### Pipeline 
``` go
package main

import (
	"crypto/md5"
	"errors"
	"fmt"
	"io/ioutil"
	"os"
	"path/filepath"
	"sort"
	"sync"
)

func sumFiles(done <-chan struct{}, root string) (<-chan result, <-chan error) {
	// For each regular file, start a goroutine that sums the file and sends
	// the result on c.  Send the result of the walk on errc.
	c := make(chan result)
	errc := make(chan error, 1)
	go func() {
		var wg sync.WaitGroup
		err := filepath.Walk(root, func(path string, info os.FileInfo, err error) error {
			if err != nil {
				return err
			}
			if !info.Mode().IsRegular() {
				return nil
			}
			wg.Add(1)
			go func() {
				data, err := ioutil.ReadFile(path)
				select {
				case c <- result{path, md5.Sum(data), err}:
				case <-done:
				}
				wg.Done()
			}()
			// Abort the walk if done is closed.
			select {
			case <-done:
				return errors.New("walk canceled")
			default:
				return nil
			}
		})
		// Walk has returned, so all calls to wg.Add are done.  Start a
		// goroutine to close c once all the sends are done.
		go func() {
			wg.Wait()
			close(c)
		}()
		// No select needed here, since errc is buffered.
		// 不需要使用 select，因为 errc 是带有 buffer 的 channel
		errc <- err
	}()
	return c, errc
}

// MD5All reads all the files in the file tree rooted at root and returns a map
// from file path to the MD5 sum of the file's contents.  If the directory walk
// fails or any read operation fails, MD5All returns an error.
func MD5All(root string) (map[string][md5.Size]byte, error) {
	m := make(map[string][md5.Size]byte)
	err := filepath.Walk(root, func(path string, info os.FileInfo, err error) error {
		if err != nil {
			return err
		}
		if !info.Mode().IsRegular() {
			return nil
		}
		data, err := ioutil.ReadFile(path)
		if err != nil {
			return err
		}
		m[path] = md5.Sum(data)
		return nil
	})
	if err != nil {
		return nil, err
	}
	return m, nil
}

type result struct {
	path string
	sum  [md5.Size]byte
	err  error
}

func main() {
	// Calculate the MD5 sum of all files under the specified directory,
	// then print the results sorted by path name.
	m, err := MD5All("/Users/zk/git/goprj/src/orm")
	if err != nil {
		fmt.Println(err)
		return
	}
	var paths []string
	for path := range m {
		paths = append(paths, path)
	}
	sort.Strings(paths)
	for _, path := range paths {
		fmt.Printf("%x  %s\n", m[path], path)
	}
}

	
```

###  json Converter

一个好用的在线工具，自动将 json 生成 stuct https://mholt.github.io/json-to-go/

一般用 struct 去承载 json。就像 java 里的 bean 一样。

但要注意，属性一定要大写，不然序列化不会成功。会认为是私有字段。

golang 与 json 的对应名称，在 \`json:"name"\` 里标示

```go

package main

import (
	"bytes"
	"encoding/gob"
	"encoding/json"
	"fmt"
	"log"
)

type People struct {
	Name        string
	Age         int
	description string //注意这个属性和上面不同，首字母小写
}
//序列化
func (people *People) Serialize() []byte {

	var result bytes.Buffer
	encoder := gob.NewEncoder(&result)

	err := encoder.Encode(people)
	if err != nil {

		log.Panic(err)
	}

	return result.Bytes()
}

//反序列化
func DeSerializeBlock(peopleBytes []byte) *People {

	var people People

	decoder := gob.NewDecoder(bytes.NewReader(peopleBytes))

	err := decoder.Decode(&people)
	if err != nil {

		log.Panic(err)
	}

	return &people
}

func main() {

	people := People{
		"chaors",
		27,
		"a good good boy",
	}

	fmt.Println()
	fmt.Println("序列化前：", people)
	//序列化
	peopleBytes := people.Serialize()
	//反序列化取出
	fmt.Println("反序列化取出：", DeSerializeBlock(peopleBytes))

	//Json解析
	if peopleJson, err := json.Marshal(people); err == nil {

		fmt.Println("Json解析：", string(peopleJson)) //description无法解析
	}

}

```
#### json 转化为 map

基本不用，太麻烦	

``` go 
func main () {
    msg := []byte(`{
    "type": "UPDATE",
    "database": "blog",
    "table": "blog",
    "data": [
        {
            "blogId": "100001",
            "title": "title",
            "content": "this is a blog",
            "uid": "1000012",
            "state": "1"
        }
    ]}`)
    var event map[string]interface{}
    if err := json.Unmarshal(msg, &event); err != nil {
        panic(err)
    }
	// interface 要强转一下，才能打印出来
	fmt.Println(event["data"].([]interface{})[0].(map[string]interface{})["state"])
}
```

#### json 部分转化为 struct

比 map稍强点，但是内部数据又退回 map了

```go
type Event struct {
    Type     string              `json:"type"`
    Database string              `json:"database"`
    Table    string              `json:"table"`
    Data     []map[string]string `json:"data"`
}
e := Event{}
if err := json.Unmarshal(msg, &e); err != nil {
    panic(err)
}

	fmt.Println(e.Data[0]["title"])

```



#### mapstructure（一般推荐）

```
go get https://github.com/mitchellh/mapstructure
```



要注意， json 的 value 必须全是 string。

在 decode 时，可以自动转换。

```go
package main

import (
	"encoding/json"
	"fmt"
	"github.com/mitchellh/mapstructure"
)

func main() {
	msg := []byte(`{
    "type": "UPDATE",
    "database": "blog",
    "table": "blog",
    "data": [
        {
            "blogId": "100001",
            "title": "title",
            "content": "this is a blog",
            "uid": "1000012",
            "state": "1"
        }
    ]}`)
	type Event struct {
		Type     string              `json:"type"`
		Database string              `json:"database"`
		Table    string              `json:"table"`
		Data     []map[string]string `json:"data"`
	}

	type Blog struct {
		BlogId  int `mapstructure:"blogId"`
		Title   string `mapstructrue:"title"`
		Content string `mapstructure:"content"`
		Uid     int `mapstructure:"uid"`
		State   int `mapstructure:"state"`
	}

	e := Event{}
	if err := json.Unmarshal(msg, &e); err != nil {
		panic(err)
	}

	if e.Table == "blog" {
		var blogs []Blog

		if err := mapstructure.WeakDecode(e.Data, &blogs); err != nil {
			panic(err)
		}

		fmt.Println(blogs)
	}
}

```



#### gojson (强烈推荐)

通过 json 生成所有 struct。

```bash
# 安装
go get github.com/ChimeraCoder/gojson/gojson
# 通过网络 生成
curl -s https://api.github.com/repos/chimeracoder/gojson | gojson -name=Repository
# 通过本地 生成
gojson -input api.json -name=Repository > repoisoty.go
```



repository.json

```json
{
	"hello": "world",
	"age": 1,
	"data": [
		{
			"id": 1,
			"name": "zk"
		}
	]
}

```

entity/repository.go

```go
package entity

type Repository struct {
	Age  int64 `json:"age"`
	Data []struct {
		ID   int64  `json:"id"`
		Name string `json:"name"`
	} `json:"data"`
	Hello string `json:"hello"`
}

```



main.go

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"test3/entity"
)

func main() {
	msg, err := ioutil.ReadFile("./repository.json")
	if err != nil {
		return
	}
	e := entity.Repository{}
	if err := json.Unmarshal(msg, &e); err != nil {
		panic(err)
	}

	fmt.Println(e.Data[0].Name)
}

```



#### gjson （强烈推荐）

```
go get -u github.com/tidwall/gjson
```

通过表达式操作 json 

```go
package main

import "github.com/tidwall/gjson"

const json = `{"name":{"first":"Janet","last":"Prichard"},"age":47}`

func main() {
	value := gjson.Get(json, "name.last")
	println(value.String())
}

```



### 时间格式化
可以按照样例输出，而不是必写格式化字符。很方便
``` go
package main

import (
    "fmt"
    "time"
)

func main() {
    p := fmt.Println

    t := time.Now()
    p(t.Format(time.RFC3339))

    t1, e := time.Parse(
        time.RFC3339,
        "2012-11-01T22:08:41+00:00")
    p(t1)

    p(t.Format("3:04PM"))
    p(t.Format("Mon Jan _2 15:04:05 2006"))
    p(t.Format("2006-01-02T15:04:05.999999-07:00"))
    form := "3 04 PM"
    t2, e := time.Parse(form, "8 41 PM")
    p(t2)

    fmt.Printf("%d-%02d-%02dT%02d:%02d:%02d-00:00\n",
        t.Year(), t.Month(), t.Day(),
        t.Hour(), t.Minute(), t.Second())

    ansic := "Mon Jan _2 15:04:05 2006"
    _, e = time.Parse(ansic, "8:41PM")
    p(e)
}
out ---------------------
2014-04-15T18:00:15-07:00
2012-11-01 22:08:41 +0000 +0000
6:00PM
Tue Apr 15 18:00:15 2014
2014-04-15T18:00:15.161182-07:00
0000-01-01 20:41:00 +0000 UTC
2014-04-15T18:00:15-00:00
parsing time "8:41PM" as "Mon Jan _2 15:04:05 2006": ...

```
### 调用命令行
``` go
package main

import (
    "fmt"
    "io/ioutil"
    "os/exec"
)

func main() {

    dateCmd := exec.Command("date")

    dateOut, err := dateCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> date")
    fmt.Println(string(dateOut))

    grepCmd := exec.Command("grep", "hello")

    grepIn, _ := grepCmd.StdinPipe()
    grepOut, _ := grepCmd.StdoutPipe()
    grepCmd.Start()
    grepIn.Write([]byte("hello grep\ngoodbye grep"))
    grepIn.Close()
    grepBytes, _ := ioutil.ReadAll(grepOut)
    grepCmd.Wait()

    fmt.Println("> grep hello")
    fmt.Println(string(grepBytes))

    lsCmd := exec.Command("bash", "-c", "ls -a -l -h")
    lsOut, err := lsCmd.Output()
    if err != nil {
        panic(err)
    }
    fmt.Println("> ls -a -l -h")
    fmt.Println(string(lsOut))
}

```
### 文件夹的操作
``` go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "path/filepath"
)

func check(e error) {
    if e != nil {
        panic(e)
    }
}

func main() {

    err := os.Mkdir("subdir", 0755)
    check(err)

    defer os.RemoveAll("subdir")

    createEmptyFile := func(name string) {
        d := []byte("")
        check(ioutil.WriteFile(name, d, 0644))
    }

    createEmptyFile("subdir/file1")

    err = os.MkdirAll("subdir/parent/child", 0755)
    check(err)

    createEmptyFile("subdir/parent/file2")
    createEmptyFile("subdir/parent/file3")
    createEmptyFile("subdir/parent/child/file4")

    c, err := ioutil.ReadDir("subdir/parent")
    check(err)

    fmt.Println("Listing subdir/parent")
    for _, entry := range c {
        fmt.Println(" ", entry.Name(), entry.IsDir())
    }

    err = os.Chdir("subdir/parent/child")
    check(err)

    c, err = ioutil.ReadDir(".")
    check(err)

    fmt.Println("Listing subdir/parent/child")
    for _, entry := range c {
        fmt.Println(" ", entry.Name(), entry.IsDir())
    }

    err = os.Chdir("../../..")
    check(err)

    fmt.Println("Visiting subdir")
    err = filepath.Walk("subdir", visit)
}

func visit(p string, info os.FileInfo, err error) error {
    if err != nil {
        return err
    }
    fmt.Println(" ", p, info.IsDir())
    return nil
}

```

### 文件路径的处理

```go
package main

import (
    "fmt"
    "path/filepath"
    "strings"
)

func main() {

    p := filepath.Join("dir1", "dir2", "filename")
    fmt.Println("p:", p)

    fmt.Println(filepath.Join("dir1//", "filename"))
    fmt.Println(filepath.Join("dir1/../dir1", "filename"))

    fmt.Println("Dir(p):", filepath.Dir(p))
    fmt.Println("Base(p):", filepath.Base(p))

    fmt.Println(filepath.IsAbs("dir/file"))
    fmt.Println(filepath.IsAbs("/dir/file"))

    filename := "config.json"

    ext := filepath.Ext(filename)
    fmt.Println(ext)

    fmt.Println(strings.TrimSuffix(filename, ext))

    rel, err := filepath.Rel("a/b", "a/b/t/file")
    if err != nil {
        panic(err)
    }
    fmt.Println(rel)

    rel, err = filepath.Rel("a/b", "a/c/t/file")
    if err != nil {
        panic(err)
    }
    fmt.Println(rel)
}
```



### 创建 cmdline 管道程序

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

func main() {

    scanner := bufio.NewScanner(os.Stdin)

    for scanner.Scan() {

        ucl := strings.ToUpper(scanner.Text())

        fmt.Println(ucl)
    }

    if err := scanner.Err(); err != nil {
        fmt.Fprintln(os.Stderr, "error:", err)
        os.Exit(1)
    }
}
```



cat /tmp/lines | go run line-filters.go



### base64

注意  URLEncoding StdEncoding 在编码上的区别(trailing `+` vs `-`)

```go
package main

import (
    b64 "encoding/base64"
    "fmt"
)

func main() {

    data := "abc123!?$*&()'-=@~"

    sEnc := b64.StdEncoding.EncodeToString([]byte(data))
    fmt.Println(sEnc)

    sDec, _ := b64.StdEncoding.DecodeString(sEnc)
    fmt.Println(string(sDec))
    fmt.Println()

    uEnc := b64.URLEncoding.EncodeToString([]byte(data))
    fmt.Println(uEnc)
    uDec, _ := b64.URLEncoding.DecodeString(uEnc)
    fmt.Println(string(uDec))
}

out -----------------
YWJjMTIzIT8kKiYoKSctPUB+
abc123!?$*&()'-=@~
YWJjMTIzIT8kKiYoKSctPUB-
abc123!?$*&()'-=@~
```



### URL 处理

```go
package main

import (
    "fmt"
    "net"
    "net/url"
)

func main() {

    s := "postgres://user:pass@host.com:5432/path?k=v#f"

    u, err := url.Parse(s)
    if err != nil {
        panic(err)
    }

    fmt.Println(u.Scheme)

    fmt.Println(u.User)
    fmt.Println(u.User.Username())
    p, _ := u.User.Password()
    fmt.Println(p)

    fmt.Println(u.Host)
    host, port, _ := net.SplitHostPort(u.Host)
    fmt.Println(host)
    fmt.Println(port)

    fmt.Println(u.Path)
    fmt.Println(u.Fragment)

    fmt.Println(u.RawQuery)
    m, _ := url.ParseQuery(u.RawQuery)
    fmt.Println(m)
    fmt.Println(m["k"][0])
}

out----------------
postgres
user:pass
user
pass
host.com:5432
host.com
5432
/path
f
k=v
map[k:[v]]
v
```



### 数字字符转换

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {

    f, _ := strconv.ParseFloat("1.234", 64)
    fmt.Println(f)

    i, _ := strconv.ParseInt("123", 0, 64)
    fmt.Println(i)

    d, _ := strconv.ParseInt("0x1c8", 0, 64)
    fmt.Println(d)

    u, _ := strconv.ParseUint("789", 0, 64)
    fmt.Println(u)

    k, _ := strconv.Atoi("135")
    fmt.Println(k)

    _, e := strconv.Atoi("wat")
    fmt.Println(e)
}
```



### 随机数

如果需要机密的随机， 用 crpyto/rand

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {

	s1 := rand.NewSource(time.Now().UnixNano())
	r1 := rand.New(s1)

	fmt.Print(r1.Intn(100), ",")
	fmt.Print(r1.Intn(100))
}

```



### 字符串处理

Go语言的字符串处理很不同，`string()`只能用于`[]byte`类型转换成字符串，其他基础类型的转换需要用`strconv`包，另外，其他类型转换成为`string`类型除了用`strconv`包，还可以用`fmt.Sprintf`函数：

```go
package main

import (
	"fmt"
	"log"
	"strconv"
)

func main() {
	s := "100"
	i, err := strconv.Atoi(s)

	if err != nil {
		log.Fatal(err)
	}

	println("i", i)
	ss := strconv.Itoa(100)
	println("ss", ss)

	s2 := fmt.Sprintf("%d", 456)
	println(s2)
}

```



### 时间处理

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    p := fmt.Println

    now := time.Now()
    p(now)

    then := time.Date(
        2009, 11, 17, 20, 34, 58, 651387237, time.UTC)
    p(then)

    p(then.Year())
    p(then.Month())
    p(then.Day())
    p(then.Hour())
    p(then.Minute())
    p(then.Second())
    p(then.Nanosecond())
    p(then.Location())

    p(then.Weekday())

    p(then.Before(now))
    p(then.After(now))
    p(then.Equal(now))

    diff := now.Sub(then)
    p(diff)

    p(diff.Hours())
    p(diff.Minutes())
    p(diff.Seconds())
    p(diff.Nanoseconds())

    p(then.Add(diff))
    p(then.Add(-diff))
}

out -----------
2012-10-31 15:50:13.793654 +0000 UTC
2009-11-17 20:34:58.651387237 +0000 UTC
2009
November
17
20
34
58
651387237
UTC
Tuesday
true
false
false
25891h15m15.142266763s
25891.25420618521
1.5534752523711128e+06
9.320851514226677e+07
93208515142266763
2012-10-31 15:50:13.793654 +0000 UTC
2006-12-05 01:19:43.509120474 +0000 UTC
```

 

### 正则

```go
package main

import (
    "bytes"
    "fmt"
    "regexp"
)

func main() {

    match, _ := regexp.MatchString("p([a-z]+)ch", "peach")
    fmt.Println(match)

    r, _ := regexp.Compile("p([a-z]+)ch")

    fmt.Println(r.MatchString("peach"))

    fmt.Println(r.FindString("peach punch"))

    fmt.Println(r.FindStringIndex("peach punch"))

    fmt.Println(r.FindStringSubmatch("peach punch"))

    fmt.Println(r.FindStringSubmatchIndex("peach punch"))

    fmt.Println(r.FindAllString("peach punch pinch", -1))

    fmt.Println(r.FindAllStringSubmatchIndex(
        "peach punch pinch", -1))

    fmt.Println(r.FindAllString("peach punch pinch", 2))

    fmt.Println(r.Match([]byte("peach")))

    r = regexp.MustCompile("p([a-z]+)ch")
    fmt.Println(r)

    fmt.Println(r.ReplaceAllString("a peach", "<fruit>"))

    in := []byte("a peach")
    out := r.ReplaceAllFunc(in, bytes.ToUpper)
    fmt.Println(string(out))
}

out ---------------------
true
true
peach
[0 5]
[peach ea]
[0 5 1 3]
[peach punch pinch]
[[0 5 1 3] [6 11 7 9] [12 17 13 15]]
[peach punch]
true
p([a-z]+)ch
a <fruit>
a PEACH
```



### 格式化打印

```go
package main

import (
    "fmt"
    "os"
)

type point struct {
    x, y int
}

func main() {

    p := point{1, 2}
    fmt.Printf("%v\n", p)

    fmt.Printf("%+v\n", p)

    fmt.Printf("%#v\n", p)

    fmt.Printf("%T\n", p)

    fmt.Printf("%t\n", true)

    fmt.Printf("%d\n", 123)

    fmt.Printf("%b\n", 14)

    fmt.Printf("%c\n", 33)

    fmt.Printf("%x\n", 456)

    fmt.Printf("%f\n", 78.9)

    fmt.Printf("%e\n", 123400000.0)
    fmt.Printf("%E\n", 123400000.0)

    fmt.Printf("%s\n", "\"string\"")

    fmt.Printf("%q\n", "\"string\"")

    fmt.Printf("%x\n", "hex this")

    fmt.Printf("%p\n", &p)

    fmt.Printf("|%6d|%6d|\n", 12, 345)

    fmt.Printf("|%6.2f|%6.2f|\n", 1.2, 3.45)

    fmt.Printf("|%-6.2f|%-6.2f|\n", 1.2, 3.45)

    fmt.Printf("|%6s|%6s|\n", "foo", "b")

    fmt.Printf("|%-6s|%-6s|\n", "foo", "b")

    s := fmt.Sprintf("a %s", "string")
    fmt.Println(s)

    fmt.Fprintf(os.Stderr, "an %s\n", "error")
}

out -----------------
{1 2}
{x:1 y:2}
main.point{x:1, y:2}
main.point
true
123
1110
!
1c8
78.900000
1.234000e+08
1.234000E+08
"string"
"\"string\""
6865782074686973
0x42135100
|    12|   345|
|  1.20|  3.45|
|1.20  |3.45  |
|   foo|     b|
|foo   |b     |
a string
an error
```



### 给任意类型加排序

```go
package main

import (
    "fmt"
    "sort"
)

type byLength []string

func (s byLength) Len() int {
    return len(s)
}
func (s byLength) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}
func (s byLength) Less(i, j int) bool {
    return len(s[i]) < len(s[j])
}

func main() {
    fruits := []string{"peach", "banana", "kiwi"}
    sort.Sort(byLength(fruits))
    fmt.Println(fruits)
}
```



### 原子写 

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {

	var ops uint64

	var wg sync.WaitGroup

	for i := 0; i < 50; i++ {
		wg.Add(1)

		go func() {
			for c := 0; c < 1000; c++ {

				atomic.AddUint64(&ops, 1)
			}
			wg.Done()
		}()
	}

	wg.Wait()

	fmt.Println("ops:", ops)
}

```



### 限流

```go
package main

import (
	"fmt"
	"time"
)

func main() {

	requests := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		requests <- i
	}
	close(requests)

	limiter := time.Tick(200 * time.Millisecond)

	for req := range requests {
		<-limiter
		fmt.Println("request", req, time.Now())
	}

	burstyLimiter := make(chan time.Time, 3)

	for i := 0; i < 3; i++ {
		burstyLimiter <- time.Now()
	}

	go func() {
		for t := range time.Tick(200 * time.Millisecond) {
			burstyLimiter <- t
		}
	}()

	burstyRequests := make(chan int, 5)
	for i := 1; i <= 5; i++ {
		burstyRequests <- i
	}
	close(burstyRequests)
	for req := range burstyRequests {
		<-burstyLimiter
		fmt.Println("request", req, time.Now())
	}
}

```





### 等待所有协程结束

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {

    defer wg.Done()

    fmt.Printf("Worker %d starting\n", id)

    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {

    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }

    wg.Wait()
}
```



### pool

worker 做事的一般是 func

任务，结果，可以想像成一个传送带，所以是 chan

```go
package main

import (
    "fmt"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "started  job", j)
        time.Sleep(time.Second)
        fmt.Println("worker", id, "finished job", j)
        results <- j * 2
    }
}

func main() {

    const numJobs = 5
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= numJobs; a++ {
        <-results
    }
}
```

### UI
use html css golang build ui
https://wails.app/gettingstarted/installing/

use native library to build ui , smallest 
https://github.com/andlabs/ui

### orm

### web

### cmdline

### 爬虫

https://segmentfault.com/a/1190000019969473

##  pitfalls
what will prints?

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	i := 2
	if i > 1 {
		i, err := doDivision(i, 2)
		if err != nil {
			panic(err)
		}
		fmt.Println(i)
	}
	fmt.Println(i)
}

func doDivision(x, y int) (int, error) {
	if y == 0 {
		return 0, errors.New("input is invalid")
	}
	return x / y, nil
}

	
```
prints:1 2 

why? because i is overwited.


fixed:
notice there is no `:`
```go
var err error
i, err = doDivision(i,2)
```



## intellij 配置
不用关心　sdk　里的配置，只要环境变量配对了就行
可以配置新的　GOPATH,　比较方便
![Image](.././assets/0e6f7a4b-f853-408a-847a-a8dcda94c6bf.png)

## 参考文档

(大道至简—GO语言最佳实践- 腾讯技术工程)https://blog.csdn.net/Tencent_TEG/article/details/80589548

https://github.com/golang/go/wiki/CodeReviewComments

 (python3 与 golang 对比）https://segmentfault.com/a/1190000019167287

https://blog.csdn.net/m0_37125796/article/details/89447627

golang 内存模型 https://golang.org/ref/mem

包的制作 https://studygolang.com/articles/16765

基础学习 https://miek.nl/go/

项目练习： https://gophercises.com/

gobyexample https://gobyexample.com/

vim-go 推荐的学习网站 https://farazdagi.com/2015/vim-as-go-language-ide/

go 学习笔记（很全）https://www.kancloud.cn/liupengjie/go/

关于包名 https://stackoverflow.com/questions/19998250/proper-package-naming-for-testing-with-the-go-language	
