# (ChatGPT幫幫我) Day7 - 學習 Golang 的 Package、Workspace與一些命令

## 1. Go 包（Package）系統
在 Go 語言中，包是將相關 Go 源碼文件組織在一起的方式，主要用於模組化和重用代碼。每個 Go 文件都屬於一個包，且文件的第一行非註釋語句應聲明其所屬的包。

### 包的聲明和導入
#### 聲明包：
在 Go 文件的開頭使用 package 關鍵字聲明這個文件屬於哪個包。例如，package main 聲明了一個可執行程序的入口包。

#### 導入包：
使用 import 關鍵字導入需要的包。可以是標準庫中的包或者其他第三方包。例如：
```go!
import (
    "fmt"
    "os"
)
```
### 常用的 Go 標準庫包
以下是一些常用的 Go 標準庫包及其用途：
1. fmt：
用於格式化輸出和字符串相關的操作。
例子：打印輸出、格式化字符串等。
2. net/http：
提供 HTTP 客戶端和服務器實現。
例子：建立網頁服務器、處理 HTTP 請求。
3. os：
提供操作系統功能的接口，包括文件操作、環境變量、命令行參數等。
例子：讀寫文件、檢查和設置環境變量。
4. time：
提供時間和日期的功能。
例子：獲取當前時間、時間計算和轉換。
5. encoding/json：
用於處理 JSON 數據的編碼和解碼。
例子：解析 JSON 數據、生成 JSON 數據。
6. io/ioutil：
提供便捷的 I/O 操作函數，雖然從 Go 1.16 開始，推薦使用 io 和 os 包的新函數。
例子：讀取和寫入文件。
7. strings 和 bytes：
提供操作字符串和字節切片的函數。
例子：搜索、替換、比較字符串等。

另外像是之前使用過的第三方的 yaml 就需要另外下載：
```go!
go get gopkg.in/yaml.v2   // terminal 執行
import "gopkg.in/yaml.v2" // import 方式
```

## 2. Go 工作區（Workspace）

在 Go 模組出現之前，Go 使用一個稱為 GOPATH 的環境變數來定義工作區的位置。一個典型的 GOPATH 工作區包含以下三個目錄：

1. src：
存放 Go 源碼文件的地方。所有的 Go 源碼（包括自己的項目和第三方庫）都放在這個目錄下的具體路徑中。
2. pkg：
存放 Go 包的編譯產物（如 .a 檔案）。
3. bin：
存放編譯後的可執行檔。
實際例子~
假設你的 GOPATH 設定為 /home/user/go，則你的工作區目錄結構就會看起來像這樣：
```go!
/home/user/go/
├── bin/
├── pkg/
└── src/
    └── github.com/
        └── example/
            └── myproject/
                ├── main.go
                └── helper.go
```
在這個例子中：

#### main.go 和 helper.go 是你的項目文件。
#### 你的項目位於 GitHub 上的 example/myproject 路徑下。

## Go 模塊的引入
你可以直接任何地方創建了一個新的 Go 項目。
實際例子：
```go!
mkdir mymodule
cd mymodule
go mod init github.com/example/mymodule
```
就會產生出這樣的項目架構：
```go!
/mymodule/
├── go.mod
└── main.go
```
就會在 mymodule 目錄下創建一個 go.mod 的文件

## 3. Go 的基本命令

#### go build
這個命令可以將所有相關的 .go 文件編譯成一個執行檔。

#### go run
基本運行 Go 程式，基本用在程式開發階段~
```go!
go run main.go
```
其他常用命令

#### go test 
這個命令會自動找到所有標有 *_test.go 的檔案，並執行這些檔案中定義的測試函式。
測試特定的函式：
使用 -run 參數指定要執行的測試函式的名稱。
```go!
go test -run 函式名稱
```
#### go get
`go get `套件路徑@版本，上面下載 yaml 包就是個例子~
#### go mod tidy

go mod tidy 命令用於清理模組中未使用的 Library。


## 回家作業
今天的東西比較偏觀念，暫時沒有回家作業，最近因為本人在刷刷 python 的題目準備面試，可能也會暫時停止這邊的活動一兩天，非常抱歉。
