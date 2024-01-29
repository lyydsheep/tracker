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

### 4、切片

一个切片具有三个元素：

- 指向底层数组中某个元素的指针：切片是对底层数组的引用，可以是部分引用，也可以是全部引用，因此指针所指向的元素不一定是数组的首个元素，但一定是切片的首位元素
- 长度：切片的长度指切片中元素的个数
- 容量：切片的容量是指底层数组中被指向的元素到末尾元素之间的长度大小

切片的类型是[]type

切片初始时没有底层数组，初始值为nil

> 创建切片的三种方式

- 截取数组：array[begin:end]，得到的切片包含数组中==[begin,end)==的元素。如果省略begin那么默认为0，如果省略end；那么默认为末尾
- 直接创建：==注意类型是[]type==
- 调用内置函数make([]type, len, cap)，如果省略cap参数，那么默认cap和len相等，make函数创建的切片len个元素均为对应零值

![image-20240122172301273](https://gitee.com/lyydsheep/pic/raw/master/202401221728161.png)

> 切片的扩容

调用append(slice, elements...)方法向切片末尾添加元素，并返回新生成的切片，==若想append改变原切片，就需要接受返回值==,==elements...中...表示是一个切片==

```go
foodSlice1 = append(foodSlice1, "rice")
```



当切片的容量不足时，切片会自动扩容

切片的扩容并不是扩容底层数组，而是生成一个更大的底层数组，并将原数据复制到新数组中，即更换一个更大的底层数组。

这一点可以从foodSlice1和foodSlice2的首个元素地址中看出：

当foodSlice1未扩容时，两个元素的地址一致，但foodSlice1扩容之后，两个元素的地址不一致，证明了切片的扩容并非时改变底层数组的大小，而是改变对底层数组的指向。

![image-20240122173704674](https://gitee.com/lyydsheep/pic/raw/master/202401221737759.png)

> 遍历切片

和遍历数组的方法一样：

```go
//使用
for idx, val := range slice {
    
}
//对切片进行遍历
```

> 复制切片

复制切片有两种方法：

- 对源切片进行截取，并省略begin和end，此时默认截取全部元素
- 调用内置copy(dest_slice, src_slice)函数对切片进行复制，如果两个切片的大小不同，则以较小的切片为标准

![image-20240122175117887](https://gitee.com/lyydsheep/pic/raw/master/202401221751955.png)

> 对切片中的元素进行修改

对切片中元素操作无非是修改、删除、添加、插入

修改可以直接原地对元素的值进行修改

删除、添加、插入这些操作都是基于append()函数和切片截取操作上的

![image-20240122181136040](https://gitee.com/lyydsheep/pic/raw/master/202401221811181.png)

通过代码可以看出，灵活使用append和截取操作能对切片进行多种元素操作

但值得注意的是，第18行代码中，第二个append先执行导致第一个append函数第二个参数变成了对新生成的slice进行截取最后一个元素，即“熘大虾”

最终导致元素重复，插入失败

### 5、流程控制

> switch语句

当switch表达式结果和case表达式结果一致时，就执行该case表达式后面的语句，只有case表达式后面的语句中含有fall through关键字时，才会进行执行下一个case

switch表达式结果的类型必须和case表达式结果类型一致才能进行比较

![image-20240123175924252](https://gitee.com/lyydsheep/pic/raw/master/202401231759433.png)

> for range

range表达式会在每次循环执行时求值一次，被迭代的对象时range表达式结果值的副本，之后的修改只会影响range的值，而不改变循环的次数

![image-20240123182257952](https://gitee.com/lyydsheep/pic/raw/master/202401231822033.png)

### 6、字典

> 字典简介

字典存储的不是单一元素，而是键值对。

键值对总是在一起存储的，在Go语言规范中规定，键的类型==不能是函数、切片、字典==。如果键的类型时接口类型，实际类型==也不能是==以上三种

相反地，任何Go语言对象都能成为字典的值

> 创建字典

有两种方式创建字典：

1、字面量

```go
var m map[string]int = map[string]int {
    "1":1, "2":2,
}
```

2、使用内置函数make

```go
foodsMap := make(map[string]int)
foodsMap["1"] = 1
foodsMap["2"] = 2
```

> 删除键值对

使用内置函数delete(map, key)对字典进行删除操作

![image-20240125213937917](https://gitee.com/lyydsheep/pic/raw/master/202401252139101.png)

> 遍历字典

使用for range对字典进行遍历，可以使用_占位符加速遍历

字典遍历的顺序是不固定的，即迭代顺序和插入顺序没有关系

![image-20240125214326385](https://gitee.com/lyydsheep/pic/raw/master/202401252143501.png)

### 7、定义函数

Go语言中完整的函数有以下部分：

- 函数名
- 形参列表
- 返回列表
- 函数体

![image-20240126160658427](https://gitee.com/lyydsheep/pic/raw/master/202401261607515.png)

当返回值未命名或只有两个以下的返回值时，返回列表可以无需括号

如果函数名以大写开头，那么该函数时可见的；反之，以小写字母开头则是不可见的

函数类型至取决于==参数列表和返回列表==

一般来说，修改函数的形参不会改变实参，但如果提供的实参时引用类型，例如：切片、字典、通道、函数指针等，对形参的修改会影响实参

> 传递数组

将数组作为实参传递给函数是很低效的，因为在函数内操作形参只是一个副本，作用不大

但如果传递的是指针数组，那么在函数内也可以对实参进行修改

![image-20240126164554309](https://gitee.com/lyydsheep/pic/raw/master/202401261645450.png)

> 传递切片

向函数中提供的实参包含引用类型，例如：指针、切片、字典、通道、函数，即使形参是实参的副本，对形参的修改仍有可能影响到实参

> 函数变量

在使用函数变量之前，需要使用type关键字定义函数变量的类型，相当于取别名

前文提到，函数变量的类型仅与形参列表和返回列表有关

```Go
type 类型名称 具体类型
type Hi func(string) string
```

![image-20240126173354886](https://gitee.com/lyydsheep/pic/raw/master/202401261733058.png)

与函数声明不同，由于函数变量的类型仅与参数列表和返回列表有关，因此在定义函数变量时，func关键字后面紧跟参数列表而非函数名

> 匿名函数

匿名函数就是在声明函数时略去函数名即可，匿名函数同样可以获取和更新外层函数的局部变量

![image-20240126180247869](https://gitee.com/lyydsheep/pic/raw/master/202401261802018.png)

> 闭包

闭包是一个特殊的函数，这个函数引用了外部函数的变量，并维护了这个环境，当外部函数的外部引用这个闭包时，就可以使用这些变量

闭包的核心就是==函数+引用环境==

![image-20240126202820008](https://gitee.com/lyydsheep/pic/raw/master/202401262028142.png)

> 变长函数

参数列表用...type表示形参个数不确定

```go
func SayHello(words ...string) string {
    //do something
}
```

> defer延迟调用

defer关键字表示延迟调用，无论程序是正常结束还是异常退出，defer都会执行它后面的语句

若有多个defer语句，那么执行顺序为出栈顺序

> panic

当函数调用panic时，正常函数执行会立即停止，但函数中的defer正常执行，之后将panic向上抛给调用函数，直至goroutine中所有函数被终止为止

![image-20240126204620464](https://gitee.com/lyydsheep/pic/raw/master/202401262046584.png)

> recover

recover函数可以用于处理程序运行时错误，终止panic上抛流

panic一般结合defer语句使用

```go
defer func() {
    if err := recover(); err != nil {
        fmt.Println(err)
    } else {
        //do something
    }
}
```

如果在defer语句中调用了recover函数，并且定义defer语句的函数内发生了panic，recover能返回panic的信息并终止panic上抛，函数终止并正常返回

![image-20240126205613193](https://gitee.com/lyydsheep/pic/raw/master/202401262056349.png)

### 8、结构体

> 概述

结构体的格式

```go
type name struct {
    var xx type1
    var xx type2
}
```

如果结构体中变量的首字母是大写的，那么该变量是可导出的

Go语言中可见性是包级别的，而不是类型级别的

空结构体没有长度，也没有任何信息

> 结构体初始化方式

结构体有两种初始化方式

- 直接使用字面值进行初始化，此时变量和值一一对对应
- 显示指明变量名和值

![image-20240129221330190](https://gitee.com/lyydsheep/pic/raw/master/202401292213334.png)

> 匿名成员与结构体嵌套

Go语言在结构体中可以定义不带变量名的结构体成员，这种成员叫做匿名成员

匿名成员必须是一个命名类型或者是命名类型的指针

值得注意的是，出现结构体嵌套时，切记初始化内部的结构体，否则易出错

![image-20240129222401063](https://gitee.com/lyydsheep/pic/raw/master/202401292224212.png)

当结构体有嵌套关系时，外层结构体可以忽略中间结构体直接访问内层结构体的变量（前提是变量可导出）

当结构体只需使用一次时，可以在变量复制时创建匿名结构体并初始化，省略type 和 name

![image-20240129222806975](https://gitee.com/lyydsheep/pic/raw/master/202401292228158.png)

> 结构体与JSON

- 结构体转JSON

通过json.Marshal()方法将结构体转为json，该方法返回一个字节切片和一个error

- JSON转结构体

通过json.UnMarshal()方法将json字节切片转为结构体并赋值到一个具体的变量上

注意；json字符串是以``（飘号）包裹的

![image-20240129225306551](https://gitee.com/lyydsheep/pic/raw/master/202401292253629.png)