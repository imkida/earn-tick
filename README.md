# 每秒 · Earn Tick

> 看着数字走，比看着时钟走舒服一点。

一个只有几个文件的 PWA：把你今天的收入换算成**每秒进账**，实时累加。
工作的时候时间过得很慢，但钱是按秒进来的——这个页面只做一件事，把这件事显示出来。

<p align="center">
  <img src="assets/preview-day.webp" width="300" alt="白天">
  <img src="assets/preview-night.webp" width="300" alt="夜间">
</p>

## 它长什么样

- **主数字**：今天到目前为止赚了多少，小数第三位是橙色的，一直在动
- **每秒速率**：按你的月薪自动算出，一眼看到自己的价格
- **下一件想要的东西**：还差多少钱、还要多久，达成时有一小把纸屑和一次震动
- **一个按钮**：上班 → 摸鱼一下 → 继续搬砖。摸鱼的时长会如实记下来

没有登录、没有云、没有排行榜、没有打卡。月薪只存在你自己的浏览器里。

## 人生类型

同样一笔钱，不同的人想买的东西不一样。设置里可以切换四种「人生类型」，
目标阶梯、文案语气、换算单位会整套换掉（换来换去不影响你已经攒下的账）：

| 类型 | 阶梯（从便宜到贵） |
| --- | --- |
| 打工人 | 冰美式 → 牛肉面 → 奶茶 → 一本书 → 一顿好的 → 帆布鞋 → 外套 → 一天房租 |
| 旅行爱好者 | 地铁票 → 机场咖啡 → 一晚青旅 → 一天租车 → 景区门票 → 海景房 → 短途机票 → 周末旅行 |
| 居家博主 | 香薰 → 马克杯 → 一束花 → 铸铁锅 → 亚麻四件套 → 半自动咖啡机 → 一把好椅子 → 全屋收纳 |
| 游戏玩家 | 一次十连 → 皮肤包 → 手柄 → 电竞椅 → 显示器 → 耳机 → 掌机 → 顶配外设 |

## 战果

主界面下方一行小字：`今天到手 6 档 · 累计 8 档 ›`。点开是按阶梯顺序兑换的清单——
从第一档开始，攒够就换一件，换完往上走，一轮走完再从头。所以件数和金额永远对得上，
也跟目标卡的「还差多少」是同一套账。

## 模拟人生

设置 → 时间 → 模拟人生：选**工作几年**（1–40）和**多少倍速**（×60 / ×600 / ×3600 / ×86400），
页面按新速率往前推，主数字、目标、战果全部跟着走，跑满年限自动收尾。

- ×3600 = 现实 1 秒走过 1 个工时小时
- 模拟只在页面开着时推进，离开自动暂停（不然挂一晚上就"工作"了几十年）
- **模拟赚的钱不进真实账本**，两边是两本账

默认永远按真实时间走，模拟只是可选的另一双眼睛。

## 存档

- 每 1.5 秒、切后台、关页面都会存一次，按时间戳结算，关掉再打开金额是对的
- 冷启动（离开超过 2 分钟）顶部会出一条 `上次停在 15:28 · ¥359.20`，可以选择**继续**或**从头开始**
- 设置里还有：重置今天 / 从头开始 / 回到上一份（重置前自动备份一份，可以反悔）

## 分享

战果面板里可以导出一张**单文件 HTML 战报**：内联样式、内联图标、零外部请求（约 5KB），
发给自己或朋友，双击就能看，末尾带这个项目的公开地址。

- **带上金额**：累计、今天、每档件数
- **只露战果**：只给档位和件数

两种模式都**不含工时**——这一点是刻意的，见下面的隐私说明。

## 特性

- **单个 HTML**：无框架、无构建、无依赖，打开就能跑
- **纯本地**：数据只在 `localStorage`，不联网、不上报、没有后端
- **关掉也不丢**：按时间戳结算而不是靠页面常驻，回来看到的是正确金额
- **可加到主屏**：manifest + service worker，离线可用
- **自动夜间模式**：20 点后自动转暖黑
- **手感细节**：状态点每秒跳一次、达成目标撒纸屑 + 震动、数字进位有轻微弹动

