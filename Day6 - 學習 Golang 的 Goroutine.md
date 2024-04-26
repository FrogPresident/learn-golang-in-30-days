# (ChatGPT幫幫我) Day6 - 學習 Golang 的 Goroutine

## 什麼是 Goroutine？

Goroutine 是 Go 語言中實現並行的基本單元，它比傳統的線程更輕量，因此在使用時占用的資源更少，創建和銷毀的成本也更低。每個 Go 程序至少有一個 goroutine —— 主 goroutine（運行 main 函數的那個）。當啟動一個新的 goroutine 時，它將與 main 的 goroutine 平行運行。

##  如何創建 Goroutine？

在 Go 中創建一個 goroutine 非常簡單，只需在函數調用前加上關鍵字 go。例如：

```go!
go func() {
    fmt.Println("Running in a goroutine")
}()
```
以 go 開頭的函式叫用可以使 function 跑在另一個 Goroutine 上。
##  Goroutine 的工作機制

Goroutines 在 Go 的運行時系統中調度，這允許成千上萬的 goroutine 可以在幾個 OS 線程上高效運行。這種模型稱為 M:N 調度，意味著它可以在 M 個 goroutine 和 N 個操作系統線程之間進行映射，這有助於有效利用多核心處理器。

### 單執行緒
```go!
func say(s string) {

	for i := 0; i < 5; i++ {
		fmt.Println(s)
		time.Sleep(1 * time.Second)
	}
}

func main() {
	say("hello")
	say("world")
}
```
輸出：
```go!
hello
hello
hello
hello
hello
world
world
world
world
world
```
### 多執行緒
一次就能執行與CPU數相等的Goroutine了~
```go!
func say(s string) {

	for i := 0; i < 5; i++ {
		fmt.Println(s)
		time.Sleep(1 * time.Second)
	}
}

func main() {
	go say("hello")
	say("world")
}
```
這邊要time.Sleep是避免他開啟新的Goroutine時，main()的say("world")已經先處理完了
輸出：
```go!
world
hello
hello
world
world
hello
hello
world
world
hello
```

## 等待(Wait)

當我們談到 goroutines 的“等待”，通常是指在一個或多個 goroutines 完成它們的任務之前，讓一個或多個其他的 goroutines 暫停執行。這種等待是必要的，因為在許多場景中，一些操作依賴於其他操作的結果。

### 1.Time.Sleep
```go!
time.Sleep(1 * time.Second)
```
休息指定時間可能會比 Goroutine 需要執行的時間長或短，太長會耗費多餘的時間，太短會使其他 Goroutine 無法完成

### 2.使用 sync.WaitGroup

sync.WaitGroup 是用來等待一組 goroutines 完成的標準方法。WaitGroup 具有三個主要的方法：`Add(delta int)`、`Done() `和 `Wait()`。

#### Add(delta int)：
設定需要等待的 goroutines 數量。
#### Done()：
每當一個 goroutine 完成其任務時，它呼叫 Done()，這會將等待組的計數減一。
#### Wait()：
在所有 goroutines 呼叫 Done()，即計數變為零之前，Wait() 會阻塞。
#### defer()：
是go中一種延遲調用機制，defer後面的函數只有在當前函數執行完畢後才能執行，將延遲的語句按defer的逆序進行執行，也就是說先被defer的語句最後被執行，最後被defer的語句，最先被執行，通常用於釋放資源。

這裡有一個使用 WaitGroup 的例子：
```go!
package main

import (
	"fmt"
	"sync"
)

func worker(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	fmt.Printf("Worker %d starting\n", id)
	// 做一些工作...
	fmt.Printf("Worker %d done\n", id)
}

func main() {
	var wg sync.WaitGroup
	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}
	wg.Wait()  // 等待所有worker完成
	fmt.Println("All workers done")
}
```
輸出：
```go!
Worker 1 starting
Worker 1 done    
Worker 4 starting
Worker 4 done    
Worker 2 starting
Worker 2 done    
Worker 5 starting
Worker 5 done    
Worker 3 starting
Worker 3 done    
All workers done 
```

### 3.Channel
通道（Channels）不僅用於 goroutines 之間的通信，也可以用於同步。一個 goroutine 可以通過向通道發送數據來通知事件的完成，而其他 goroutine 可以等待（阻塞）直到從通道接收到數據。

因其阻塞的特性，使其可以當作等待 Goroutine 的方法：
```go!
func worker(done chan bool) {
    fmt.Print("Working...")
    time.Sleep(time.Second)
    fmt.Println("done")

    // 發送一個值來通知工作已完成
    done <- true
}

func main() {
    done := make(chan bool)
    go worker(done)

    // 等待工作完成的通知
    <-done
}
```
#### 1.創建 Channel：
```go!
done := make(chan bool)
```
這行代碼創建了一個布林型的channel。

#### 2.啟動 worker Goroutine：
```go!
go worker(done)
```
這行代碼在新的 goroutine 中執行 worker 函數，並將 done channel 傳給它。worker 函數將進行一些工作，然後向 done channel 發送一個信號（true），表示工作已完成。


#### 3.接收完成信號：

```go!
<-done
```
在 main 函數中，這行代碼從 done channel 接收值。這個操作會阻塞 main goroutine，直到有值可被接收。當 worker goroutine 發送了 true 之後，main goroutine 從 done 接收這個值並繼續執行。接收操作本身不會將值指派給任何變量，因為這裡只關心信號的到來，而不在乎具體的值，當然你也可以把他輸出出來結果會是`true`。


今天這些就是 Goroutine 的介紹了，補齊昨天只有介紹 Channel 的部分，這次的內容比較多，尤其是sync.WaitGroup的程式碼可能需要多練習一下，真的花了不少時間在看Goroutine....以上是Day6的全部內容，感謝各位。