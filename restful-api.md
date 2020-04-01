# Restful API

上次IBM面試被考restful api 是什麼？什麼格式？json?有哪幾種方法？

答案如下

GET：取得\(想要的服務\)的資料或是狀態。（safe & idempotent）   
POST：新增一項資料。   
PUT：利用更新的方式於"指定位置"新增一項資料。 （idempotent）  
PATCH：在現有的資料欄位中，增加或部分更新一筆新的資料。   
DELETE：指定資料刪除。 （idempotent）

> **不同的Method就是對同一件事情做不同的操作**。

------

以POST來說，我要一雙鞋，我就會得到一雙鞋。但是如果再用POST發一次我要一雙鞋，會出現兩種可能：

1. 我"再"得到一雙鞋，而且這雙鞋跟上一雙是不一樣的，以資料庫來說，就是ID不一樣。

2. 我只能有一雙鞋，所以他會跟我說"錯誤"，我已經得到一雙鞋了！

但以PUT來說，無論我發多少次，他都會回我，我有一雙鞋，而我並不會變成兩雙鞋。當然如果商店沒鞋，則變成我沒有鞋。

------

如果我們在寫一隻商品的WebAPI，讓工程師隨便寫可能會有以下方式來作interface：

獲得商品資料 GET   /getAllItems  
獲得商品資料 GET   /getItem/11  
新增商品資料 POST /createItem  
更新商品資料 POST  /updateItem/  
刪除商品資料 POST  /deleteItem/

若是以使用 RESTful API 開發的話:

獲取商品資料 /GET     /items  
獲取商品資料 /GET     /items/1  
新增商品資料 /POST   /items  
更新商品資料 /PATCH /items/1   
刪除商品資料 /DELETE /items/1

