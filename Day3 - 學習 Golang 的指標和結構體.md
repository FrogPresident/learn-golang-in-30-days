# (ChatGPT幫幫我) Day3 - 學習 Golang 的指標和結構體

這是我們第三天的學習計畫指標、結構體，我原本還想寫`Map`，但寫光是寫完指標就發現篇幅實在太長了~~ 我只能說進度嚴重超車~甚至連範圍`Range`都一起講了我還沒有意識到XD，但我覺得這不是壞事，我們可以在這個章節多留一些時間來好好複習指標和一些重點技術，畢竟在我大一學習C語言時碰到最抽象的東西就是指標，這東西也變成我的陰影，導致我到現在研究所畢業了都沒有好好學習指標，畢竟我都在寫`Python`、`JavaScript`之類的語言也碰不到，這次話說得有點太多了，我們就趕緊來看一下今天的重點吧。

## 結構體

在 Go 語言中，結構體（Structs）是一種復合數據類型，允許你將多個不同類型的數據組合成一個有意義的單元。這是 Go 中實現封裝（一個面向對象編程的核心概念）的主要方式之一。結構體特別適合用來創建和管理大量相關數據的模型，如用戶信息、產品規格等。
#### 基本結構體的定義和使用
定義一個結構體涉及到指定其名稱和它包含的欄位。每個欄位都有一個名稱和類型。
#### 定義和初始化結構體
```go!
// 定義一個 Person 結構體
type Person struct {
	Age  int
	Name string
} 

// 為 Person 結構體添加一個方法
func sayMyName(p Person) string {
	return p.Name
}

func main() {
    // 使用結構體
	person := Person{
		Name: "Heisenberg",
		Age:  50,
	}
	fmt.Println(sayMyName(person))
}
```

我們先大致了解了結構體的常見架構，稍後我們可以先瞧瞧指標，下面也會配合著結構體進行更深入的用法。

## 指標(Pointers)

指標是一種存儲變數內存地址的變數。每個變數都存儲在內存的一個位置上，這個位置可以通過一個地址來訪問。指標變數保存了這個地址，因此通過指標，你可以直接讀取或修改該地址處的數據。

#### 宣告指標
在 Go 中，你可以使用 * 跟在類型前面來宣告一個指標類型的變數。例如，*int 表示一個指向整數的指標。
例如：
```go!
var p *int
```
#### 獲取變數的地址
使用 & 操作符可以獲取一個變數的內存地址，並將這個地址賦值給一個指標。
```go!
var a int = 58
p = &a
// 使用 * 操作符可以訪問指標指向的內存地址中的數據（即 dereferencing 指標）
fmt.Println(*p) // 輸出 58
// 不使用* 直接Print，則會直接輸出內存地址位置，而非數據
fmt.Println(p) // 輸出 0xc000096068
```

### 指標的常用用法
指標在 Go 中有幾個非常有用的應用場景：
1. 改變函數內部變數的值：在 Go 中，所有函數參數都是通過值傳遞的。如果你希望一個函數能夠修改其參數的值，你可以將指標作為參數傳遞給函數。

```go!
func increment(p *int) {
    *p = *p + 1
}

func main() {
    a := 1
    increment(&a)
    fmt.Println(a) // 輸出 2
}
```
另外一個實際常會用到的例子是這個：
```go!
type Person struct {
	Age  int
	Name string
}

func birthday(p *Person) {
	// 自動的dereference，不用再使用*p.Age
	p.Age += 1
}

func main() {
	person := Person{
		Age:  24,
		Name: "FrogBoy69",
	}
	birthday(&person)
	fmt.Println(person.Age) // 輸出 25
}
```
`Person`是一個結構體，那為什麼我會說這是一個比較常用且實際的例子是因為，使用指標是改變函數外部數據的唯一方法。這不同於其他一些語言，如 `Python` 或 `JavaScript`，其中對象或數組通過參考傳遞，函數可以直接修改這些參數，像是簡單使用`this`表示，而Golang就需要用到指標。

當然你也可以把`(p *Person)`放在`birthday`的前面像是這樣：
```go!
func (p *Person) birthday() {
	// 自動的dereference，不用再使用*p.Age
	p.Age += 1
}

func main() {
	person := Person{
		Age:  24,
		Name: "FrogBoy69",
	}
	person.birthday()
	fmt.Println(person.Age)
}
```


2. 優化性能，避免大數據結構的複製：對於大型數據結構（如大陣列或結構體），使用指標可以避免在參數傳遞時的複製，從而提高性能。

```go!
type LargeStruct struct {
	a [10000]int
}

func modify(ls *LargeStruct) {
	ls.a[0] = 11111
}

func main() {
	data := LargeStruct{}
	modify(&data)
	fmt.Println(data.a[0])
}
```
如果我們不使用指標，modify 函數將會接收到 LargeStruct 的一個副本，這包含複製 a 陣列的所有 10000 個 int 元素。


## 回家作業：簡單的員工管理系統
每每提到回家作業我都好懶惰XD，不過今天還是得寫一下，先來點簡單的就行了
### 目標
使用結構體和指標來創建一個簡單的員工管理系統。

### 步驟 1: 定義員工結構體
- 定義一個 `Employee` 結構體，包含員工的名字、部門和入職年份。

### 步驟 2: 實現添加經驗年數的功能
- 為 `Employee` 結構體添加一個方法 `AddExperience`，該方法接受一個整數，表示表示進入公司的年資，並更新員工的入職年份。

### 步驟 3: 在 main 函數中使用這些結構體和方法
- 在 `main` 函數中創建一個員工實例，打印初始狀態。
- 調用 `AddExperience` 方法，然後再次打印狀態來看到更新。

## 輸入、輸出示例
輸入例子為：
```go!
fmt.Println("Before:", emp)
emp.AddExperience(3) // 假設添加 3 年經驗
fmt.Println("After:", emp)
```
這將顯示如下輸出，表明員工的資料在調用 `AddExperience` 方法後已被更新：
```go!
Before: {John Doe IT 2020}
After: {John Doe IT 2017}
```

以下為我實作出來的作業：
```go!
package main

import "fmt"

type Employee struct {
	Name       string
	Department string
	JoinYear   int
}

func (employee *Employee) AddExperience(years int) {
	employee.JoinYear -= years
}

func main() {
	emp := Employee{
		Name:       "FrogBoy69",
		Department: "backend",
		JoinYear:   2024,
	}
	fmt.Println("Before:", emp)
	emp.AddExperience(3) // 假設添加 3 年經驗
	fmt.Println("After:", emp)
}
```
