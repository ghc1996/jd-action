## Github Action使用教程说明
 - Fork [此仓库项目](https://github.com/lxk0301/scripts) > 点击右上角fork按钮即可, [再不会可看此图](icon/fork.png)

 - 然后参考github@ruicky写的特别详细的小白教程[@ruicky教程](https://ruicky.me/2020/06/05/jd-sign/) (**注：此 [@ruicky教程](https://ruicky.me/2020/06/05/jd-sign/) 里面获取ck的方法不对。参考下面两种获取京东cookie的方式才对.**)

   






####  **fork后必须修改一下文件，才能执行定时任务**
  - 比如修改一下`README.md`文件(enter键回车)，再提交   
  - 不知怎么修改README.md文件的看[这个图](icon/action3.png);
    



#### 京东Cookie

  - Secret新增`JD_COOKIE`，填入cookie信息，多账号的cookie， 使用`&`或者换行隔开(两种方法)
  
  - 方式已一：`&`号隔开示例(注:后面的英文引号`;`不可缺失)
    如 `账号一cookie&账号二cookie&账号三cookie`，再多账号就依次类推即可
    ```
    pt_key=xxx1;pt_pin=xxx1;&pt_key=xxx2;pt_pin=xxx2;&pt_key=xxx3;pt_pin=xxx3;
    ```
  - 方式二：按`Enter`键换行隔开示例(这里给下三个账号的示例)
    ```
    pt_key=bbbbbbbbbbbbbb;pt_pin=aaaaaaa;
    pt_key=cccccccc;pt_pin=dddddddd;
    pt_key=eeeeeeeee;pt_pin=ffffffff;
    ```
  - 京东cookie获取看这里
    - [浏览器获取京东cookie教程](https://github.com/lxk0301/scripts/blob/master/backUp/GetJdCookie.md) 或者 [插件获取京东cookie教程](https://github.com/lxk0301/scripts/blob/master/backUp/GetJdCookie2.md)
    - IOS代理软件(Surge, Quantumult X, Loon)等用户有使用过BoxJs的,可在BoxJs里面提取京东cookie(打开BoxJs -> 底部中间的 `应用` -> NobyDa脚本订阅 -> 京东(多合一签到) -> 点击会话右上方的三个点点 -> 修改会话 -> 全选复制即可)，再不会看此[图文教程](icon/jd8.png)
    
    
#### Action里面cron时间

  - 此时间是国际标准时间，与北京时间不同，github action写16点才表示北京时间0点，具体可参考下面两个链接写cron

  -  [参考链接一](https://datetime360.com/cn/utc-beijing-time/) ， [参考链接二](http://www.timebie.com/cn/universalbeijing.php)

  - 根据使用经验发现github action 会有延迟现象，一般会延迟15分钟左右吧。比如action设置`北京时间16:00`运行，action其实要`16:15左右`才会执行脚本的。



#### 如何查看Action运行情况

   - [查看运行状态](https://raw.githubusercontent.com/lxk0301/scripts/master/icon/action1.png)
    
   - [查看运行日志](https://raw.githubusercontent.com/lxk0301/scripts/master/icon/action2.png)  



#### 如何禁用单个或者多个脚本(Action)

   - 操作步骤[看此图](./icon/disable-action.jpg) 



#### Fork后Action未运行

  > 是因为`/.github/workflows/`路径里面的`.yml`后缀文件里面的cron时间未到，如需立马看到效果

  - 方法：在自己仓库，手动点击仓库的右上角`star图标按钮`即可，稍后就能看到运行 
  - 注：之后如果想单独运行某一个脚本(此处的前提条件是执行过上面的方法)，手动点击 Run workflow [根据此图片示例操作](https://user-images.githubusercontent.com/21308593/93980945-e28ab000-fdb1-11ea-977c-c50705e79ac3.png) ，再次点一下`Actions`图标即可看到效果(或者等待10秒左右也可) 



#### 自动同步Fork后的代码

  > 此部分内容由tg@wukongdada和tg@goukey提供

  - 方案A - 强制远程分支覆盖自己的分支(**新手推荐使用**)
  
      1. 参考tg@wukongdada这篇教程 [保持自己github的forks自动和上游仓库同步的教程](https://github.com/lxk0301/scripts/blob/master/backUp/gitSync.md) ， 安装[pull插件](https://github.com/apps/pull) 并确认此项目已在pull插件的作用下（参考@twukongdada这篇教程文中1-d）
      
      2. 确保.github/pull.yml文件正常存在，yml内上游作者填写正确(此项目已填好，无需更改)。
      
      3. 确保pull.yml里面是`mergeMethod: hardreset`(默认就是`hardreset`)。
      
      4. ENJOY!上游更改三小时左右就会自动发起同步。
    ```
    # 方案A可参考这里
    version: "1"
    rules:                      # Array of rules
      - base: master            # Required. Target branch
        upstream: lxk0301:master    # Required. Must be in the same fork network.
        mergeMethod: hardreset  # Optional, one of [none, merge, squash, rebase, hardreset], Default: none.
        mergeUnstable: true    # Optional, merge pull request even when the mergeable_state is not clean. Default: false
    ```
  - 方案B - 保留自己仓库已修改过的文件(**需修改脚本或者提PR的使用**)
    
    > 上游变动后pull插件会自动发起pr，但如果有冲突需要自行**手动**确认。
    > 如果上游更新涉及workflow里的文件内容改动，需要自行**手动**确认。
    
    1. 参考tg@wukongdada这篇教程 [保持自己github的forks自动和上游仓库同步的教程](https://github.com/lxk0301/scripts/blob/master/backUp/gitSync.md) ， 安装[pull插件](https://github.com/apps/pull) 并确认此项目已在pull插件的作用下（参考@twukongdada这篇教程文中1-d）
    2. 确保.github/pull.yml文件正常存在，yml内上游作者填写正确(此项目已填好，无需更改)。
    3. 将pull.yml里面的`mergeMethod: hardreset`修改为`mergeMethod: merge`保存。
    4. ENJOY!上游更改三小时左右就会自动发起同步。
    ```
    # 方案B可参考这里
    version: "1"
    rules:                      # Array of rules
      - base: master            # Required. Target branch
        upstream: lxk0301:master    # Required. Must be in the same fork network.
        mergeMethod: merge  # Optional, one of [none, merge, squash, rebase, hardreset], Default: none.
        mergeUnstable: true    # Optional, merge pull request even when the mergeable_state is not clean. Default: false
    ```
  - 方案C - 利用github-action定时cron更新同步(**新手推荐使用**)
  
    > 效果和方案A一样（即：强制更新覆盖）
    
    新建secret，`Name`为`PAT`，填写的`Value`值需要去申请Personal access tokens，申请教程[看此处](https://www.jianshu.com/p/bb82b3ad1d11) 记得勾选`repo`权限就行



#### 下方提供使用到的 **Secrets全集合**

| Name                    |   归属   | 属性   | 说明                                                         |
| :---------------------: | :----------: | --------- | ------------------------------------------------------------ |
| `JD_COOKIE`             |   京东   | 必须   | 京东cookie,多个账号的cookie使用`&`隔开或者换行。具体获取参考[浏览器获取京东cookie教程](https://github.com/lxk0301/scripts/blob/master/backUp/GetJdCookie.md) 或者 [插件获取京东cookie教程](https://github.com/lxk0301/scripts/blob/master/backUp/GetJdCookie2.md) |
| `JD_DEBUG`              |   脚本打印log   | 非必须   | 运行脚本时，是否显示log,默认显示。改成false表示不显示，注重隐私的人可以在设置secret -> `Name:JD_DEBUG,Value:false` |
| `PUSH_KEY`              |   微信推送   | 非必须 | cookie失效推送[server酱的微信通知](http://sc.ftqq.com/3.version) |
| `BARK_PUSH`             |   BARK推送   | 非必须 | cookie失效推送BARK这个APP,填写内容是app提供的`设备码`，例如：https://api.day.app/123 ，那么此处的设备码就是`123`，再不懂看 [这个图](icon/bark.jpg) |
| `BARK_SOUND`            |   BARK推送   | 非必须 | bark推送声音设置，例如`choo`,具体值请在`bark`-`推送铃声`-`查看所有铃声` |
| `TG_BOT_TOKEN`          |   telegram推送   | 非必须 | tg推送,填写自己申请[@BotFather](https://t.me/BotFather)的Token,如`10xxx4:AAFcqxxxxgER5uw` , [具体教程](https://github.com/lxk0301/scripts/pull/37#issuecomment-692415594) |
| `TG_USER_ID`            |   telegram推送   | 非必须 | tg推送,填写[@getuseridbot](https://t.me/getuseridbot)中获取到的纯数字ID, [具体教程](https://github.com/lxk0301/scripts/pull/37#issuecomment-692415594) |
| `DD_BOT_TOKEN`          |   钉钉推送   | 非必须 | 钉钉推送[官方文档](https://ding-doc.dingtalk.com/doc#/serverapi2/qf2nxq) ,只需`https://oapi.dingtalk.com/robot/send?access_token=XXX` 等于符号后面的XXX， 注：如果钉钉推送只填写`DD_BOT_TOKEN`，那么安全设置需勾选`自定义关键词`，内容输入输入`账号`即可，其他安全设置不要勾选 |
| `DD_BOT_SECRET`         |   钉钉推送   | 非必须 | 密钥，机器人安全设置页面，加签一栏下面显示的SEC开头的字符串 , 注:填写了`DD_BOT_TOKEN`和`DD_BOT_SECRET`，钉钉机器人安全设置只需勾选`加签`即可，其他选项不要勾选,再不懂看 [这个图](icon/DD_bot.png) |
| `PET_NOTIFY_CONTROL`    | 东东萌宠推送开关  | 非必须 | 控制京东萌宠是否静默运行,`false`为否(发送推送通知消息),`true`为是(即：不发送推送通知消息)              |
| `FRUIT_NOTIFY_CONTROL`  | 东东农场推送开关  | 非必须 | 控制京东农场是否静默运行,`false`为否(发送推送通知消息),`true`为是(即：不发送推送通知消息)              |
| `JD_JOY_REWARD_NOTIFY`  | 宠汪汪兑换京豆推送开关  | 非必须 | 控制jd_joy_reward.js脚本是否静默运行,`false`为否(发送推送通知消息),`true`为是(即：不发送推送通知消息)              |
| `JOY_FEED_COUNT`        | 宠汪汪喂食数量  | 非必须 | 控制jd_joy_feedPets.js脚本喂食数量  ,可以填的数字10,20,40,80 , 其他数字不可.              |
| `JOY_HELP_FEED`         | 宠汪汪帮好友喂食  | 非必须 | 控制jd_joy_steal.js脚本是否给好友喂食,`false`为否,`true`为是(给好友喂食)              |
| `JOY_RUN_FLAG`          | 宠汪汪参加双人赛跑  | 非必须 | 控制jd_joy.js脚本是否参加双人赛跑,`false`为否,`true`为是，脚本默认是`true`              |
| `MARKET_COIN_TO_BEANS`  | 京小超兑换京豆数量  | 非必须 | 控制jd_blueCoin.js兑换京豆数量,可输入值为1到20或者1000的数字，其他数字不可.              |
| `MARKET_REWARD_NOTIFY`  | 京小超兑换奖品推送开关  | 非必须 | 控制jd_blueCoin.js兑换奖品成功后是否静默运行, `false`为否(发送推送通知消息),`true`为是(即：不发送推送通知消息)             |
| `SUPERMARKET_UPGRADE`   |  京小超自动升级  | 非必须 | 自动升级,顺序:解锁升级商品、升级货架,`true`表示自动升级,`false`表示关闭自动升级 |
| `BUSINESS_CIRCLE_JUMP`  |  京小超自动更换商圈  | 非必须 | 小于对方300热力值自动更换商圈队伍,`true`表示运行,`false`表示禁止 |
| `SUPERMARKET_LOTTERY`   |  京小超抽奖  | 非必须 | 每天运行脚本是否使用金币去抽奖,`true`表示抽奖,`false`表示不抽奖 |
| `FRUIT_BEAN_CARD`       |  农场使用水滴换豆卡  | 非必须 | 农场使用水滴换豆卡(如果出现限时活动时100g水换20豆,此时比浇水划算,推荐换豆),`true`表示换豆(不浇水),`false`表示不换豆(继续浇水),脚本默认是浇水 |
| `UN_SUBSCRIBES`         |  jd_unsubscribe.js  | 非必须 | 共四个参数,换行隔开. 四个参数分别表示`取关商品数量`,`取关店铺数量`,`遇到此商品不再进行取关`, `遇到此店铺不再进行取关`，[具体使用往下看](#取关店铺secret的说明)|
| `FruitShareCodes`       |  东东农场互助码  | 非必须 | 填写规则请看 [jdFruitShareCodes.js](https://github.com/lxk0301/scripts/blob/master/jdFruitShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `PETSHARECODES`         |  东东萌宠互助码  | 非必须 | 填写规则请看 [jdPetShareCodes.js](https://github.com/lxk0301/scripts/blob/master/jdPetShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `PLANT_BEAN_SHARECODES` |  种豆得豆互助码  | 非必须 | 填写规则请看 [jdPlantBeanShareCodes.js](https://github.com/lxk0301/scripts/blob/master/jdPlantBeanShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |
| `SUPERMARKET_SHARECODES`|  京小超商圈互助码  | 非必须 | 填写规则请看 [jdSuperMarketShareCodes.js](https://github.com/lxk0301/scripts/blob/master/jdSuperMarketShareCodes.js) 或见下方[互助码的填写规则](#互助码的填写规则) |



##### 互助码的填写规则

  > 互助码如何获取：运行相应脚本后，在日志里面可以找到。(如何查看日志上面有写，详见 如何查看action运行情况)

同一个京东账号的好友互助码用@隔开,不同京东账号互助码用&或者换行隔开,下面给一个文字示例和具体互助码示例说明

两个账号各两个互助码的文字示例：

  ```
京东账号1的shareCode1@京东账号1的shareCode2&京东账号2的shareCode1@京东账号2的shareCode2
  ```

 两个账号各两个互助码的真实示例：
  ``` 
0a74407df5df4fa99672a037eec61f7e@dbb21614667246fabcfd9685b6f448f3&6fbd26cc27ac44d6a7fed34092453f77@61ff5c624949454aa88561f2cd721bf6&6fbd26cc27ac44d6a7fed34092453f77@61ff5c624949454aa88561f2cd721bf6
  ```



#### 取关店铺secret的说明

 > secret依次是`取关商品数`,`取关店铺数`,`遇到此商品不再进行取关`,`遇到此店铺不再进行取关`

例如我要取关 `100`个商品，`100`个店铺，商品遇到商品关键字 `iPhone12` 停止取关，店铺遇到 `Apple京东自营旗舰店` 不再取关
使用换行隔开即可,例如下面示例:

 ```
100
100
iPhone12
Apple京东自营旗舰店
 ```



#### 关于脚本推送通知(微信server酱推送通知，bark app推送通知，telegram机器人推送通知，钉钉机器人推送通知)

  > 如果你填写了上面四种推送通知方式中的某一个通知所需secret，那么脚本通知情况如下：

  > 目前默认只有jd_fruit.js,jd_pet.js,jd_bean_sign.js三个脚本每次运行后都通知

  ```
jd_plantBean.js是每周一收集京豆后通知一次，
jd_joy_reward.js是每次兑换到了京豆通知一次，
jd_blueCoin.js是每次兑换到了奖品通知一次，
其余的脚本平常运行都是不通知，只有在京东cookie失效后，才会推送通知    
  ```
  - 如果填写了推送通知所需的secret后,运行上面有通知的脚本,还没收到通知的话,请自行查看action运行日志(如何查看日志教程请看上面的`如何查看action运行情况`),里面会推送通知发送失败的log


​    
##### 参考文献
[GitHub Actions 手动触发方式进化史](https://p3terx.com/archives/github-actions-manual-trigger.html)
    
[GitHub Actions 入门教程](https://p3terx.com/archives/github-actions-started-tutorial.html)

[github@ruicky教程](https://ruicky.me/2020/06/05/jd-sign/)
