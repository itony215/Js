# base64編碼

Base64是一種基於64個可打印字符來表示二進制數據的表示方法。由於2的6次方等於64，所以每6個比特為一個單元，對應某個可打印字符。三個字節有24個比特，對應於4個Base64單元，即3個字節可表示4個可打印字符。它可用來作為電子郵件的傳輸編碼。在Base64中的可打印字符包括字母A-Z、a-z、數字0-9，這樣共有62個字符，此外兩個可打印符號在不同的系統中而不同。一些如uuencode的其他編碼方法，和之後binhex的版本使用不同的64字符集來代表6個二進制數字，但是它們不叫Base64。

為什麼要編碼

在計算機中任何數據都是按ascii碼存儲的，而ascii碼的128～255之間的值是不可見字符。而在網絡上交換數據時，比如說從A地傳到B地，往往要經過多個路由設備，由於不同的設備對字符的處理方式有一些不同，這樣那些不可見字符就有可能被處理錯誤，這是不利於傳輸的。所以就先把數據先做一個Base64編碼，統統變成可見字符，這樣出錯的可能性就大降低了。



那搶掛號思路是這樣的：  
我在寫爬蟲的時候發現其實可以得知下兩週網路掛號的網址，  
但是網址長這樣：  
[https://reg.ntuh.gov.tw/webadministration/RegistForm.aspx?newx=UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkANAAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAyADcAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA0KAA2](https://reg.ntuh.gov.tw/webadministration/RegistForm.aspx?newx=UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkANAAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAyADcAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA0KAA2)  
  
看起來一坨噁心的東西如下，根據經驗應該是某種編碼方式ㄅ  
[UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkANAAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAyADcAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA0KAA2](https://reg.ntuh.gov.tw/webadministration/RegistForm.aspx?newx=UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkANAAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAyADcAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA0KAA2)  
於是丟到 [http://base64.org/](http://base64.org/) 解出來發現：  


```text
UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkAMgAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAxADMAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA2
Base64 解密後：
ServiceIDSE=3800392&EncryptCode=T0DENT0820190613&useDrRestCnt=


UwBlAHIAdgBpAGMAZQBJAEQAUwBFAD0AMwA4ADAAMAAzADkAMwAmAEUAbgBjAHIAeQBwAHQAQwBvAGQAZQA9AFQAMABEAEUATgBUADAAOAAyADAAMQA5ADAANgAyADAAJgB1AHMAZQBEAHIAUgBlAHMAdABDAG4AdAA9AA2
Base64 解密後：
ServiceIDSE=3800393&EncryptCode=T0DENT0820190620&useDrRestCnt=

Base64 解密前：？
ServiceIDSE=3800394&EncryptCode=T0DENT0820190627&useDrRestCnt=
```

可以觀察到解密後，ServiceID和SEEncryptCode 應該是有規則的，  
聰明如你應該知道下一個規則是  


1. ServiceIDSE=3800394&EncryptCode=T0DENT0820190627&useDrRestCnt=

  
但你會發現你要base64 encode回去會完全跟原本網址不一樣，  
原來是中間有塞一些渣渣呢...但是不會顯示在網頁上，  
總之最後長這樣：  
[![](https://1.bp.blogspot.com/-nvKHSsoxV2w/XQZXkVPG1aI/AAAAAAAAUYY/KOJ4PvCapiMp9ZGKjaSjiPJ-M6awB7eJgCLcBGAs/s1600/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%2B2019-06-16%2B%25E4%25B8%258B%25E5%258D%258810.50.42.png)](https://1.bp.blogspot.com/-nvKHSsoxV2w/XQZXkVPG1aI/AAAAAAAAUYY/KOJ4PvCapiMp9ZGKjaSjiPJ-M6awB7eJgCLcBGAs/s1600/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%2B2019-06-16%2B%25E4%25B8%258B%25E5%258D%258810.50.42.png)  
之後編碼回去後就得到下兩週後的網址啦，  
但訪問之後輸入完資料、驗證碼後，  
**"母湯歐，時間還沒到你不能預約唷"**  
  
但由於他們驗證碼不會刷新，  
聰明的你應該知道有種東西叫做連點程式，  
隨便找個連點程式瘋狂連點就好了XD  
到這邊還滿有信心應該可以搶贏的，  
就算機器人有用CNN辨識驗證碼的圖片也是需要時間  


