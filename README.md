# ETF策略回测系统

一个基于 Python Backtrader回测框架的 ETF 量化交易策略回测系统，使用 Streamlit 构建 Web 界面，支持多种交易策略的回测和分析。

## 功能特点

- 🚀 支持多种交易策略，采用工厂模式便于扩展
- 📊 实时数据获取（支持 Tushare 和 AKShare）
- 📈 交互式图表展示（K线、均线、交易点位等）
- 💹 详细的回测指标（夏普比率、最大回撤、胜率等）
- 🔄 T+1 交易规则支持
- ⚠️ 风险控制（追踪止损、最大回撤限制等）
- 💰 精确的交易费用计算（佣金等）
- 📝 完整的交易日志

## 内置策略

### 双均线策略
- 使用快速和慢速移动平均线的交叉产生交易信号
- 支持追踪止损进行风险控制
- 基于 ATR 动态计算持仓规模
- 考虑 T+1 交易限制
- 支持涨跌停板限制

## 安装使用

1. 克隆仓库：
```bash
git clone https://github.com/sencloud/ETF-Strategies.git
cd ETF-Strategies
```

2. 安装依赖，使用miniconda：
```bash
conda create -n etf python=3.10
conda activate etf
pip install -r requirements.txt
```

3. 运行系统：
```bash
streamlit run app.py
```

## 系统要求

- Python 3.10+
- 依赖包：
  - streamlit
  - backtrader
  - pandas
  - numpy
  - plotly
  - tushare
  - akshare
  - loguru

## 目录结构

```
ETF-Strategies/
├── app.py                 # 主程序入口
├── requirements.txt       # 依赖包列表
├── src/
│   ├── data/             # 数据加载模块
│   ├── strategies/       # 交易策略模块
│   ├── indicators/       # 技术指标模块
│   └── utils/           # 工具函数模块
└── logs/                # 日志文件目录
```

## 如何添加新策略

1. 在 `src/strategies` 目录下创建新的策略类：
```python
import backtrader as bt

class YourStrategy(bt.Strategy):
    params = (
        # 定义策略参数
    )
    
    def __init__(self):
        # 初始化策略
        pass
        
    def next(self):
        # 实现交易逻辑
        pass
```

2. 在 `strategy_factory.py` 中注册策略：
```python
from .your_strategy import YourStrategy
StrategyFactory.register_strategy("策略名称", YourStrategy)
```

3. 在 `app.py` 中修改添加策略需要的参数，如下示意：
```python
# 移动平均线参数（仅在选择双均线策略时显示）
if strategy_name == "双均线策略":
    st.subheader("均线参数")
    col1, col2 = st.columns(2)
    with col1:
        fast_period = st.number_input("快线周期", value=10, min_value=1)
    with col2:
        slow_period = st.number_input("慢线周期", value=30, min_value=1)
...
# 如果是双均线策略，添加特定参数
if strategy_name == "双均线策略":
    strategy_params.update({
        'fast_period': fast_period,
        'slow_period': slow_period,
    })
...
```

## 回测指标说明

- 总收益率：策略最终收益相对于初始资金的百分比
- 夏普比率：超额收益相对于波动率的比率
- 最大回撤：策略执行期间的最大亏损百分比
- 胜率：盈利交易占总交易次数的比例
- 盈亏比：平均盈利交易额与平均亏损交易额的比值
- 系统质量指数(SQN)：衡量交易系统的稳定性

## 风险提示

本系统仅供学习和研究使用，不构成任何投资建议。使用本系统进行实盘交易需要自行承担风险。

## 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进这个项目。

## 许可证

MIT License
