# MAAD：跨链异常消息接受事件证据集

[English](README.en.md) · v0.2.0 · 检索截止：2026-09-22 · MIT

围绕跨链消息生命周期与验证语义，整理公开安全报告中的异常接受事件。关注的问题是：目标端接受、执行或记账的请求，是否具备协议预期的源端事实、认证、资产语义和执行权限？范围包括链下错误观察、签名信任失守、证明验证缺陷、消息重放及应用接收器越权。

**本版是事件证据集，不是已完成取证的消息级机器学习基准。** 部分攻击存在真实源消息或合法签名，仅做源交易配对不足以识别。

## 本版实际规模

<!-- MAAD:STATS:BEGIN -->
核心异常接受事件：**31**（恶意事件 31；白帽事件 0）。

目录 206 条记录；其中 41 条有报告支持，33 条为未解析组。

证据包含 44 个来源、84 条主张、15 个唯一报告交易入口、46 条检索记录。

来源访问状态：{"partial_text_read": 42, "search_snippet_only": 1, "unavailable": 1}。

保留的 v0.1.0 消息层：61 笔交易观察、24 个消息候选、22 个接受观察、0 个语义验证消息、0 个严格基准样本。
<!-- MAAD:STATS:END -->

精确统计由脚本生成，见 [statistics.json](event-evidence/statistics.json)。多标签类别计数不能相加作为事件总数。33 条未解析组不算独立事件。核心集当前白帽计数为 0；Ronin 2024 的技术机制已支持，但本版未仅凭资金返还推断白帽动机。

## 获取和使用

- [事件 CSV](event-evidence/data/events.csv) / [JSONL](event-evidence/data/events.jsonl)：标注、范围、原因、局限及来源 ID。
- [来源表](event-evidence/data/sources.csv) 与 [主张表](event-evidence/data/claims.csv)：报告 URL、访问状态、章节定位和逐条支持关系。
- [交易入口](event-evidence/data/transaction_links.csv)：链、完整哈希、报告描述角色和验证等级。
- [上游映射](event-evidence/data/upstream_map.csv)、[27 条既有线索映射](event-evidence/data/case_map.csv)、[检索日志](event-evidence/data/search_log.csv)。
- [数据卡](DATA_CARD.md)、[字段字典](event-evidence/DATA_DICTIONARY.md)、[质量缺口](event-evidence/quality_issues.json)。

核心筛选条件为 `record_type=event`、`scope=in_scope`、`review_status=report_supported`、`case_nature` 为 `malicious_attack` 或 `whitehat`，且不存在阻止纳入的冲突。验证器额外要求至少一条已读取正文的 `acceptance_mechanism` 支持主张。推荐直接使用统计文件中的 `core_event_ids`，不要只筛 `scope`。

若建立消息级检测器，应继续采集 source/destination receipts、logs、traces、消息标识及历史配置。未找到源消息可能来自检索不全，不能自动标为异常。按事件或同源攻击 campaign 划分训练/测试集；不要把同一次事件的交易随机拆分，也不要把范围外或未核实候选当成正常负样本。

## 离线复现

Python 3.10+，无需第三方依赖。在仓库根目录执行：

```sh
python -B event-evidence/build.py
python -B -m unittest discover -s event-evidence -p 'test_*.py'
python -B event-evidence/validate.py
python -B -m unittest discover -s message-acceptance -p 'test_*.py'
python -B message-acceptance/validate.py
python -B -m unittest discover -s release -p 'test_*.py'
python -B release/package.py
```

人工标注输入只在 `event-evidence/curation/*.jsonl`；`data/`、统计及缺口由脚本确定性生成。冻结的旧数据与消息层按 SHA-256 核对。`message-acceptance/manifest.json` 描述保留的 **v0.1.0** 层，不能与 v0.2.0 事件统计混用。

## 证据边界与来源

采用项目方复盘及 CertiK、SlowMist、BlockSec、Halborn、Verichains、Immunefi、Dedaub、SolidityScan 等机构公开分析。每个来源按实际阅读状态登记；来源独立性未能确认时明确记为 unknown。多数事件只有一个可确认的证据根源，**没有经过双人独立事实标注或全量链上重放**。检索是人工定向、非穷尽的，年份分布不能代表全行业发生率。

继承记录的名称、日期和链可能仍含上游未经证实的描述；`inherited_candidate` 只表示描述层初筛。必须结合 `review_status`、`date_basis`、主张及局限使用。事件性质是事件级概括，不适用于事件内每个参与者或交易。对恢复资金不自动推断攻击者善意，不把有效签名等同于真实源状态。

上游：[Justin Zhou / Cross-chain-anomaly-event-dataset](https://github.com/justinzjj/Cross-chain-anomaly-event-dataset)，冻结版本 `bdb0893c51cca7ce2dee52f8228b90bab4d0f290`。保留上游许可与署名，见 [NOTICE](NOTICE.md)。外部报告只提供链接与简短事实转述，其版权未被本仓库重新许可。引用信息见 [CITATION.cff](CITATION.cff)。
