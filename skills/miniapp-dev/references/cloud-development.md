# WeChat Cloud Development Guide

Official reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html

WeChat Cloud Development (云开发) provides serverless backend services for Mini Programs: cloud database, cloud functions, and cloud storage. No server setup or authentication management required.

## Setup

Enable Cloud Development in WeChat DevTools: **Cloud Development → Enable Cloud Environment**.

```javascript
// app.js
App({
  onLaunch() {
    wx.cloud.init({
      env: 'your-env-id',      // Cloud environment ID
      traceUser: true          // Trace user access
    });
  }
});
```

Add permission in `app.json`:

```json
{
  "permission": {
    "scope.userLocation": {
      "desc": "用于获取位置信息"
    }
  },
  "cloud": true
}
```

## Cloud Database

Official reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/database.html

A document-oriented NoSQL database. Supports CRUD from both Mini Program client and Cloud Functions.

### Initialize

```javascript
const db = wx.cloud.database();
const _ = db.command;
```

### Collection Operations

```javascript
// Get collection reference
const todos = db.collection('todos');

// Add document
const { _id } = await todos.add({
  data: {
    title: 'Buy groceries',
    done: false,
    createdAt: db.serverDate()
  }
});

// Query single document
const { data } = await todos.doc(_id).get();

// Query with conditions
const { data: list } = await todos
  .where({ done: false, priority: _.gt(1) })
  .orderBy('createdAt', 'desc')
  .skip(0)
  .limit(10)
  .get();

// Update
await todos.doc(_id).update({
  data: { done: true }
});

// Set (replace entire document)
await todos.doc(_id).set({
  data: { title: 'Updated', done: true }
});

// Delete
await todos.doc(_id).remove();

// Count
const { total } = await todos.where({ done: false }).count();
```

### Query Operators

| Operator | Purpose | Example |
|----------|---------|---------|
| `_.eq(val)` | Equal | `{ age: _.eq(18) }` |
| `_.neq(val)` | Not equal | `{ age: _.neq(18) }` |
| `_.lt(val)` | Less than | `{ age: _.lt(18) }` |
| `_.lte(val)` | Less than or equal | `{ age: _.lte(18) }` |
| `_.gt(val)` | Greater than | `{ age: _.gt(18) }` |
| `_.gte(val)` | Greater than or equal | `{ age: _.gte(18) }` |
| `_.in(array)` | In array | `{ status: _.in(['active', 'pending']) }` |
| `_.nin(array)` | Not in array | `{ status: _.nin(['deleted']) }` |
| `_.exists(bool)` | Field exists | `{ name: _.exists(true) }` |
| `_.and([...])` | And | `_.and([{a: 1}, {b: 2}])` |
| `_.or([...])` | Or | `_.or([{a: 1}, {b: 2}])` |

### Aggregation

```javascript
const { list } = await todos.aggregate()
  .match({ status: 'active' })
  .group({
    _id: '$category',
    total: _.sum(1),
    avgPrice: _.avg('$price')
  })
  .sort({ total: -1 })
  .limit(10)
  .end();
```

## Cloud Functions

Official reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions.html

Server-side JavaScript running in the cloud. Used for sensitive operations and complex business logic.

### Directory Structure

```text
cloudfunctions/
├── login/
│   ├── config.json
│   ├── index.js
│   └── package.json
├── sum/
│   ├── config.json
│   ├── index.js
│   └── package.json
```

### Cloud Function Template

```javascript
// cloudfunctions/login/index.js
const cloud = require('wx-server-sdk');
cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV });

exports.main = async (event, context) => {
  const wxContext = cloud.getWXContext();

  return {
    openid: wxContext.OPENID,
    unionid: wxContext.UNIONID,
    appid: wxContext.APPID,
    env: wxContext.ENV
  };
};
```

```json
// cloudfunctions/login/config.json
{
  "permissions": {
    "openapi": ["wxacode.get"]
  }
}
```

### Call from Client

```javascript
// Call cloud function
const { result } = await wx.cloud.callFunction({
  name: 'login',
  data: { action: 'getUserInfo' }
});

console.log(result.openid);
```

### Cloud Call (免鉴权调用微信 API)

```javascript
// cloudfunctions/getPhone/index.js
const cloud = require('wx-server-sdk');
cloud.init();

exports.main = async (event) => {
  const { code } = event;

  // Call WeChat API without access_token
  const res = await cloud.openapi().phonenumber.getPhoneNumber({ code });
  return res;
};
```

## Cloud Storage

Official reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/storage.html

CDN-backed file storage with direct upload/download from client.

### Upload File

```javascript
// Upload from client
const { fileID } = await wx.cloud.uploadFile({
  cloudPath: `images/${Date.now()}-${filename}`,
  filePath: tempFilePath  // From wx.chooseImage
});

console.log(fileID);  // cloud://envid.xxx/xxx
```

### Download File

```javascript
// Get temporary download URL
const { fileList } = await wx.cloud.getTempFileURL({
  fileList: [fileID]
});

const tempURL = fileList[0].tempFileURL;
```

### Delete File

```javascript
await wx.cloud.deleteFile({
  fileList: [fileID]
});
```

### FileID Format

```
cloud://env-id.xxx.cloud.tencent.com/path/to/file.png
```

## Security Rules

Official reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/security.html

Define who can read/write data in Cloud Console → Database → Security Rules.

```javascript
// Example: only authenticated users can read their own data
{
  "read": "auth != null",
  "write": "auth != null && doc._openid == auth.openid"
}
```

## Environment Sharing

One cloud environment can be shared across multiple Mini Programs:

```javascript
// In another mini program
wx.cloud.init({
  env: 'shared-env-id',
  traceUser: true
});
```

## Performance Tips

- Use Cloud Functions for sensitive operations (payment, admin, data validation).
- Use client-side database for simple queries to reduce latency.
- Batch operations with `Promise.all()` for multiple reads/writes.
- Use indexes for frequently queried fields (set in Cloud Console).
- Monitor function cold start — keep functions warm for latency-sensitive operations.

## Resources

- Cloud Database: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/database.html
- Cloud Functions: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions.html
- Cloud Storage: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/storage.html
- SDK Reference: https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-sdk-api/
