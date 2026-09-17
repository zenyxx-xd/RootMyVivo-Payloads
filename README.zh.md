# RootMyVivo Payloads（中文）

[RootMyVivo](https://github.com/zenyxx-xd/RootMyVivo) 应用的载荷目录。
应用不含任何漏洞利用代码，按 [`catalog/devices.json`](catalog/devices.json)
的描述下载二进制。

[Русский](README.md) · [English](README.en.md)

## 结构

```
catalog/devices.json        唯一目录清单（schemaVersion 5）
bin/                        规范 .so，文件名 = 内核构建 git-id
                            （bin/g1f71897ac249.so）；无 git 后缀的构建使用
                            可读 id（pd2405-ap3a.so）
support/targets-vivo.json   旧版应用（v4 格式）的冻结目录，勿改。
```

## devices.json（v5）

- `builds`：按 GKI git-id 索引的**内核构建**表。一个构建 = 一个 Image = 一个
  载荷。字段：`match`（uname 匹配模式，完整 GKI 串比短版本更严格）、
  `exploit`、`status`、`file`（{name,url,mirrors,sha256,size}）、`env`、
  `matchCondition`（来源与验证依据）。
- `devices`：每台物理机器一条：`marketName` + `code`（V 型号），别名
  `models`（Build.DEVICE）/ `names`（Build.MODEL），`kernels` 列出该机所有
  已知内核构建 `{build, note}`。
- `status`：`ready` · `off` · `patched`（该内核已修复）· `unsupported`。
- 匹配：先按 models/names 找机器，再按 match 找内核——短版本须与 uname
  release 完全相等，完整串按子串匹配（区分同型号不同构建）。没有“任意该
  型号载荷”的兜底。

## 添加设备

1. 获取该构建准确的 kernel Image（完整 OTA → boot → uname/kallsyms，确认
   remove_waiter 未打补丁）。
2. 编译 .so 放入 `bin/`（按 git-id 命名），记录 sha256/大小。
3. 补充 `builds` 与 `devices`（note 注明数据来源）。不得臆造型号代码与状态。

旧版应用读取冻结的 `support/targets-vivo.json`（v4）。
