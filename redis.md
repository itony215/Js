# Redis

## 什麼是 Redis？ <a id="5a5c"></a>

Redis 是一種內存資料庫\(in-memory\)的架構，可以被當作一個簡易的資料庫\(key-value database\)，這功能就讓我想起 Android 的 Shared Preferences 一樣是可以將資料存在用戶自己的裝置中，也等同於網頁中的 cookie、localStorage，Redis 使用 RAM 來存取資料所以相對來得快速，缺點是重開機後記憶體就會被釋放掉，但 Redis 有個功能 persistence 可以解決這個問題，此外 Redis 也能設定資料的生命週期\(Expire\)當你在儲存資料的時候，你可以新增一個 Expire time 的參數，時間一到之後，這個 key 就會自動被清除。

