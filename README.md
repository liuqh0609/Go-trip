# Go 笔记

## Go 模块的进程

- 无法锁定依赖,就导致没办法进行可重现的构建（可重现的构建：因为你在本地开发，和别人拉去代码后在另一个电脑上开发时依赖的包可能会变化，所以可能没办法重现问题）
- vendor 目录,将依赖缓存到该目录，需要上传到 git 仓库用来锁定依赖，繁琐让项目变大
- Go Module,采用 go.mod 和 go.sum 来记录依赖（类似 package.json)**使用最小版本机制来进行包依赖选择**
  - go mod init 初始化 Go Module 模式，创建 go.mod 文件
  - go mod tidy 自动记录项目中用到的依赖，输入到 go.mod 文件中
  - go get xxxx 为当前项目新增依赖； xxx@version 可以新增指定版本的依赖

## 数据类型

- 基本数据类型
  - 数值类型
  - 字符串类型
- 复合类型
  - **数组**：`var [N]T` 长度固定(`N`)、类型相同(`T`) 
    - 数组类型不仅是逻辑上的连续序列，而且在实际内存分配时也占据着一整块内存
    - Go中传递数组的方式都是**纯粹的值拷贝**（Go中的数组代表的就是整个数组，而不是指针），这会带来额外的内存拷贝开销，如果想通过指针的方式传递，那么就使用**切片**
  - **切片**：`var []T`
    - 没有固定长度，可以通过`nums = append(nums,1)`来动态添加元素
    - 对已有数组进行切片`splice = arr[low,hight,max]` `low`是其实下标，切片长度为`(hight-low)`，max是切片容量（常省略max），对切片的操作会影响原数组
    - 在基于数组进行切片后，当使用append追加变量超过原数组的边界时，切片会与原数组进行解绑，即之后切片的操作不会影响到原数组。这是因为append在自动扩容的时候，发现原数组容量已经不符合，就会新创建一个数组区域，将原数组的元素复制到新的区域中，因此进行解绑

## 变量

`var 变量名称 变量类型 = 值`

`var a int = 1`
`var a,b,c = 1,2,3`

- 变量块声明

```go
  var (
  a int = 1
  b string = "hello"
  )
```

- 短变量声明： `a:=1` (适用局部变量)

## 常量

常量的类型只局限于基本数据类型：数值、字符串和布尔类型。

- `untyped constant`(无类型常量): 变量的类型是不确定的，可以是任意类型的值，存在隐式转换

- 常量实现枚举
  - 隐式重复上一行
  - `iota` 行偏移量（类似数组的指针）

```go
    // b，c 没有赋值，所以隐式的重复上面 a=1 的赋值，所以都是 1
    const (
      a = 1
      b
      c
    )

    const (
    	_     = iota //_ 空白字符可以忽略，来实现枚举从1开始
    	red          //1
    	blue         //2
    	black        //3
    	_            // 4的位置用空白字符表示，就证明跳过了4
    	white        //5
    )
```
