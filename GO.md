# GO

## 基础知识

### 1、起步

#### 1.1源代码与程序

.go文件存放在包中，一个包由一个或多个.go源文件组成，包名通常用来描述该包的作用；源代码是从package开始的，表示该源文件属于哪个包。

main包是一个特殊的包，它定义了一个独立的可执行程序。

```go
go run main.go	//对源文件编译、链接、生成可执行文件

go build main.go	//生成可复用的程序——main.exe
```

#### 1.2变量的命名

变量首字母的大小写决定了该变量是否是可导出的；如果变量首字母是大写，则对外包是可见、可使用的，反之不可见、不可使用。

> var可以用来创建某种类型的变量，并设置值

```go
var name type = expression
```

type和expression只能==省略一个==

- 省略type: name的类型由expression决定
- 省略expression: name默认设置为type类型的零值

>短==变量==

```go
name := expression
//变量name的类型由expression决定
```

> 内置函数new

```
p := new(Type)
```

使用内置函数new(Type)，可以创建一个Type类型的变量，并初始化为相应的零值，返回对应的指针

![image-20240120145746971](https://gitee.com/lyydsheep/pic/raw/master/202401201457007.png)

> 包初始化

包初始化依照依赖顺序进行初始化，即先执行的包后初始化，即main包最后初始化。

每一个包可以有多个init函数用于包初始化，init函数只能自动执行不能主动调用。

![image-20240120150958238](https://gitee.com/lyydsheep/pic/raw/master/202401201509287.png)

### 2、变量与简单类型

> 字符串相关操作

- 修改字符串的大小写

  - strings.ToUpper(string)
  - strings.ToLower(string)

- 字符串拼接

  - +
  - strings.Join([]string, string)
  - fmt.Sprintf("%s:%s", str1, str2)
  - bytes.Buffer{}.WriteString(string)

  ![image-20240120153943832](https://gitee.com/lyydsheep/pic/raw/master/202401201539928.png)

- 删除指定部分
  - strings.Trim(string, cutset)：删除字符串中==前导和末尾==cutset部分，返回修改后的结果，但不修改原字符串

![image-20240120154846105](https://gitee.com/lyydsheep/pic/raw/master/202401201548204.png)

- 消息体中镶嵌变量

  - 通过strconv.Itoa(int)函数将int变量转为string，再通过字符串拼接的方法将变量镶嵌进消息体中

  ![image-20240120155832900](https://gitee.com/lyydsheep/pic/raw/master/202401201558944.png)

### 3、数组

数组是具有固定长度且拥有0个或多个相同数据类型元素的集合

#### 3.1初始化数组元素的三种方式

第一种初始化方式：将变量设置为固定长度的数组类型，数组内的元素初始化对应类型的零值

第二种初始化方式：指定变量类型的同时用明文初始化数组内的元素

第三种初始化方式：短变量形式

![image-20240121162124073](https://gitee.com/lyydsheep/pic/raw/master/202401211621178.png)

数组的长度必须是==固定常数==，即在编译成二进制文件阶段就能确定数组的长度

不同长度数组之间不能进行赋值，因为长度不同，变量的类型也不相同，否则会报错

如果数组元素是可比较的，那么同长度的数组也是可比较的，可以使用“==”对数组进行比较，比较的原则是一一比对数组内的元素

![image-20240121162828617](https://gitee.com/lyydsheep/pic/raw/master/202401211628707.png)

#### 3.2遍历数组

使用for range对数组进行遍历

```go
for idx, val := range list {
    //some operations
}
```

![image-20240121163920728](https://gitee.com/lyydsheep/pic/raw/master/202401211639825.png)