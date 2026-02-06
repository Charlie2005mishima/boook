## 多因子选股
### 1. 策略介绍

| 模型                         | 出处                       | 所含因子           |
| -------------------------- | ------------------------ | -------------- |
| Fama-French 三因子            | Fama and Farench(1993)   | 市场、规模、价值       |
| Carhart 四因子                | Carhart（1997）            | 市场、规模、价值、动量    |
| Novy-Marx 四因子              | Novy-Marx（2013）          | 市场、规模、价值、盈利    |
| Fama-French 五因子            | Fama and Farench(2015)   | 市场、规模、价值、盈利、投资 |
| Hou-Xue-Zhang 四因子          | Hou et al                | 市场、规模、盈利、投资    |
| Stambaugh-Yuan 四因子         | Stambaugh and Yuan(2017) | 市场、规模、管理、表现    |
| Daniel-Hirshleifer-Sun 三因子 | Daniel et al（2020）       | 市场、长周期行为、短周期行为 |
#### Fama-French 三因子模型
$E[R_{i}]-R_{f}=\alpha+\beta_{i,MKT}(E[R_{M}-R_{f}])+\beta_{i,SMB}E[R_{SMB}]+\beta_{i,HML}E[R_{HML}]$ 

|     |     | BM分组 |     |     |
| --- | --- | ---- | --- | --- |
|     |     | H    | M   | L   |
| 市值  | S   | S/H  | S/M | S/L |
|     | B   | B/H  | B/M | B/L |
- $SMB=\frac{1}{3}(S/H+S/M+S/L)-\frac{1}{3}(B/H+B/M+B/L)$
- $HML=\frac{1}{2}(S/H+B/H)-\frac{1}{2}(S/L+B/L)$
- 规模因子是三个小市值组合的等权平均减去三个大市值组合的等权平均
- 价值因子是两个高 BM 组合的等权平均减去两个低 BM 组合的等权平均

### 2. 策略逻辑
- 第一步：获取股票市值以及账面市值比数据。
- 第二步：将股票按照各个因子进行排序分组，分组方法如上表所示。
- 第三步：依据式 2 式 3，计算 SMB、HML 因子。
- 第四步：因子回归，计算 alpha 值。获取 alpha 最小并且小于 0 的 10 只的股票买入开仓。

### 3. 代码
#### init(context)
context.index_symbol = 'SHSE.000300' # 成分股指数
context.date = 20 # 数据滑窗
context.ratio = 0.8 # 设置开仓的最大资金量
context.BM_HIGH = 3.0 # 账面市值比的大/中/小分类
context.BM_MIDDLE = 2.0
context.BM_LOW = 1.0
context.MV_BIG = 2.0 # 市值大/小分类
context.MV_SMALL = 1.0
schedule(schedule_func=algo, date_rule='1d', time_rule='09:30:00') # 每个交易日的09:40 定时执行algo任务
#### algo(context)
now = context.now 
now_str = now.strftime('%Y-%m-%d')
last_day = *get_previous_n_trading_dates*(exchange='SHSE', date=now_str, n=1)[0]
stock300 = *stk_get_index_constituents*(index=context.index_symbol, trade_date=last_day)['symbol'].tolist()
stocks_info =  get_symbols(sec_type1=1010, symbols=stock300, trade_date=now.strftime('%Y-%m-%d'), skip_suspended=True, skip_st=True)
stock300 = [item['symbol'] for item in stocks_info if item['listed_date']<now and item['delisted_date']>now]