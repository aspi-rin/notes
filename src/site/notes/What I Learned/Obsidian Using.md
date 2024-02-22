---
{"dg-publish":true,"permalink":"/What I Learned/Obsidian Using/"}
---

## 关于 Digital Garden 插件的使用

## Advanced Codeblock 显示行号，高亮整行

[GitHub - lijyze/obsidian-advanced-codeblock: An obsidian plugin that give additional features to code blocks.](https://github.com/lijyze/obsidian-advanced-codeblock)

![](https://s2.loli.net/2024/02/07/bZoCfSmPux5A3et.jpg)

## Picgo 上传图片到图床配置

### Cloudflare R2 桶

安装 S3 插件

![](https://pic.aspi-rin.top/2024/02/b1bddaaf690ce7b1671a8ea12b680049.jpeg)

Picgo 配置

ID 和密钥通过创建 R2 API 令牌获取。

文件按年月存，不用改。

自定义节点在「存储桶详细信息 - S3 API」中，由于前面填过桶名，因此要把后面的桶名删掉。

![](https://pic.aspi-rin.top/2024/02/a5ced32d550afb1790717d54bff67b56.jpeg)

### SMMS 图床

![](https://s2.loli.net/2024/02/07/O4q6TSGAQl12kmW.jpg)

![](https://s2.loli.net/2024/02/07/RdgGb4BLjs29rqo.jpg)

## Excalidraw 字体设置

设置页面 - 非官方支持的特性 - 字体

[[Clipped/Obsidian 的 Excalidraw 插件自定义中文字体\|Obsidian 的 Excalidraw 插件自定义中文字体]]

---

## 超链接语法 - 嵌入引用

嵌入引用的语法 `![[Note Name]]`，即在使用「双向链接」的时候，我们可以在「双向链接」前边输入一个 `!`（叹号），这种「双向链接」的添加方式称为「嵌入引用」。

## 超链接语法 - 块引用

块引用的语法 `[[Note Name ^]]` ，既在使用「双向链接」的时候，我们可以在「双向链接」的后边输入 `^` ，此时我们可以将被链接的笔记中的某一行（包括它的从属段落）引入到当前笔记中。这种方式成为「块引用」。

## 快速插入 Callout

安装 Admonition 插件后，使用 `/ad` 指令调用 `insert callout`

> [!success]+
> xxxx

> [!example]

> [!faq]
> xxxx

---
