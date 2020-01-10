# Javascript順序

Javascript是『非同步語言』但也是 『**單執行緒**』

不管是「同步」或是「非同步」，在執行時一定都有順序之分。

只是「同步」任務的順序我們可以掌握，而「非同步」的任務會因執行時的各種狀況導致執行的順序不同。

**不管是哪一種都不會出現「同時」修改某個變數的情況喔。**

一般對同步、非同步的認知是同步即為在主執行緒執行，非同步則是開另一個執行緒進行處理，兩邊各自獨自運作以避免耗時過長造成阻塞。

_JS_ 同樣也有非同步的函數用來避免前端 _UI_ 因執行過久而造成頁面被 _LOCK_住

比如_ajax、settimeout_、各種event事件\(_click,onchange…_\)都是非同步事件

既然是單執行緒那不論同步與非同步都是在同一個線程中做處理，_JS_ 是如何處理非同步的需求來達成線程不阻塞呢？其實只是將非同步的程式丟到最後的最後再來處理而已。

當 _JS_ 偵測到非同步需求，會將其放到queue中直到所有程式碼都跑完了，_JS_會再回頭到queue中按順序將function 拉回來處理。

假設在執行過程中插入一個0秒立刻執行的_setTimeout_

```text
console.log(1)

setTimeout(function(){
console.log(2)
},0)

console.log(3)
-----輸出結果-------
1
3
2
```



```javascript
for( var i = 0; i < 5; i++ ) {
  window.setTimeout(function() {
    console.log(i);
  }, 1000);
}
```

我們知道像上面這樣的寫法，在執行 `window.setTimeout` 的時候， `i` 早已變成了 `5`，那麼為了保留每一次執行迴圈時那個「當下的」 `i`，我們可以用一個立即被呼叫的特殊函式將它包覆起來，然後將 `i` 作為參數傳入：

```javascript
for( var i = 0; i < 5; i++ ) {

  // 為了凸顯差異，我們將傳入後的參數改名為 x
  // 當然由於 scope 的不同，要繼續在內部沿用 i 也是可以的。
  (function(x){
    window.setTimeout(function() {
      console.log(x);
    }, 1000);
  })(i);

}
```

這時候你會發現，執行的結果就會是我們預期的 `0 1 2 3 4` 了。

但還是在一秒鐘後同時出現啊？

嘿嘿，相信聰明的你已經發現，由於 `for` 迴圈在一瞬間就跑完，等於那一瞬間它向 `window` 依序註冊了五次 timer，每個 timer 都只等待一秒鐘，當然同時出現囉。  
所以我們稍微修改一下：

```javascript
for( var i = 0; i < 5; i++ ) {

  (function(x){
    // 將原本的 1000 改成 1000 * x
    window.setTimeout(function() {
      console.log(x);
    }, 1000 * x);
  })(i);

}
```

像這樣，就可以依序印出我們要的結果囉！

* ES6 以後新增了 `let` 與 `const`，且改以 `{ }` 作為它的 Scope。  
  換句話說，將範例中的 `for` 改為 `let` 就可以做到保留 `i` 在執行迴圈當下的「值」的效果：

  ```text
  for( let i = 0; i < 5; i++ ) {
    window.setTimeout(function() {
      console.log(i);
    }, 1000);
  }
  ```

