---
title      : 利用快捷指令 URL Scheme 创建苹果备忘录链接
description: 构造 URL 运行特定快捷指令，将备忘录标题作为快捷指令输入，使用快捷指令打开备忘录。
date       : 2022-11-23T10:34:05+08:00
RevisionHistory:
- Date: &ModDate 2022-11-23T10:34:05+08:00
  Mods:
  - 创建。
- Date: &ModDate 2022-11-23T19:35:33+0800
  Mods:
  - 更正：第一段改为 “……快捷指令提供 URL Scheme 功能……”，原为 “……备忘录提供 URL Scheme 功能……”。
lastmod: *ModDate

Sections:
  - Title: 链接区域
    Groups:
    - Title: 相关内容
      Links:
      - Type       : URL
        URL        : https://support.apple.com/zh-cn/guide/shortcuts/apd621a1ad7a/ios
        Title      : “快捷指令”中的 URL 方案介绍
        Description: 快捷指令使用手册
    - Title: 索引
      Links:
      - Type: Page
        URL : /
---

{{<aside warning>}}
标题带有空格的备忘录无法打开。见下文 “问题”。
{{</aside>}}

　　苹果备忘录不能在不共享的情况下创建备忘录链接，但是快捷指令提供 URL Scheme 功能，可以通过构造一个 URL 运行特定快捷指令并传入参数。

## 快捷指令 URL Scheme
　　快捷指令 URL Scheme 格式如下：

```
shortcuts://run-shortcut?name=[名称]&input=[输入]&text=[文本]
```
　　参数说明：
name
: 快捷指令标题。推荐使用逆 DNS 命名方式避免重名。

input
: 制定快捷指令输入的来源。可选值：
  - `text`: 从 text 参数输入。
  - `clipboard`: 从剪贴板输入。

text
: 输入快捷指令的实际内容。需要注意：
  1. 需要进行 URL 编码。
  1. `input` 参数的值必须为 `text`。

## 构建快捷指令
{{< figure src="快捷指令内容.png" >}}

　　本示例将快捷指令命名为 `com.example.OpenNote`。

## 通过另一个快捷指令生成 URL
{{< figure src="生成备忘录链接.png" >}}

　　注意勾选 “作为快捷操作使用 > 服务菜单”，这样在备忘录中选择一个备忘录标题后右键单击，在最底部（或者服务菜单）和 macOS 菜单栏的“备忘录 > 服务”就能看到 “生成备忘录链接” 快捷指令。点击后链接被拷贝到剪贴板，在需要的位置粘贴即可。

## 问题
　　经过测试，如果备忘录标题包含空格会导致链接无法打开。链接在存储到备忘录中时 `text` 参数会被解码。存储在其他地方（例如 iMessage）的链接，在 `text` 参数正确编码的情况下可以打开备忘录。