# Complete API Quick Reference

Official reference: https://developers.weixin.qq.com/miniprogram/dev/api/

WeChat Mini Program provides **1,671+ APIs** across 20+ categories. Below is the most commonly used subset for daily development.

## Foundation

| API | Purpose | Notes |
|-----|---------|-------|
| `wx.canIUse(string)` | Check if API/component is available | Always check before using new APIs |
| `wx.env` | Environment variables | `USER_DATA_PATH`, `VERSION` |
| `wx.base64ToArrayBuffer(str)` | Base64 → ArrayBuffer | For file/crypto processing |
| `wx.arrayBufferToBase64(buffer)` | ArrayBuffer → Base64 | For file/crypto processing |

## System Information

| API | Purpose | Sync/Async |
|-----|---------|-----------|
| `wx.getSystemInfo({success})` | Full system info | Async |
| `wx.getSystemInfoSync()` | Full system info | **Sync** |
| `wx.getSystemInfoAsync()` | Full system info | Async |
| `wx.getDeviceInfo()` | Device model, brand, benchmark | Async |
| `wx.getWindowInfo()` | Screen width, height, safe area | Async |
| `wx.getAppBaseInfo()` | WeChat version, SDK version | Async |
| `wx.getAppAuthorizeSetting()` | Current authorization status | Async |
| `wx.getSystemSetting()` | Bluetooth, location, WiFi settings | Async |
| `wx.openAppAuthorizeSetting()` | Jump to system WeChat auth settings | — |
| `wx.openSystemBluetoothSetting()` | Jump to system Bluetooth settings | — |

## App Lifecycle & Events

| API | Purpose |
|-----|---------|
| `wx.getLaunchOptionsSync()` | Get startup params (scene, path, query) |
| `wx.getEnterOptionsSync()` | Get current entry params |
| `wx.getApiCategory()` | Get current API category |
| `wx.onAppShow(callback)` | Listen app foreground event |
| `wx.onAppHide(callback)` | Listen app background event |
| `wx.onError(callback)` | Listen global error event |
| `wx.onPageNotFound(callback)` | Listen 404 page event |
| `wx.onThemeChange(callback)` | Listen system dark/light mode change |
| `wx.onUnhandledRejection(callback)` | Listen unhandled Promise rejection |
| `wx.offAppShow(callback)` | Remove listener |

## Update

| API | Purpose |
|-----|---------|
| `wx.getUpdateManager()` | Get update manager instance |
| `UpdateManager.onUpdateReady(callback)` | New version downloaded |
| `UpdateManager.onUpdateFailed(callback)` | Download failed |
| `UpdateManager.applyUpdate()` | Force restart with new version |

## Network

| API | Purpose | Promise |
|-----|---------|---------|
| `wx.request({url, method, data, header, success})` | HTTP request | ❌ |
| `wx.uploadFile({url, filePath, name, formData})` | File upload | ❌ |
| `wx.downloadFile({url, success})` | File download | ❌ |
| `wx.connectSocket({url})` | WebSocket connect | ❌ |
| `SocketTask.send({data})` | Send WebSocket message | — |
| `SocketTask.onMessage(callback)` | Listen WebSocket message | — |
| `SocketTask.close()` | Close WebSocket | — |

```javascript
// Promise wrapper for wx.request (recommended)
const request = (url, method = 'GET', data = {}, header = {}) => {
  return new Promise((resolve, reject) => {
    wx.request({
      url: `${getApp().globalData.apiBase}${url}`,
      method,
      data,
      header: { 'Content-Type': 'application/json', ...header },
      success: (res) => res.statusCode >= 200 && res.statusCode < 300 ? resolve(res.data) : reject(res),
      fail: reject
    });
  });
};
```

## Storage

