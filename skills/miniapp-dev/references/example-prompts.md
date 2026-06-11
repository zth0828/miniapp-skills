# Example Prompts — Miniapp Development Guide

## Trigger examples

- "帮我创建一个微信小程序，有一个首页展示商品列表"
- "写一个带微信登录的小程序页面"
- "我需要一个小程序，有 tabBar，三个页面：首页、分类、我的"
- "帮我写一个自定义组件，接收一个标题和图片地址"
- "小程序怎么调用微信支付？帮我写个示例"
- "我要用 TypeScript 写小程序，帮我配置项目"

## Anti-patterns to avoid

- Generating pages with only `.js` and `.wxml`, missing `.wxss` and `.json`
- Using `wx.request` with `http://` URLs
- Calling `wx.getUserInfo` (deprecated) instead of `wx.getUserProfile`
- Hard-coding `appid` or `secret` in client-side code
- Inventing WeChat API names or signatures not in official docs

## Evaluation notes

- The skill should produce a file tree before showing code.
- The skill should flag when an API needs MP Admin configuration.
- The skill should prefer `async/await` wrappers for `wx.request` in modern projects.
