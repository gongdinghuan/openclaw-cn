---
name: longport-wrapper
description: LongPort股票数据查询封装工具。提供简化的命令行接口来查询港股、美股、A股的实时行情、K线数据、盘口深度、资金流向、逐笔交易、期权链等。适用于股票投资研究、行情监控、数据分析等场景。
metadata:
  emoji: "📈"
  requires:
    bins: []
---

## 使用条件

### ✅ 何时使用
- ✅ 使用场景：
- 查询港股、美股、A股的实时行情
- 获取股票K线数据（日线、周线、月线、分钟线）
- 查询盘口深度（五档、十档）
- 分析资金流向（主力、机构、散户）
- 查询逐笔交易记录
- 查询期权链数据
- 批量监控多只股票
- 设置价格提醒
- 导出数据到CSV进行分析
- 输入示例：
- "查询腾讯股价"
- "获取苹果股票K线"
- "查看特斯拉资金流向"
- "监控自选股"

### ❌ 何时不使用
- ❌ 不适用场景：
- 不支持加密货币（如 BTC、ETH）
- 不支持外汇交易
- 实时交易执行（此技能仅查询数据）
- 需要登录订单页面的数据
- 需要复杂的技术分析（应使用 data-intelligence 或 code_interpreter）

## 命令

# LongPort 股票查询技能

## 📊 技能说明

**用途**：封装 LongPort API，提供简化的股票数据查询命令

**核心能力**：
- 实时行情查询
- K线数据获取
- 盘口深度查询
- 资金流向分析
- 逐笔交易查询
- 期权链查询
- 每股数据查询

---

## 🚀 快速开始

### 安装依赖

```bash
pip install openquote pandas numpy
```

配置 LongPort Token（首次使用需要）：
```bash
export LONGPORT_APP_KEY="your-app-key"
export LONGPORT_APP_SECRET="your-app-secret"
export LONGPORT_ACCESS_TOKEN="your-access-token"
```

---

## 📋 基础查询命令

### 1️⃣ 实时行情查询

```bash
# 查询单只股票实时行情
longport query 700.HK

# 查询多只股票
longport query 700.HK 0700.HK AAPL.US TSLA.US

# 查询 A 股
longport query 600519.SH 000001.SZ

# 实时监控（自动刷新）
longport watch 700.HK
```

**输出示例**：
```
📊 700.HK - 腾讯控股
─────────────────────────────────────
当前价:  385.60 HKD
涨跌额:  +2.40 (+0.63%)
今开:    383.20
最高:    386.40
最低:    382.00
成交量:  12.5M
成交额:  4.8B
─────────────────────────────────────
更新时间: 2026-03-09 11:35:51
```

---

### 2️⃣ K线数据查询

```bash
# 日线 K 线（默认）
longport kline 700.HK

# 周线 K 线
longport kline 700.HK --period week

# 月线 K 线
longport kline 700.HK --period month

# 1分钟 K 线
longport kline 700.HK --period 1min

# 5分钟 K 线
longport kline 700.HK --period 5min

# 30分钟 K 线
longport kline 700.HK --period 30min

# 60分钟 K 线
longport kline 700.HK --period 60min

# 指定数量
longport kline 700.HK --count 200

# 导出到 CSV
longport kline 700.HK --output tencent_kline.csv
```

**输出格式**：
```
日期        | 开盘    | 最高    | 最低    | 收盘    | 成交量   | 成交额
────────────────────────────────────────────────────────────────────────
2026-03-09 | 383.20  | 386.40  | 382.00  | 385.60  | 12.5M   | 4.8B
2026-03-08 | 380.00  | 383.80  | 379.20  | 382.80  | 15.2M   | 5.8B
...
```

---

### 3️⃣ 盘口深度查询

```bash
# 查询五档盘口
longport depth 700.HK

# 查询十档盘口
longport depth 700.HK --level 10

# 实时监控盘口变化
longport depth 700.HK --watch
```

**输出示例**：
```
📊 700.HK - 盘口深度
─────────────────────────────────────
卖十  386.00  500K
卖九   385.80  800K
卖八   385.60  1.2M
卖七   385.40  900K
卖六   385.20  700K
卖五   385.00  1.5M
卖四   384.80  1.1M
卖三   384.60  800K
卖二   384.40  600K
卖一   384.20  1.8M
─────────────────────────────────────
当前价  385.60
─────────────────────────────────────
买一   385.20  1.2M
买二   385.00  900K
买三   384.80  800K
买四   384.60  600K
买五   384.40  500K
买六   384.20  400K
买七   384.00  300K
买八   383.80  250K
买九   383.60  200K
买十   383.40  150K
─────────────────────────────────────
```

---

### 4️⃣ 资金流向查询

```bash
# 查询资金流向
longport capital 700.HK

# 机构资金
longport capital 700.HK --institutional

# 主力资金
longport capital 700.HK --main
```

**输出示例**：
```
💰 700.HK - 资金流向
─────────────────────────────────────
主力资金:    +1.2B (+15.3%)
机构资金:    +800M (+10.2%)
散户资金:    -400M (-5.1%)
─────────────────────────────────────
今日流入:    +1.6B
今日流出:    +400M
净流入:      +1.2B
─────────────────────────────────────
近3日净流入: +2.8B
近5日净流入: +3.5B
近10日净流入: +5.2B
─────────────────────────────────────
```

---