## 隐私

这个产品的核心数字就是收入，所以有几条硬规矩：

1. **不出网**：没有后端、没有统计、没有字体 CDN。CI 会拦住任何第三方域名和外部脚本。
2. **分享页不出现工时**。工资 = 金额 ÷ 工时，这两个数只要同时出现在一张卡片上，
   任何人都能一步算出你的时薪；只出现其中一个，反推不出来。
3. **分享是快照，不是链接**：文件里只有你导出那一刻的数字，之后不带任何回调、不带 sid。
4. 数据只有一份，在你自己浏览器的 `localStorage` 里，删掉这个站点的数据就等于彻底删除。

## 快速开始

本地跑起来只需要一个静态服务器：

```bash
git clone https://github.com/imkida/earn-tick.git
cd earn-tick
python3 -m http.server 8000
# 打开 http://localhost:8000
```

> 直接双击 `index.html` 也能用，但 service worker 和「添加到主屏幕」需要 http(s)。

在线版：**https://imkida.github.io/earn-tick/**

部署到任何静态托管都可以（GitHub Pages / Vercel / Netlify / Nginx / Caddy），
全部是相对路径，放在子目录也没问题。Caddy 示例：

```caddy
handle /earn* {
    root * /var/www/earn-tick
    uri strip_prefix /earn
    file_server
}
```

## 怎么算的

```
每秒进账 = 月薪 ÷ (每月工作日 × 每天工时 × 3600)
今日已赚 = 每秒进账 × 今天的工作秒数        # 暂停的时长不计
```

例：月薪 25000、每月 21.75 天、每天 8 小时 → **每秒 ¥0.0399**。

## 一些设计上的取舍

- **不做数据看板**：一屏只有数字、目标、按钮。多一个图表就多一分"又在一个仪表盘里"的感觉
- **不做账号**：这件事不需要云，也就不需要登录
- **不催你**：没有连续打卡、没有排行榜；摸鱼不会被批评，只会被记下来
- **第三位小数故意做橙色**：按秒结算时，它是唯一动得够快、能让人感知到"在赚"的东西
- **目标用实物而不是金额**：`还差 ¥11.55` 很难有感觉，`还差一点就够一杯手冲` 才有
- **阶梯最后一档约等于一天的收入**：太高就永远摸不到，太快就没有等待感

## 路线图

- [ ] 节奏卡：只分享「今天走了多久」，不含金额（隐私友好的另一种分享形态）
- [ ] 一键遮金额，方便截图
- [ ] 目标阶梯自定义
- [ ] 中英文界面切换

有想法就开 issue。

## 已知限制

- 口径只有「月薪 × 工时」，不含奖金、年终、股票、副业
- 跨天自动清零；跨时区未特殊处理
- iOS 需要"添加到主屏幕"才是真正的全屏体验
- 分享页是快照，不会随后续进度自动更新

## English

**Earn Tick** is a tiny PWA that shows how much money you're making *right now, per second*.
One big number, your rate, the next thing you're saving up for, and a single button.

- One HTML file. No framework, no build step, no dependencies.
- 100% local — your salary never leaves `localStorage`. No backend, no analytics.
- Survives being closed: earnings are computed from timestamps, not from keeping the page open.
- Four "life modes" (worker / traveler / home blogger / gamer) with their own reward ladders.
- A stats sheet that converts earnings into things you actually got, in ladder order.
- A simulation mode: pick how many years and how fast, watch a working life go by. Optional, and off by default.
- Shareable single-file HTML battle report — amounts only, never hours worked, so nobody can divide one by the other and back out your salary.

```bash
git clone https://github.com/imkida/earn-tick.git && cd earn-tick
python3 -m http.server 8000   # → http://localhost:8000
```

Formula: `perSecond = salary / (workdaysPerMonth × hoursPerDay × 3600)`.

## License

[MIT](LICENSE) © imkida
