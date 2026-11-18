# Remotion Paper

English | [简体中文](README.zh-CN.md)

A Codex skill for producing paper explainer videos with Remotion, grounded in source evidence and accessible to beginners. It covers paper reading, research background and motivation, scripting, narration, subtitles, animation, rendering, validation, and paper library management.

The default deliverable is **one video with British English narration and Chinese subtitles**. The explanation focuses on the paper's central question, method, decisive evidence, and limitations. Aim for roughly 15 minutes and usually under 20, with flexibility for the explanation each paper needs.

This repository contains skill instructions, configuration, and reference documents. Video source code, paper PDFs, audio, assets, and rendered outputs belong in a separate production workspace. The skill instructions and reference documents are currently written in Chinese.

## Installation and usage

In Codex, ask the built-in installer to install the skill from the repository root:

```text
$skill-installer Install the skill at the root of https://github.com/navfour/remotion_skill with the name remotion-paper.
```

Alternatively, install it manually in your user-level skill directory on macOS or Linux:

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/navfour/remotion_skill.git "$HOME/.agents/skills/remotion-paper"
```

These commands are for a first installation. If you already have this skill, update it in its existing location to avoid duplicate installations. Codex discovers newly installed skills automatically; restart it if the skill does not appear. See the [official OpenAI skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) for installation locations and discovery behaviour.

In your paper workspace, place a PDF in `candidate_paper/`, then invoke the skill:

```text
$remotion-paper "candidate_paper/Your paper title.pdf" Create an explainer video for this paper.
```

You can also request a specific change to an existing video:

```text
$remotion-paper Revise the related work section of the existing paper video. Use a table to explain the prior studies' methods, contributions, and connections to this paper, and animate any key mechanisms needed for understanding.
```

Include your intended audience, questions of interest, language preferences, or duration preferences when relevant. Explicit instructions for the current task take precedence. Targeted revisions reuse existing content that remains suitable.

## Default production approach

| Area | Approach |
| --- | --- |
| Source evidence | Read the specified paper version, verify its bibliographic details, and trace key claims, equations, and numerical results to their locations in the source. |
| Background and motivation | Explain the actual contributions of key related work, the research gap, the motivation, and the paper's response. Consult the original studies that support the central gap. |
| Opening and length | Keep the paper introduction and content overview to about 30 seconds in total. Aim for roughly 15 minutes overall, allocating detail according to what viewers need to understand. |
| Script and prerequisites | Use direct, natural language. Introduce terminology, inputs, outputs, and metrics through concrete questions and examples, while preserving important conditions. |
| Narration and subtitles | Use British English narration, starting with Ryan (`en-GB-RyanNeural`) unless otherwise specified. Chinese subtitles faithfully follow the spoken English; names and technical terms may remain in English. |
| Main visuals | Explain with comparison tables, mechanism animations, worked examples, and data plots that support paused reading. Show references alongside the relevant content. |
| Semantic timing | Use actual English word timings or reliable alignment results to schedule subtitles and visual actions. A single narration segment can trigger several actions. |
| Rendering and checks | Review representative frames and short previews before rendering with Remotion. Check technical synchronisation, semantic correspondence, and visual information separately. |

Narration and visuals focus on the research. Production details, such as voice-language labels and the literature search or download process, stay in the project records.

## Paper workspace

Production takes place in a separate workspace with the following structure:

```text
workspace/
├── candidate_paper/    # PDFs awaiting production or currently in progress
├── paper/              # PDFs archived after delivery validation
│   ├── catalog.json    # One registry for all registered papers and their status
│   └── README.md       # Generated index for browsing the paper library
└── remotion/           # Per-paper projects, assets, and deliverables
    └── P0001__YYYY-MM-DD__Full English title/
        ├── docs/
        │   └── paper-notes.md
        └── ...
```

- Submit PDFs under their original filenames. Registration, duplicate checks, and filename normalisation are handled for the selected paper.
- Registered PDFs use `P0001__YYYY-MM-DD__Full English title.pdf`; the project directory uses the same basename. IDs are permanent. The date records when the paper explanation work began; use `pending` for papers whose explanation work has not yet started.
- Keep the PDF in `candidate_paper/` during initial production. After the video and required deliverables pass validation, move it to `paper/`, preserving its ID, start date, filename, and contents, and update references and the registry.
- Reuse one project for each paper. Revisions of a completed work continue to use its original project and archived PDF.

See [Paper library management](references/library-management.md) for the detailed rules and compatibility requirements for existing workspace tools.

## Deliverables and validation

A complete production normally includes the rendered video, Chinese SRT subtitles, a cover image, the English script, chapters, an editable Remotion project, and source and validation records. The English master note at `docs/paper-notes.md` records the paper interpretation, video design, and delivery links throughout production, supporting later reviews across papers.

Record the results and coverage of build checks, rendering, full-video decoding, audio synchronisation, subtitle alignment, animation semantics, main-visual information, and layout checks separately. Report human listening review separately as well. Do not claim experimental reproduction unless the paper's experiments were actually run.

## Runtime requirements

Production requires a Codex environment that can read local PDFs, run commands, and manage files, together with a working Remotion / Node.js rendering environment, a compatible browser, and a British English text-to-speech service. Configure PDF parsing, media processing, and validation tools as needed for the project. Reuse validated dependency combinations in existing projects and verify compatibility in new ones.

This repository does not bundle video templates, executable scripts, or media assets, and does not install the runtime environment automatically. The `library.py` commands in the library documentation illustrate interfaces for existing workspace tools; check that the current workspace supports those operations before using them.

## Repository guide

| File | Contents |
| --- | --- |
| [SKILL.md](SKILL.md) | Skill entry point, defaults, production workflow, and validation boundaries. |
| [agents/openai.yaml](agents/openai.yaml) | Display name and short description. |
| [references/production-workflow.md](references/production-workflow.md) | Paper reading, scripting, background analysis, visual design, narration, subtitles, rendering, and targeted revisions. |
| [references/library-management.md](references/library-management.md) | Paper IDs, dates, registry, candidate and completed directories, archiving, and migration. |
| [references/case-lessons.md](references/case-lessons.md) | Lessons from previous productions, including semantic timing, visual information, and duration. |

For the full production requirements, follow [SKILL.md](SKILL.md) and its linked references. Versions, durations, and parameters in the case studies explain particular experiences; they are not fixed requirements for every paper.
