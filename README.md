# 婚礼邀请函使用指南

三个单文件 HTML 邀请函，无需任何服务器即可运行；部署到 GitHub Pages 后，把链接发给微信好友，**安卓 / 苹果 / 电脑微信内点开即看**。

| 文件 | 风格 |
|------|------|
| `index.html` | 风格选择页（预览入口） |
| `invitation-modern.html` | 现代简约 · 暖白香槟金 |
| `invitation-chinese.html` | 中式红金 · 囍字祥云 |
| `invitation-forest.html` | 森系清新 · 花艺手绘 |

---

## 一、30 秒快速替换（最重要）

每个邀请函文件里都有一个 **CONFIG 配置区**，位于 `<script>` 开头、被大分隔线包围。用任意文本编辑器（推荐 [VS Code](https://code.visualstudio.com/) 或记事本）打开文件，搜索 `CONFIG`，只改引号内的文字即可：

```js
var CONFIG = {
  groomName: "张伟",        // ← 改成新郎名字
  brideName: "李婷",        // ← 改成新娘名字
  dateText: "2026年10月1日 · 星期四",
  lunarText: "农历八月廿一", // 不需要农历可留空 ""
  timeText: "11:08",        // 仪式时间
  hotelName: "某某大酒店 · 三楼宴会厅",
  address: "XX市XX区XX路88号",
  invitee: "尊敬的宾客",     // 诚邀人名称
  schedule1Time: "10:00", schedule1Text: "迎宾签到",
  ...
  traffic1: "地铁 X 号线至 X 站，出站步行 5 分钟",
  ...
  coverPhoto: "",           // 封面照片，见下文
  musicSrc: "music.mp3"     // 背景音乐，见下文
};
```

**留空规则**：任何字段留空 `""`，对应那一行会自动隐藏（例如 `lunarText: ""` 则农历行不显示；`musicSrc: ""` 则音乐按钮消失）。

改完保存 → 双击文件刷新浏览器即可看到效果。

## 二、配置字段对照表

| 字段 | 含义 | 说明 |
|------|------|------|
| groomName / brideName | 新郎 / 新娘名字 | 会出现在封面、邀请词、落款 |
| dateText | 婚礼日期 | 如 `2026年10月1日 · 星期四` |
| lunarText | 农历日期 | 可留空 |
| timeText | 仪式时间 | 如 `11:08` |
| hotelName | 酒店 / 宴会厅 | 如 `某某大酒店 · 三楼宴会厅` |
| address | 详细地址 | |
| invitee | 诚邀人名称 | 如 `尊敬的宾客` 或具体宾客姓名 |
| schedule1~4 | 婚礼流程 4 组 | 时间 + 内容，可留空 |
| traffic1~3 | 交通方式 3 条 | 可留空 |
| coverPhoto | 封面照片 | `""` / `"cover.jpg"` / base64 |
| musicSrc | 背景音乐 | `""` / `"music.mp3"` / base64 |

> 三个 HTML 文件**各自独立配置**，改完一个记得同步修改另外两个（或只发布你选定的那一版）。

## 三、封面照片

照片放入**本文件夹**，然后：

1. 简单方式：把照片命名为 `cover.jpg`，在 CONFIG 里写 `coverPhoto: "cover.jpg"`。
2. 压缩建议：宽 ≤ 1080px、JPEG 质量 70~80、单张 ≤ 300KB（推荐 [tinypng.com](https://tinypng.com) 在线压缩）。
3. **base64 内嵌**（把照片焊进 HTML 文件里，发单个文件也不丢照片）：
   - PowerShell 运行：
     ```powershell
     [Convert]::ToBase64String([IO.File]::ReadAllBytes('cover.jpg')) | Out-File -Encoding ascii base64.txt
     ```
   - 打开 `base64.txt` 复制全部内容，在 CONFIG 里写：
     ```js
     coverPhoto: "data:image/jpeg;base64,粘贴刚才复制的内容",
     ```

## 四、小地图（已嵌入高德地图，无需 Key）

三版文件均已嵌入**十堰市武当国际酒店**的地图（高德 URI 接口，无需 API Key，访客可缩放查看、可唤起手机高德 App 导航）。

**更换地点**：打开任意 HTML 文件，搜索 `uri.amap.com/marker`，修改 iframe 的 `src` 参数：

- `position=经度,纬度`（高德坐标系，两个数字用英文逗号隔开）
- `name=地点名称`

**获取新地点坐标的方式**：高德网页地图 [www.amap.com](https://www.amap.com) 搜索地点 → 点「分享」→ 复制链接，链接里 `position=` 后的两个数字就是坐标（无需登录）。手机高德 App 同理。

**注意**：地图会吞掉手指在地图区域内的滑动手势，属正常现象，在地图外滑动页面即可。

## 五、背景音乐

- 把 mp3 文件放入本文件夹，命名为 `music.mp3`（或改 CONFIG 的 `musicSrc` 指向你的文件名）。
- 建议 30~60 秒、64kbps 单声道，文件 ≤ 1MB，加载更快。
- **默认静音**：手机浏览器和微信都禁止网页自动播放音乐，所以访客需**点击右下角音乐按钮**才会播放——这是平台限制，无法绕过，按钮已做成明显可见的样子。
- 同理可 base64 内嵌（方法同照片，前缀为 `data:audio/mpeg;base64,`）。
- 没放音乐文件时按钮会变灰，放入文件后自动恢复。

## 六、线上地址与后续更新

**已部署完成** ✅ 线上地址：

> **https://wzywahaha.github.io/wedding-invitation/**

在电脑微信或手机微信里把这个链接发给好友即可（对方点开直接看）。

**后续更新**（改完内容让线上生效）：
1. 改完 HTML 文件保存；
2. 在本文件夹打开终端（Git Bash），运行：
   ```bash
   git add -A && git commit -m "更新" && git push
   ```
3. 约 1 分钟后线上生效（微信里让对方下拉刷新页面）。

> 部署通道说明：本机访问 GitHub 主站受限，已配置 SSH over 443 推送通道（`~/.ssh/config`），后续 push 直接可用，无需重复配置。仓库地址：`github.com/wzywahaha/wedding-invitation`。

## 七、微信发送注意事项

- ✅ 发**链接**给好友：安卓 / iOS / 电脑微信均可直接打开，体验最好。
- ✅ 发 HTML **文件**：安卓可直接打开；**iOS 无法预览**（长按 → 用其他应用打开 → Safari 才行），不推荐发给用苹果手机的长辈。
- ✅ 音乐需点击页面右下角按钮后播放（微信限制）。
- ✅ 分享链接卡片显示标题与缩略图：靠文件 `<head>` 里的 `og:title` / `og:image` 标签（`og:image` 已填好线上绝对地址）。**替换名字/照片后记得同步更新 og:title 并放置 cover.jpg**（部署后微信卡片即显示真实照片）。
- ⚠️ GitHub Pages 仓库默认公开：邀请函内容本就面向所有宾客公开，属正常情况；但请勿把家庭住址等隐私写进邀请函。
- ⚠️ 若个别网络环境打开慢，可让对方稍后重试，或直接把 HTML 文件发给对方在浏览器打开。

## 八、常见问题

**Q：改了 CONFIG 但页面没变化？**
A：浏览器缓存。强制刷新（Ctrl+F5），手机上微信里点右上角 → 刷新。部署场景需等约 1 分钟并刷新。

**Q：想只用一个风格？**
A：把选定版本的链接直接发给宾客即可（如 `https://你的用户名.github.io/wedding-invitation/invitation-chinese.html`），不必发目录页。

**Q：照片不显示？**
A：检查照片文件名与 CONFIG 一致、文件确实在本文件夹；base64 模式下检查是否漏了 `data:image/jpeg;base64,` 前缀。

**Q：音乐按钮灰色？**
A：`music.mp3` 不在同目录，或文件名不一致。放入文件后强制刷新。

**Q：想让花瓣更多/更少？**
A：搜索 `window.innerWidth < 480 ? 14 : 22`，把 14 和 22 改成你想要的数量。
