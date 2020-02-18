# Lodash - 使用debounce做事件延遲

想做一個事件延遲3秒再執行才對，而且如果按鈕連續點擊，會以最後一次點擊完3秒後才執行事件。

## 解決方法 <a id="&#x89E3;&#x6C7A;&#x65B9;&#x6CD5;"></a>

這個其實是我在學Vue.js的過程碰巧看到的函式庫Lodash，它的debounce方法可以延遲執行。

> 註：debounce可以翻防抖動或防反動都行

## 程式實作 <a id="&#x7A0B;&#x5F0F;&#x5BE6;&#x4F5C;"></a>

### 引用Lodash <a id="&#x5F15;&#x7528;lodash"></a>

引入CDN

```markup
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.10/lodash.min.js"></script>
```

NPM安裝

```bash
	$ npm i --save lodash
```

```javascript
var _ = require('lodash');
```

### 建立HTML物件 <a id="&#x5EFA;&#x7ACB;html&#x7269;&#x4EF6;"></a>

先做出一個`Button`和一個訊息`DIV`

```markup
<button id="showBtn" type="button">顯示訊息</button>
<div id="Message"></div>
```

### 建立Debounce事件 <a id="&#x5EFA;&#x7ACB;debounce&#x4E8B;&#x4EF6;"></a>

寫一個函數顯示訊息，並使用`_.debounce()`延遲執行訊息消失事件

```javascript
var showBtn = document.getElementById('showBtn');
var msg = document.getElementById('Message');
// 消失事件
var hideMsg = function(){
    msg.innerHTML = '';
}
// 延遲一秒消失
var debounce = _.debounce(hideMsg, 1000);
```

### 建立Button Click <a id="&#x5EFA;&#x7ACB;button-click"></a>

```javascript
// 點擊事件
showBtn.onclick = function(){
    msg.innerHTML = '點擊成功';
    debounce();
};
```

> 註：如果\_.debounce\(\)要放在funtion或變數裡，不然會沒效果

### Demo <a id="demo"></a>

點完一次一秒會消失，如果一直點的話不會消失，直到停止點擊一秒後才消失。

