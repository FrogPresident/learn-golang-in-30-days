# (ChatGPT幫幫我) Day4 - 學習 Golang 的映射(Map)再搭配一點類型轉換與介面(Interface)

今天來講講Map、類型轉換和Interface，但會出一題比較進階的題目是由教授(susautw)所給的題目，我也會來多多講解一下這題。

## 介面(Interface)
在 Go 語言中，介面是一種規範，定義了一組方法，但並不實作這些方法。Go 語言中的介面特點是「隱式實作」，意即不需要在類型定義中顯示聲明實作哪個介面，只要一個類型擁有介面中所有的方法，就認為它實作了該介面。

不過當定義一個結構體或任何類型時，如果你打算讓它實作某個介面，那麼必須實作介面中聲明的所有方法。這種方式增加了 Go 程式的類型安全性，保證了程式的行為一致性。

這個範例展示了如何透過介面來實作多型性，我們定義一個 Painter 介面和兩個實作了這個介面的結構體：`Artist` 和 `Robot`。

```go!
package main

import "fmt"

// Painter 介面
type Painter interface {
    Paint() string
}

// Artist 結構體實作了 Painter 介面
type Artist struct{}

func (a Artist) Paint() string {
    return "Artist is painting"
}

// Robot 結構體也實作了 Painter 介面
type Robot struct{}

func (r Robot) Paint() string {
    return "Robot is painting"
}

// ShowArt 函數接受一個 Painter 介面的參數
func ShowArt(p Painter) {
    fmt.Println(p.Paint())
}

func main() {
    artist := Artist{}
    robot := Robot{}

    ShowArt(artist)
    ShowArt(robot)
}
```

## 映射（Maps）
在 Go 語言中是一種非常有用的數據結構，類似於其他語言中的Dictionary或Hash Tabel。它是一個存儲鍵-值對的無序集合，其中每個鍵都是唯一的，並且每個Keyy映射到一個特定的Value。

#### 聲明和初始化

```go!
// 使用 make 函數創建映射
var map1 = make(map[string]int)

// 使用映射字面量初始化映射
map2 := map[string]int{"apple": 5, "banana": 2}

// 可以這樣拿key對應的value
fmt.Println(map2["apple"]) 

// 當然也可以修改修改映射中的元素
map1["banana"] = 10
```

如果在搜找時對應的Key不存在，則返回值類型的零值。
下面是語法測試：
```go!
count, ok := b["apple"]
if ok {
	fmt.Printf("apple count：%d", count)

} else {
	fmt.Printf("There is no apple.")
}
```
#### 删除元素
使用內建的 delete 函數可以從映射中删除Key及其對應的Value。
```go!
delete(b, "apple")
```
也可以結合我們之前用的`struct`，也來把它放進去唄~
```go!
type Employee struct {
	Department string
	Name       string
}

func main() {
	department := map[string]Employee{
		"0001": {Department: "IT", Name: "FrogBoy69"},
		"6969": {Department: "AI", Name: "sus"},
	}
	for id, employee := range department {
		// 這邊的id會直接尋找相對應的key值
		fmt.Println(id, employee)
	}
}
```
輸出：
```go!
6969 {AI sus}
0001 {IT FrogBoy69}
```

## 基本的類型轉換
基本的類型轉換語法是：T(x)，其中 T 是你想要轉換成的類型，x 是被轉換的變數。
```go!
var i int = 42
var f float64 = float64(i)  // 將整數轉換為浮點數
var i2 int = int(f)         // 將浮點數轉換為整數
var s string = strconv.Itoa(i)  // 整數轉字符串
var i2, _ = strconv.Atoi(s)    // 字符串轉整數
fmt.Println(s)  // 輸出: "65"
fmt.Println(i2) // 輸出: 65

//當然也有string轉float的
s2 := fmt.Sprintf("%.2f", f)
```

## 回家作業
這次回家作業請大家寫一個程式，能夠接住外面的json檔，並將這個json的內容產生出相對應的yaml的格式

### 基本認知
#### 安裝 YAML 套件
在Terminal中輸入：
```go!
go get gopkg.in/yaml.v2
```
若要看自己有哪些套件可以使用
```go!
go list -m all
```

#### os.ReadFile()
```go!
jsonData, err := os.ReadFile("data.json")
```
#### os.WriteFile()

```go!
// 第三個參數 0644 是文件的權限設置，表示文件擁有者具有讀寫權限，而組和其他用戶具有讀權限。
err = os.WriteFile("output.yaml", yamlData, 0644)
```
#### 解析 JSON 數據

此步驟使用 encoding/json 包的 Unmarshal 函數將讀取的 JSON 數據解析到一個空的 interface{} 中。這允許處理任意結構的 JSON 數據。

#### 思路點：
1. 使用 interface{} 可以接受任何 JSON 結構，無論是對象、數組、字串、數字還是布林值。這在你不確定 JSON 結構或者需要處理多種不同結構的情況下非常有用。
2. json.Unmarshal 需要一個字節切片和一個指向目標變數的指針。這裡直接將解析結果存儲在 data 變數中，該變數是一個空接口。
3. yaml.Marshal 同樣接受任意類型的變數，這裡傳入的是一個空interface。它會根據變數的實際類型來進行序列化。

```go!
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "os"
    "gopkg.in/yaml.v2"
)

func main() {
    // 假設有一個 JSON 文件名為 "data.json"
    jsonData, err := os.ReadFile("data.json")
    if err != nil {
        log.Fatalf("error reading file: %v", err)
    }

    // 將 JSON 數據解析到一個 interface{} 中
    var data interface{}
    err = json.Unmarshal(jsonData, &data)
    if err != nil {
        log.Fatalf("error unmarshalling json: %v", err)
    }

    // 將數據轉換為 YAML 格式
    yamlData, err := yaml.Marshal(data)
    if err != nil {
        log.Fatalf("error marshalling yaml: %v", err)
    }

    // 將 YAML 數據寫入文件
    err = os.WriteFile("output.yaml", yamlData, 0644)
    if err != nil {
        log.Fatalf("error writing yaml to file: %v", err)
    }

    fmt.Println("YAML data has been written to 'output.yaml'")
}

```