### 5️⃣ 逐笔交易查询

```bash
# 查询最近100笔交易
longport trades 700.HK

# 查询指定数量
longport trades 700.HK --count 500

# 只看大单（>100万）
longport trades 700.HK --big
```

**输出示例**：
```
📝 700.HK - 逐笔交易（最近50笔）
─────────────────────────────────────
时间        | 价格    | 成交量   | 方向 | 类型
─────────────────────────────────────
11:35:48   | 385.60  | 10K      | 买   | 主力
11:35:47   | 385.50  | 5K       | 卖   | 散户
11:35:46   | 385.60  | 50K      | 买   | 机构
11:35:45   | 385.40  | 8K       | 买   | 主力
...
─────────────────────────────────────
大单统计:  15笔 (+12.5B)
主力占比:  62.3%
机构占比:  28.1%
散户占比:  9.6%
─────────────────────────────────────
```

---

### 6️⃣ 期权链查询

```bash
# 查询期权链
longport options AAPL.US

# 指定到期日
longport options AAPL.US --expiry 2026-03-20

# 只看看涨期权
longport options AAPL.US --type call

# 只看看跌期权
longport options AAPL.US --type put
```

**输出示例**：
```
📊 AAPL.US - 期权链
─────────────────────────────────────
到期日: 2026-03-20
─────────────────────────────────────
行权价  | 看涨买价 | 看涨卖价 | 看涨成交量 | 看跌买价 | 看跌卖价 | 看跌成交量
─────────────────────────────────────
150.00  | 98.50   | 99.00    | 1.2K      | 0.02    | 0.03    | 500
155.00  | 93.80   | 94.20    | 2.5K      | 0.05    | 0.08    | 800
160.00  | 89.20   | 89.60    | 3.1K      | 0.12    | 0.15    | 1.2K
...
─────────────────────────────────────
```

---

### 7️⃣ 每股数据查询

```bash
# 查询股票基本信息
longport info 700.HK

# 查询财务数据
longport info 700.HK --financial

# 查询公司资料
longport info 700.HK --profile
```

**输出示例**：
```
🏢 700.HK - 基本信息
─────────────────────────────────────
公司名称:   腾讯控股有限公司
股票代码:   700.HK
上市日期:   2004-06-16
行业:       科技
板块:       恒生科技指数
─────────────────────────────────────
总股本:     96.3B
流通股:     94.8B
总市值:     37.1T
流通市值:   36.5T
─────────────────────────────────────
市盈率:     25.6
市净率:     4.8
股息率:     0.8%
每股收益:   15.02
每股净资产:  80.26
─────────────────────────────────────
```

---

## 📊 批量查询

### 查询自选股

```bash
# 从文件读取股票列表
longport query --file watchlist.txt

# 监控自选股
longport watch --file watchlist.txt
```

### 监控价格变化

```bash
# 设置价格提醒
longport alert 700.HK --above 390
longport alert 700.HK --below 380

# 查询所有提醒
longport alerts
```

---

## 🎯 实战案例

### 案例1：查询腾讯控股完整数据

```bash
# 1. 实时行情
longport query 700.HK

# 2. 获取日线K线
longport kline 700.HK --count 100 --output tencent.csv

# 3. 查询盘口
longport depth 700.HK

# 4. 查询资金流向
longport capital 700.HK
```

### 案例2：监控美股科技股

```bash
# 创建监控列表
echo "AAPL.US
MSFT.US
GOOGL.US
TSLA.US
NVDA.US" > tech_stocks.txt

# 实时监控
longport watch --file tech_stocks.txt
```

### 案例3：期权策略分析

```bash
# 查询期权链
longport options AAPL.US --expiry 2026-03-20

# 分析看涨期权
longport options AAPL.US --type call --expiry 2026-03-20

# 分析看跌期权
longport options AAPL.US --type put --expiry 2026-03-20
```

---

## 📚 Python API

如果需要更复杂的分析，可以直接使用 Python API：

```python
from openquote import QuoteContext
from datetime import datetime, timedelta

# 初始化
ctx = QuoteContext()

# 查询实时行情
quote = ctx.quote(["700.HK"], fields=["last_price", "change_percent"])
print(f"腾讯股价: {quote['700.HK']['last_price']}")

# 获取K线
klines = ctx.daily_klines("700.HK", count=100)
for kline in klines:
    print(f"{kline.timestamp} | 开:{kline.open} 高:{kline.high} 低:{kline.low} 收:{kline.close}")

# 查询资金流向
capital = ctx.capital_flow("700.HK")
print(f"主力资金: {capital.main_net_inflow}")

# 关闭连接
ctx.close()
```

---

## ⚠️ 注意事项

1. **API 限制**：
   - 每分钟请求次数限制
   - 请合理控制查询频率

2. **数据延迟**：
   - 港股：实时
   - 美股：15分钟延迟（免费版）
   - A股：实时

3. **依赖安装**：
   ```bash
   pip install openquote pandas numpy
   ```

4. **认证配置**：
   需要申请 LongPort App Key 和 Access Token
   - 访问 https://open.longport.app
   - 创建应用并获取凭证

---

## 📖 更多信息

- LongPort 官网: https://open.longport.app
- API 文档: https://open.longport.app/docs
- SDK 下载: https://github.com/longportapp/openquant

---
*此技能由 JARVIS 自动创建*

---
*此技能由 JARVIS 自动创建*
