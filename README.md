# 中国股票风格因子构造与复现  “Size and Value in China (Journal of Financial Economics, Vol 134,
Issue 1, 2019, Pages 48-69)” 

本项目是一次机器学习量化投资课程作业，核心目标是基于中国 A 股月频数据，按照 `LSY (2019)` 风格构造并分析一组风格因子。项目最终在 `hwfinal.ipynb` 中完成数据清洗、因子构造、统计汇总和可视化，并生成可用于后续研究的月度因子序列。

当前仓库已经包含整理后的主数据文件 `completedata.csv` 和完整实验 notebook `hwfinal.ipynb`，适合直接阅读思路、复现实验主流程，或在此基础上继续扩展。

## 项目内容

- `hwfinal.ipynb`：项目主 notebook，包含数据预处理、因子构造、统计表输出与累计收益绘图。
- `completedata.csv`：整理后的月频股票面板数据，是后半段因子构造脚本的直接输入。

## 研究目标

项目主要完成以下任务：

- 构造月度股票特征，包括异常换手率、盈利变量、无风险利率、月收益率与市值等。
- 基于滞后信息构造中国市场风格因子。
- 输出因子统计结果、相关系数矩阵和累计收益曲线。
- 比较 `2000-01 ~ 2016-12` 与扩展样本 `2000-01 ~ 2025-10` 两个区间下的结果。

## 因子说明

在 notebook 后半部分，项目基于 `completedata.csv` 构造了以下因子：

- `MKT`：市场因子，定义为样本宇宙内市值加权收益减去无风险利率。
- `SMB_EP`：基于盈利价格比（EP）的规模因子。
- `VMG`：价值减成长因子。
- `SMB_TO`：基于异常换手率分组得到的规模因子。
- `PMO`：情绪因子，使用低换手组合减高换手组合构造。
- `SMB_CH4`：四因子版本中的规模因子，定义为 `0.5 * SMB_EP + 0.5 * SMB_TO`。

项目构造组合时主要使用滞后一期信息，避免未来函数问题，包括：

- `MV_lag`：滞后一期市值
- `earning_lag`：滞后一期盈利
- `abturn_lag`：滞后一期异常换手率
- `EP_lag`：`earning_lag / MV_lag`

## 数据流程

notebook 前半部分展示了数据整理过程，大致包括：

1. 从日频换手率数据计算月度异常换手率 `abturn`。
2. 从财务报表与披露日期数据提取盈利变量 `earning`。
3. 从无风险利率表中提取月末无风险收益 `rf_month`。
4. 从月交易数据中提取个股月收益率 `monrtn` 和总市值 `MV`。
5. 将以上变量按股票和月份合并，得到最终样本 `completedata.csv`。

`completedata.csv` 当前可见的主要字段包括：

- `Stkcd`：股票代码
- `Trdmnt`：月份
- `abturn`：异常换手率
- `earning`：盈利变量
- `rf_month`：月度无风险利率
- `MV`：月末总市值
- `monrtn`：月收益率
- `Stkcd_str`：补零后的股票代码字符串

## 如何运行

### 1. 环境依赖

建议使用 Python 3.10+，并安装以下常用库：

```bash
pip install pandas numpy matplotlib jupyter openpyxl
```

### 2. 启动 notebook

```bash
jupyter notebook hwfinal.ipynb
```

### 3. 运行说明

- 如果你只想复现因子构造与统计结果，直接使用仓库中的 `completedata.csv` 即可。
- 如果你想从最原始数据开始完整复现前半段清洗流程，需要额外准备 notebook 中引用的源数据文件，例如换手率、财报、公告日、无风险利率和月交易数据文件。
- notebook 中后半段会读取 `completedata.csv`，并输出月度因子序列 `CH_factors_monthly.csv`、统计表和累计收益图。

## 当前仓库的复现边界

这个仓库更适合复现“整理后的主实验流程”，而不是完全从零获取所有底层原始数据。原因是 notebook 前半部分引用了若干未随仓库一起上传的外部文件，例如：

- `turnover1.csv`
- `turnover2.csv`
- `FI_T2.xlsx`
- `FS_Comins.xlsx`
- `IAR_Rept.csv`
- `TRD_Nrrate.xlsx`
- `TRD_Mnth.xlsx`

因此：

- 仓库当前状态下，可以直接基于 `completedata.csv` 运行因子构造部分。
- 若要完整复现最前面的数据清洗过程，需要自行补齐这些原始数据文件。

## 主要输出

运行 notebook 后，可以得到以下结果：

- 月度因子时序文件 `CH_factors_monthly.csv`
- 四因子统计表（均值、波动率、t-stat）
- 因子相关系数矩阵
- `MKT`、`SMB_CH4`、`VMG`、`PMO` 的累计收益曲线

## 项目特点

- 面向中国股票市场，而非直接照搬美股经典因子设定。
- 使用滞后变量进行组合划分，尽量保持实证研究中的时序一致性。
- 同时报告经典样本区间和扩展样本区间结果，方便比较稳定性。
- 数据准备与因子构造都保留在同一个 notebook 中，便于课程展示与结果复查。

## 后续可扩展方向

- 将 notebook 重构为独立的 `.py` 脚本和模块化函数。
- 增加 `requirements.txt` 或 `environment.yml` 以便环境复现。
- 补充图表导出、结果缓存和日志记录。
- 在 README 中加入结果截图或表格示例，提升 GitHub 展示效果。

## 说明

本 README 根据当前仓库实际内容整理，重点介绍已经上传的文件和可直接复现的部分。如果你后续补充了原始数据、脚本或结果文件，可以继续扩展本说明文档，使其更适合公开展示和课程提交。
