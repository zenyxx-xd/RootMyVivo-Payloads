# RootMyVivo Payloads

[RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo) 的 payload 目录。应用本体不带任何漏洞利用——全部从这里下载。

[Русский](README.md) · [English](README.en.md)

## 仓库内容

- `support/targets-vivo.json` —— 目录：哪些设备受支持、用哪个漏洞、二进制在哪
- Releases —— payload 本体。一个设备一个构建，偏移量在内核之间不通用

## 应用怎么挑 payload

先看机型（`models` / `marketNames`），再看内核。`kernelVersions` 有三种写法：

```json
"6.6.89"                              // 短格式：任意 6.6.89
"6.6.89-android15-8-gb57af212129c"    // 完整格式：指定一个内核构建（对照 uname）
"6.6.89-android15-8-*"                // 前缀匹配
```

完整格式用在同一个机型存在多种内核构建的情况——比如 Neo10 Pro 既有 `gf2c960562dc8` 的，也有和 X200 共用的 `b57af212129c`。用短格式会拿错二进制，slide 那一步直接就不收敛。

## 构建

| Release | 内容 |
|---|---|
| [v0.4.0-ports](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-ports) | RMV 引擎移植：iQOO 13 印度版 (I2401)、Neo10 Pro (PD2426)、X200 (PD2415)、X200 Pro (PD2405) |
| [v0.4.0-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-neo11) | Neo 11，内核 6.6.89 |
| [v0.4.0-6.6.127-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.4.0-6.6.127-neo11) | Neo 11，内核 6.6.127 |
| [v0.2.1-neo11](https://github.com/zenyxx-xd/RootMyVivo-Payloads/releases/tag/v0.2.1-neo11) | Neo 11，早期构建 |

移植用的偏移量来自各机型作者公开的仓库（AmarnathCJD、sgswzglwlx、xiaohj233、CyberMeowfia）——署名在目录每条记录的 `verifiedBy` 字段里。

## 添加自己的设备

需要你固件的 `boot.img`（OTA 包就够，不用 root），或者 kallsyms 转储。然后：

1. 用 kallsyms+BTF 生成 target.h —— 见 [RootMyVivo-Exploit](https://github.com/zenyxx-xd/RootMyVivo-Exploit)，生成器在那里
2. 编译 preload.so，确认链条至少能过 slide 那步
3. 向 `targets-vivo.json` 提 PR：机型、完整内核字符串、二进制链接、sha256

也欢迎第三方的 payload，前提是它不会在安装中途把手机软重启——这套东西存在的意义就在这。

## 免责声明

只用于自己的设备，后果自负。
