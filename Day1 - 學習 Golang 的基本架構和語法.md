# (ChatGPT幫幫我) Day1 - 學習 Golang 的基本架構和語法

範例基本由我撰寫，ChatGPT則安排我第一天的編排與解釋。
## 語言架構

``` go
// 定義了一個名為 main 的包，這是每個 Go 程式的入口包。
// 在 Go 語言中，每個程式都需要有一個名為 main 的包，並且這個包中應該包含一個名為 main 的函數。
package main

// 導入了 fmt 套件，這個套件包含輸入輸出函數，例如打印到控制台。
// fmt 是 format 的縮寫，提供格式化輸出和輸入的功能。
import "fmt"

// main 函數是每個可執行 Go 程式的入口點。
// 當 Go 程式被執行時，執行流程會從 main 函數開始。
func main() {
    // 調用 fmt 套件中的 Println 函數來輸出文字到標準輸出（通常是命令行或終端機界面）。
    // Println 是 print line 的縮寫，這個函數會在輸出的結束添加一個新行。
    fmt.Println("Hello, world!")
}
```
以上是一個基本的Golang範例

## Go 語言變數基礎
在 Go 語言中，你可以使用 var 來聲明一個變數
宣告的格式為 var[變數名稱][變數型態]

變數聲明的例子：

``` go
// 明確指定類型
var a int =10
 // 類型推斷
var b = 10
// 短變數聲明，最常用的方式
c:=10
```
## 變數命名規則

### 基本規則

1. **字母或底線開頭**：變數名必須以字母（Unicode 字母）或底線（_）開頭，這之後可以包含任何數量的字母、底線或數字。
2. **大小寫敏感**：Go 語言區分大小寫，因此 `Variable` 和 `variable` 被認為是兩個不同的變數。
3. **避免使用關鍵字**：不應使用 Go 的關鍵字（如 `if`, `else`, `func` 等）作為變數名，因為這會引起語法錯誤。

```go!
userName:="Frogboy69"
user_age:=24
_temporaryValue = "This is temporaryValue."
```

## if 語句

在 Go 語言中，if 語句不需要圍繞條件表達式的括號，但是需要大括號 {} 包圍代碼塊

if語句的例子：
```go!
if a > 10{
    fmt.Println("a is greater than 10.")
}else{
    fmt.Println("a is not greater than 10.")
}
```

## for 迴圈

for迴圈跟if一樣不需要條件式的括號，而且for迴圈是唯一的迴圈語句，可以透過不同的方式實現傳統的 for、while 和 do-while 迴圈的功能。

傳統for迴圈的例子：
```go!
for i:=0;i<10;i++{
    fmt.Println(i)
}
```

當成while時:
```go!
i:=0
for i<10{
    fmt.Println(i)
}
```

### 進階用法
題外話，Go語言的for非常靈活，也可以實現類似enumerate的功能來遍歷Array,Map,Slice等等~~

```go!
package main

import "fmt"

func main() {
	//這是叫做切片(Slice)的陣列結構，可以先大概理解一下就好了，跟Array很相似
	fruits := []string{"banana", "apple", "coconut"}
	// index 是指當前對應到水果的索引，簡單的用":= range"，就能實現enumerate
	for index, fruit := range fruits {
		//另外Printf的%d與%s則是和"C語言"沒什麼兩樣
		fmt.Printf("Index:%d,Fruits:%s\n", index, fruit)
	}
}
```
得到的輸出會長這樣：
```go!
Index:0,Fruits:banana
Index:1,Fruits:apple  
Index:2,Fruits:coconut
```

## 回家作業

我請ChatGPT來幫我安排了一個 Go 程式的作業，大家沒事可以試試~多練一點語法!沒損失的。
### 題目：
聲明一個整數變數 n，初始化為任意值，然後使用 for 迴圈將從 1 到 n 的數字打印出來。如果當前數字是偶數，則使用 if 語句打印 "偶數"，如果是奇數則打印 "奇數"。

結果我們ChatGPTさん還是相當仁慈，出了一個非常簡單的作業，大家也可以自己嘗試看看，我會把我的寫法放在下面，下次我會出一個比較難的題目是由(susautw)所提供，大家可以期待一下。

```go!
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	// set n is a radon number ,range(1~100)
	n := rand.Intn(100) + 1
	if n%2 == 1 {
		fmt.Printf("%d is odd", n)
	} else {
		fmt.Printf("%d is even", n)
	}
}
```