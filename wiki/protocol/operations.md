# DCP 运营手册

> Debate Commons Protocol — Operations Manual
>
> 本文件是 DCP 章程的配套运营文档。章程定义规则（what），本手册定义执行细节（how）。
> 手册内容可由 Maintainer（维护者）提议、Founding Maintainer（创始维护者）批准后修改，无需触发修宪程序。
>
> 最后更新：2026-07-14
>
> **用语规范**：同章程——正文中文，专用术语保持英文（Badge、Probation、Terminated、Banished、Maintainer、Contributor），技术术语不翻译（LLM-Wiki、Git、PR、Base36、frontmatter）。

---

## 一、LLM-Wiki 技术运营

LLM-Wiki 是协议的公共知识库——一个由结构化 Markdown 文件组成的 Git 仓库。它存储协议的全部信息：成员目录、资源索引、章程、运营手册、赛事记录。LLM-Wiki 是协议的**唯一真相来源**——不存在 Wiki 之外的"另一个版本"。

LLM-Wiki 的组织方式：每个成员、每份资源、每场赛事都是一个独立的 Markdown 页面，通过 YAML frontmatter 标注元数据、通过 [[wikilinks]] 实现交叉引用。这种结构使 AI 代理可以一次性加载整个 Wiki 的上下文——不只是"搜索"，而是"理解"。

### 1.1 目录结构

```
wiki/
  index.md               — LLM-Wiki 首页，含完整导航
  log.md                  — 操作日志（人类可读的关键事件摘要）
  members/                — 成员目录
    _index.md             — 成员全景索引
    individuals/          — Individual 成员页面
    communities/          — Community 成员页面
  protocol/               — 章程正文、本运营手册、规则修改提案
  resources/              — 共享 Database
    _index.md             — 资源全景索引
    motions/              — Motion bank（辩题库）
    methods/              — 训练方法论文档
    transcripts/          — 比赛 transcript
    ai-tools/             — AI coach 使用方法与 prompt 分享
    curated-video/        — 非原创视频整理
    curated-audio/        — 非原创音频整理
    curated-text/         — 非原创文字整理
    english-learning/     — 英语学习资源
    news/                 — 相关新闻文章
    casefiles/            — 辩题案例文件
  events/                 — 赛事与活动日历
  concepts/               — 可跨辩题迁移的理论概念与方法论
```

### 1.2 写入权限

LLM-Wiki 的写入权限分为两级：

| 级别 | 谁可以 | 权限 |
|------|--------|------|
| 提交草稿 | 所有成员（通过协议接入渠道） + 自动化 AI Agent | 创建新页面草稿、修改自己贡献的页面 |
| 合并发布 | Maintainer 成员 + Founding Maintainer | 审核并合并草稿、修改结构页面 |

所有提交通过 Git Pull Request 机制实现。成员不需要知道 Git 是什么——协议接入端负责将其投稿转化为 PR。成员不需要登录 GitHub。

### 1.3 技术实现

LLM-Wiki 托管在 GitHub 公开仓库 `SenranTao/debate-commons-protocol` 中，通过 GitHub Pages 自动渲染为可浏览网页。成员通过接入渠道提交投稿 → 后端自动生成 Pull Request → AI 质量初评 → 通过后合并入 main 分支。平台迁移须经 Maintainer 集体讨论决定。

---

## 二、贡献系统

### 2.1 投稿流程

成员通过协议接入渠道提交资源 → 原始文件存入 `_inbox/raw/`（不被修改）→ AI 自动预处理并生成 Wiki 页面草稿 + 质量初评 → 通过后合并入库。

投稿指南见仓库根目录的 `CONTRIBUTING.md`。详细的 AI 端处理流程见 dcp-ingest 工作流。

**Event 投稿**采用轻量流程：成员提交赛事信息 → Maintainer 确认 → 直接入库。Event 不走 AI 质量初评。

### 2.2 质量初评

AI 对每份投稿进行质量初评，基于完整性、独创性、可用性、格式正确性四个维度。

| 结果 | 处理 |
|------|------|
| 通过 | 自动合并入 LLM-Wiki |
| 不通过 | 退回投稿人，附改进建议 |

初期阶段不设人工审核队列——AI 通不过的稿直接退回。待社群成员规模足够后，可启用人工审核机制（见 `[[future-features|未来功能规划]]`）。

