# Head First Stock Investment 数据字段草案

> 本文件为后续个人交易数据库、Dashboard、Risk Agent、Review Agent 与 Trading Coach Agent 提供字段边界。当前为第一版草案，后续应与 Chapter 22、Chapter 40、Chapter 45-47 对齐。

## 1. MarketSnapshot

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `date` | date | 交易日期 |
| `index_trend` | enum | 指数趋势：up / range / down |
| `market_turnover` | number | 市场成交额 |
| `turnover_change_rate` | number | 成交额变化率 |
| `advance_decline_ratio` | number | 涨跌家数比 |
| `risk_appetite` | enum | risk_on / neutral / risk_off |
| `market_state` | enum | attack / trend / conflict / defense |

## 2. StockSnapshot

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `symbol` | string | 股票代码 |
| `name` | string | 股票名称 |
| `industry` | string | 所属产业 |
| `market_cap` | number | 市值 |
| `close_price` | number | 收盘价 |
| `turnover_amount` | number | 个股成交额 |
| `turnover_baseline` | number | 成交额基准 |
| `volume_expansion_ratio` | number | 成交额跃迁倍率 |
| `trend_state` | enum | base / breakout / uptrend / divergence / distribution |

## 3. CapitalSignal

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `symbol` | string | 股票代码 |
| `date` | date | 日期 |
| `net_inflow` | number | 净流入 |
| `inflow_persistence_days` | number | 资金持续天数 |
| `price_efficiency` | number | 价格效率 |
| `accumulation_score` | number | 吸筹评分 |
| `smart_money_score` | number | 机构资金评分 |
| `signal_quality` | enum | true_accumulation / weak / fake_inflow / distribution |

## 4. AccountSnapshot

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `date` | date | 日期 |
| `equity` | number | 当前净值 |
| `high_water_mark` | number | 历史最高净值 |
| `drawdown_rate` | number | 相对 HWM 回撤 |
| `cash_ratio` | number | 现金比例 |
| `gross_exposure` | number | 总仓位 |
| `single_stock_max_weight` | number | 单股最大仓位 |
| `risk_zone` | enum | expansion / normal / warning / crisis |

## 5. TradeLedger

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `trade_id` | string | 交易 ID |
| `symbol` | string | 股票代码 |
| `side` | enum | buy / sell |
| `executed_at` | datetime | 成交时间 |
| `price` | number | 成交价格 |
| `quantity` | number | 成交数量 |
| `position_weight_after_trade` | number | 成交后仓位 |
| `trade_thesis` | text | 交易逻辑 |
| `risk_snapshot_id` | string | 对应风险快照 |
| `rule_violations` | array | 违反的规则 |
| `review_status` | enum | pending / reviewed / upgraded |

## 6. ReviewRecord

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `review_id` | string | 复盘 ID |
| `trade_id` | string | 关联交易 |
| `original_hypothesis` | text | 原始假设 |
| `actual_outcome` | text | 实际结果 |
| `error_type` | enum | research / timing / position / exit / emotion / none |
| `lesson` | text | 复盘结论 |
| `new_rule` | text | 新增或更新规则 |

## 使用原则

- 先保证人工可填写，再逐步自动化。
- 每个字段都必须服务于“决策、风控、复盘、规则升级”之一。
- 不为炫技采集无动作意义的数据。
