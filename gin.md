# Gin
## 1 Web
### 1.1 原生Go实现Web网页
    package main

    import (
        "fmt"
        "net/http"
    )

    func sayHello(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "<h1>Hello Golang<h1>") // 把"Hello Golang"写入响应w
    }

    func main() {
        http.HandleFunc("/hello", sayHello) // 将 "/hello" URL 路径映射到 sayHello 函数
        err := http.ListenAndServe(":9090", nil)    // 启动 Web 服务器，传入要监听的端口号
        if err != nil {
            fmt.Printf("err: %v\n", err)
            return
        }
    }
>也可以把文件给过去，其中的文件符合HTML等语法。

    package main

    import (
        "fmt"
        "io/ioutil"
        "net/http"
    )

    func sayHello(w http.ResponseWriter, r *http.Request) {
        b, _ := ioutil.ReadFile("./hello.txt")
        fmt.Fprintln(w, string(b))
    }

    func main() {
        http.HandleFunc("/hello", sayHello)
        err := http.ListenAndServe(":9090", nil)
        if err != nil {
            fmt.Printf("err: %v\n", err)
            return
        }
    }
### 1.2 Gin实现Web网页
    package main

    import (
        "github.com/gin-gonic/gin"
    )

    func sayHello(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Hello!",
        })
    }

    func main() {
        // 创建一个默认的路由引擎
        r := gin.Default()

        // 当客户端以GET方法请求/hello路径时，会执行后面的函数
        r.GET("/hello", sayHello)

        // 启动HTTP服务，默认在0.0.0.0:8080启动服务
        r.Run(":9090")
    }
### 1.3 RESTful API
REST与技术无关，代表的是一种软件架构风格，REST是Representational State Transfer的简称，中文翻译为“表征状态转移”或“表现层状态转化”。简单来说，REST的含义就是客户端与Web服务器之间进行交互的时候，使用HTTP协议中的4个请求方法代表不同的动作。

- GET用来获取资源
- POST用来新建资源
- PUT用来更新资源
- DELETE用来删除资源。

我们按照RESTful API设计如下：

|请求方法|URL|含义|
|:--:|:--:|:--:|
|GET|/book|查询书籍信息|
|POST|/book|创建书籍信息|
|PUT|/book|更新书籍信息|
|DELETE|/book|删除书籍信息|

URL一致，返回json，只是请求方法不同，以完成不同的功能。但是假如前端和后端统一，把删除的动作也用GET方式实现，那技术上也没问题，只是脑子有坑而已。
## 2 template
### 2.1 认识template
>这是一种模板，可以把模板中的内容替换成想要的内容这里主要涉及到了模板的创建、解析和渲染。

>main.go:

    package main

    import (
        "fmt"
        "html/template"
        "net/http"
    )

    func sayHello(w http.ResponseWriter, r *http.Request) {
        t, err := template.ParseFiles("./hello.tpl")
        if err != nil {
            fmt.Printf("err: %v\n", err)
            return
        }
        name := "Jerry"
        t.Execute(w, name)
    }

    func main() {
        http.HandleFunc("/", sayHello)
        err := http.ListenAndServe(":9000", nil)
        if err != nil {
            fmt.Printf("err: %v\n", err)
            return
        }
    }

>hello.tpl:

    <!DOCTYPE html>
    <html lang="zh-CN">
    <head>
        <title>Hello</title>
    </head>
    <body>
        <p>Hello {{.}}</p>
    </body>
    </html>