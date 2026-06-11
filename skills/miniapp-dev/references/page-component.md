# Page Lifecycle and Custom Components

## Page Entry — Four Files

Official reference: https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html

Every page must be registered in its `.js` file with initial data, lifecycle callbacks, and event handlers.

### page.js

```javascript
const app = getApp();

Page({
  data: {
    title: 'Hello',
    list: [],
    loading: false,
    text: 'This is page data.'
  },

  // Lifecycle callbacks
  onLoad(options) {
    // Triggered when page is created; receives navigation params
    console.log(options);
    this.loadData();
  },
  onShow() {
    // Triggered when page appears in foreground
  },
  onReady() {
    // Triggered when page is first rendered
  },
  onHide() {
    // Triggered when page moves to background
  },
  onUnload() {
    // Triggered when page is destroyed
  },

  // Pull-down refresh
  onPullDownRefresh() {
    this.loadData().finally(() => wx.stopPullDownRefresh());
  },

  // Reach bottom
  onReachBottom() {
    this.loadMore();
  },

  // Share
  onShareAppMessage() {
    return {
      title: this.data.title,
      path: '/pages/index/index'
    };
  },

  // Page scroll
  onPageScroll() {
    // Triggered when page scrolls
  },

  // Tab item tap
  onTabItemTap(item) {
    console.log(item.index, item.pagePath, item.text);
  },

  // Custom methods
  async loadData() {
    this.setData({ loading: true });
    try {
      const res = await wx.request({
        url: `${app.globalData.apiBase}/items`,
        method: 'GET'
      });
      this.setData({ list: res.data });
    } catch (err) {
      console.error(err);
    } finally {
      this.setData({ loading: false });
    }
  },

  onTapItem(e) {
    const { id } = e.currentTarget.dataset;
    wx.navigateTo({
      url: `/pages/detail/detail?id=${id}`
    });
  },

  viewTap() {
    this.setData({
      text: 'Set some data for updating view.'
    }, () => {
      // setData callback: executed after view update
    });
  },

  // Free data (not reactive)
  customData: {
    hi: 'MINA'
  }
});
```

Rules:
- Use `Page()` constructor for simple pages.
- Use `Component()` constructor (with methods inside `methods: {}`) for complex pages with behaviors.
- `onLoad` receives `options` from navigation query string.
- `setData` is the only way to change data and trigger view updates.
- `customData` is not reactive; use it for static config.

### page.wxml

```xml
<view class="container">
  <view class="header">{{title}}</view>
  <view wx:if="{{loading}}" class="loading">Loading...</view>
  <view wx:else>
    <view
      wx:for="{{list}}"
      wx:key="id"
      class="item"
      data-id="{{item.id}}"
      bindtap="onTapItem"
    >
      <image class="thumb" src="{{item.image}}" mode="aspectFill" />
      <text class="name">{{item.name}}</text>
    </view>
  </view>
</view>
```

### page.wxss

```css
.container {
  padding: 20rpx;
}
.header {
  font-size: 36rpx;
  font-weight: bold;
  margin-bottom: 20rpx;
}
.item {
  display: flex;
  align-items: center;
  padding: 20rpx 0;
  border-bottom: 1rpx solid #eee;
}
.thumb {
  width: 120rpx;
  height: 120rpx;
  border-radius: 8rpx;
  margin-right: 20rpx;
}
.name {
  font-size: 30rpx;
  color: #333;
}
```

### page.json

```json
{
  "navigationBarTitleText": "Home",
  "enablePullDownRefresh": true,
  "backgroundTextStyle": "dark",
  "usingComponents": {
    "my-component": "/components/my-component/my-component"
  }
}
```

## Custom Components

Official reference: https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/

Every custom component needs four files and `"component": true` in its `.json`.

### component.json
```json
{
  "component": true
}
```

### component.js
```javascript
Component({
  properties: {
    // Public attributes (set from parent)
    innerText: {
      type: String,
      value: 'default value'
    },
    image: {
      type: String,
      value: ''
    }
  },

  data: {
    // Private internal data
    someData: {}
  },

  lifetimes: {
    attached() {
      // When component enters page tree
      this.setData({ ready: true });
    },
    detached() {
      // When component is removed from page tree
    }
  },

  methods: {
    customMethod() {
      // Custom methods go here
    },
    onTap() {
      // Trigger custom event to parent
      this.triggerEvent('itemtap', { title: this.properties.innerText });
    }
  }
});
```

### component.wxml
```xml
<view class="inner">
  {{innerText}}
</view>
<slot></slot>
```

### component.wxss
```css
/* Styles here apply ONLY to this component */
.inner {
  color: red;
}
```

### Usage in page

Page `.json`:
```json
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}
```

Page `.wxml`:
```xml
<component-tag-name innerText="Hello" bind:itemtap="onCardTap" />
```

Rules:
- Component styles are scoped; they do not leak to parent or child components.
- Use `slot` for content projection (similar to Vue/Angular slots).
- `properties` are reactive and receive data from parent.
- `triggerEvent(name, detail)` emits custom events upward.
- `lifetimes.attached` / `lifetimes.detached` replace old `attached` / `detached` top-level fields since base library 2.2.3.

## Behaviors

```javascript
// my-behavior.js
module.exports = Behavior({
  data: { sharedText: 'This is shared data.' },
  methods: {
    sharedMethod() {
      console.log(this.data.sharedText);
    }
  }
});

// page-a.js
const myBehavior = require('./my-behavior.js');
Page({
  behaviors: [myBehavior],
  onLoad() {
    this.data.sharedText === 'This is shared data.';
  }
});
```
