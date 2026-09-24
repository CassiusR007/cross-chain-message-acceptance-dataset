# MAAD：跨链异常消息接受事件证据集

[English](README.en.md) · v0.2.0 · 检索截止：2026-09-22 · MIT

基于公开安全报告，按照跨链消息生命周期与验证语义整理异常消息接受事件。包含源端观察异常、签名信任失守、证明验证缺陷、重放及执行权限异常。

## 下载完整数据集

**[下载 MAAD v0.2.0 完整数据包](MAAD-v0.2.0.zip?raw=true)**

本次以完整 ZIP 包发布。请下载并解压后使用；事件表、来源表、原始保留数据、验证脚本及详细文档均在包内。包内 README 的相对链接对应解压后的目录。

[数据卡](DATA_CARD.md) · [来源与许可说明](NOTICE.md) · [更新记录](CHANGELOG.md) · [发布校验记录](verification.json)

## 数据规模与证据边界

- 31 个核心异常接受事件，均为报告支持的恶意事件。
- 206 条目录记录，其中 41 条有报告支持、33 条为未解析组；目录记录不等于独立攻击数量。
- 44 个来源登记、84 条主张、15 个报告提供的完整交易入口、46 条检索记录。
- 保留消息层包含 61 笔链上交易观察、24 个消息候选、22 个接受观察；语义验证消息与严格基准样本均为 0。

**这是事件证据集，不是已经完成链上取证的消息级机器学习基准。** 未核实线索、未解析组和范围外案例不能直接用作正常负样本。报告中的交易入口也不等于本版重新核验的链上证据。2025 年未纳入核心事件不代表该年没有攻击。来源独立性未建立时保留未知。

## 解压后使用

核心数据位于 `event-evidence/data/`，包含 `events.csv`、`sources.csv`、`claims.csv`、`transaction_links.csv`、映射表和检索日志，并提供对应 JSONL。

`event-evidence/statistics.json` 中的 `core_event_ids` 给出核心集合。字段与质量局限见 `event-evidence/DATA_DICTIONARY.md`、`event-evidence/quality_issues.json`。采集来源、主张定位与访问状态可逐项追溯。

在解压后的根目录运行：

```console
python event-evidence/validate.py
python message-acceptance/validate.py
python -m unittest discover -s event-evidence -p "test_*.py"
```

建立检测器时，应以消息实例作为检测样本，并继续恢复源/目标链日志、证明与历史配置；按事件或同源攻击活动划分训练和测试集，避免同一攻击泄漏到两侧。

## 可复现性与许可

数据包包含 324 个文件，其中 323 个内容文件列于 `release/manifest.json`，另一个文件是清单本身。已通过匿名下载逐项比对本地发布文件，结果见 [verification.json](verification.json)。检索不声称全球穷尽覆盖，来源正文不完整时已明确记录。

保留上游 281 个文件及原有 MIT 许可与署名；本版新增标注、方法和代码详见包内文档。引用元数据见 [CITATION.cff](CITATION.cff)。公开报告的版权仍归原作者。
