# WXML and WXSS Reference

Official references:
- WXML: https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/
- WXSS: https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html

## WXML Patterns

### Data Binding

```xml
<!-- wxml -->
<view> {{message}} </view>
```

```javascript
// page.js
Page({
  data: { message: 'Hello MINA!' }
});
```

### List Rendering

```xml
<view wx:for="{{array}}" wx:key="*this">
  {{item}} — {{index}}
</view>
```

```javascript
Page({
  data: { array: [1, 2, 3, 4, 5] }
});
```

Use `wx:for-item="customName"` and `wx:for-index="customIndex"` to rename loop variables.

### Conditional Rendering

```xml
<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
<view wx:elif="{{view == 'APP'}}"> APP </view>
<view wx:elif="{{view == 'MINA'}}"> MINA </view>
<view wx:else> UNKNOWN </view>
```

`wx:if` removes/adds element from DOM; `hidden="{{!show}}"` toggles display only. Prefer `hidden` for frequent toggles.

### Templates

```xml
<template name="staffName">
  <view>FirstName: {{firstName}}, LastName: {{lastName}}</view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
```

```javascript
Page({
  data: {
    staffA: { firstName: 'Hulk', lastName: 'Hu' }
  }
});
```

### Include vs Import

- `<include src="/templates/header.wxml" />` — includes entire WXML (except `<template>`).
- `<import src="/templates/foo.wxml" />` — imports only `<template>` definitions for reuse.

### Event Binding

| Directive | Behavior |
|-----------|----------|
| `bindtap="onTap"` | Event bubbles up |
| `catchtap="onTap"` | Event does not bubble |
| `capture-bind:tap="onTap"` | Capturing phase |
| `capture-catch:tap="onTap"` | Capturing phase, stops propagation |

### Input Binding

```xml
<input value="{{val}}" bindinput="onInput" />
```

```javascript
Page({
  data: { val: '' },
  onInput(e) {
    this.setData({ val: e.detail.value });
  }
});
```

### Quick Reference Table

| Pattern | Syntax |
|---------|--------|
| Data binding | `{{variable}}` |
| List rendering | `wx:for="{{array}}" wx:key="id"` |
| Condition | `wx:if="{{condition}}"`, `wx:elif`, `wx:else` |
| Hidden | `hidden="{{!show}}"` |
| Input binding | `<input value="{{val}}" bindinput="onInput" />` |
| Template | `<template name="foo"><view>{{text}}</view></template>` |
| Include | `<include src="/templates/header.wxml" />` |
| Import | `<import src="/templates/foo.wxml" />` |

## WXSS Rules

- Use `rpx` for responsive sizing (1 rpx = 0.5 px on iPhone 6/7/8 width).
- `@import '/styles/common.wxss';` for shared styles.
- No `*` selector; no child selector `>`; limited pseudo-classes (`:before`, `:after`).
- `page` selector sets page-level background.
- Max recommended stylesheet size: 200 KB per page.

### @import

```css
/** common.wxss **/
.small-p { padding: 5px; }

/** app.wxss **/
@import "common.wxss";
.middle-p { padding: 15px; }
```

### Inline Styles

```xml
<view style="color:{{color}};" />
```

Dynamic styles via data binding.
