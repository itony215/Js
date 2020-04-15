# Tsung 壓力測試



## Tsung

 [http://tsung.erlang-projects.org/](http://tsung.erlang-projects.org/)  


### What

* Tsung is an open-source multi-protocol distributed load testing tool
* Jmeter 是一個常見的 performance test tool , Tsung 則是一個效能很好的分散式 performance test tool，雖然Tsung的效能比較好，但Jmeter的使用有GUI介面可用
* 其他 tools 比較: [https://www.blazemeter.com/blog/open-source-load-testing-tools-which-one-should-you-use](https://www.blazemeter.com/blog/open-source-load-testing-tools-which-one-should-you-use)

### How

* Use Erlang, 需撰寫測試所需的 xml 檔
* &lt;?xml version="1.0"?&gt; &lt;!DOCTYPE tsung SYSTEM "/usr/share/tsung/tsung-1.0.dtd"&gt; &lt;tsung loglevel="notice" version="1.0"&gt;   &lt;!-- Client side setup --&gt;  &lt;clients&gt;  &lt;client &gt;&lt;/client&gt;  &lt;/clients&gt;   &lt;!-- Server side setup --&gt;  &lt;servers&gt;  &lt;server &gt;&lt;/server&gt;  &lt;/servers&gt;   &lt;!-- optional --&gt;  &lt;monitoring&gt;  &lt;monitor &gt;&lt;/monitor&gt;  &lt;/monitoring&gt;   &lt;load&gt;  &lt;arrivalphase &gt;&lt;/arrivalphase&gt;  &lt;/load&gt;   &lt;!-- optional --&gt;  &lt;options&gt;  &lt;option &gt;&lt;/option&gt;  &lt;/options&gt;   &lt;sessions&gt;  &lt;session &gt;  &lt;request &gt;&lt;/request&gt;  &lt;/session&gt;  &lt;/sessions&gt;  &lt;/tsung&gt;
* &lt;?xml version="1.0"?&gt; &lt;!DOCTYPE tsung SYSTEM "/usr/share/tsung/tsung-1.0.dtd"&gt; &lt;tsung&gt;  &lt;clients&gt;  &lt;!-- 在这里添加测试主机（客户端） --&gt;  &lt;/clients&gt;   &lt;servers&gt;  &lt;!-- 在这里添加被测试的主机（服务端） --&gt;  &lt;/servers&gt;   &lt;load&gt;  &lt;!-- 在这里添加加载过程配置 --&gt;  &lt;/load&gt;   &lt;sessions&gt;  &lt;!-- 在这里添加会话（即一系列HTTP请求） --&gt;  &lt;/sessions&gt; &lt;/tsung&gt;
* client 設定 client 端設定，要用 host name 來設定
* server 設定可用 ip 或是 domain name 來設定
* 腳本編寫從定義客戶端和服務器開始。客戶端即對服務器施加負載壓力。

#### Reference

* [https://www.cnblogs.com/2freedom/p/4726617.html](https://www.cnblogs.com/2freedom/p/4726617.html)
* [https://my.oschina.net/u/3390582/blog/1621392](https://my.oschina.net/u/3390582/blog/1621392)
* [https://www.cnblogs.com/lemon-flm/p/7885509.html](https://www.cnblogs.com/lemon-flm/p/7885509.html)
* [https://blog.csdn.net/u012244016/article/details/51547593](https://blog.csdn.net/u012244016/article/details/51547593)
* [https://www.awaimai.com/628.html](https://www.awaimai.com/628.html)
* [https://blog.csdn.net/u012973744/article/details/29341029](https://blog.csdn.net/u012973744/article/details/29341029)

### Run

* tsung -f {xml file} start

### Report

* 執行出的結果會置於
* ~/.tsung/log/{datetime}
* * datetime type Example: 20190125-1731
* 需再利用 [tsung\_stats.pl](http://tsung_stats.pl) 轉化結果成 html
  * 至結果資料夾下執行
  * /usr/lib/x86\_64-linux-gnu/tsung/bin/tsung\_stats.pl

### VS

* [https://www.blazemeter.com/blog/open-source-load-testing-tools-which-one-should-you-use](https://www.blazemeter.com/blog/open-source-load-testing-tools-which-one-should-you-use)
* [https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/553480/](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/553480/)

### XML 解析

 [https://www.cnblogs.com/lemon-flm/p/7885509.html](https://www.cnblogs.com/lemon-flm/p/7885509.html)  


### load

```text
<load>
    <arrivalphase phase="1" duration="1" unit="minute">
        <users arrivalrate="2" unit="second"></users>
    </arrivalphase>
</load>
```

* 上述的寫法是 以每秒2人的速度持續1分鐘
* 也可寫成
* &lt;load&gt;  &lt;arrivalphase phase="1" duration="1" unit="minute"&gt;  &lt;users interarrival="0.5" unit="second"&gt;&lt;/users&gt;  &lt;/arrivalphase&gt; &lt;/load&gt;
* 若要限制人數上限
* &lt;load&gt;  &lt;arrivalphase phase="1" duration="1" unit="minute"&gt;  &lt;users maxnumber="30000" arrivalrate="2" unit="second"&gt;&lt;/users&gt;  &lt;/arrivalphase&gt; &lt;/load&gt;
* * 加上 maxnumber="xxx” 設定

### Options

 大致上寫法固定，copy 即可  


```text
<options>
    <option type="ts_http" name="user_agent">
        <user_agent probability="80">Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.7.8) Gecko/20050513 Galeon/1.3.21</user_agent>
        <user_agent probability="20">Mozilla/5.0 (Windows; U; Windows NT 5.2; fr-FR; rv:1.7.8) Gecko/20050511 Firefox/1.0.4</user_agent>
    </option>
</options>
```

### Sessions

#### dynamic variables

* random string
* &lt;setdynvars sourcetype="random\_string" length="10"&gt;  &lt;var name="user\_rnd" /&gt; &lt;/setdynvars&gt;
* random number
* &lt;setdynvars sourcetype="random\_number" start="1" end="100"&gt;  &lt;var name="userid\_rnd" /&gt; &lt;/setdynvars&gt;

#### POST

```text
<request> <http url='/xxx' contents='yyy' method="POST" version="1.1"></http></request>
```

* 若有用到 dynamic variables, 則 &lt;request&gt; → &lt;request subst="true"&gt;
* form-data
* contents="account=%%\_user\_rnd%%&amp;phone=%%\_phone\_rnd%%"
* json
* contents='{&quot;account&quot;:&quot;%%\_user\_rnd%%&quot;,&quot;phone&quot;:&quot;%%\_phone\_rnd%%&quot;}'
* 其中 dynamic variables 在使用時
  * user\_rnd → %%\_user\_rnd%%, 即變數前要加上 \_，再用 %%包起來
* 其中 post 所帶資料型態不同，在 xml 設定上也有所不同
  * 如原先是 'account=1 & phone = 2' → 'account=1&amp;phone=2'
    * 即 & 要用 &amp; 來取代
  * 如原先是 json 格式 '{“account”:”1”, “phone”:”2”}' → '{&quot;account&quot;:&quot;1&quot;,&quot;phone&quot;:&quot;2&quot;}'
    * 即 " 要用 &quot; 來取代

### Examples

```text
<?xml version="1.0"?>
<!DOCTYPE tsung SYSTEM "/usr/share/tsung/tsung-1.0.dtd">
<tsung loglevel="notice" version="1.0">

  <!-- Client side setup -->
  <clients>
    <client host="localhost" use_controller_vm="true" maxusers="60000"/>
  </clients>

  <!-- Server side setup -->
  <servers>
    <server host="172.17.0.30" port="8080" type="tcp"></server>
    <!--
    <server host="172.17.0.189" port="8080" type="tcp"></server>
    <server host="172.17.0.30" port="8080" type="tcp"></server>
    <server host="172.17.0.6" port="8080" type="tcp"></server>
    <server host="172.17.0.50" port="8080" type="tcp"></server>
    -->
  </servers>

  <!--
  <monitoring>
    <monitor host="54.150.230.6" type="snmp"></monitor>
  </monitoring>
  -->

  <load>
    <arrivalphase phase="1" duration="10" unit="minute">
      <users maxnumber="30000" interarrival="0.0005" unit="second"></users>
    </arrivalphase>
  </load>

  <options>
    <option type="ts_http" name="user_agent">
      <user_agent probability="80">Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.7.8) Gecko/20050513 Galeon/1.3.21</user_agent>
      <user_agent probability="20">Mozilla/5.0 (Windows; U; Windows NT 5.2; fr-FR; rv:1.7.8) Gecko/20050511 Firefox/1.0.4</user_agent>
    </option>
    <option name="global_ack_timeout" value="60000"></option>
  </options>

  <sessions>
    <session name="http-example" probability="100" type="ts_http">
      <setdynvars sourcetype="random_string" length="10">
        <var name="user_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="1" end="100">
        <var name="userid_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="10000000" end="99999999">
        <var name="phone_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="1" end="100">
        <var name="age_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="1" end="10">
        <var name="region_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="0" end="1">
        <var name="gender_rnd" />
      </setdynvars>
      <setdynvars sourcetype="random_number" start="51" end="60">
        <var name="event_rnd" />
      </setdynvars>
      <!--
      <request subst="true"> <http url="/api/createUser" contents="account=%%_user_rnd%%&amp;phone=%%_phone_rnd%%&amp;region=%%_region_rnd%%&amp;age=%%_age_rnd%%&amp;gender=%%_gender_rnd%%" method="POST" version="1.1"></http> </request> 
      <request subst="true"> <http url="/api/createUser" content_type="application/json" contents='{&quot;account&quot;:&quot;%%_user_rnd%%&quot;,&quot;phone&quot;:&quot;%%_phone_rnd%%&quot;,&quot;region&quot;:&quot;%%_region_rnd%%&quot;,&quot;age&quot;:&quot;%%_age_rnd%%&quot;,&quot;gender&quot;:&quot;%%_gender_rnd%%&quot;}' method="POST" version="1.1"></http> </request> 

      <thinktime value="2" random="true"></thinktime>

      <request subst="true"> <http url="/api/updateUser" contents="account=%%_userid_rnd%%&amp;phone=%%_phone_rnd%%&amp;region=%%_region_rnd%%&amp;age=%%_age_rnd%%&amp;gender=%%_gender_rnd%%" method="POST" version="1.1"></http> </request> 

      <thinktime value="2" random="true"></thinktime>

      <request subst="true"> <http url="/api/eventInfo?event_id=%%_event_rnd%%" method="GET" version="1.1"></http> </request>
      <request subst="true"> <http url="/api/eventInfo/%%_event_rnd%%" method="GET" version="1.1"></http> </request>
      -->

      <request subst="true"> <http url="/api/eventInfo?event_id=%%_event_rnd%%" method="GET" version="1.1"></http> </request>

    </session>
  </sessions>
</tsung>
```

