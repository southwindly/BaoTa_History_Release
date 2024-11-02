## 宝塔Linux原版历史版本存档仓库
#### 尽管最新版本的宝塔面板提供了许多新功能和优化，但并不是所有用户都需要或想要这些新特性，比如这个垃圾的绑定账号功能。有些用户可能更喜欢使用他们熟悉的宝塔老版本。

#### 我们可以通过安装宝塔Linux原版历史版本存档仓库来解决这个问题。

#### （1）下载curl包
```javascript
yum install curl
```
#### （2）下载离线包
```javascript
curl -L https://raw.githubusercontent.com/yigexiaoyunwei/BaoTa_History_Release/main/LinuxPanel-7.7.0.zip > LinuxPanel-7.7.0.zip
```
#### （3）解压
```javascript
unzip LinuxPanel-*
```
#### （4）切换到降级包目录
```javascript
cd panel
```
#### （5）执行脚本
```javascript
bash update.sh
```
#### （6）删除降级包
```javascript
cd .. && rm -f LinuxPanel-*.zip && rm -rf panel
```

### 为了防止自动升级改成离线模式，并修改hosts
```javascript
echo "127.0.0.1 www.bt.cn" >> /etc/hosts
```

### 宝塔老版本屏蔽账号绑定
```javascript
sed -i "s|bind_user == 'True'|bind_user == 'XXXX'|" /www/server/panel/BTPanel/static/js/index.js
```
### 或者直接删除
```javascript
rm -f /www/server/panel/data/bind.pl
```

### 如何使用专业版插件
#### 直接把/www/server/panel/data/plugin.json替换其中`"endtime": -1`全部替换成999999999999。

### 给plugin.json文件上锁防止自动修复为免费版
```javascript
chattr +i /www/server/panel/data/plugin.json
```
### 或者直接修改代码
```javascript
cat /www/server/panel/class/panelPlugin.py | grep tmpList
vim /www/server/panel/class/panelPlugin.py
```

### 找到`softList[‘list’] = tmpList`，在其下方添加（记得用空格，不然python不识别）：
```javascript
// 解锁专业版
                softList['pro'] = 1
        for soft in softList['list']:
            soft['endtime'] = 0
```
### 修改完成
