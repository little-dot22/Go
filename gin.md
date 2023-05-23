# Gin
## 1 Web
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
