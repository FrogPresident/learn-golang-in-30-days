# (ChatGPT幫幫我) Day5 - 學習 Golang 的 Channel
第5天原本想就直接來介紹 Goroutine 和 Channel，直接搶著做第11-12天的內容XD，不過因為一開始看真的花了不少時間理解整個邏輯，所以我才打算分開介紹。
另外這篇寫的算是相對具體 https://ithelp.ithome.com.tw/articles/10212068 ，我會整合這篇與個人觀點來解釋，Channel 以及 Goroutine 的。
這篇我會先優先講解 Channel 的部分，明天我會再把 Goroutine 的完整概念補上。

## Channel

channel 是一種用於在不同 goroutine 之間進行通信和同步的數據結構，gorountine 像是其他語言的 thread，這是什麼意思呢?我用比較簡單的例子來解釋一下，channel 可以譬喻為一條水管，goroutine 則像是使用這條水管的不同家庭。如果一家想要給另一家送水（或者任何東西），他們會用這條水管來傳送。這樣的設計讓各家可以安全地、順利地交換東西，不會互相干擾或弄錯。

這個“水管”——也就是 channel，確保了在多個 goroutine（多個家庭）之間傳遞信息或數據（水）的時候，這些信息不會丟失，也不會被錯誤地接收。所以 channel 是幫助不同的 goroutine 有效並安全地通信的工具。

而這個Channel呢可以把他想像成管線，你可以將數值推入，也能進行拉出，Channel 也需要等"另一端"完成推入或是拉出動作後才會進繼續往下處理，可以替代掉lock, unlock的方法。

### Channel 的基本概念

####  創建 Channel

可以使用 make 函數創建一個 channel：
```go!
ch := make(chan int) // 創建一個未緩衝的 int 類型通道
```
待會會解釋緩衝與未緩衝的差別。

#### 發送和接收數據
通過 channel 你可以在 goroutine 之間發送和接收數據，這些操作預設是阻塞的：
```go!
ch <- 10 // 發送數據到 channel
value := <-ch // 從 channel 接收數據
```
結果會是這樣的：
```go!
fatal error: all goroutines are asleep - deadlock!
```
為什麼會這樣呢?容我以上面的例子來解釋一下，想像你有一條水管（channel），這條水管的一頭是你（main goroutine），另一頭是另一個 goroutine。當你想通過這條水管發送一桶水（數據）時，你必須等待另一頭有人（另一個 goroutine）準備好接收這桶水。如果沒有人在那裡等著接收，你就必須站在那裡持續等待，直到有人來接手你的水桶。這就是所謂的「阻塞」：你不能繼續做其他事情，因為你必須等待這個操作完成。


Buffered Channel 的宣告會在第二個參數中定義 buffer 的長度，它只會在 Buffered 中資料填滿以後才會阻塞造成等待。
所以這邊如果如果以有緩衝的方式來創建一個channel的話：
```go!
// 第2個參數： 50 為緩衝容量大小
ch := make(chan int, 50)
ch  <- 10
value := ch
```
以上例子他會在第51個資料推入後，channel才會阻塞。

#### 關閉 Channel
可以使用 close() 函數來關閉一個 channel，關閉後不能再向其中發送數據，但仍可以接收數據：

```go!
close(ch)
```


這次就不出回家作業了，這個部分分成兩個part，我感覺我這部分也沒有到很熟，我會在問人來補齊這方面相關的知識點。

