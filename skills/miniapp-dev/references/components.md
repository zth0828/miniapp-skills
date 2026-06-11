# Built-in Components Quick Reference

Official reference: https://developers.weixin.qq.com/miniprogram/dev/component/

These are the native components provided by WeChat. Use them directly in WXML without registration.

## View Containers

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `view` | Generic container (like `div`) | `hover-class`, `hover-start-time` |
| `scroll-view` | Scrollable area | `scroll-x`, `scroll-y`, `scroll-top`, `scroll-into-view` |
| `swiper` | Carousel / slider | `indicator-dots`, `autoplay`, `interval`, `circular` |
| `swiper-item` | Slide inside `swiper` | Must be direct child of `swiper` |
| `movable-area` | Draggable area | — |
| `movable-view` | Draggable view inside `movable-area` | `direction`, `x`, `y`, `bindchange` |
| `cover-view` | Text overlay on native components | Use over `video`, `map`, `canvas` |
| `cover-image` | Image overlay on native components | `src`, `bindload` |
| `match-media` | Media query match detection | `min-width`, `max-width` |
| `page-container` | Page-like container with transitions | `show`, `position`, `z-index` |
| `root-portal` | Detach subtree (like fixed position) | `enable` |

## Basic Content

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `text` | Text display | `selectable`, `user-select`, `space` |
| `icon` | System icons | `type` (success/warning/info), `size`, `color` |
| `progress` | Progress bar | `percent`, `show-info`, `active` |
| `rich-text` | Rich HTML-like content | `nodes` (array of objects) |
| `image` | Image display | `src`, `mode` (aspectFill/aspectFit/widthFix), `lazy-load` |

## Form Components

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `button` | Button | `type` (primary/default/warn), `size`, `open-type` (getUserInfo/contact/share), `bindtap` |
| `input` | Single-line input | `type`, `placeholder`, `value`, `bindinput`, `bindfocus` |
| `textarea` | Multi-line input | `placeholder`, `maxlength`, `auto-height`, `bindinput` |
| `checkbox` / `checkbox-group` | Multiple choice | `value`, `checked`, `bindchange` |
| `radio` / `radio-group` | Single choice | `value`, `checked`, `bindchange` |
| `picker` | Bottom popup selector | `mode` (selector/time/date/region), `range`, `value`, `bindchange` |
| `picker-view` | Inline picker | `value`, `bindchange` |
| `slider` | Slider | `min`, `max`, `step`, `show-value`, `bindchange` |
| `switch` | Toggle | `checked`, `type` (switch/checkbox), `bindchange` |
| `form` | Form wrapper | `bindsubmit`, `bindreset` |
| `label` | Label for form controls | `for` (target control id) |
| `editor` | Rich text editor | `placeholder`, `bindready`, `bindinput` |

## Navigation

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `navigator` | Page link | `url`, `open-type` (navigate/redirect/switchTab/reLaunch), `hover-class` |

## Media

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `image` | Image (duplicate of Basic Content) | — |
| `video` | Video player | `src`, `controls`, `autoplay`, `bindplay`, `bindended` |
| `audio` | Audio player | `src`, `controls`, `loop`, `bindplay` |
| `camera` | Camera | `mode` (normal/scanCode), `device-position`, `binderror` |
| `live-player` | Live stream playback | `src`, `mode` (live/RTC), `autoplay` |
| `live-pusher` | Live stream push | `url`, `mode`, `autopush` |

## Map & Canvas

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `map` | Map (Tencent Map) | `longitude`, `latitude`, `scale`, `markers`, `bindmarkertap` |
| `canvas` | Canvas 2D | `type`, `canvas-id`, `bindtouchstart` |

## Open Capabilities

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `web-view` | Embedded web page | `src` (must be in business domain whitelist) |
| `ad` | Banner ad | `unit-id`, `ad-type` |
| `open-data` | Display WeChat open data (deprecated, use `getUserProfile`) | — |
| `official-account` | Official account follow | — |

## Navigation Bar & Page Meta

| Component | Purpose | Key Attributes |
|-----------|---------|----------------|
| `navigation-bar` | Configure navigation bar | `title`, `front-color`, `background-color` |
| `page-meta` | Page properties & event listeners | `page-style`, `page-orientation` |

## Component Usage Rules

- **Native components** (`video`, `map`, `canvas`, `textarea`) render above all other elements; use `cover-view` / `cover-image` to overlay them.
- **`swiper` + `swiper-item`**: `swiper-item` must be direct child of `swiper`, width/height auto-set to 100%.
- **`movable-view`** must be inside `movable-area`.
- **`picker-view-column`** must be inside `picker-view`.
- **`checkbox`** / `radio` must be inside `checkbox-group` / `radio-group`.
- **All form components** with `name` inside a `<form>` are included in `form` submit data.
