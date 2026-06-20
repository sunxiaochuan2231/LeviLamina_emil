# LeviLamina_emil
LeviLamina插件加载器下通过lse编写的邮件插件
=========================================
  EmailBox 服务器邮箱系统 - 使用说明
  作者: 孙笑川天皇   版本: 1.0.0
=========================================

【功能】
- 玩家 /emil (或 /email) 打开邮箱，首次使用需绑定自己的邮箱
- 管理员(OP)在游戏里用GUI给玩家发件：同时写入游戏内邮箱 + 发送到对方绑定的邮箱(SMTP)
- 发件可附带奖励：勾选"附带手持物品"会把你当前手上的物品作为奖励，外加可填金币数量
- 玩家在游戏内查看邮件、领取奖励
- 玩家上线自动提醒未读邮件数

【安装】
1. 需要 legacy-script-engine-nodejs 引擎(nodejs版, 非quickjs)
2. 把 EmailBox 整个文件夹(含 node_modules)放进 plugins/
   ★ node_modules 里是 nodemailer(SMTP库)，必须一起拷贝 ★

【SMTP配置  plugins/EmailBox/config.json】
已为你预填163邮箱(授权码方式)：
{
  "smtp": {
    "host": "smtp.163.com",
    "port": 465,
    "secure": true,
    "user": "你的@163.com",
    "pass": "客户端授权码(不是登录密码!)"
  },
  "fromName": "服务器邮箱"
}
★★ 安全提醒：config.json 里的 pass 是发件授权码，等同密码，切勿把此文件发给任何人/上传公开仓库 ★★
- 163邮箱：host=smtp.163.com, port=465, secure=true
- QQ邮箱：host=smtp.qq.com, port=465, secure=true
- 启动日志若显示 "SMTP连接正常，可发件" 说明配置OK；
  若 "SMTP连接校验未通过" 多半是授权码错/端口被封/服务器没开放外网465端口。

【玩家用法】
/emil            打开邮箱
  - 绑定我的邮箱：第一次先绑定(用于接收SMTP邮件)
  - 查看我的邮件：点开某封 -> 自动标记已读 -> 有奖励则点"领取奖励"

【管理员(OP)用法】
/emil -> 点【管理】发送邮件
  - 选择在线玩家(或手动输入ID，可发给离线玩家)
  - 填标题、内容
  - 开关"同时发送到对方绑定邮箱"：对方绑过邮箱才会真正发邮件
  - 开关"附带我当前手持物品作为奖励"：把你手上拿的那个物品作为奖励(带完整NBT/附魔)
  - 金币奖励数量：需要服务器装了经济系统(money接口)才会到账

【数据文件】
plugins/EmailBox/
  config.json   SMTP配置(含授权码，保密)
  binds.json    玩家->邮箱
  mails.json    玩家->邮件列表(含奖励)

【常见问题】
- 邮件发不出去：看后台日志 [EmailBox] 报错；多为授权码错误或主机网络不能连 smtp.163.com:465。
- 金币没到账：服务器没有经济系统(money)。物品奖励不受影响。
- 物品奖励为空：发件时手上没拿东西，或勾选了附带物品但手是空的。
- /emil 打不开：确认引擎是 nodejs 版且 node_modules 拷全。
