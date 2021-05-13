# 问题描述
在原生 app 上 WebSocket 模块无法正常连接

# 原因
在某些原生平台需要添加证书才能确保 WebSocket 正常使用

# 解决方案
```typescript
// cacert 证书位于 resources 根目录
let uuid = cc.resources.getInfoWithPath('cacert').uuid;
let nativeUrl = cc.assetManager.utils.getUrlWithUuid(uuid, { isNative: true, nativeExt: '.pem' });
this._ws = new WebSocket(url, null, nativeUrl);
this._ws.binaryType = options.binaryType ? options.binaryType : "arraybuffer";
this._ws.onmessage = (event) => {
    this.onMessage(event.data);
};
this._ws.onopen = this.onConnected;
this._ws.onerror = this.onError;
this._ws.onclose = this.onClosed;
```

# 附件列表
* [WebSocket 证书](../../source/attach/cacert.pem)