| API | Sync | Purpose |
|-----|------|---------|
| `wx.setStorage({key, data})` | ❌ | Save data |
| `wx.setStorageSync(key, data)` | ✅ | Save data (blocks thread) |
| `wx.getStorage({key, success})` | ❌ | Read data |
| `wx.getStorageSync(key)` | ✅ | Read data (blocks thread) |
| `wx.removeStorage({key})` | ❌ | Delete single key |
| `wx.removeStorageSync(key)` | ✅ | Delete single key |
| `wx.clearStorage()` | ❌ | Clear all storage |
| `wx.clearStorageSync()` | ✅ | Clear all storage |

Limit: 10 MB total. Sync APIs block JS thread — avoid in animation/render paths.

## Open Interface (Login, User, Payment)

| API | Purpose | Promise | Trigger |
|-----|---------|---------|---------|
| `wx.login({success})` | Get login code | ❌ | Programmatic |
| `wx.checkSession({success})` | Check session valid | ❌ | Programmatic |
| `wx.getUserProfile({desc, success})` | Get user profile | ❌ | **User tap only** |
| `wx.getPhoneNumber({success})` | Get phone number | ❌ | **Button trigger** |
| `wx.requestPayment({...})` | Launch payment | ❌ | **User confirm** |
| `wx.chooseAddress({success})` | Select address | ✅ | **User confirm** |
| `wx.chooseInvoiceTitle({success})` | Select invoice title | ✅ | **User confirm** |
| `wx.chooseInvoice({success})` | Select invoice | ✅ | **User confirm** |
| `wx.startSoterAuthentication({...})` | Biometric auth | ❌ | **User confirm** |

```javascript
// Login flow
wx.login({
  success(res) {
    if (res.code) {
      // Send to backend to exchange for openid/session_key
      request('/auth/login', 'POST', { code: res.code });
    }
  }
});

// User profile (must be triggered by button tap)
wx.getUserProfile({
  desc: '用于完善会员资料',
  success: (res) => {
    console.log(res.userInfo);
  }
});
```

## Location

| API | Purpose | Promise | Needs Permission |
|-----|---------|---------|------------------|
| `wx.getLocation({type, success})` | Get current location | ❌ | ✅ |
| `wx.chooseLocation({success})` | Pick location on map | ✅ | — |
| `wx.openLocation({latitude, longitude})` | Open map to location | ❌ | — |
| `wx.startLocationUpdate({success})` | Start continuous location | ❌ | ✅ |
| `wx.stopLocationUpdate()` | Stop continuous location | ❌ | — |
| `wx.onLocationChange(callback)` | Listen location updates | — | ✅ |

```json
// app.json permission required
{
  "permission": {
    "scope.userLocation": { "desc": "需要获取您的位置信息" },
    "scope.userLocationBackground": { "desc": "后台定位需要" }
  }
}
```

## Media — Image

| API | Purpose | Promise |
|-----|---------|---------|
| `wx.chooseImage({count, sizeType, sourceType})` | Select/take photo | ❌ |
| `wx.previewImage({urls, current})` | Preview images | ❌ |
| `wx.getImageInfo({src, success})` | Get image info | ❌ |
| `wx.saveImageToPhotosAlbum({filePath})` | Save to album | ❌ |
| `wx.compressImage({src, quality})` | Compress image | ❌ |
| `wx.editImage({src, success})` | Edit image | ❌ |

## Media — Video & Audio

| API | Purpose | Promise |
|-----|---------|---------|
| `wx.chooseVideo({sourceType, maxDuration})` | Select/record video | ❌ |
| `wx.chooseMedia({count, mediaType})` | Select image/video mix | ❌ |
| `wx.getVideoInfo({src, success})` | Get video info | ❌ |
| `wx.saveVideoToPhotosAlbum({filePath})` | Save video to album | ❌ |
| `wx.createVideoContext(id)` | Control `<video>` component | — |
| `wx.createInnerAudioContext()` | Audio playback control | — |
| `wx.createBackgroundAudioManager()` | Background audio | — |
| `wx.getRecorderManager()` | Audio recording | — |
| `wx.createLivePlayerContext(id)` | Live stream player | — |
| `wx.createLivePusherContext()` | Live stream push | — |

## File

