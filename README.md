Linux 主機收集以下資訊，提供惡意活動分析使用：(寫成shellscript)
以下指令不全都是shell內容。
指令來源參考:https://www.gushiciku.cn/pl/2KpY/zh-tw，在變化而成。


一、檢查系統登陸日誌，統計IP重試登陸的次數。
lastb root | awk '{print $3}' | sort | uniq -c | sort -nr| more 

二、檢查系統使用者

2.1檢視是否有異常的系統使用者
cat /etc/passwd 

2.2檢查是否有新使用者尤其是UID和GID為0的使用者
awk -F":" '{if($3 == 0){print $1}}' /etc/passwd 

三、檢查系統異常程序

3.1 使用ps -ef命令檢視程序
ps -ef 

3.2 檢視該程序所開啟的埠和檔案
lsof -p pid 

3.3 檢查隱藏程序
ps -ef | awk '{print $2}'| sort -n | uniq >1; ls /proc |sort -n|uniq >2;diff -y -W 40 1 2 

四、檢查系統異常檔案

4.1 檢查一下 SUID的檔案
find / -uid 0 -perm 4000 -print 

4.2 檢查大於10M的檔案
find / -size +10000k –print 

4.3 檢查空格檔案
find / -name “...” –print 
find / -name “..” –print 
find / -name “.” –print 
find / -name ” ” –print 

4.4 檢查系統中的core檔案
find / -name core -exec ls -l {} () 

五、檢查系統檔案的完整性
（若客戶系統無法安裝，則跳過此項目）
利用工具AIDE檢查系統檔案的完整性
(https://www.zendei.com/article/71313.html)

六、檢查網路
6.1 檢查網絡卡模式
ip link
6.2 檢查惡意程式開放的埠及開啟的檔案
netstat -ntlup  
lsof -i: 埠號 

七、檢查系統計劃任務
crontab –u root –l 
cat /etc/crontab 
ls /etc/cron.* 

八、檢查系統服務
chkconfig —list 
systemctl list-unit-files

九、檢查rootkit
chkrootkit -q 
rkhunter -c
