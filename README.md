# 每秒 · Earn Tick

> 看着数字走，比看着时钟走舒服一点。

一个只有几个文件的 PWA：把你今天的收入换算成**每秒进账**，实时累加。
工作的时候时间过得很慢，但钱是按秒进来的——这个页面只做一件事，把这件事显示出来。

<p align="center">
  <img src="assets/preview-day.webp" width="330" alt="白天">
  <img src="assets/preview-night.webp" width="330" alt="夜间">
</p>

## 它长什么样

- **主数字**：今天到目前为止赚了多少，小数第三位是橙色的，一直在动
- **每秒速率**：按你的月薪自动算出，一眼看到自己的价格
- **下一件想要的东西**：还差多少钱、还要多久，达成时有一小把纸屑和一次震动
- **一个按钮**：上班 → 摸鱼一下 → 继续搬砖。摸鱼的时长会如实记下来

没有登录、没有云、没有排行榜、没有打卡。月薪只存在你自己的浏览器里。

## 特性

- **单个 HTML**：无框架、无构建、无依赖，打开就能跑
- **纯本地**：数据只在 `localStorage`，不联网、不上报、没有后端
- **关掉也不丢**：按时间戳结算而不是靠页面常驻，回来看到的是正确金额
- **可加到主屏**：manifest + service worker，离线可用
- **自动夜间模式**：20 点后自动转暖黑
- **手感细节**：状态点每秒跳一次、达成目标撒纸屑 + 震动、数字进位有轻微弹动

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

目标卡按固定阶梯推进（冰美式 ¥15 → 牛肉面 ¥28 → 奶茶 ¥38 → 书 ¥68 → …），
进度、还差多少、还要等多久都从同一个速率推出来。

## 一些设计上的取舍

- **不做数据看板**：一屏只有数字、目标、按钮。多一个图表就多一分"又在一个仪表盘里"的感觉
- **不做账号**：这件事不需要云，也就不需要登录
- **不催你**：没有连续打卡、没有排行榜；摸鱼不会被批评，只会被记下来
- **第三位小数故意做橙色**：按秒结算时，它是唯一动得够快、能让人感知到"在赚"的东西
- **目标用实物而不是金额**：`还差 ¥11.55` 很难有感觉，`还差一点就够一杯手冲` 才有

## 路线图

- [ ] 目标清单可自定义（现在写死在代码里）
- [ ] 一周 / 一月趋势，纯本地
- [ ] 多种视角切换（时薪 / 日薪 / 月薪）
- [ ] 中英文界面切换

有想法就开 issue。

## 已知限制

- 口径只有「月薪 × 工时」，不含奖金、年终、股票、副业
- 跨天自动清零；跨时区未特殊处理
- iOS 需要"添加到主屏幕"才是真正的全屏体验

## English

**Earn Tick** is a tiny PWA that shows how much money you're making *right now, per second*.
It's deliberately minimal: one big number, your rate, the next thing you're saving up for, and a single button.

- One HTML file. No framework, no build step, no dependencies.
- 100% local — your salary never leaves `localStorage`. No backend, no analytics.
- Survives being closed: earnings are computed from timestamps, not from keeping the page open.
- Installable (manifest + service worker), offline-capable, auto dark mode after 20:00.

```bash
git clone https://github.com/imkida/earn-tick.git && cd earn-tick
python3 -m http.server 8000   # → http://localhost:8000
```

Formula: `perSecond = salary / (workdaysPerMonth × hoursPerDay × 3600)`.

## License

[MIT](LICENSE) © imkida
