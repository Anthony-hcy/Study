# BulletinIT 校园通知聚合 —— GitHub Actions 全自动部署教程

> 本教程记录如何零服务器、零本地部署，把 [BulletinIT](https://github.com/YDX-2147483647/bulletin-issues-transferred)（北京理工大学通知聚合项目）改造成 **GitHub Actions 定时抓取 + 自动提交结果回仓库 + 钉钉机器人按校区分组推送** 的私人通知系统。
>[Notice: Notice BIT各种网站的通知](https://github.com/Anthony-hcy/Notice)
> 全程只在浏览器里操作即可完成，不需要在本地安装任何东西。

---

## 目录

1. [方案架构总览](#一方案架构总览)
2. [前置准备：创建钉钉机器人](#二前置准备创建钉钉机器人)
3. [Fork 仓库并启用 Actions](#三fork-仓库并启用-actions)
4. [配置 GitHub Secrets](#四配置-github-secrets)
5. [关键决策：要不要 WebVPN 代理（方案 A / B）](#五关键决策要不要-webvpn-代理方案-a--b)
6. [修改推送代码（去代理 + 分组 + 条数）](#六修改推送代码)
7. [添加 / 修改通知源](#七添加--修改通知源)
8. [创建 Workflow 文件](#八创建-workflow-文件)
9. [手动运行验证](#九手动运行验证)
10. [定时规则：改成每天早上发送](#十定时规则)
11. [RSS 的使用](#十一rss-的使用)
12. [微信推送（可选扩展）](#十二微信推送可选扩展)
13. [日常维护](#十三日常维护)
14. [踩坑记录 / FAQ](#十四踩坑记录--faq)

---

## 一、方案架构总览

```
GitHub Actions 定时触发 (cron, UTC 时区)
   │
   ├─► deno task update-server-ding
   │      抓取各网站公开通知 → 更新 output/notices.json
   │      → 生成 output/feed.rss
   │      → 新通知推送到钉钉群（按 BIT / BITZH 分两条消息）
   │
   └─► git 强制提交 output/ 回仓库（持久化，方式二）
          notices.json 与 feed.rss 以 commit 形式永久保存
```

**为什么选这套**：

| 对比项 | 本方案 | 官方部署（自有服务器） | GitHub Pages |
|---|---|---|---|
| 需要服务器 | ❌ 不需要 | ✅ 需要 | ❌ |
| 结果持久化 | ✅ commit 回仓库 | ✅ | ✅ |
| 推送提醒 | ✅ 钉钉/微信 | ✅ | ❌ 无主动推送 |
| 费用 | ✅ 公共仓库 Actions 免费无限量 | 服务器费用 | 免费 |

**仓库必须 Public**：

- 公共仓库 Actions 完全免费；私有仓库每月仅 2000 分钟额度（本任务每月约耗 1500~3500 分钟，会超额）
- 所有抓取的都是公开通知，`notices.json` 无隐私
- 钉钉密钥存在 GitHub Secrets 里，public 仓库也绝对不可见，日志中自动打码
- public 仓库 fork 出来的必然也是 public，顺其自然即可

---

## 二、前置准备：创建钉钉机器人

1. 钉钉电脑端 → 打开一个你管理的群（可以建一个只有自己的群）→ 群设置 → **机器人** → 添加机器人 → **自定义（通过 Webhook 接入自定义服务）**
2. 安全设置**务必选择「加签」**
   > ⚠️ 千万不要选「IP 白名单」——GitHub Actions 的出口 IP 不固定，选了就永远发不出去
3. 记下两样东西，稍后要放进 GitHub Secrets：
   - **Webhook 地址**：形如 `https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxx`
   - **加签密钥**：`SEC` 开头的一串字符
4. 已知行为：原项目的逻辑是**每次运行都发消息**，没有新通知时也会发一条"未发现新通知"。本文第六节的代码已将其改为"无新通知才发提示"，且支持自定义每条消息最多列出的通知数。

---

## 三、Fork 仓库并启用 Actions

### 1. Fork

打开 <https://github.com/YDX-2147483647/bulletin-issues-transferred> → 右上角 **Fork**

- 弹出的 *Copy the main branch only* **勾选 ✅**
  （定时任务只认默认分支 main，其他分支无用；想看作者的实验分支直接去原仓库看）

### 2. 启用 Actions（最容易漏的一步！）

进入你 fork 出来的仓库 → 顶部 **Actions** 标签页 → 点击绿色按钮

> **I understand my workflows, go ahead and enable them**

⚠️ **Fork 下来的仓库 Actions 默认是禁用的**，不点这个按钮一切都不会运行。

### 3. （可选）改名

仓库 Settings → General → Rename，可以改成自己想要的名字（如 `Notice`）。改名不影响任何功能。

---

## 四、配置 GitHub Secrets

进入你仓库的 **Settings → Secrets and variables → Actions** → 点 **New repository secret**，逐条添加：

| Name（严格照抄） | Value |
|---|---|
| `DING_WEBHOOK` | `https://oapi.dingtalk.com/robot/send?access_token=你的token` |
| `DING_SECRET` | `SEC` 开头的加签密钥 |

**Secret 命名规则报错怎么办**：Name 框只允许字母、数字、下划线，且必须字母开头。

- ❌ `DING-WEBHOOK`（连字符不行）→ ✅ `DING_WEBHOOK`
- ❌ `DING WEBHOOK`（空格不行）→ ✅ `DING_WEBHOOK`
- ❌ 把 URL 粘进了 Name 框 → URL 属于 Value，填到下面的框里

> 若采用[方案 A](#五关键决策要不要-webvpn-代理方案-a--b)，还需要再加两条：
>
> | Name | Value |
> |---|---|
> | `PROXY_USERNAME` | 北理工统一身份认证账号（学号） |
> | `PROXY_PASSWORD` | 统一身份认证密码 |

---

## 五、关键决策：要不要 WebVPN 代理（方案 A / B）

读了项目源码会发现：`src/examples/server-ding-cli.ts` 在 import 时就会**无条件执行 WebVPN 登录**，而 `config/config.yml` 里 `proxy.match` 列表中的域名（`student.bit.edu.cn` 等）本来就是要走 WebVPN 才能访问的校内地址。所以有两条路：

### 方案 B：不用代理（推荐起步）✅

- 做法：注释掉 `server-ding-cli.ts` 中 proxy 相关的两行（见第六节）
- 优点：**不需要提供校园账号密码**，纯公网抓取
- 代价：`student.bit.edu.cn` 等校内域名的源会抓取失败（日志出现 `TypeError: fetch failed` 和 `未从"Bit_资助公示"获取到任何通知。将忽略`）
- 处置：程序会自动跳过失败源继续运行，**不影响其他源**；若确实需要这些源，要么删掉对应 source 配置，要么升级到方案 A

### 方案 A：完整还原官方部署

- 做法：保留 proxy 代码，Secrets 里配好 `PROXY_USERNAME` / `PROXY_PASSWORD`，workflow 中取消生成 `config/proxy.secrets.yaml` 段落的注释
- 优点：所有源都能抓
- 代价：校园账号密码进入 CI 环境（存于 Secrets，风险可控但需知情）；WebVPN 异地登录理论上可能与本人登录互踢

**建议**：先用方案 B 跑通，发现必需的源抓不到再切方案 A。

---

## 六、修改推送代码

网页编辑 `src/examples/server-ding-cli.ts`，做两件事：

### 1. 注释掉代理（方案 B 必做）

```typescript
// import add_proxy_hook from '../plugin/proxy/index.ts'   ← 行首加 //

add_hook.verbose(hook)
add_hook.progress_bar(hook)
// add_proxy_hook(hook)                                    ← 行首加 //
```

> ⚠️ **TypeScript 的行注释是 `//`，不是 `#`！**
> 写成 `# import ...` 会直接报语法错误：
> `SyntaxError: Expected ';', '}' or <eof>`
> 且 import 和调用两处必须一起处理，只改一处会报 `Cannot find name 'add_proxy_hook'`

### 2. 替换末尾推送逻辑（按校区分组 + 可调条数）

把从 `const { new_notices, change } = await update_notices()` 开始到文件结尾的部分，整体替换为：

```typescript
const { new_notices, change } = await update_notices()

const MAX_ITEMS = 20 // ← 每条钉钉消息最多列出的通知数，想发多少改这里

function group(prefix: string) {
  return new_notices.filter((n) => n.source.name.startsWith(prefix))
}

async function send_group(title: string, list: typeof new_notices) {
  if (list.length === 0) return
  const rows = [
    `发现 ${list.length} 项新通知。`,
    ...list.slice(0, MAX_ITEMS).map((n) => "- " + n.to_markdown()),
    ...(list.length > MAX_ITEMS ? [`（其余 ${list.length - MAX_ITEMS} 项见 RSS）`] : []),
  ]
  await robot.markdown(title, rows.join("\n\n"))
}

await send_group(`【BIT】发现新通知`, group("Bit_"))
await send_group(`【BITZH】发现新通知`, group("Bitzh_"))

if (new_notices.length === 0) {
  await robot.markdown("未发现新通知", `未发现新通知。（过期 ${change.drop} 项）`)
}
```

效果说明：

- 来源 `name` 以 `Bit_` 开头的归入 **【BIT】** 消息，以 `Bitzh_` 开头的归入 **【BITZH】** 消息，分别推送
- 某组没有新通知则跳过不发；全部没有才发一条"未发现新通知"
- ⚠️ **所有来源的 `name` 都必须带 `Bit_` 或 `Bitzh_` 前缀**，否则会被漏发（例如原配置里的 `"人文素质"` 就需要手动改成 `"Bit_人文素质"`）
- 数据层无损：`notices.json` 和 RSS 仍收录全部通知，分组只影响钉钉展示
- 钉钉机器人限流为每分钟 20 条，这里最多发 2~3 条，安全

---

## 七、添加 / 修改通知源

编辑 `config/sources_by_selectors.json`，在 `"sources"` 数组末尾追加对象（注意上一个对象后补逗号）。

> 💡 在 GitHub 网页按 `.` 键可打开网页版 VS Code，编辑该文件时有 JSON Schema 自动补全和校验。

### 字段说明

| 字段 | 必填 | 说明 |
|---|---|---|
| `name` | ✅ | 来源简称，**全局唯一**，会展示给读者；同时是钉钉分组的依据（须带 `Bit_`/`Bitzh_` 前缀） |
| `full_name` | | 全名 |
| `alt_name` / `obsolete_name` | | 别名 / 旧名（用于搜索和历史兼容） |
| `url` | ✅ | 通知列表页地址 |
| `guide` | | 导航指引（告诉别人怎么一步步点到这个页面） |
| `selectors.rows` | ✅ | 定位"每一条通知"的 CSS 选择器（通常是 `<li>`） |
| `selectors.link` | | 定位链接，默认 `"a"` |
| `selectors.title` | | 定位标题；**仅当标题不在链接元素内时需要**（默认复用 link） |
| `selectors.date` | | 定位日期；页面没有可用日期时填 `null` |

选择器都是在 `rows` 匹配到的每一行内部**相对查找**的。

### 实际添加过的源（选择器均已验证）

<details>
<summary><strong>北京主站（Bit_ 前缀）</strong></summary>

```json
{
  "name": "Bit_报到须知",
  "full_name": "迎新网·报到须知",
  "url": "https://hi.bit.edu.cn/rxbd/bdxz/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_入学提示",
  "full_name": "迎新网·入学提示",
  "url": "https://hi.bit.edu.cn/rxbd/rxts/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_助困入学",
  "full_name": "迎新网·助困入学",
  "url": "https://hi.bit.edu.cn/zkjx/zkrx/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_贫困资助",
  "full_name": "迎新网·贫困资助",
  "url": "https://hi.bit.edu.cn/zkjx/pkzz/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_奖学设置",
  "full_name": "迎新网·奖学设置",
  "url": "https://hi.bit.edu.cn/zkjx/jxsz/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_勤工助学",
  "full_name": "迎新网·勤工助学",
  "url": "https://hi.bit.edu.cn/zkjx/qgzx/index.htm",
  "selectors": { "rows": ".rt01_list > ul > li" }
},
{
  "name": "Bit_研院培养",
  "full_name": "北京理工大学研究生院·培养信息",
  "url": "https://grd.bit.edu.cn/pygz/pyxx/index.htm",
  "selectors": {
    "rows": ".page-list5 .block-list > li",
    "title": ".gpArticleTitle",
    "date": ".gpArticleDate"
  }
}
```

迎新网（hi.bit.edu.cn）全站列表结构统一为 `<div class="rt01_list"><ul><li><span>[日期]</span><a>`，故共用同一组选择器。
</details>

<details>
<summary><strong>珠海校区（Bitzh_ 前缀）</strong></summary>

```json
{
  "name": "Bitzh_研究生院",
  "full_name": "北京理工大学（珠海）研究生院",
  "url": "https://grd.bitzh.edu.cn/tzgg/index.htm",
  "guide": ["研究生院官网", "通知公告"],
  "selectors": {
    "rows": ".page-list33 ul.block-list > li",
    "link": "a",
    "title": ".title",
    "date": null
  }
},
{
  "name": "Bitzh_研院招生",
  "full_name": "北京理工大学（珠海）研究生院·硕士研究生招生",
  "url": "https://grd.bitzh.edu.cn/zsgz/ssyjs/index.htm",
  "selectors": {
    "rows": ".page-list33 ul.block-list > li",
    "link": "a",
    "title": ".title",
    "date": null
  }
},
{
  "name": "Bitzh_研院培养",
  "full_name": "北京理工大学（珠海）研究生院·培养信息",
  "url": "https://grd.bitzh.edu.cn/pygz/pyxx/index.htm",
  "selectors": {
    "rows": ".page-list33 ul.block-list > li",
    "link": "a",
    "title": ".title",
    "date": null
  }
}
```

珠海研院整站模板统一；其日期被拆分为 `<div class="day">08.08</div><div class="month">2026</div>` 两个元素，无法用一个选择器取出完整日期，故 `date` 填 `null`（仅影响 RSS 条目不带发布时间，功能无损）。`title` 单独指定是因为标题文字在内层 `<div class="title">`，与 `<a>` 不是同一元素。
</details>

### 如何为新网站找选择器（通用方法）

1. 浏览器打开通知列表页 → F12 审查元素
2. 找到包裹"每一条通知"的最小重复容器（常见 `<li>`），记下它的父级 class → 这就是 `rows`
3. 看行内链接是不是直接 `<a>`：是则 `link` 用默认值；标题文字若在别处（如 `.title`），补 `title`
4. 看日期元素：有独立元素就写选择器，拆分/缺失就写 `null`
5. 提交后手动跑一次 workflow 验证；**某源显示 0 条通常意味着 rows 选择器没写对**（而不是报错）

---

## 八、创建 Workflow 文件

网页新建文件 `.github/workflows/update.yml`，内容如下（默认方案 B；若选方案 A，取消注释 proxy 段落并恢复 server-ding-cli.ts 中的 proxy 两行）：

```yaml
name: Update notices

on:
  schedule:
    - cron: '50 23 * * *'   # UTC 23:50 → 北京时间每天早上 07:50
  workflow_dispatch:         # 允许在 Actions 页面手动触发

permissions:
  contents: write            # 允许 GITHUB_TOKEN 提交回仓库

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: denoland/setup-deno@v2
        with:
          deno-version: v2.x # 若报依赖兼容性错误，改成 v1.46.3 再试

      - name: Generate secrets file
        run: |
          cat > config/ding.secrets.yaml <<EOF
          webhook: ${{ secrets.DING_WEBHOOK }}
          secret: ${{ secrets.DING_SECRET }}
          EOF
          # ── 仅方案 A 需要：取消以下注释 ──
          # cat > config/proxy.secrets.yaml <<EOF
          # username: ${{ secrets.PROXY_USERNAME }}
          # password: ${{ secrets.PROXY_PASSWORD }}
          # EOF

      - name: Install dependencies
        run: deno install

      - name: Fetch + RSS + DingTalk
        run: deno task update-server-ding

      - name: Commit results back to repo
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -f output/
          git diff --cached --quiet || (git commit -m "chore: 更新通知数据" && git push)
```

两个细节的原因：

- `git add -f`：项目根 `.gitignore` 忽略了 `/output` 目录，不加 `-f` 会什么也提交不上
- `git diff --cached --quiet ||`：没有变化就不产生空提交

---

## 九、手动运行验证

1. 仓库 → **Actions** → 左侧 *Update notices* → 右侧 **Run workflow** → Run
2. 点进运行记录查看日志，正常应看到：
   - `info: 发现N个通知来源。（cli）`
   - 各源抓取进度条
   - `发现 N 项新通知。` 或 `未发现新通知。`
   - 最后一步 Commit 成功
3. 检查三个产物：
   - ✅ 钉钉群收到 `【BIT】…` / `【BITZH】…` 消息
   - ✅ 仓库出现新 commit，包含 `output/notices.json` 和 `output/feed.rss`
   - ✅ 日志无 Uncaught Error

---

## 十、定时规则

cron 写在 `update.yml` 的 `on.schedule` 下，五个字段为 `分 时 日 月 周`。

**⚠️ GitHub Actions 使用 UTC 时间，北京时间 = UTC + 8**：

| 想要的效果（北京） | cron 表达式 |
|---|---|
| 每天 07:50 发送（当前配置） | `'50 23 * * *'` |
| 每天 07:35 触发（保证最晚 7:50 收到） | `'35 23 * * *'` |
| 每 2 小时一次 | `'17 */2 * * *'` |

预期管理：GitHub 定时任务高峰期普遍延迟几分钟到二三十分钟，"7:50 的定时"实际大约 7:52~8:20 之间收到。若要求"最晚 7:50 必达"，就把触发时间提前（如上表第二行）。

抓取和推送在同一流程内完成，天然同步，无需分别设置。

---

## 十一、RSS 的使用

`output/feed.rss` 每次 run 后自动更新在仓库里，订阅地址：

```
# jsDelivr CDN（国内一般可用，推荐）
https://cdn.jsdelivr.net/gh/<你的用户名>/<仓库名>@main/output/feed.rss

# GitHub raw（国内不稳定，备用）
https://raw.githubusercontent.com/<你的用户名>/<仓库名>/main/output/feed.rss
```

顺手把 `config/config.yml` 里 `rss:` 的 `href` 改成上面的 jsDelivr 地址（它会写入 RSS 元数据，部分严格的阅读器会校验一致性）。

订阅方式：任选一款 RSS 阅读器，把上面的 URL 添加为订阅源即可。

| 平台 | 推荐 App |
|---|---|
| iOS | NetNewsWire（免费）、Reeder |
| Android | Feedme、Follow |
| 桌面/全平台 | Follow、Feedly、Inoreader |

> 由于钉钉每天早上已经推送了全部新通知，RSS 的定位是"随时回看历史 + 多设备浏览"的补充。

---

## 十二、微信推送（可选扩展）

**微信公众号**：自己建号做自动推送需要常驻服务器响应微信的开发者签名验证，GitHub Actions 无法应答，此路不通。
**微信读书/微信阅读**：封闭生态，无 RSS 能力，与此无关。

✅ 正确姿势是借用第三方已认证公众号当管道——**Server酱**（sct.ftqq.com）或 **PushPlus**（pushplus.plus）：

1. 微信扫码登录 Server酱 → 复制你的 `SendKey`
2. GitHub Secrets 添加：Name 填 `SERVERCHAN_SENDKEY`，Value 填 SendKey
3. workflow 的 secrets 生成步骤追加环境变量传递，并在 `server-ding-cli.ts` 的 `send_group` 里追加一段 POST（消息即以公众号会话形式出现在你的微信里）

体验上等同于"通知进了微信"，而你要做的只是填一个 Key，依然零部署。

---

## 十三、日常维护

| 事项 | 操作 |
|---|---|
| 同步上游更新 | 仓库首页点 **Sync fork**；若你对同文件有改动产生冲突需手动合并（注意合并后检查自己的源配置是否还在） |
| 调整每次发送的通知条数 | 改 `server-ding-cli.ts` 顶部的 `MAX_ITEMS` |
| 调整历史保留期 | 改 `config/config.yml` 的 `save_for`（默认 90 天，超期旧通知自动清理，消息中"过期 X 项"即本次被清理数） |
| 定时任务突然不跑了 | GitHub 对 60 天无活动的仓库会暂停 schedule。本项目 bot 每 24h 有 commit，一般不会触发；万一收到 GitHub 邮件通知，去 Actions 页面重新 enable 即可 |
| 密钥轮换 | 钉钉机器人删除重建后，同步更新 Secrets 里的两个值 |
| Deno 版本报错 | 把 workflow 里 `deno-version: v2.x` 改为 `v1.46.3` |

---

## 十四、踩坑记录 / FAQ

以下是本次部署过程中真实遇到或极易遇到的坑，按出现顺序排列：

### ① Fork 时
- *Copy the main branch only* → 勾选
- Fork 后 **Actions 默认禁用**，必须去 Actions 页手动启用

### ② Secrets 命名报错
- `Secret names can only contain alphanumeric characters...`
- Name 只能字母数字下划线、字母开头；URL 是 Value 不是 Name；不能有空格/连字符

### ③ TypeScript 注释符号
- 注释代码用 `//`，不是 Python/YAML 的 `#`；import 与调用两处要一起处理

### ④ 运行报 `找不到来源：留学生`（Uncaught Error）
- **原因**：workflow 会把 `output/notices.json` 提交回仓库持久化；之后如果**删除或重命名**了某个源（如把"人文素质"改为"Bit_人文素质"），历史库里旧来源的通知就成了"孤儿"，RSS 渲染时按名字反查失败而崩溃
- **修复**：删除仓库中的 `output/notices.json`（连同 `output/feed.rss`），重新运行，从头积累
- **铁律**：凡是删除源、给源改名，先删 `notices.json` 再跑；只新增源不受影响

### ⑤ 个别源 `TypeError: fetch failed`
- `student.bit.edu.cn` 等校内域名公网直连不通属预期（官方靠 WebVPN 抓），程序会跳过并继续
- 需要这些源 → 切方案 A 配置 WebVPN Secrets；不需要 → 无视或删掉对应 source

### ⑥ 日志末尾 `Object.prototype.__proto__` hint
- 仅是提示信息，非失败原因，无需理会 `--unsafe-proto`

### ⑦ 其他概念澄清
- **`save_for: 90`**：notices.json 历史通知保留 90 天后自动清理，属正常维护
- **Artifact 90 天**：本方案没用 artifact，无关
- **钉钉限流**：每分钟 20 条，本流程最多发几条，安全
- **RSS 无发布日期**：部分源页面日期结构特殊（如珠海研院拆分式日期），`date: null` 只影响 RSS 条目时间戳，不影响功能

---

## 附：最终改动清单（相对 upstream/main）

| 文件 | 改动 |
|---|---|
| `.github/workflows/update.yml` | 新增：定时抓取 + secrets 注入 + 强制提交 output/ |
| `src/examples/server-ding-cli.ts` | 移除 proxy 依赖（方案 B）；推送逻辑改为 BIT/BITZH 分组、MAX_ITEMS 可调、无新通知才发提示 |
| `config/sources_by_selectors.json` | 新增 9 个源（6 主站 + 3 珠海）；"人文素质"更名"Bit_人文素质" |
| `config/config.yml` | （建议）`rss.href` 改为 jsDelivr 地址；`save_for` 按需调整 |
| GitHub Secrets | `DING_WEBHOOK`、`DING_SECRET`（+ 可选 `PROXY_USERNAME`、`PROXY_PASSWORD`、`SERVERCHAN_SENDKEY`） |

祝使用愉快！有问题优先对照第十四节排查，其次翻 Actions 运行日志定位是哪个源、哪一步出错。
