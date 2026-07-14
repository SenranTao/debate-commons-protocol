# DCP 运营手册

> Debate Commons Protocol — Operations Manual
>
> 本文件是 DCP 章程的配套运营文档。章程定义规则（what），本手册定义执行细节（how）。
> 手册内容可由 Maintainer（维护者）提议、Founding Maintainer（创始维护者）批准后修改，无需触发修宪程序。
>
> 最后更新：2026-07-14
>
> **用语规范**：同章程——正文中文，专用术语保持英文（Badge、Probation、Terminated、Banished、Maintainer、Contributor、Star），技术术语不翻译（LLM-Wiki、Git、PR、Base36、frontmatter）。

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
  protocol/               — 章程正文、本运营手册、规则修改提案历史
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
  highlights/             — 精选内容
    weekly/               — AI 周选
    monthly/              — AI 月选
```


### 1.2 写入权限

LLM-Wiki 的写入权限分为两级：

| 级别 | 谁可以 | 权限 |
|------|--------|------|
| 提交草稿 | 所有成员（通过协议接入渠道） + 自动化 AI Agent | 创建新页面草稿、修改自己贡献的页面 |
| 合并发布 | Maintainer 成员 + 协议技术维护者 | 审核并合并草稿、修改结构页面 |

所有提交通过 Git Pull Request 机制实现。成员不需要知道 Git 是什么——协议接入端负责将其投稿转化为 PR。成员不需要登录 GitHub。

### 1.3 技术实现

LLM-Wiki 托管在 GitHub 公开仓库 `SenranTao/debate-commons-protocol` 中，通过 GitHub Pages 自动渲染为可浏览网页。成员通过接入渠道提交投稿 → 后端自动生成 Pull Request → AI 质量初评 → 通过后合并入 main 分支。平台迁移须经全体 Maintainer 投票通过。

### 1.4 月度资讯

协议 AI Agent 每月自动扫描 LLM-Wiki，生成月度资讯简报——包含本月赛事日历、新入库高质量资源（AI初评 ≥4.0）、本月新加入成员。简报通过接入端推送至成员通知群。

成员转发月度资讯的义务见章程第四条第4.4款。

---

## 二、贡献系统

### 2.1 投稿流程

成员通过协议接入渠道提交资源 → 原始文件存入 `_inbox/raw/`（不被修改）→ AI 自动预处理并生成 Wiki 页面草稿 + 质量初评 → 草稿进入审核队列。

投稿指南见仓库根目录的 `CONTRIBUTING.md`。完整的 AI 端处理流程见 dcp-ingest 工作流。

**Event 投稿**采用轻量流程：成员提交赛事信息 → Maintainer 确认 → 直接入库。Event 不走 AI 质量初评。

### 2.2 质量初评

AI 对每份投稿进行质量初评（1-5分），基于完整性、独创性、可用性、格式正确性四个维度。

| 初评分数 | 处理 |
|----------|------|
| ≥4.0 | 自动通过，合并入 LLM-Wiki |
| 2.5–3.9 | 进入人工审核队列 |
| <2.5 | 退回投稿人，附改进建议 |

### 2.3 人工审核

Contributor（贡献分 ≥5）及以上级别的成员可认领审核队列中的投稿。审核通过 → 合并 → 审核者获得审查贡献分（+0.2）。审核否决 → 退回投稿人，不扣分。

### 2.4 贡献分

成员的贡献值由贡献分（Contribution Score）衡量。贡献分由四维度加权合成：

| 维度 | 权重 | 说明 |
|------|------|------|
| 内容贡献 | 40% | 投稿被approve + 质量乘数 |
| 审查贡献 | 20% | 审核他人投稿 |
| 使用信号 | 25% | 贡献被其他页面引用 / 资源被查询 / 被Star |
| 时效性 | 15% | 贡献随时间温和衰减（12个月后 ×0.3） |

贡献分仅成员本人可见（通过接入渠道私聊查询）。不设公开排行榜。分数产出功能权限（见第三章）。

**质量乘数**：一篇质量初评 4.5 分的投稿，内容贡献得 1 × 1.5 = 1.5 分。一篇初评 2.8 分勉强通过的投稿，得 1 × 0.8 = 0.8 分。灌水没有收益。

### 2.5 概念提取

AI 定期扫描 LLM-Wiki 中新入库的资源，自动建议可提取为独立概念页面的内容。建议由任意 Maintainer 确认后执行。概念页面构成协议的"集体思维"——可跨辩题、跨赛事迁移的分析框架与方法论。

### 2.6 Star 认可

成员可通过接入渠道对 LLM-Wiki 中任何资源页面给予 Star 认可。Star 是成员对资源质量的背书——不是收藏夹、不是书签。

**技术实现**：每个资源页面的 frontmatter 维护 `stars` 计数与 `starred_by` 名单。成员发送指令 → 系统更新 frontmatter → 提交 PR。一次 Star、一次 PR、不可撤销。

**不公开计数**：资源的 Star 数不公开显示在页面上。成员查询时，系统返回"此资源当前获得 N 位协议成员的认可"。这是信号，不是排行榜。

**月度发现**：协议 AI Agent 每月扫描 Star 数据，生成"近期获得最多新增认可的 Top 10 资源"清单，随月度资讯一并推送。清单不含具体 Star 数——仅列出资源页面与一句话摘要。目的是让高价值资源被更多成员看到，而非制造数字竞赛。

Star 数据同时作为贡献分"使用信号"维度的输入之一。

---

## 三、功能权限

### 3.1 权限表

贡献分产出**功能权限**，而非社会身份。不存在"大佬"等级——等级仅表示功能权限的开启状态。

| 功能权限 | 贡献分阈值 | 开启的能力 |
|----------|-----------|-----------|
| **Member** | 0–4.9 | 基本权利：访问资源、置顶互助、优惠费率（需满足贡献前置条件） |
| **Contributor** | 5–29.9 | + 投稿快速审核通道 + 审核他人投稿 + 参与终止投票（见投票权） |
| **Maintainer** | 30+ | + 发起规则修改提案 + 参与规则修改投票 + LLM-Wiki 写入权限 |

升级由系统自动计算，无需申请。贡献分因时效衰减而跌破阈值时，对应功能权限自动关闭——这不是惩罚，不是降级，是功能随贡献量的自然波动。

### 3.2 投票权

| 权限级别 | 投票权 |
|----------|--------|
| Member | 无投票权 |
| Contributor | 可参与针对个人的投诉投票（如：是否终止某成员资格） |
| Maintainer | 可发起协议规则修改提案、可参与规则修改投票 |

### 3.3 关键约束

这些名称不是头衔。"我是 Contributor"不是身份声明——是"我的投稿走快速通道"的功能描述。任何成员不得以其功能权限等级论证其意见权重（章程第九条第9.5款：非权威原则）。

---

## 四、违规处理

### 4.1 社区举报

协议不设中央监督机构。合规监督的唯一机制是社区举报。

任何成员可通过接入渠道举报其他成员的违规行为。举报匿名提交、人工核实。核实确认 → 按4.2条处理表执行。无效举报累计三次 → 举报者贡献分扣0.5（防止滥用举报）。

### 4.2 处理表

| 违规类型 | 处理 |
|----------|------|
| 置顶义务未履行 | Probation 30天，补上后恢复 |
| 月度资讯连续两个季度未转发 | Probation 60天，续发后恢复 |
| 声明不实（经举报核实） | Probation 60天 + 贡献分减半 |
| 连续12个月未贡献 → 再3个月仍未 | Terminated |
| 连续12个月未贡献 → Terminated后再次加入、再次12个月未贡献 | Banished |
| 冒用Badge / 伪造贡献记录 / 蓄意破坏LLM-Wiki | Banished |
| 违反 CC BY-SA 4.0 许可条款 | 首次：告知函 + 公开违规记录。持续不纠正 → 若违规者为成员 → Terminated；若违规者非成员 → 永久禁止入会 |

### 4.3 争议解决

成员对违规判定有异议时，可提出申诉。申诉由随机选取的三名 Contributor 及以上成员组成临时小组裁决。临时小组的裁决为最终裁决。Banished 判定不可申诉。

---

## 五、术语表

| 术语 | 英文 | 定义位置 |
|------|------|----------|
| 辩论信息公地协议 / DCP | Debate Commons Protocol | 章程第一条 |
| 成员 | Member | 章程第二条 |
| Individual | Individual | 章程第二条第2.4款 |
| Community | Community | 章程第二条第2.4款 |
| LLM-Wiki | LLM-Wiki | 章程第三条 |
| 共享 Database | Shared Database | 章程第一条 / 第三条第3.2款 |
| Recognition Badge | Recognition Badge | 章程第一条 / 第七条 |
| Preferential Access | Preferential Access | 章程第一条 / 第五条 |
| CC BY-SA 4.0 | Creative Commons Attribution-ShareAlike 4.0 | 章程第一条第1.2款 / 第三条第3.2款 |
| 置顶义务 | Pinning Obligation | 章程第四条第4.1款 |
| 月度资讯 | Monthly Digest | 本手册第一章 / 章程第四条第4.4款 |
| 贡献分 | Contribution Score | 本手册第二章 |
| Star | Star | 本手册第2.6节 |
| Contributor | Contributor | 本手册第三章 |
| Maintainer | Maintainer | 本手册第三章 |
| Founding Maintainer | Founding Maintainer | 章程第九条第9.3款 |
| Active | Active | 章程第七条第7.2款 |
| Probation | Probation | 章程第七条第7.2款 / 本手册第四章 |
| Terminated | Terminated | 章程第七条第7.2款 / 本手册第四章 |
| Banished | Banished | 章程第七条第7.2款 / 本手册第四章 |
| 协议分叉 | Protocol Fork | 章程第九条第9.4款 |
| 非权威原则 | Non-Authority Principle | 章程第九条第9.5款 |