| API | Purpose | Promise |
|-----|---------|---------|
| `wx.saveFile({tempFilePath})` | Save temp file to local | ❌ |
| `wx.getFileSystemManager()` | Full FS operations | — |
| `fs.readFile({filePath, encoding})` | Read file | ❌ |
| `fs.writeFile({filePath, data})` | Write file | ❌ |
| `fs.mkdir({dirPath})` | Create directory | ❌ |
| `fs.readdir({dirPath})` | List directory | ❌ |
| `fs.stat({path})` | Get file/directory info | ❌ |
| `fs.unlink({filePath})` | Delete file | ❌ |
| `fs.removeSavedFile({filePath})` | Delete saved file | ❌ |
| `wx.getSavedFileList({success})` | List saved files | ❌ |

```javascript
const fs = wx.getFileSystemManager();
fs.writeFileSync(`${wx.env.USER_DATA_PATH}/hello.txt`, 'Hello', 'utf8');
const content = fs.readFileSync(`${wx.env.USER_DATA_PATH}/hello.txt`, 'utf8');
```

## Device — Scan, Clipboard, Vibrate, Screen

| API | Purpose | Promise |
|-----|---------|---------|
| `wx.scanCode({onlyFromCamera, scanType})` | Scan QR/barcode | ✅ |
| `wx.setClipboardData({data})` | Copy to clipboard | ❌ |
| `wx.getClipboardData({success})` | Read clipboard | ❌ |
| `wx.vibrateShort({type})` | Short vibration | ❌ |
| `wx.vibrateLong()` | Long vibration | ❌ |
| `wx.setScreenBrightness({value})` | Set brightness (0-1) | ❌ |
| `wx.getScreenBrightness({success})` | Get brightness | ❌ |
| `wx.setKeepScreenOn({keepScreenOn})` | Keep screen on | ❌ |
| `wx.onUserCaptureScreen(callback)` | Listen screenshot | — |
| `wx.addPhoneContact({...})` | Add to contacts | ❌ |

## Device — Bluetooth

| API | Purpose |
|-----|---------|
| `wx.openBluetoothAdapter({success})` | Initialize Bluetooth |
| `wx.closeBluetoothAdapter()` | Close Bluetooth |
| `wx.getBluetoothAdapterState({success})` | Get adapter state |
| `wx.startBluetoothDevicesDiscovery({services})` | Start scanning |
| `wx.stopBluetoothDevicesDiscovery()` | Stop scanning |
| `wx.getBluetoothDevices({success})` | Get discovered devices |
| `wx.createBLEConnection({deviceId})` | Connect to device |
| `wx.closeBLEConnection({deviceId})` | Disconnect |
| `wx.getBLEDeviceServices({deviceId})` | Get device services |
| `wx.getBLEDeviceCharacteristics({...})` | Get characteristics |
| `wx.writeBLECharacteristicValue({...})` | Write value |
| `wx.readBLECharacteristicValue({...})` | Read value |
| `wx.notifyBLECharacteristicValueChange({...})` | Enable notify |
| `wx.onBLECharacteristicValueChange(callback)` | Listen value change |

## Device — WiFi, NFC, Contact

| API | Purpose |
|-----|---------|
| `wx.startWifi()` / `wx.stopWifi()` | WiFi manager |
| `wx.connectWifi({SSID, password})` | Connect to WiFi |
| `wx.getWifiList()` / `wx.onGetWifiList(callback)` | Scan WiFi |
| `wx.getConnectedWifi({success})` | Current WiFi info |
| `wx.getNFCAdapter()` | NFC adapter |
| `wx.startHCE({aid_list})` | HCE (Host Card Emulation) |
| `wx.onHCEMessage(callback)` | Listen HCE message |

## UI — Interaction

Official reference: https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/

