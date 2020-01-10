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



```text
var countFunc = counter();
```

[https://ithelp.ithome.com.tw/articles/10193009](https://ithelp.ithome.com.tw/articles/10193009)

