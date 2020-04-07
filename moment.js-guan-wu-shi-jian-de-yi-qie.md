# moment.js - 關於時間的一切

{% embed url="https://ithelp.ithome.com.tw/articles/10208995" %}

在前端開發時經常會遇到需要處理日期或是時間的問題，而在 JavaScript 撰寫上經常都會需要用很多函數來處理與轉換，因此推薦大家使用 moment.js。moment.js 是一切和時間有關的的解析、轉換、設置、格式化日期的 JavaScript 函式庫，透過 moment.js 就可以簡化使用JavaScript 的時間處理，更快速的達到與時間相關的效果呈現。

網站：[https://momentjs.com/](https://momentjs.com/)

### 安裝

安裝 moment.js 有很多方法，像是使用 Node.js、cdn 等等的方法，以下就列舉兩個，更多的方法請看：[https://momentjs.com/docs/\#/use-it/](https://momentjs.com/docs/#/use-it/)

#### Node.js

```text
npm install moment
```

```text
var moment = require('moment');
moment().format();
```

#### cdn

```text
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.js"></script>
```

```text
<script src="moment.js"></script>
```

安裝完畢後我們馬上就來一一介紹它的各種實用功能吧！這邊會以比較常實際使用到的樣式來做為範例介紹。

### 範例

#### Unix 轉換

首先第一個要介紹的是很被使用到的 Unix timestamp，要轉換 timestamp 並美觀的輸出，一般做法都是一個一個 get，相對來說就會比較複雜也花時間，而運用 moment.js 根本可以一行解決呀！

由現在時間轉 Timestamp：

```text
moment().valueOf(); //1541411221345
```

timestamp 轉時間，再加上自訂的時間格式：

```text
moment(Timestamp).format('MM-DD-YYYY');
```

例如：

```text
moment(1541411221345) // Mon Nov 05 2018 17:47:01 GMT+0800
moment(1541411221345).format('YYYY-MM-DD') // 2018-11-05
moment(1541411221345).format('YYYY-MM-DDTHH:mm:ss.SSS') // 2018-11-05T17:47:01.345
```

#### 驗證時間

驗證時間可以用在確認時間是否為有效的時間，或是是否有在設定的時間以內

第一個例子就是指陣列中的日期是否為有效時間，要特別注意這邊陣列的月份是以數列的方式表示，舉例來說：2018/1/1 也就是\[2018, 0, 1\]，只有月份會使用 0-11 的方式顯示，年份和日期仍然是依照平常的顯示

```text
moment([2015, 25, 35]).format(); // Invalid date
```

這邊再舉一個常用的例子，驗證是否有在一段時間以內或是在截止日以前。這個方法通常會用在投稿的部分，驗證是否有在時間內提交或是在截止日期前繳交。

```text
moment('要驗證的日期').isBetween('起始日', '截止日'); // true or false
moment('2018-11-02').isBetween('2018-11-01', '2018-11-13'); // true
moment('2010-10-20').isBefore('2010-10-19'); // false
```

#### 運算

moment.js 可以運算時間，像是取出時間的最大值，或是增加天數。

第一個例子是時間的增減，日期、月份等等的都可以加減，甚至還可以用鏈式寫法來加上不同的屬性。

**現在的日期加上七天**

```text
moment().add(7, 'days'); // Tue Nov 13 2018 17:25:31 GMT+0800
```

**現在的日期加上七天再加上一個月（鏈式寫法）**

```text
moment().add(7, 'days').add(1, 'months'); // Thu Dec 13 2018 17:26:28 GMT+0800
```

第二個例子是找出這個陣列中，日期最大的，這邊要注意的是，日期最大的也就是最接近現在的，相對於年齡來說就是最小的。不只有 max 可以使用，min 也同樣可以被使用

```text
var friends = [{name: 'Angel', birthday: '11.12.1996'}, 
               {name: 'Eric' , birthday: '12.12.1989'}, 
               {name: 'Mark' , birthday: '5.01.1993'}]
var friendsBirthDays = friends.map(function(friend){
    return moment(friend.birthday, 'DD.MM.YYYY');
});
moment.max(friendsBirthDays).format('DD.MM.YYYY'); // 11.12.1996
```

#### 顯示

**特殊顯示格式**

除了常見的 '13.11.2018' , '2018-11-13' , 'Tue Nov 13 2018' 這幾種日期的格式轉換，透過 moment.js 還有幾種特別的格式顯示，每個項目都來舉個例子吧！

**年**：  
YY 是用來表示年份的最後的兩碼，例如：

```text
moment().format("YY"); // 18
```

**月**：  
MMMM 是顯示全英文的月份

```text
moment().format("MMMM"); // November
```

**日**：

DDDD 是表示現在是一年中的第幾天

```text
moment().format("DDDD"); // 310
```

**時間**：  
hA 是用 PM , AM 來表示時間

```text
moment().format("hA"); // 5PM
```

在運用時間表示中最常使用的 Timestamp，也可以用 X 來表示，

```text
moment().format("X"); // 1541497385
```

如果要以毫秒為計算的 Timestamp，和上述差別只差在大小寫，也就是可以用 x 來表示，這個效果也和一開始介紹的moment\(\).valueOf\(\) 是一樣的，都是取得毫秒制的 Timestamp

```text
moment().format("x"); // 1541497571862
```

**相對時間**

顯示除了可以快速的顯示出特殊的日期格式以外，還可以直接讀取相對時間

**formNow\(\)**  
formNow\(\) 也就是可以知道距離現在是多少時間  
例如：

```text
moment("20111031").fromNow(); // 7 years ago
moment().startOf('day').fromNow(); // 13 hours ago
```

**from\(\), to\(\)**  
如果你得到某個時間到另一個時間之間的差，就可以使用 from\(\) 的方法，而相似的方法還也 to\(\)，to\(\) 是用來計算到較後面的天數剩下多少天，舉例如下

```text
var a = moment([2018, 0, 28]);
var b = moment([2018, 0, 29]);

a.from(b); // a day ago
a.to(b); // in a day
```

除了可以用參數的方式，還可以直接將日期代入：

```text
a.to([2007, 0, 29]);         // "in a day"
a.to(new Date(2007, 0, 29)); // "in a day"
```

#### 查詢

查詢可以用來了解時間的先後，或是是否在一個區間，需要注意的地方是在如果設定時間後有加上 year 參數的話會有不同的結果，這邊也有列出比較結果

**isBefore 是用來查詢是否在早於後面的時間**

```text
moment('2010-10-20').isBefore('2010-10-21') // true
moment('2010-10-20').isBefore('2010-12-31', 'year') // false
```

**isSame 是用來查詢是否和後面的時間相等**

```text
moment('2010-10-20').isSame('2009-12-31', 'year') // false
moment('2010-10-20').isSame('2010-01-01', 'year') // true
```

**isAfter 是用來查詢是否和晚於後面的時間**

```text
moment('2010-10-20').isAfter('2010-10-19') // true
moment('2010-10-20').isAfter('2010-01-01', 'year') // false
```

**isSame 是用來查詢是否在設定的時間範圍內**

```text
moment('2010-10-20').isBetween('2010-10-19', '2010-10-25') // true
moment('2010-10-20').isBetween('2010-01-01', '2012-01-01', 'year') // false
```

#### 更換語言

如果使用 CDN 預設就是 en，如果要更改為中文顯示可以先定義切換的 locale 模式  
例如：

```text
moment.locale('zh-tw', {
      months: '一月_二月_三月_四月_五月_六月_七月_八月_九月_十月_十一月_十二月'.split('_'),
      monthsShort: '1月_2月_3月_4月_5月_6月_7月_8月_9月_10月_11月_12月'.split('_'),
      weekdays: '星期日_星期一_星期二_星期三_星期四_星期五_星期六'.split('_'),
      weekdaysShort: '周日_周一_周二_周三_周四_周五_周六'.split('_'),
      weekdaysMin: '日_一_二_三_四_五_六'.split('_'),
      longDateFormat: {
        LT: 'Ah點mm分',
        LTS: 'Ah點m分s秒',
        L: 'YYYY-MM-DD',
        LL: 'YYYY年MMMD日',
        LLL: 'YYYY年MMMD日Ah點mm分',
        LLLL: 'YYYY年MMMD日ddddAh點mm分',
        l: 'YYYY-MM-DD',
        ll: 'YYYY年MMMD日',
        lll: 'YYYY年MMMD日Ah點mm分',
        llll: 'YYYY年MMMD日ddddAh點mm分'
      },
      meridiemParse: /凌晨|早上|上午|中午|下午|晚上/,
      meridiemHour: function (h, meridiem) {
        let hour = h;
        if (hour === 12) {
          hour = 0;
        }
        if (meridiem === '凌晨' || meridiem === '早上' ||
          meridiem === '上午') {
          return hour;
        } else if (meridiem === '下午' || meridiem === '晚上') {
          return hour + 12;
        } else {
          // '中午'
          return hour >= 11 ? hour : hour + 12;
        }
      },
      meridiem: function (hour, minute, isLower) {
        const hm = hour * 100 + minute;
        if (hm < 600) {
          return '凌晨';
        } else if (hm < 900) {
          return '早上';
        } else if (hm < 1130) {
          return '上午';
        } else if (hm < 1230) {
          return '中午';
        } else if (hm < 1800) {
          return '下午';
        } else {
          return '晚上';
        }
      },
      calendar: {
        sameDay: function () {
          return this.minutes() === 0 ? '[今天]Ah[點整]' : '[今天]LT';
        },
        nextDay: function () {
          return this.minutes() === 0 ? '[明天]Ah[點整]' : '[明天]LT';
        },
        lastDay: function () {
          return this.minutes() === 0 ? '[昨天]Ah[點整]' : '[昨天]LT';
        },
        nextWeek: function () {
          let startOfWeek, prefix;
          startOfWeek = moment().startOf('week');
          prefix = this.diff(startOfWeek, 'days') >= 7 ? '[下]' : '[本]';
          return this.minutes() === 0 ? prefix + 'dddA點整' : prefix + 'dddAh點mm';
        },
        lastWeek: function () {
          let startOfWeek, prefix;
          startOfWeek = moment().startOf('week');
          prefix = this.unix() < startOfWeek.unix() ? '[上]' : '[本]';
          return this.minutes() === 0 ? prefix + 'dddAh點整' : prefix + 'dddAh點mm';
        },
        sameElse: 'LL'
      },
      ordinalParse: /\d{1,2}(日|月|周)/,
      ordinal: function (number, period) {
        switch (period) {
          case 'd':
          case 'D':
          case 'DDD':
            return number + '日';
          case 'M':
            return number + '月';
          case 'w':
          case 'W':
            return number + '周';
          default:
            return number;
        }
      },
      relativeTime: {
        future: '%s内',
        past: '%s前',
        s: '幾秒',
        m: '1 分鐘',
        mm: '%d 分鐘',
        h: '1 小時',
        hh: '%d 小時',
        d: '1 天',
        dd: '%d 天',
        M: '1 個月',
        MM: '%d 个月',
        y: '1 年',
        yy: '%d 年'
      },
      week: {
        // GB/T 7408-1994《数据元和交换格式·信息交换·日期和时间表示法》与ISO 8601:1988等效
        dow: 1, // Monday is the first day of the week.
        doy: 4  // The week that contains Jan 4th is the first week of the year.
      }
    });
```

接著得到的結果就會是中文的訊息

```text
moment().format('LLLL'); // 2018年11月7日星期三下午2點24分
```

moment.js 可以設定的東西實在太多了，除了這邊舉例的一些，想看更多 moment.js 的設置與應用可以參考：[https://momentjs.com/docs/](https://momentjs.com/docs/)

寫了這麼多關於 moment.js 的應用，那如何應用至專案中呢？這邊就找了幾個在 codepen 上利用 moment.js 來製作的範例。

### 應用

首先介紹這個是顯示各時區時間，除了有用到 moment.js 本身，還有加入了同系列的 Moment Timezone，可以更快讀取各時區時間。  
Moment Timezone：[https://momentjs.com/timezone/](https://momentjs.com/timezone/)  
![https://ithelp.ithome.com.tw/upload/images/20181109/201114492EkmAL6p7g.png](https://ithelp.ithome.com.tw/upload/images/20181109/201114492EkmAL6p7g.png)

作者：Vasilis Tsirimokos  
作品網址：[https://codepen.io/Silisav/pen/RrXOYJ](https://codepen.io/Silisav/pen/RrXOYJ)

第二個有趣的作品類似打卡的方式，是透過按鈕來記錄時間，並計算總時間。  
![https://ithelp.ithome.com.tw/upload/images/20181109/20111449CstPNUy7xT.png](https://ithelp.ithome.com.tw/upload/images/20181109/20111449CstPNUy7xT.png)

作者：Nolan Wagner  
作品網址：[https://codepen.io/NolWag/pen/YNGZgq](https://codepen.io/NolWag/pen/YNGZgq)

第三個作品是活動中最常用到的倒數計時  
![https://ithelp.ithome.com.tw/upload/images/20181109/20111449cgzIwsiGfh.png](https://ithelp.ithome.com.tw/upload/images/20181109/20111449cgzIwsiGfh.png)

作者：Catherine  
作品網址：[https://codepen.io/\_catherinemc/pen/MXqvRK](https://codepen.io/_catherinemc/pen/MXqvRK)

### 小結

這篇介紹了從基本的 moment.js 認識到後面的例子，可以看出 moment.js 可以應用的部分非常多，也可以使用它就可以做出很多變化，讓時間處理變得簡單又快速，大家也可以嘗試看看利用 moment.js 寫出屬於自己的時間應用！