| API | Purpose | Notes |
|-----|---------|-------|
| `wx.showToast({title, icon, duration})` | Toast message | Max 7 chars with icon; 2 lines with `icon: 'none'` |
| `wx.hideToast()` | Hide toast | — |
| `wx.showLoading({title, mask})` | Loading spinner | Always pair with `hideLoading` |
| `wx.hideLoading()` | Hide loading | — |
| `wx.showModal({title, content, showCancel})` | Confirm/cancel dialog | Returns `confirm` / `cancel` |
| `wx.showActionSheet({itemList})` | Bottom action menu | Max 6 items |
| `wx.enableAlertBeforeUnload({message})` | Unload confirmation | — |
| `wx.disableAlertBeforeUnload()` | Disable unload confirmation | — |

Rules:
- `showLoading` and `showToast` can only display **one at a time**.
- Always pair `showLoading` with `hideLoading`.

## UI — Navigation Bar

| API | Purpose |
|-----|---------|
| `wx.setNavigationBarTitle({title})` | Dynamic title |
| `wx.setNavigationBarColor({frontColor, backgroundColor})` | Nav bar color |
| `wx.showNavigationBarLoading()` | Show loading icon in nav bar |
| `wx.hideNavigationBarLoading()` | Hide loading icon |
| `wx.hideHomeButton()` | Hide home button (pages not in tabBar) |

## UI — Tab Bar

| API | Purpose |
|-----|---------|
| `wx.showTabBar({animation})` | Show tab bar |
| `wx.hideTabBar({animation})` | Hide tab bar |
| `wx.setTabBarItem({index, text, iconPath})` | Set tab item |
| `wx.setTabBarStyle({color, selectedColor})` | Set tab style |
| `wx.showTabBarRedDot({index})` | Show red dot |
| `wx.hideTabBarRedDot({index})` | Hide red dot |
| `wx.setTabBarBadge({index, text})` | Show badge |
| `wx.removeTabBarBadge({index})` | Remove badge |

## UI — Background & Font

| API | Purpose |
|-----|---------|
| `wx.setBackgroundColor({backgroundColor, backgroundColorTop})` | Set page background |
| `wx.setBackgroundTextStyle({textStyle})` | Pull-down refresh text style |
| `wx.loadFontFace({family, source})` | Load custom font |

## Canvas

| API | Purpose |
|-----|---------|
| `wx.createCanvasContext(canvasId)` | Create 2D canvas context |
| `wx.canvasToTempFilePath({canvasId})` | Export canvas to image |
| `wx.canvasPutImageData({canvasId, data})` | Put pixel data |
| `wx.canvasGetImageData({canvasId})` | Get pixel data |

## Worker

| API | Purpose |
|-----|---------|
| `wx.createWorker(scriptPath)` | Create worker thread |
| `worker.postMessage(msg)` | Send message to worker |
| `worker.onMessage(callback)` | Listen worker message |
| `worker.terminate()` | Terminate worker |

## Performance

| API | Purpose |
|-----|---------|
| `wx.getPerformance()` | Get performance data |
| `Performance.createObserver(callback)` | Create performance observer |
| `wx.requestIdleCallback(callback)` | Execute in idle time |
| `wx.reportPerformance({id, value})` | Report speed data |
| `wx.preloadWebview()` | Preload next page WebView |
| `wx.preloadAssets({data})` | Preload fonts/images |

## Debug & Log

| API | Purpose |
|-----|---------|
| `console.log/warn/error/info/debug` | Console output |
| `wx.getLogManager({level})` | Log manager |
| `LogManager.log/info/warn/debug` | Write logs |
| `wx.getRealtimeLogManager()` | Realtime log (to MP Admin) |
| `RealtimeLogManager.info/warn/error` | Write realtime logs |
| `wx.setEnableDebug({enableDebug})` | Toggle debug mode |

## Routing Events

| API | Purpose |
|-----|---------|
| `wx.onAppRoute(callback)` | After route logic |
| `wx.onAppRouteDone(callback)` | After route animation |
| `wx.onBeforeAppRoute(callback)` | Before route logic |
| `wx.onBeforePageLoad(callback)` | Before new page instance |
| `wx.onAfterPageLoad(callback)` | After new page instance |
| `wx.onBeforePageUnload(callback)` | Before page destroy |
| `wx.onAfterPageUnload(callback)` | After page destroy |

