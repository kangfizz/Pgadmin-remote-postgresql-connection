# Pgadmin-remote-postgresql-connection
This topic is talking about how to connecting to your remote server's postgresql table  
(Language:Chinese)  
這篇教導如何在本機端(Windows)使用pgadmin介面去連接遠端伺服器(linux)的postgresql。  
(本篇同時也會教導如何直接匯出postgresql的table成為sql檔)  
*(更新時間:08.04.16)*
***  
####前置小知識須知:  
一般而言,postgresql的位置有兩個(linux)  
一個是在```/var/lib/postgresql```,在此是匯入匯出sql檔的地方。  
另一個則是在```/etc/postgresql```,在此則是資料庫本身的設定地方。  
另外也需本身已經安裝好Postgresql(版本為9.3版)。  
本文開始:  
####  1.安裝pgadmin套件
<code>sudo apt-get install pgadmin3</code>  

####2.設定postgresql相關  
#####a.進入postgresql.conf修改  
```
vi /etc/postgresql/9.3/main/postgresql.conf
```
用vi進入後案i開始修改以下(如果前面只有#其他沒變的話就把#去除即可,都找的到)  
基本上addresses的部分就是從```'localhost'```變為```'*'```  
```
listen_addresses = '*'
password_encryption = on
```
(如果<code>port=5432</code>前面有<code>#</code>的話請去除)  
修改完後按```esc```,之後```:wq```儲存後離開  
#####b.進入pg_hba.conf修改  
```
vi /etc/postgresql/9.3/main/pg_hba.conf
```
在大概同類型的位子新增一行:  
```
host all all 0.0.0.0/0 password
```
最後再做postgresql重啟的動作:  
```
sudo /etc/init.d/postgresql restart 
```

####3.設定windows端的heidiSQL的部分  
首先下載好heidiSQL,到以下網址下載installer  
[這裡這裡](http://www.heidisql.com/download.php)  
安裝好後的介面大概會像這樣子:  
![text alt](https://github.com/kangfizz/Pgadmin-remote-postgresql-connection/blob/master/heidiSQL.png?raw=true)  
這裡的設定:  
+ 類型:我們是用postgresql,其他也有MySQL之類的  
+ 主機名:就你的遠端主機  
+ 用戶:你的postgresql的帳號(已經受grant到資料庫的)  
+ 密碼:同上  
+ 連接:基本上是5432  
+ 資料庫:如果連接上的話,基本上按下右邊的下拉三角可以看選擇(可當測試成不成功)  

連進去差不多可以看到下面這種:(就完成摟!)  
![text alt](https://github.com/kangfizz/Pgadmin-remote-postgresql-connection/blob/master/heidiSQL2.png?raw=true)  

額外補充:  
####如果因為從windows heidiSQL匯出的SQL檔無法直接用指令再匯入Postgresql(即是不透過heidiSQL)  
####要如何直接從CMD直接匯出Table到SQL檔呢?  
用以下即可直接匯出:  
1.	將資料表匯出(export)成為一個.sql檔(包含創建與輸入資料)
```
pg_dump -d <database_name> -t <table_name> > file.sql
```
這個file.sql(名字自訂)會在 /var/lib/postgresql/ 裡面  
2.	將sql檔匯入database中:
```
psql –f file.sql django_car postgres
```

大概就這樣吧! 感謝收看!!
