# Project Scaffold Reference

Official reference: https://developers.weixin.qq.com/miniprogram/dev/framework/config.html

## File Tree

```text
miniprogram/
├── app.js                 # App entry
├── app.json               # Global config
├── app.wxss               # Global styles
├── project.config.json    # DevTools / IDE config
├── sitemap.json           # Search indexing
├── pages/
│   └── index/
│       ├── index.js
│       ├── index.wxml
│       ├── index.wxss
│       └── index.json
├── components/
│   └── my-component/
│       ├── my-component.js
│       ├── my-component.wxml
│       ├── my-component.wxss
│       └── my-component.json
├── utils/
│   └── request.js         # wx.request wrapper
└── images/                # Static assets (keep small)
```

Rules:
- Every page must have four files with matching base name.
- Every custom component must also have four files.
- `images/` should not exceed 200 KB per file; use CDN for large assets.

## app.json

```json
{
  "pages": [
    "pages/index/index",
    "pages/detail/detail"
  ],
  "window": {
    "navigationBarTitleText": "My App",
    "navigationBarBackgroundColor": "#ffffff",
    "navigationBarTextStyle": "black",
    "backgroundColor": "#f5f5f5"
  },
  "tabBar": {
    "list": [
      { "pagePath": "pages/index/index", "text": "Home", "iconPath": "images/home.png", "selectedIconPath": "images/home-active.png" }
    ]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "permission": {
    "scope.userLocation": {
      "desc": "Your location is used to show nearby stores"
    }
  },
  "requiredBackgroundModes": ["audio"],
  "lazyCodeLoading": "requiredComponents"
}
```

Rules:
- `pages[0]` is the startup page.
- `tabBar.list` items must match existing page paths in `pages`.
- `permission` is required for user-sensitive APIs (location, camera, record, etc.).
- `lazyCodeLoading: "requiredComponents"` defers component loading for performance.

## app.js

Official reference: https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html

```javascript
App({
  onLaunch(options) {
    // App initialization, triggered once on cold start
    console.log('App launched', options);
    // Check for updates
    const updateManager = wx.getUpdateManager();
    updateManager.onUpdateReady(() => {
      wx.showModal({
        title: 'Update ready',
        content: 'Restart to apply update?',
        success: (res) => res.confirm && updateManager.applyUpdate()
      });
    });
  },
  onShow(options) {
    // Triggered when app enters foreground
  },
  onHide() {
    // Triggered when app enters background
  },
  onError(msg) {
    console.error('App error:', msg);
  },
  globalData: {
    userInfo: null,
    apiBase: 'https://your-api-domain.com'
  }
});
```

Rules:
- Every mini program has exactly one `App` instance shared across all pages.
- Use `getApp()` anywhere to access the global app instance and its `globalData`.
- `onLaunch` receives `options` including `path`, `query`, `scene`.
- `onShow` also receives `options` when app is brought to foreground.

```javascript
// xxx.js
const appInstance = getApp();
console.log(appInstance.globalData); // 'I am global data'
```

## project.config.json

```json
{
  "description": "Project configuration file",
  "packOptions": { "ignore": [] },
  "setting": {
    "bundle": false,
    "userConfirmedBundleSwitch": false,
    "urlCheck": true,
    "compileHotReLoad": false,
    "lazyloadPlaceholderEnable": false,
    "useMultiFrameRuntime": true,
    "useApiHook": true,
    "useApiHostProcess": true,
    "babelSetting": {
      "ignore": [],
      "disablePlugins": [],
      "outputPath": ""
    },
    "enableEngineNative": false,
    "useIsolateContext": true,
    "userConfirmedUseShieldPlugin": false,
    "minified": true
  },
  "compileType": "miniprogram",
  "libVersion": "2.32.0",
  "appid": "wx...",
  "projectname": "my-app",
  "condition": {}
}
```

## Subpackages

For large apps, split pages into subpackages to reduce initial download size and improve startup speed.

```json
{
  "pages": [
    "pages/index/index",
    "pages/logs/logs"
  ],
  "subpackages": [
    {
      "root": "packageA",
      "pages": [
        "pages/cat",
        "pages/dog"
      ]
    },
    {
      "root": "packageB",
      "name": "pack2",
      "pages": [
        "pages/apple",
        "pages/banana"
      ]
    }
  ],
  "preloadRule": {
    "pages/index/index": {
      "network": "all",
      "packages": ["packageA"]
    }
  }
}
```

File structure with subpackages:

```text
├── app.js
├── app.json
├── app.wxss
├── pages/              # Main package
│   ├── index/
│   └── logs/
├── packageA/           # Subpackage A
│   └── pages/
│       ├── cat/
│       └── dog/
└── packageB/           # Subpackage B
    └── pages/
        ├── apple/
        └── banana/
```

Rules:
- Main package pages go in `pages/`.
- Subpackage pages must NOT be in `pages/`.
- `tabBar` pages must be in the main package.
- `preloadRule` triggers subpackage download in background when a page loads.
- Use `wx.loadSubpackage({root})` to load subpackage manually.

## Cloud Development Config

```json
{
  "cloud": true,
  "cloudfunctionRoot": "cloudfunctions/"
}
```

## sitemap.json

```json
{
  "desc": "关于本小程序的索引",
  "rules": [
    {
      "action": "allow",
      "page": "*"
    },
    {
      "action": "disallow",
      "page": "pages/admin/*"
    }
  ]
}
```

Rules:
- `"action": "allow"` — pages are searchable.
- `"action": "disallow"` — pages are hidden from search.
- `"page": "*"` — wildcard for all pages.
- Search indexing requires `sitemap.json` to be configured.
