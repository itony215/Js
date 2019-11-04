---
description: 將相同的輸入丟入，永遠都會回傳相同的輸出，並且不對任何該函數以外的任何作用域產生影響。
---

# Pure Function

Pure Function 的定義十分簡單（所以更適合其字面語意）：

{% hint style="info" %}
將相同的輸入丟入，永遠都會回傳相同的輸出，並且不對任何該函數以外的任何作用域產生影響。
{% endhint %}

以上定義可以分成兩個部分，一是相同輸入將獲得相同輸出。對於一個函數 ，輸入便是指其參數（Parameter），而輸出自然就是其回傳值。

舉個例子，實作一個加法函數，其會收進二個參數，並且回傳其總和。在任何時候，給定1 與 1 則會回傳2，而給定8 與 7 則將會獲得15。這樣的結果不會受到其他作用域（scope）影響，無論外部做了什麼，此函數依舊遵照此模式。

定義的第二部分，延續上一段，此函數不但不會受到其他作用域的影響，也不會影響其他作用域的值，也就是沒有副作用（Side Effect）。

以下一個簡單的範例碼：

```javascript
// impure function
let intercept = 2
function math(x) {
  return (3 * x) + intercept
}
const result = math(4) // 14
// pure function
function math(itr) {
  return function(x) {
    return (3 * x) + itr
  }
}
const result = math(2)(4) // 14
```

小結，Pure Function 就是一個誰找他他就回一樣的話，他作的事情不受人影響，也不影響任何人。套個行話，Pure Function就是個_邊緣人_。

## Pure Function 對開發有什麼益處 <a id="50f6"></a>

### 有利重構（Refactor） <a id="2253"></a>

由於Pure Function 同輸入同輸出的特性，讓它具備高**可預測性**，這對於重構你的程式碼是十分有利的。在開發過程中，由於各種可或不可抗拒的因素，可能會求快而先寫出一個版本的程式碼，接下來或許會排時間進行重構來達到穩定性、可擴充性等更高品質的水準。

[Workaround 雖可恥但有用](https://weekly.codetengu.com/issues/72)。我自己在初期開發時，儘管預先知道是個簡單的初版，仍會盡可能將內容抽成各個函數，並確保其為Pure Function 。將來替換時，僅需要理解該函數的作用，便能合理且快速地重構它。

反之若函數是個Impure Function，除了理解這個函數變不容易外，更可怕的是改動他可能會影響到其他地方；讓重構需要經過更長遠的計畫、花更多的時間，才能夠進行重構。如此一來，快速開發所產生的債則會多上許多倍。

### 方便測試 <a id="cfb4"></a>

上一段提到了Pure Function 的高可預測性，事實上這點就已經滿足**方便測試**的條件。一個容易預測的函數，會很容易想到Test Case ，以及對應的actual value 與expect value 。

然而Pure Function 的高測試性不僅限於此。由於Pure Function 不受到其他作用域影響，只有輸入值才會導致結果的改變。換句話說，我們將不必再費心去準備許多環境就能做到測試的目的。在同一段測試區塊中，也大幅減少事後還原的動作。

### 易與其他函數組合（Compose） <a id="5ad7"></a>

在Functional Programming 中，有一個運用數學上結合律（associative laws）的組合技巧（function compose）。

```javascript
function someMath(n) {
  return 6 + (5 * n)
}function g(n) {
  return n * 5
}function f(n) {
  return n + 6
}const z = someMath(x)// 等同於
const y = g(x)
const z = f(y)// 能利用組合率
z = f(g(x))
```

這樣的技巧是將資料（上例的x）視作進入一道管線（pipeline）。如果我們將對資料的處理流程梳理得更加明確時，能夠進一步組合更多數量的函數。以Elixir 的pipe operator來看更顯直觀：

```text
iex> "Hello World"
  |> String.upcase
  |> String.split
  |> Enum.map(fn x -> x <> "oo" end)["HELLOoo", "WORLDoo"]
```

對我而言，除了看起來酷炫以外，函數組合的優勢在於讓多個只有一些小功能，但可靠的函數組合成一個值得信賴的「管線」。而這管線中每一道閥，都建議是個Pure Function 。除了能便於達到所謂的「可靠」外（前文所敘之高預期性、高測試性），更重要的是我們並不希望管線中的函數會在不同的情境、時間下會獲得不同的結果，這會導致管線結構的不單純。進一步說，由Pure Function 組合出的管線（函數組），自然也是個Pure Function。

強化Pure Function 概念的一大好處是讓開發者更能夠不拘束地使用**函數組合**的技巧。

### 並行運作 <a id="ced0"></a>

在開多個執行緒來讓程式碼並行運作時，有一個可能會遇見的問題是Race Condition 。產生Race Condition 的其中一個可能，發生於某a 執行緒改變了某一項外在條件，導致b 甚至更多執行緒的結果不合預期，甚至發生錯誤。

由於Pure Function 不產生副作用也不受到外在影響的特性，不必擔心各個執行緒會影響他者運行的結果。我們能夠輕易地讓Pure Function 並行運作，不必擔憂產生Race Condition 的情況。

### 利於建快取（Cache） <a id="e3b6"></a>

Pure Function 的結果只取決於傳進的Parameter ，因此若結合閉包（closure）的作法，能夠妥善建立快取機制。判斷Parameter 是否是已經給進過的，若是則提取快取中的值，反之則加入快取並重新計算一個數值回傳。範例如下：

```javascript
function cacheMath(n){
  const cache = {}
  return function(){
    if (n in cache) return cache[n]    const result = doSomeComplexMathMethod(n)
    cache[n] = result
    return result
  }
}
```

可想而知的是，若回傳值除了受到n 亦會受到其他因素影響，由於回傳值的可預測性降低了，我們將無法有效的建立該快取制度。

### 專注於特定情境（Context） <a id="24ee"></a>

最後這一點就相對開發時的心態。在理解Pure Function後，抱持著每個函數都盡可能達成其概念前提，這有助於開發者專注於各個情境。畢竟，真的很難在含有多個目的、情境的前提下，還能夠滿足Pure Function 的條件。

而這樣的專注能讓開發者較為不易陷入一個需求所導致的過度複雜思考，在起始時分離需求成不同目標，將目標轉換為不同的小函數，一一擊破。feature 一二三，寫code 多簡單。

