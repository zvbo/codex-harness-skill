# codex-harness-skill

`workspace-meta-harness` 的公开版 skill 包。

它用于给 Codex、Claude Code、Cursor、Cline、Continue 等常见 AI coding
agent 提供一套可复用的 Meta-Harness 工作流，让 agent 不只是“单轮对话助手”，
而是能够在长期项目中做到：

- 跨项目学习
- 单项目复盘
- 失败模式收敛
- 用户偏好沉淀
- Hermes 定时总结

这不是一个编译器插件，也不是一个模型本体；它更像是一套可移植的
`skill / prompt protocol / operating manual`。

## 适用场景

适合下面这些场景：

- 你在多个项目里反复与同一个 coding agent 协作
- 你希望 agent 能记住你的沟通风格、审查偏好、提示结构偏好
- 你希望每个项目都有自己的 run 轨迹、postmortem、history 和状态页
- 你希望把 Hermes 作为调度器和日报器，而不是主事实源
- 你希望把“技能提升”建立在 evidence、memory、review 和 promotion 上

## 核心能力

这个 skill 的方法论主线是：

- `Meta-Harness` 作为外环骨架
- `Hermes-style memory / skill promotion` 作为学习层
- `workspace + project` 双层记忆结构

对应能力包括：

- Workspace-first：先从整机工作区学习，再下钻到单项目
- Project harness：为每个 repo 记录 run、review、lesson、status、history
- Dual-layer memory：使用 `00~03` 作为主状态层
- Human-gated promotion：候选经验先收集，再人工晋升
- Noisy-run handling：显式忽略污染样本，而不是硬删原始证据
- Hermes integration：让 Hermes 只做 cron、摘要、投递，不做 source of truth

## 架构概念

### Workspace 层

负责：

- 跨项目 lessons
- 用户画像
- capability matrix
- daily / weekly synthesis

### Project 层

负责：

- run artifacts
- postmortems
- lesson candidates
- project packets
- project status / history

### 双层 memory

主状态源为：

- `00_objectives.yaml`
- `01_idea_registry.jsonl`
- `02_agent_roles.md`
- `03_synth_knowledge.jsonl`

## 兼容的 agent / IDE 工作流

这份 skill 不依赖某一个专有 IDE。只要你的工具支持以下任意一种方式，就能使用：

1. 原生 skill / prompt pack 加载
2. 在系统提示词或项目规则中引用本仓库的 `SKILL.md`
3. 允许 agent 读取本地 Markdown 指南文件

通常适配良好的对象包括：

- OpenAI Codex / Codex CLI
- Claude Code
- Cursor
- Cline
- Continue
- 其他支持本地规则文件的 coding agent

## 隐私说明

公开版已经去除了以下内容：

- 本地绝对路径
- 用户名 / home 目录
- 机器专属 workspace root
- live runtime 路径
- company-specific storage root
- 真实 run id 和私有项目路径

公开版只保留方法论、结构和占位变量。

## 仓库结构

```text
codex-harness-skill/
  README.md
  SKILL.md
  references/
    runtime-map.md
```

## 本地安装

### 1. 克隆仓库

```bash
git clone https://github.com/zvbo/codex-harness-skill.git
cd codex-harness-skill
```

### 2. 准备本地变量

建议先配置你自己的 harness 路径：

```bash
export CODEX_SELFOPT_REPO=/path/to/codex-workspace-selfopt
export CODEX_SELFOPT_WRAPPER=$CODEX_SELFOPT_REPO/selfopt
export CODEX_WORKSPACE_SELFOPT_HOME=$HOME/.codex/workspace-selfopt
```

如果你没有完整的 `codex-workspace-selfopt` 实现，也可以先只把这份 skill
当作工作流文档使用。

### 3. 接入到常见工具

### A. Codex

最简单的两种方式：

1. 直接把本仓库放到你自己的 skills 目录，并让 Codex 读取 `SKILL.md`
2. 在项目的 `AGENTS.md` 中引用本 skill

例如：

```md
Use the workflow in `/path/to/codex-harness-skill/SKILL.md`
for workspace reviews, project postmortems, noisy-run handling,
and Hermes summary integration.
```

### B. Claude Code

如果 Claude Code 支持项目规则或本地 instruction 文件，推荐做法是：

1. 保留本仓库在本地
2. 在项目规则中引用 `SKILL.md`
3. 让 Claude Code 在需要时继续读取 `references/runtime-map.md`

可以使用类似这段项目说明：

```md
When the task involves meta-harness, self-optimization, project reviews,
run postmortems, noisy-run handling, or Hermes summaries, follow the
workflow in `/path/to/codex-harness-skill/SKILL.md`.
```

### C. Cursor / Cline / Continue / 其他 agent

如果工具没有“原生 skill 目录”机制，也可以这样接入：

1. 把本仓库保存在你的 workspace 或 dotfiles 目录
2. 在项目规则、system prompt 或 team playbook 中引用 `SKILL.md`
3. 保证 agent 能继续读取 `references/runtime-map.md`

换句话说，这个 skill 可以被当作：

- 一份 agent operating manual
- 一份 review / memory / promotion protocol
- 一份跨工具可复用的 prompt pack

## 最小使用方式

即使你还没有完整 harness，也可以先只用这 3 步：

1. 让 agent 先读 `SKILL.md`
2. 在做项目复盘时按 `workspace / project` 两层思考
3. 把动态经验写回你自己的 `00~03 memory`

## 推荐配套仓库

如果你想真正跑起来，而不是只看文档，通常还需要一个配套实现仓来提供：

- `selfopt` wrapper
- run collection
- review synthesis
- memory materialization
- Hermes bridge scripts

这份 skill 本身不包含运行时代码，只定义工作流和读取顺序。

## 文件说明

- [SKILL.md](./SKILL.md)
  - 主 skill 文件，定义职责、规则、工作流、命令入口
- [references/runtime-map.md](./references/runtime-map.md)
  - 通用路径映射和命令入口示例

## 分享边界

如果你基于这个 skill 再二次定制，建议继续保持下面原则：

- 规则公开，数据私有
- 结构公开，live evidence 私有
- workflow 公开，用户画像和公司记忆私有

## 备注

- 这是一份 workflow skill，不是 SaaS 服务。
- Runtime artifacts 应保留在你的本地工作区，不应直接放进公开仓库。
- Confirmed memory 和 candidate memory 必须分开。
- 如果你想把它配给 Hermes 使用，建议单独再导出一份 `hermes-meta-harness` 变体。