### 2.3 资源类型

原创资源 4 类：motions（辩题库）、methods（训练方法）、transcripts（比赛 transcript）、ai-tools（AI 工具）。

非原创整理（curatorial）6 类：curated-video、curated-audio、curated-text、english-learning、news、casefiles。Curatorial 投稿必须标注原始来源与版权归属——缺来源标注直接退回。

---

## 三、功能权限

功能权限基于投稿数量累计，而非社会身份。不存在"大佬"等级——等级仅表示功能权限的开启状态。

| 功能权限 | 累计投稿数 | 开启的能力 |
|----------|-----------|-----------|
| **Member** | ≥1 | 基本权利：访问资源、置顶互助、优惠费率（需满足贡献前置条件） |
| **Contributor** | ≥3 | + 可参与终止投票（章程第五条） |
| **Maintainer** | 由 Founding Maintainer 任命 | + 发起规则修改提案 + LLM-Wiki 直接写入权限 |

初期手动统计，Founding Maintainer 在成员页更新权限。自动化贡献分系统为未来功能（见 `[[future-features|未来功能规划]]`）。

### 3.1 关键约束

这些名称不是头衔。"我是 Contributor"不是身份声明——是"我贡献了 3 份以上的资源"的功能描述。任何成员不得以其功能权限等级论证其意见权重（章程第九条第 9.5 款：非权威原则）。

---

## 四、违规处理

### 4.1 社区举报

协议不设中央监督机构。合规监督的唯一机制是社区举报。

任何成员可通过接入渠道举报其他成员的违规行为。举报匿名提交、Founding Maintainer 核实。核实确认 → 按 4.2 条处理表执行。无效举报累计三次 → 举报者警告。

### 4.2 处理表

| 违规类型 | 处理 |
|----------|------|
| 置顶义务未履行 | Probation 30 天，补上后恢复 |
| 月度资讯连续两个季度未转发 | Probation 60 天，续发后恢复 |
| 声明不实（经举报核实） | Probation 60 天 |
| 连续 12 个月未贡献 → 再 3 个月仍未 | Terminated |
| 连续 12 个月未贡献 → Terminated 后再次加入、再次 12 个月未贡献 | Banished |
| 冒用 Badge / 伪造贡献记录 / 蓄意破坏 LLM-Wiki | Banished |
| 违反 CC BY-SA 4.0 许可条款 | 首次：告知函 + 公开违规记录。持续不纠正 → 若违规者为成员 → Terminated；若违规者非成员 → 永久禁止入会 |

### 4.3 争议解决

成员对违规判定有异议时，可向 Founding Maintainer 提出申诉。Banished 判定不可申诉。

---

## 五、术语表

| 术语 | 英文 | 定义位置 |
|------|------|----------|
| 辩论信息公地协议 / DCP | Debate Commons Protocol | 章程第一条 |
| 成员 | Member | 章程第二条 |
| Individual | Individual | 章程第二条第 2.4 款 |
| Community | Community | 章程第二条第 2.4 款 |
| LLM-Wiki | LLM-Wiki | 章程第三条 |
| 共享 Database | Shared Database | 章程第一条 |
| Recognition Badge | Recognition Badge | 章程第一条 / 第七条 |
| Preferential Access | Preferential Access | 章程第一条 / 第五条 |
| CC BY-SA 4.0 | Creative Commons Attribution-ShareAlike 4.0 | 章程第三条第 3.3 款 |
| 置顶义务 | Pinning Obligation | 章程第四条第 4.1 款 |
| 月度资讯 | Monthly Digest | 章程第四条第 4.4 款 |
| Contributor | Contributor | 本手册第三章 |
| Maintainer | Maintainer | 本手册第三章 |
| Founding Maintainer | Founding Maintainer | 章程第九条第 9.3 款 |
| Active | Active | 章程第七条第 7.2 款 |
| Probation | Probation | 章程第七条第 7.2 款 |
| Terminated | Terminated | 章程第七条第 7.2 款 |
| Banished | Banished | 章程第七条第 7.2 款 |
| 协议分叉 | Protocol Fork | 章程第九条第 9.4 款 |
| 非权威原则 | Non-Authority Principle | 章程第九条第 9.5 款 |

> 社群跑起来后将重新激活的复杂机制（贡献分、Star、概念提取、Highlights 等）见 `[[future-features|未来功能规划]]`。
