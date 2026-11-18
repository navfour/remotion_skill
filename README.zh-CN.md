# Remotion Paper

[English](README.md) | 简体中文

使用 Remotion 制作有依据、面向初学者的论文解读视频的 Codex skill。覆盖论文研读、研究背景与动机分析、讲稿、配音、字幕、动画、渲染验收和论文库管理。

默认交付一个**英式英语配音、中文字幕**的视频。内容聚焦论文的核心问题、方法、决定性证据与适用边界，优先组织为约 15 分钟，通常争取在 20 分钟以内，并根据论文信息量灵活调整。

本仓库提供 skill 指令、配置和参考文档。视频源码、论文 PDF、音频、素材与渲染结果保存在各自的制作工作区。

## 安装与使用

在 Codex 中可请求内置安装器安装本仓库根目录中的 skill：

```text
$skill-installer 从 https://github.com/navfour/remotion_skill 安装根目录中的 skill，名称为 remotion-paper。
```

也可以在 macOS / Linux 上手动安装到用户级 skill 目录：

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/navfour/remotion_skill.git "$HOME/.agents/skills/remotion-paper"
```

以上命令用于首次安装；已有同名 skill 时，沿用现有安装位置进行更新，避免重复安装。Codex 会自动发现新安装的 skill；若未显示，可重启后再检查。安装目录与发现机制见 [OpenAI 官方 skill 文档](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)。

在论文工作区中，将待解读的 PDF 放入 `candidate_paper/`，然后调用：

```text
$remotion-paper "candidate_paper/Your paper title.pdf" 制作这篇论文的解读视频。
```

已有作品也可按具体范围修改，例如：

```text
$remotion-paper 修改现有论文视频的 related work 部分：用表格说明前人研究的具体做法、贡献和与本文的关系；必要的关键机制增加动画。
```

调用时可以补充受众、关注问题、语言或时长偏好。用户当次明确要求优先；局部修改沿用已有合格内容。

## 默认制作方式

| 环节 | 要求 |
| --- | --- |
| 论文依据 | 阅读指定版本，核对书目信息，将关键论断、公式与数值对应到原文位置。 |
| 背景与动机 | 说明关键 related work 的实际贡献、research gap、motivation 及本文的回应；选读支撑核心 gap 的原始研究。 |
| 开场与篇幅 | 论文基本介绍和内容导览合计约 30 秒；主体优先约 15 分钟，按理解需要安排详略。 |
| 讲稿与入门解释 | 直接、自然地陈述逻辑；从具体问题与例子引入术语、输入输出和指标，保留重要条件。 |
| 配音与字幕 | 默认英式英语配音，Ryan `en-GB-RyanNeural` 为声线起点；中文字幕忠实对应实际英文旁白，专名可保留英文。 |
| 主画面 | 用比较表、机制动画、算例和数据图承载具体解释，支持暂停阅读；相关研究随内容展示引用。 |
| 语义同步 | 按实际英文语音的词语时间信息或可靠的对齐结果安排字幕和视觉动作；一段旁白可触发多个动作。 |
| 渲染与检查 | 先检查代表画面与短样片，再用 Remotion 实际渲染，并分别核对技术同步、动画语义和画面信息。 |

画面与旁白聚焦研究内容；配音语言标签、检索下载过程等制作信息保留在工程记录中。

## 论文工作区

实际制作在独立工作区进行，采用以下目录分工：

```text
workspace/
├── candidate_paper/    # 待制作和制作中的论文 PDF
├── paper/              # 完成生成与验收后的论文 PDF
│   ├── catalog.json    # 候选、制作中及已完成论文的统一台账
│   └── README.md       # 生成的论文阅读入口
└── remotion/           # 各论文的工程、素材和交付物
    └── P0001__YYYY-MM-DD__Full English title/
        ├── docs/
        │   └── paper-notes.md
        └── ...
```

- 用户可直接投递原文件名 PDF；处理指定论文时再登记、查重和规范命名。
- 登记后的 PDF 使用 `P0001__YYYY-MM-DD__完整英文题名.pdf`，工程采用相同 basename；编号永久固定，日期表示开始解读日期，未开始时使用 `pending`。
- 首次制作期间 PDF 留在 `candidate_paper/`；成片及交付物通过验收后移入 `paper/`，保持编号、开始日期、文件名和 PDF 内容，并同步引用与台账。
- 同一论文沿用一个工程。已有作品修订继续使用原工程与已归档 PDF。

详细规则及已有管理工具的兼容要求见 [论文库管理](references/library-management.md)。

## 交付与验收

完整制作默认交付成片、中文 SRT、封面、英文讲稿、章节、可编辑的 Remotion 工程，以及来源和验收记录。工程内的英文主笔记 `docs/paper-notes.md` 持续记录论文理解、视频设计和交付入口，便于后续跨论文回顾。

验收分别记录构建、实际渲染、全片解码、音轨同步、字幕对齐、动画语义对应、主画面信息和布局检查的结果及覆盖范围。人工听审单独记录；未运行论文实验时不声称完成复现。

## 运行环境

实际制作需要可读取本地 PDF、执行命令和管理文件的 Codex 环境，以及可用的 Remotion / Node.js 渲染环境、兼容浏览器和英式英语语音合成服务。PDF 解析、音视频处理与验证工具按工程需要配置；已有工程沿用已验证的依赖组合，新工程先验证兼容性。

本仓库不附带视频模板、执行脚本或素材，也不自动安装上述运行环境。论文库文档中的 `library.py` 命令属于已有工作区工具的接口示例；使用前应检查当前工作区是否具备相应能力。

## 文件导航

| 文件 | 内容 |
| --- | --- |
| [SKILL.md](SKILL.md) | 技能入口、默认行为、制作主线和验收边界。 |
| [agents/openai.yaml](agents/openai.yaml) | 技能的显示名称与简介。 |
| [references/production-workflow.md](references/production-workflow.md) | 研读与讲稿、背景分析、视觉设计、配音字幕、渲染与增量修改。 |
| [references/library-management.md](references/library-management.md) | 论文编号、日期、台账、候选与完成目录、归档和迁移。 |
| [references/case-lessons.md](references/case-lessons.md) | 既有制作案例及其经验，包括语义同步、画面信息量和篇幅控制。 |

完整制作要求以 [SKILL.md](SKILL.md) 及其引用文档为准；案例中的具体版本、时长和参数用于解释经验，不作为所有论文的固定要求。