## Mini Program Jump (Navigate to Other Mini Programs)

| API | Purpose | Notes |
|-----|---------|-------|
| `wx.navigateToMiniProgram({appId, path, extraData})` | Open another mini program | Must be in `navigateToMiniProgramAppIdList` in `app.json` |
| `wx.navigateBackMiniProgram({extraData})` | Return to source mini program | Only callable in target mini program |
| `wx.openEmbeddedMiniProgram({appId, path})` | Open embedded mini program | — |
| `wx.exitMiniProgram({success})` | Exit current mini program | — |

```json
// app.json required config
{
  "navigateToMiniProgramAppIdList": ["wx1234567890abcdef"]
}
```

## Subscribe Message

| API | Purpose | Notes |
|-----|---------|-------|
| `wx.requestSubscribeMessage({tmplIds})` | Request one-time subscription | User must tap button to trigger |
| `wx.getSetting({withSubscriptions})` | Get subscription settings | Check `subscriptionsSetting` |

```javascript
wx.requestSubscribeMessage({
  tmplIds: ['template-id-1', 'template-id-2'],
  success(res) {
    console.log(res['template-id-1']); // 'accept' | 'reject' | 'ban'
  }
});
```

## Share

| API | Purpose |
|-----|---------|
| `wx.showShareMenu({withShareTicket, menus})` | Show share button |
| `wx.hideShareMenu()` | Hide share button |
| `wx.updateShareMenu({withShareTicket, isUpdatableMessage})` | Update share config |
| `wx.getShareInfo({shareTicket})` | Get encrypted share data |

## Customer Service

| API | Purpose |
|-----|---------|
| `wx.openCustomerServiceConversation({sessionFrom, sendMessageTitle})` | Open customer service chat | Must be triggered by user tap |

## Ad

| API | Purpose |
|-----|---------|
| `wx.createRewardedVideoAd({adUnitId})` | Rewarded video ad |
| `RewardedVideoAd.load()` | Load ad |
| `RewardedVideoAd.show()` | Show ad |
| `RewardedVideoAd.onLoad(callback)` | Listen load event |
| `RewardedVideoAd.onClose(callback)` | Listen close (check `isEnded`) |
| `wx.createInterstitialAd({adUnitId})` | Interstitial ad |
| `InterstitialAd.load()` / `show()` | Load and show |

```javascript
const ad = wx.createRewardedVideoAd({ adUnitId: 'adunit-xxx' });
ad.onLoad(() => console.log('Ad loaded'));
ad.onClose((res) => {
  if (res.isEnded) {
    // User watched full video — grant reward
  }
});
ad.load().then(() => ad.show()).catch(console.error);
```

## Sensors (Accelerometer / Compass / Gyroscope)

| API | Purpose |
|-----|---------|
| `wx.startAccelerometer({interval})` | Start accelerometer |
| `wx.stopAccelerometer()` | Stop accelerometer |
| `wx.onAccelerometerChange(callback)` | Listen accelerometer data |
| `wx.offAccelerometerChange(callback)` | Remove listener |
| `wx.startCompass()` | Start compass |
| `wx.stopCompass()` | Stop compass |
| `wx.onCompassChange(callback)` | Listen compass data (direction in degrees) |
| `wx.offCompassChange(callback)` | Remove listener |
| `wx.startGyroscope({interval})` | Start gyroscope |
| `wx.stopGyroscope()` | Stop gyroscope |
| `wx.onGyroscopeChange(callback)` | Listen gyroscope data |
| `wx.offGyroscopeChange(callback)` | Remove listener |
| `wx.startDeviceMotionListening({interval})` | Start device motion |
| `wx.stopDeviceMotionListening()` | Stop device motion |
| `wx.onDeviceMotionChange(callback)` | Listen device motion |

## Calendar

| API | Purpose |
|-----|---------|
| `wx.addPhoneCalendar({title, startTime, endTime})` | Add event to system calendar |
| `wx.addPhoneRepeatCalendar({title, startTime, endTime, repeatInterval})` | Add recurring event |

