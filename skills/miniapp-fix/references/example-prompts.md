# Example Prompts

Use these prompts to validate whether the skill is triggered for DevTools or build problems and whether the response stays operational.

## Prompt 1

**User prompt**

```text
微信开发者工具里预览一直失败，但我只有命令行和这个小程序仓库。请你优先用 DevTools CLI 看看 `preview` 到底报了什么，能修就直接修，不能修就告诉我还缺什么证据。
```

**Expected answer structure**

1. what CLI command was used and what it exposed
2. whether the failure is repository-scoped and safe to auto-fix
3. what changed in the repo or what further evidence is still needed
4. the exact next command or remaining evidence needed

## Prompt 2

**User prompt**

```text
我把一个已有小程序仓库错误地当成新项目导入到微信开发者工具里了，现在仓库里多出了模板页、额外的配置文件，而且启动页也不对。请你帮我判断哪些该删、哪些该还原，并告诉我 DevTools 里要怎么重新导入。
```

**Expected answer structure**

1. what DevTools likely changed in the repo
2. what should remain as the intended tracked project shape
3. what to restore first and what generated residue to delete
4. what the user must change in DevTools after cleanup

## Do Not Use This Skill When

```text
我想确认一个全新仓库的页面结构、组件文件集和 `project.config.json` 设计是不是符合微信官方规则。
```

Use `miniapp-scaffold` instead, because this is a scaffold design/validation task rather than a build or DevTools problem.

```text
页面按钮点击后白屏，我想知道是不是运行时状态或者点击事件炸了。
```

This is a GUI/runtime question, not a CLI-visible compile or DevTools pollution problem. Inspect the runtime console directly.
