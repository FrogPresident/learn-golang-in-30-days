# (ChatGPT幫幫我) Day2 - 學習 Golang 的常數、函數、陣列以及切片
第二天ChatGPT他想要我們學學常數、函數、陣列(Array)以及切片(Slice)呐，我刻意把Array和Slice放在一起講，這兩種東西比較容易混淆使用的。

## 常數 (Constants)
在 Go 中，常數是使用 const 關鍵字定義的，一旦設定，在整個程序執行期間其值不可改變。常數可以是字符、字符串、布林或數字類型。

範例：

```go!
// 像是Pi或是status code這些就不太會更變
const Pi = 3.14

const (
	StatusOk       = 200
	StatusNotFound = 404
)

```
## 函數 (Functions)
函數在 Go 中是基本的程式碼區塊，用於執行特定的任務。函數可以有參數和返回值。

基本語法：
```go!
func functionName(param1 type1, param2 type2) returnType {
	// 函數本身
	return value
}
```

簡單範例：
```go!
func add(x int, y int) int {
	return x + y
}
```

## 陣列 (Arrays)
陣列是具有"固定大小"且元素類型相同的序列，所以當你創建一個陣列時，需要指定其長度，並且這個長度在陣列的生命週期內不能改變。

例子：
```go!
var a [2]string
a[1] = "2"
fmt.Println(a[1])
```

## 切片 (Slices)
切片則是一個可動態變化大小的序列，且是基於陣列的結構，但提供了額外的功能和便利性，提供更加靈活的數據結構，切片的大小可以在運行時變化。

```go!
arr := [5]int{1, 2, 3, 4, 5} // 這是一個陣列
slice := arr[1:4]            // 這是一個切片並使用了擷取的方法，包括元素 2, 3, 4

//這是上次我們Day1的切片範例，其中[]沒有指定大小
fruits := []string{"banana", "apple", "coconut"}
```
以下是切片的常用方法：
```go!
dst := make([]int, 3, 5) // 長度為 3，容量為 5 的切片
fmt.Println(len(dst))    // 輸出: 3
fmt.Println(cap(dst))    // 輸出: 5
fmt.Println(dst)         // 輸出: [0 0 0]

```
也有其他Append、Copying之類的方法，礙於篇幅有興趣可以再去查一下。

## 回家作業
目標：實作一個簡單的程序來模擬處理和存儲學生的成績數據。
## 作業要求

### 常數定義
- 定義一個常數 `MaxStudents` 來表示最多學生數量，設定為 `5`。

### 函數實作
- 寫一個函數 `initializeGrades` 來初始化學生的成績。這個函數接受一個整數切片，並將所有成績初始化為一個隨機數（例如，0 到 100 之間）。
- 寫一個函數 `averageGrades` 計算並返回切片中所有成績的平均值。

### 陣列使用
- 使用一個陣列來存儲 `MaxStudents` 個學生的名字。

### 切片使用
- 使用一個切片來存儲相應的成績。

### 主函數實作
- 在 `main` 函數中，創建一個名字陣列和成績切片，使用 `initializeGrades` 初始化成績。
- 打印每個學生的名字和對應的成績。
- 計算並打印所有學生的平均成績。

相比於上一次的作業，這次比較能練習到一些，可以完整的熟悉上一篇的(for range)還有這一篇Slices使用~~

以下是我實作出來的作業：
```go!
package main

import (
	"fmt"
	"math/rand"
)

const MaxStudent = 5

func initializeGrades(grades []int) {
	for i := range grades {
		grades[i] = rand.Intn(100) + 1
	}
}

func averageGrades(grades []int) int {
	total := 0
	average := 0
	for _, grade := range grades {
		total += grade
	}
	average = total / len(grades)
	return average
}

func main() {
	stduent := [MaxStudent]string{"Goodman", "FrogBoy69", "Alice", "Bob", "Charlie"}
	grades := make([]int, MaxStudent)
	initializeGrades(grades)
	for i, name := range stduent {
		fmt.Printf("%s : %d\n", name, grades[i])
	}
	fmt.Printf("All student average score is %d", averageGrades(grades))
}

```