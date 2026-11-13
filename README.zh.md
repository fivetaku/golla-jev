[English](README.md) | [한국어](README.ko.md) | 中文 | [日本語](README.ja.md) | [Español](README.es.md)

# golla-jev（골라제브）

<p align="center">
  <img src="assets/golla-jev-hero.png" alt="golla-jev" width="320">
</p>

> 韩国市场的摊主会吆喝"골라 골라～（随便挑～）"。办公室里的人也整天在挑：这张收据算哪个费用科目，这封邮件要不要回，这条条款有没有风险。golla-jev 找出工作中这些**挑选的瞬间**，设计把它们交给 Jev 的工作流。

Jev（TypeSafe System One）不写文字，只从给定选项中挑选，用概率作答，每次约 0.2 秒。golla-jev 接收你的业务描述，给出这样的设计：挑选交给 Jev，计算、日期和核对交给代码，句子交给模板，照片和录音的识读交给多模态模型。

## 快速开始

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install golla-jev
```

Claude Code 2.1.275+: `/plugin install golla-jev --marketplace fivetaku/gptaku_plugins`

重启 Claude Code 后这样说：

- `/golla-jev 我想把团队的收据报销自动化`
- "golla-jev，我们公司哪里能用上 Jev？"——范围大时先列出 3～5 个候选（创意模式）
- "帮这个业务设计 Jev 工作流"——把一项业务从头设计到尾（设计模式）

## 你会得到什么

- 一眼看懂的流程：输入 → 转写 → N 个 Jev 问题 → 脚本 → 产出物
- 分步骤的负责人表（Jev、代码、LLM、人）
- 可直接发送的 Jev 问题草稿 JSON（附 state 示例）
- 产出文档预览（审批单、日志、台账），每一栏标注 [J] Jev 的值、[C] 代码计算、[원] 原文、[사람] 人工
- 相似的公开案例与数据、容易出错的地方和防范方法
- 测试计划：样本量、锁定答案、用 dev/holdout 选阈值的流程

## 设计案示例

收录了 golla-jev 实际给出的三份设计案。先看一份，了解一段业务描述会变成什么（正文为韩语）。

- [收据 → 支出审批单](examples/01-expense-report.md) — 设计模式
- [共享收件箱 → 看板与周报](examples/02-mail-board.md) — 设计模式
- [网红候选筛选](examples/03-influencer-screening.md) — 从创意模式进入设计模式

## 附带的基准测试模板

`skills/golla-jev/bench/` 里有覆盖 5 类韩语办公业务（收据费用科目、邮件看板、会议纪要、满意度、合同条款）的 108 题基准测试。题目是手写示例，不是真实客户数据。作者于 2026-11-13 用 jev-1.14.0 运行，韩语提问准确率 96.3%（208/216），英语提问 93.5%。把在 dev 上选出的阈值 0.70 用于 holdout，自动处理了 97%（57/59），自动处理部分零错误。一个月前的 jev-1.13.0 按同一流程选出的阈值是 0.85。所以模型一变，阈值也要重新选（`bench/REPORT.md`）。

```
cp -r <插件路径>/skills/golla-jev/bench ./my-bench && cd my-bench
python3 build_items.py          # 先把 WF 字典改成你的业务
python3 run.py                  # 需要 TYPESAFE_API_KEY 环境变量（不要把值直接写在命令行里）
python3 analyze.py && python3 sweep.py
```

## 环境要求

- Claude Code
- 要真正运行问题草稿，需要 TypeSafe API 密钥（付费，https://typesafe.ai）。只要设计案则不需要密钥。
- 推荐：官方 `typesafe:typesafe-ai` 插件。模型 ID、价格、限额这类会变的事实，golla-jev 会去那个技能和 docs.typesafe.ai 核实，而不是凭记忆。
- 运行基准测试：Python 3.9+，可选 `certifi`

## 不做的事

- 不会停在"Jev 合适／不合适"。不合适的部分交给别的负责方，把流程补完整。
- 不照搬别人的阈值。阈值用你自己的业务样本来定。
- 不让 Jev 写文字、做计算或数日期。

## 许可证

MIT。与 TypeSafe、Jev 无关的个人项目。[DISCLAIMER](DISCLAIMER.md)
