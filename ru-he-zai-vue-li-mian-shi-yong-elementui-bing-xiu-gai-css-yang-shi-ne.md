# 如何在Vue裡面使用Element-ui並修改CSS樣式呢？

大部分使用 Vue 開發公司內部後台系統的時候，通常大部分都會需要使用 Vue 的 Ui framework 來開發，雖然大部分的Ui framework 都有自己的 CSS 風格，我們不用再去寫 CSS，但是難免會有需要自己修改 UI 的 CSS的時候，但是每套 Ui framework 的修改方式又不太一樣，在這邊我將介紹這套 [Element-ui](http://element-cn.eleme.io/#/zh-CN) 是如何在Vue裡面去做 CSS 修改。

首先我們先掛載 [Element-ui](http://element-cn.eleme.io/#/zh-CN)

![](https://miro.medium.com/max/1764/1*86PwlfqCXcCCX-xHaaxQUA.png)

在這邊我們把所有的 element-ui 給掛載到全域，還有它的 css

這是我掛載 element-ui 的 component

![](https://miro.medium.com/max/2672/1*rtuneApnKJnsxgIRgMjyHw.png)

這就是我目前掛在畫面上的 UI 組件～

好啦！假設今天我要修改他的CSS樣是的話我要怎麼修改呢？  
其實有幾種做法啦！

**第一種**是你透過開法者工具去抓到 input 的 class，然後在 &lt;style&gt;&lt;/style&gt;去寫修改的CSS

![](https://miro.medium.com/max/2660/1*Zr6OvfuOl4okecdVtmjcJg.png)

```text
<style scoped>
   .el-input__inner{
       background-color: red;
   }
</style>
```

當你下了這個CSS的時候你會發現input組件背景顏色並沒有更改成紅色，你如果 google 的話很多答案都是叫你把 `scoped`給拿掉

```text
<style>
   .el-input__inner{
       background-color: red;
   }
</style>
```

![](https://miro.medium.com/max/2672/1*MdnlfTga3eTUxS4paxoisg.png)

你看成功了變成紅色了XD 但是這樣有優點也有缺點…

* 優點：快速修改，方便！
* 缺點：就跟上圖一樣，因為把 `scoped`給拿掉，所以 CSS 就變成共用了，這樣就會影響到其他組件的 CSS，這樣的做法非常糟糕，不建議使用…

**第二種**作法就是去 `node_modules`裡面把 element-ui 整包 CSS 給抓出來，然後放到自己指定的路徑！![](https://miro.medium.com/max/1332/1*Xxm_0_SKDs45CH8VTv5CXg.png)

這是 element-ui 的 CSS 在node\_modules裡面的位置

```text
// 原本的路徑 （就不要用 node_modules 這包）
import ‘element-ui/lib/theme-chalk/index.css’;

// 自己抓下來後指定的路徑 （改成自己抓下來這包）
import ‘./scss/theme-chalk/index.css’;
```

![](https://miro.medium.com/max/912/1*rBlUe69v5PyJ1vXRfhz1iw.png)

然後這樣就可以去修改自己抓下來這包，就可以改 element-ui 的 CSS 了。

* 優點：可以不會去跟其他 class 互相影響到，還有不是透過覆蓋，是透過直接修改，這樣整包的主導權就會在自己手上！
* 缺點：要花點時間去看完整包CSS架構，還有就是說如果 element-ui 有升級的話，對於自己抓下來修改CSS的部分可能會有問題，所以這方面可能要評估一下。

**第三種**做法的話是我個人覺得比較好得做法，不用把整個CSS包抓出改，也可以使用`scoped`的方式來寫CSS，其實可以使用 **深度作用選擇器** 來實作。

> 如果希望 scoped 樣式中的一個選擇器能夠更深入權重，例如影響子組件，你可以使用 `>>>`加深操作權重。

```text
<style scoped>
.a >>> .b { /* ... */ }
</style>這個代碼會編譯成
.a[data-v-f3f3eg9] .b { /* ... */ }
```

像 Sass 無法正確解析 `>>>`。這種情況下你可以使用 `/deep/`符號取代，這是`>>>`的別名，同樣可以正常編譯。

```text
<style lang=’scss’ scoped>
    .a /deep/ .b { /* ... */ }
</style>
```

我們回來看看我完成修改的畫面！

![](https://miro.medium.com/max/2704/1*iQIcmQj2AIfNDD-3E417fg.png)

我的階層是 .el-main &gt; .el-input &gt; .el-input\_\_inner，所以我這樣寫

![](https://miro.medium.com/max/1580/1*1s81tjGCaUko2gtODy9aeA.png)

**.el-input\_\_inner** _****_就是我最後要修改的 input，所以我最後使用了 `/deep/`去修改他的CSS樣式，是不是很簡單。

* 優點：可以不用一定要element-ui整包CSS抓出來放到自己的專案資料夾裡面去修改，還可以在vue的組件裡面使用`scoped`讓你的CSS可以組件化，不要去改到原本element-ui的程式碼，我個人比較喜歡這樣的做法。
* 缺點： 這樣就是透過覆蓋的方式來去覆寫 element-ui 的 CSS，效能的部分有待驗證，不過比起把整包 CSS 抓出來修改還要去擔心會不會因為升級或是改壞那包CSS的風險跟時間成本比起來，我覺得還好啦！

關於**深度作用選擇器的相關內容可以參考** [vue-loader 的文件](https://vue-loader.vuejs.org/zh/guide/scoped-css.html#%E6%B7%B7%E7%94%A8%E6%9C%AC%E5%9C%B0%E5%92%8C%E5%85%A8%E5%B1%80%E6%A0%B7%E5%BC%8F)裡面就有提到。  