## Contact

| API | Purpose |
|-----|---------|
| `wx.chooseContact()` | Select contact from WeChat address book |
| `wx.chooseContact({multiple})` | Select multiple contacts |

## Analytics

| API | Purpose |
|-----|---------|
| `wx.reportAnalytics(eventName, data)` | Report custom analytics event |

```javascript
wx.reportAnalytics('purchase', {
  price: 120,
  item_id: 'abc123'
});
```

## Screen Recording

| API | Purpose |
|-----|---------|
| `wx.getScreenRecordingState()` | Get current screen recording state |
| `wx.onScreenRecordingStateChanged(callback)` | Listen screen recording state change |
| `wx.offScreenRecordingStateChanged(callback)` | Remove listener |

## Subpackage Loading

| API | Purpose |
|-----|---------|
| `wx.preDownloadSubpackage({root, success})` | Pre-download subpackage |
| `PreDownloadSubpackageTask.onProgressUpdate(callback)` | Listen download progress |

## Login — Complete Flow

Official references:
- Client: https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html
- Server: https://developers.weixin.qq.com/miniprogram/dev/OpenApiDoc/user-login/code2Session.html

### Client: Get login code

```javascript
// pages/login/login.js
Page({
  async handleLogin() {
    try {
      // Step 1: Get code from WeChat (valid for 5 minutes)
      const { code } = await new Promise((resolve, reject) => {
        wx.login({ success: resolve, fail: reject });
      });

      // Step 2: Send code to your backend
      const res = await wx.request({
        url: 'https://your-api.com/auth/login',
        method: 'POST',
        data: { code }
      });

      // Step 3: Store session token
      wx.setStorageSync('token', res.data.token);
      wx.setStorageSync('openid', res.data.openid);

      // Step 4: Navigate to home
      wx.switchTab({ url: '/pages/index/index' });
    } catch (err) {
      console.error('Login failed:', err);
      wx.showToast({ title: '登录失败', icon: 'error' });
    }
  }
});
```

### Backend: Exchange code for session

```javascript
// Node.js backend example
const axios = require('axios');

async function handleLogin(req, res) {
  const { code } = req.body;

  // Call WeChat server API: jscode2session
  const { data } = await axios.get(
    'https://api.weixin.qq.com/sns/jscode2session',
    {
      params: {
        appid: process.env.APPID,        // Mini Program AppID
        secret: process.env.APP_SECRET,   // Mini Program AppSecret
        js_code: code,
        grant_type: 'authorization_code'
      }
    }
  );

  if (data.errcode) {
    return res.status(400).json({ error: data.errmsg });
  }

  // data contains: openid, unionid, session_key
  const { openid, unionid, session_key } = data;

  // session_key is sensitive — never send to client
  // Store it on server associated with the user's session

  // Generate your own session token
  const token = generateToken({ openid, unionid });

  // Save user to database (upsert)
  await db.collection('users').doc(openid).set({
    data: {
      openid,
      unionid: unionid || '',
      session_key,  // Store on server only
      lastLogin: db.serverDate()
    }
  }, { merge: true });

  res.json({ token, openid });
}
```

### Backend Response

```json
{
  "openid": "o3xxxxxxxxxxxx",
  "unionid": "o6xxxxxxxxxxxx",
  "session_key": "xxxxxxxxxxxxxxxx",
  "expires_in": 7200
}
```

### Rules

- `code` is **single-use** and expires in **5 minutes**.
- `session_key` is the encryption key for user data — **never send to client**.
- `openid` is unique per mini program per user.
- `unionid` is unique across all apps under the same WeChat Open Platform account.
- Your backend must store `session_key` securely (encrypt at rest).
- If `checkSession` fails, the user must re-login.

### Check Session Validity

```javascript
wx.checkSession({
  success() {
    // Session valid, user is logged in
  },
  fail() {
    // Session expired, call wx.login again
    wx.login({ success: (res) => sendCodeToBackend(res.code) });
  }
});
```
