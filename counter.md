# counter



```text
function counter(){
  var count = 0;

  function innerCounter(){
    return ++count;
  }

  return innerCounter;
}
```

直接呼叫 `counter()` 得到的結果會是回傳出來的 `innerCounter`，也就是那個內部的 function。

所以正確的做法是要先透過某個變數將 `counter()` 回傳的 function 存起來：

```text
var countFunc = counter();
```

然後再去呼叫 `countFunc()`

完整版：

```text
function counter(){
  var count = 0;

  function innerCounter(){
    return ++count;
  }

  return innerCounter;
}

var countFunc = counter();

console.log( countFunc() );   // 1
console.log( countFunc() );   // 2
console.log( countFunc() );   // 3
```

重點

拉到 `function counter(){ ... })` 的外面，由變數 `countFunc` 來保存 `counter()` 回傳的 `innerCounter`，同時也將裡面的 `count` 狀態給保存下來，這個時候 `count` 就不會每次都從 0 開始計算，這也是「閉包」Closure 最大的功能所在。

```text
var countFunc = counter();
```

[https://ithelp.ithome.com.tw/articles/10193009](https://ithelp.ithome.com.tw/articles/10193009)

