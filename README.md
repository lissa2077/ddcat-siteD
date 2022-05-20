# ddcat-siteD 多多猫插件
只作web-crawling學術研究用途，業餘娛樂，禁止商業營利用途 

### 完全不重要的耍帥感想
爬過蟲，才會有心思注意到別的網頁防止被爬蟲的技術真的很強。  
  
## 不專業個人開發心得
- 官方開發文檔真的不太全面，有需要還是直接去看SiteD引擎的源代碼，另外推薦多多猫插件开发指南
- 這app很冷門，開發更冷門
- get跟post的HTML parse處理  
  GET
  ```
  function search_parse(url, html) {
    var $ = cheerio.load(html);
  ```
  POST
  ```
  function search_parse(url, html) {
    var strjson = JSON.parse(html); //strjson是json object
  ```
- 有些網頁手機版網頁和電腦版網頁都用同一域名，依靠我們讀者發送的request header判斷裝置是手機還是電腦，要強行手機版網頁，可以在node加上`header="User-Agent=Mozilla/5.0(Android 2.3)`
- 不少網站有用Cloudflare，或者配合lazy load把關鍵資料全藏在伺服器中，這些爬不了或者有所限制。
- `Android/Data/org.noear.ddcat/files/sited`放置**成功**爬下來的html代碼，可以看看
  - 「有沒有成功爬出html」（空文件夾代表根本沒有連上目標網頁，例如node的設定本身就有錯，導致get/post request根本發不出去）
  - 「是否載入錯誤網站」（例如你的bookId其實是undefined）
  - 「網頁是否lazy load」（這種情況下正常只能爬出一張「載入中」的圖，要找找看是不是藏在javascript或者有別的規律）
  - 「網頁會不會自動跳去電腦版/手機版」（兩個版本的tag、html格式根本不同）
  - 「class名、attr有所不同」等  
  - 建議測試插件時先清空文件夾`sited`內所有文件夾
- lazy load而且把資料藏得太隱藏的，或者多多猫內部瀏覽器顯示不了圖片，我推薦直接在node加上`run="web"`，用外部手機瀏覽器直接看就算了吧
- 有些網頁搜尋引擎有兩條url，例如一個支援繁體一個支援簡體，可以用以下方式一次用兩個url搜尋
  ```
  <search cache="1d" method="get" parse="search_parse" url="第一條url">
	  <search cache="1d" method="get" parse="search_parse" url="第二條url"/>
  </search>
  ```
- parseUrl的主要用途是*處理一話多頁或單頁單圖等需要按「下一頁」才能完整看完一話*的情況。parseUrl所得的一組url，每一條url進行一次parse。
- parseUrl奇效：如果有網站把所有圖的url藏在javascript script中，你可以在parseUrl先把那個js script的url抽出來，然後在parse中當成文字檔處理(script的內容會放在parse的parameter 'html')，或者去除非html tag的字然後用回cheerio.load()。

## Reference
[SiteD引擎 github](https://github.com/noear/SiteD)
[多多猫 - SiteD插件容器](http://ddcat.noear.org/)  
[多多猫开发文档](http://sited.noear.org/img/sited_dev_34_66.pdf)  
[多多猫插件开发指南](http://static.kancloud.cn/magicdmer/ddcat_plugin_develop)
