# 宏定义  
GOROOT: Go程序安装路径  
GOPATH: Go工作区，存放第三方代码  
GOBIN: Go可执行程序，引用包路径  
GOPRIVATE: 第三方私有宝包下载路径  


# 语言 & 框架

[语言](https://www.runoob.com/go/go-tutorial.html)  
[go-kit框架](https://gokit.io/)  
[grpc](https://grpc.io/docs/languages/go/quickstart/)  



# 优雅代码

- 统计耗时 
```
func timeCost() func() {
	start := time.Now()
	return func() {
		tc := time.Since(start)    
		fmt.Println("time cost = %v", tc)
	}
}

使用示例
defer timeCost()()   // 注意，是对 timeCost() 返回的函数进行延迟调用，因此需要加两对小括号


```

