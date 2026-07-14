# INTO Guard — Demo 设计 Spec

> 日期:2026-07-13
> 定位:**INTO Guard — your spending gatekeeper**
> 一句话:帮你把不该花的钱,真的省下来。
> 形态:单文件 `index.html`,412px 手机外框,全英文,假数据 + localStorage,双击即开(与 INTO- After School 同一套技术房子)。

---

## 0. Demo 的唯一目标

让看的人在 30 秒内经历一次「卧槽,我一直在漏钱」的情绪:

```
Connect inbox → AI scans receipts → finds 11 subs, 3 forgotten
   → swipe open a zombie sub → see "How to cancel" → mark cancelled
   → Home counter ticks up: "Saved this year: $215 ↑"
```

产品部分的一切,都为最快抵达这个 Aha 时刻服务。

---

## 1. 范围(Scope)

**做:**
- 产品体验:5 个界面 + 完整情绪主线
- 商业蓝图页(投资人视角,全英)
- 纯前端、假数据、localStorage 状态、Reset 还原

**不做(demo 阶段明确排除):**
- 真实邮箱/银行接入(用模拟扫描动画代替)
- 真实退订(用 "Mark as cancelled" 状态代替)
- 后端、账号系统、真实数据
- 桌面/响应式布局(手机优先,单尺寸打磨)

---

## 2. 信息架构 / 导航

- 顶部:品牌行 `INTO Guard · your spending gatekeeper` + 右上 **Reset**(还原演示数据)
- 底部 Tab 四项:**Home / Subscriptions / Fixed / Blueprint**
- 首次进入先走 **Connect** 引导屏(模拟扫描),扫完落到 Home

---

## 3. 产品部分 · 5 个界面

### 3.1 Connect(首屏引导)
- 文案:*"Connect your inbox — we'll find every subscription hiding in it."*
- 一颗按钮 `Connect email`(模拟,不真连)
- 点击 → 动画 *"Scanning your receipts…"* 逐条冒出发现的服务名 → 完成后进 Home
- 目的:展示「零门槛入口 + AI 自动发现」,制造第一个惊喜

### 3.2 Home / Dashboard
- **Hero 卡**:*This month · $342 fixed*,一条进度条 *fixed 68% of income*
- **红色横幅**:**⚠ 3 forgotten subscriptions · $430/yr leaking**(点击跳 Subscriptions 并高亮)
- **省钱计数器**:*Saved this year: $0*(退订后跳动上涨,正反馈)
- **分类总览**(可点进):Subscriptions / Housing / Insurance / Utilities,各显月度小计

### 3.3 Subscriptions(订阅列表)
- 逐条卡片:图标(emoji/首字母)、名称、金额、周期、*next charge Jul 20*
- **僵尸高亮**:*unused 4 months*(醒目色),排在最前
- **涨价徽章**:*↑ was $12.99*
- 点卡片 → Detail

### 3.4 Detail + Cancel guide(退订引导 L1,核心差异点)
- 顶部:服务名、金额、last charge / next charge
- **How to cancel**:针对该服务的分步指南(如 *"Netflix: cancel on the website — you can't do it in the app"*)
- **退订前检查**:*You still have 12 days left on this cycle.*
- 按钮 **Mark as cancelled** → 🎉 *You just saved $215/yr* → 写入 localStorage,Home 计数器上涨,列表标记划掉

### 3.5 Fixed expenses(固定支出)
- 房贷 / 车贷 / 房租 / 保险 / 水电,主打「看清 + 预警」(不可砍,不给退订)
- 每项显示金额、扣款日、占比
- 一张**推送样式卡**:*Disney+ will charge $488 in 3 days*(展示「扣款前提醒 = 守门人」叙事)

---

## 4. 商业蓝图页(Blueprint,投资人视角,全英)

竖向路演,顺序:
1. **Problem** — The subscription economy is built on your forgetting.(僵尸订阅、悄悄涨价、dark pattern 退订)
2. **Solution** — The gatekeeper: **find · watch · cancel**.
3. **Why now** — Email receipts + AI parsing make auto-discovery finally feasible.
4. **Market** — TAM(占位数字,标注 illustrative)
5. **Business model** — Consumer subscription / savings cut / **B2B SaaS-spend management**
6. **Moat** — Data trust + coverage;持续监控(涨价/新订阅)治留存悖论
7. **vs Rocket Money** — 差异化:地域(它主吃美国)/ 人群(B 端 SaaS 治理)/ 深度(帮你砍价换平替)
8. **Roadmap** — P0 / P1 / P2(取自订阅守门人蓝图文档)

> 数字均为占位、标注 *illustrative*,避免误导。

---

## 5. 视觉方向

- 延续 INTO 暖纸质感,但因是**理财产品**,主色向「可信」偏移:
  - 暖纸底(paper)+ 沉稳 **teal / 墨绿**作信任主色 + **honey** 作强调(省钱正反馈高光)
  - 和 After School 一眼是一家人,但气质更「稳」
- 数字用衬线体(呼应 After School 的 `--font-num`),强调「账本」感
- 僵尸订阅 / 漏钱用暖红警示;已省金额用 honey 高光

---

## 6. 技术方案

- 单文件 `index.html`:HTML + 内联 CSS + 内联 JS,零依赖零构建
- `lang="en"`,全部文案英文
- 假数据写死在 JS 顶部一个 `DATA` 对象(订阅、固定支出、金额)
- **状态**:localStorage 存「已退订的订阅 id」「已省金额」「是否已 connect」
- **Reset**:清空 localStorage,还原初始演示数据
- 交互:Tab 切换、扫描动画、退订标记 + 计数器跳动、横幅点击跳转高亮

---

## 7. 文件结构

```
INTO- Guard/
├── index.html        # 全部 demo(单文件)
├── README.md         # 怎么打开 + 功能说明(全英 or 中英)
└── docs/
    └── 2026-07-13-into-guard-demo-design.md   # 本文档
```

---

## 8. 验收标准(demo 做完怎么算成功)

1. 双击 `index.html` 离线可用
2. 走完情绪主线:Connect → 发现漏钱 → 退订 → 看到 Saved 上涨,全程顺滑
3. 四个 Tab 都有内容,Blueprint 页可路演
4. Reset 能还原
5. 视觉精致度对齐 After School demo
