# 微信小程序开发技能集（Claude Code Skills）

> English version below

一组用于 [Claude Code](https://claude.ai/code) 的 Skills，帮助开发和维护微信小程序。

## 五个技能

| 技能 | 触发命令 | 用途 |
|------|---------|------|
| `miniapp-dev` | `/miniapp-dev` | 从零搭建或扩展小程序项目：页面、组件、WXML/WXSS/JS、API 集成、TypeScript 配置 |
| `miniapp-scaffold` | `/miniapp-scaffold` | 校验 `project.config.json`、`app.json`、页面/组件文件结构是否符合微信官方规范 |
| `miniapp-fix` | `/miniapp-fix` | 诊断并修复编译、预览、DevTools 问题：CLI 报错、配置漂移、模板残留、条件编译失效 |
| `miniapp-refactor` | `/miniapp-refactor` | 将分散的顶部导航页重组为更清晰的中心 hub 结构，明确各模块归属 |
| `miniapp-copy` | `/miniapp-copy` | 精简页面文案：缩短过长标签、空状态、横幅、提示文字，改为行动优先的简洁表达 |

## 安装

将本仓库克隆到 Claude Code 的 Skills 目录：

```bash
# macOS / Linux
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/YOUR_USERNAME/miniapp-skills.git

# 或在现有项目内直接使用
claude skill add https://github.com/YOUR_USERNAME/miniapp-skills.git
```

安装后，在 Claude Code 对话中输入 `/miniapp-dev` 等命令即可触发对应技能。

## 文件结构

```
skills/
├── miniapp-dev/           # 小程序开发指南
│   ├── SKILL.md
│   └── references/        # 官方文档索引、API 参考、组件列表
├── miniapp-scaffold/      # 脚手架校验
├── miniapp-fix/           # 问题修复
├── miniapp-refactor/      # 架构重构
└── miniapp-copy/          # 文案精简
```

每个 `SKILL.md` 定义了触发条件和执行规则，`references/` 存放参考资料和检查清单。

## 适用场景

- 新项目启动时，快速生成符合微信规范的文件结构
- 页面增多后，导航变得混乱，需要重新组织信息架构
- 线上出现编译或预览失败，需要系统性排查
- 产品文案过于冗长，需要精简为面向用户的简洁表达

## 开源协议

[MIT](LICENSE)

---

## WeChat Mini Program Skills for Claude Code

A set of [Claude Code](https://claude.ai/code) Skills for developing and maintaining WeChat Mini Programs.

## Five Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| `miniapp-dev` | `/miniapp-dev` | Build or extend a mini program from scratch — pages, components, WXML/WXSS/JS, API integration, TypeScript setup |
| `miniapp-scaffold` | `/miniapp-scaffold` | Validate `project.config.json`, `app.json`, page/component file sets against official WeChat rules |
| `miniapp-fix` | `/miniapp-fix` | Diagnose and fix build, preview, and DevTools problems — CLI failures, config drift, template residue, stale compile conditions |
| `miniapp-refactor` | `/miniapp-refactor` | Reorganize scattered top-level tabs into a clearer hub structure with stable section ownership |
| `miniapp-copy` | `/miniapp-copy` | Simplify verbose labels, empty states, banners, and toasts into concise, action-first UI text |

## Installation

Clone into your Claude Code Skills directory:

```bash
# macOS / Linux
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/YOUR_USERNAME/miniapp-skills.git

# Or use directly in your project
claude skill add https://github.com/YOUR_USERNAME/miniapp-skills.git
```

After installation, type `/miniapp-dev` (or any skill command) in a Claude Code conversation to trigger it.

## Structure

```
skills/
├── miniapp-dev/           # Mini program development guide
│   ├── SKILL.md
│   └── references/        # Official docs index, API reference, component list
├── miniapp-scaffold/      # Scaffold validation
├── miniapp-fix/           # Troubleshooting
├── miniapp-refactor/      # Architecture refactoring
└── miniapp-copy/          # Copy trimming
```

Each `SKILL.md` defines triggers and execution rules; `references/` holds guides and checklists.

## Use Cases

- Starting a new project with WeChat-compliant file structure
- Reorganizing navigation when too many top-level pages accumulate
- Systematically diagnosing build or preview failures
- Trimming verbose product copy into concise, user-facing text

## License

[MIT](LICENSE)
