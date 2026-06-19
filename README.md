# automatic translation

一个油猴网页翻译脚本。默认自动翻译网页，可按域名记住“翻译/不翻译”，支持拖动悬浮按钮、语言方向设置、翻译服务切换和自定义 GET 接口。

脚本文件：`automatic translation.txt`

## 功能

- 默认翻译方向：英语 -> 简体中文。
- 支持“默认是否自动翻译”开关。
- 支持按域名单独保存“翻译/不翻译”选择。
- 可选择是否在未手动设置过的网站自动弹出“是否翻译”提示。
- 右上角“译”按钮可拖动，位置自动保存。
- 图标 5 秒无操作后淡化，透明度可设置。
- 支持微软翻译、Google 翻译、MyMemory 翻译和自定义接口。
- 支持设置来源语言和目标语言，例如“所有语言 -> 简体中文”“简体中文 -> 法语”。
- 设置页可搜索并修改手动操作过的网站。

## 安装

1. 安装 Tampermonkey、Violentmonkey 或 Greasemonkey。
2. 新建用户脚本。
3. 将 `automatic translation.txt` 的全部内容复制进去。
4. 保存并启用脚本。
5. 打开网页，右上角会出现“译”按钮。

## 使用

### 当前网站

- 点击“译”按钮可打开当前网站弹窗。
- 点击“翻译”或“不翻译”后，选择会按当前域名保存。
- 如果某个域名已经手动选择过，后续即使开启“每个网页都提示”，也不会再自动弹出提示。

### 设置页

点击“译”按钮，再点击齿轮进入设置页。

可设置：

- 默认是否自动翻译。
- 每个网页是否提示翻译。
- 图标淡化后的透明度。
- 翻译服务。
- 来源语言和目标语言。
- 手动操作过的网站翻译选择。

设置会自动保存，不需要点击保存按钮。

### 图标

- 按住“译”按钮可拖动位置。
- 松开后位置自动保存。
- 5 秒无操作后图标会按设置透明度淡化。

## 翻译服务

### 默认微软接口

默认“微软翻译”使用接近 Edge 网页翻译的接口链路：

```text
https://edge.microsoft.com/translate/auth
```

先获取临时 token，再请求：

```text
https://api-edge.cognitive.microsofttranslator.com/translate
```

这个 token 不是你的微软账号 token，也不会读取本机 Edge 登录账号或 Cookie。它更像网页翻译功能的短期访问票据。该接口不是正式公开 API，微软可能调整规则、限流或失效。

### 自定义接口

自定义接口目前只支持简单 GET 请求。接口地址支持：

- `{text}`：要翻译的文本。
- `{from}`：来源语言，可能是 `auto`。
- `{to}`：目标语言。

如果返回 JSON，在“结果字段”里填写结果路径，例如：

```text
responseData.translatedText
```

MyMemory 示例：

```text
接口地址：https://api.mymemory.translated.net/get?q={text}&langpair={from}|{to}
结果字段：responseData.translatedText
```

有道/网易网页接口示例：

```text
接口地址：https://fanyi.youdao.com/translate?doctype=json&type=AUTO&i={text}
结果字段：translateResult.0.0.tgt
```

有道网页接口不是正式开放平台接口，可能限流或失效。正式 API 通常需要 app key、secret 和动态签名，当前自定义接口不能直接生成签名。

## 注意事项

- 翻译内容会发送到你选择的翻译服务，敏感页面谨慎开启。
- 某些动态网页可能出现少量文字未翻译，等待或滚动后可能补翻。
- 如果网站禁止脚本运行，脚本可能无法生效。
- MyMemory 等免费接口可能有频率限制。
- 如果默认微软接口失效，可切换 Google、MyMemory 或自定义接口。

## 脚本信息

- 名称：automatic translation
- 版本：v1
- 默认翻译方向：英语 -> 简体中文
- 默认翻译服务：微软翻译
- 运行范围：`http://*/*` 和 `https://*/*`
