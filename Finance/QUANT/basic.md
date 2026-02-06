## 策略程序框架
### 策略程序初始化
- init 函数
	- 定义全局变量 context
		- 固有属性
			- 系统当前时间 now
				- 输出范例 `2020-09-01 09:40:00+08:00`
			- 获取订阅的数据滑窗 data
				- 输入范例 `context.data(symbol=bars[0]['symbol'],frequency='60s',count=10,fields)`
					- symbol 当前的
					- frequency 默认为'1d'
					- count 滑窗大小 需小于等于subscribe中的
					- fields 对象字段
				- 当subscribe的format="df"（默认）时，返回dataframe 输出范例 
					- `  symbol close eob`
					- `0 SHSE.600519 1629.96 2024-01-22 14:56:00+08:00`
				- subscribe的format ="row"时，返回list[dict]
					- `[{'symbol': 'SHSE.600519', 'price': 1642.0, 'quotes': [{'bid_p': 1640.0, 'bid_v': 100, 'ask_p': 1642.0, 'ask_v': 4168}], 'created_at': datetime.datetime(2024, 1, 23, 9, 14, 1, 91000, tzinfo=datetime.timezone(datetime.timedelta(seconds=28800), 'Asia/Shanghai'))}]
				- subscribe的format ="col"时，返回dict
			- 订阅代码集合 symbols
				- 输出范例 `{'SHSE.600519','SHSE.600419'}`
			- 模式 mode
				- 输出范例 执行实时模式：`1` 执行回测模式：`0` 
			- 动态参数 parameters
				- 示例-添加动态参数和查询所有设置的动态参数
				- `add_parameter(key='k_value', value=context.k_value, min=0, max=100, name='k值阀值', intro='k值阀值',group='1', readonly=False)`
				- `context.parameters`
				- `{'k_value': {'key': 'k_value', 'value': 80.0, 'max': 100.0, 'name': 'k值阀值', 'intro': 'k值阀值', 'group': '1', 'min': 0.0, 'readonly': False}}`
			- 账户信息 account
				- 账户 account_id
				- 资金字典 cash
				- 单一标的情况 position
				- 持仓情况列表 positions
				- 交易账户状态 status
				- 示例-获取当前持仓
					- `Account_positions = context.account().positions() # 所有持仓`
					- `Account_position = context.account().position(symbol='SHSE.600519',side = PositionSide_Long) # 指定持仓`
					- `[{' account_id':,'symbol':,'side':,'volume':,'volume_today':,'vwap':,……}]`
				- 示例-获取当前账户资金
					- `context.account().cash`
				- 示例-获取账户连接状态
					- `context.account().status`
					- `{'state': 3, 'error': {'code': 0, 'type': '', 'info': ''}}`
			- 自定义属性 xxx
	- 定义调度任务 schedule
	- 准备历史数据 
	- 订阅实时行情 subscribe
### 行情事件处理函数
- 盘口数据事件 on-tick
- 分时数据事件 on-bar
### 交易事件处理函数
- 汇报数据事件 execrpt
- 委托状态变化数据事件
- 交易账户状态变化事件
### 其他时间处理函数
- 错误事件
- 动态参数修改事件
- 处理绩效指标对象
- 处理实时行情网络连接成功事件
- 处理实时行情网络连接断开事件
- 处理交易通道网络连接成功事件
- 处理交易通道网络连接断开事件
### 策略入口
- 交易策略 run
	- token
	- strategy_id
	- 策略工作模式 mode
		- 实时模式 `mode=MODE_LIVE`
		- 回测模式 `mode=MODE_BACKTEST`
		- 

- 代码标识 symbol
	- 具体标的内容见附表（这里省去了期货的虚拟合约）

| 市场中文名      | 市场代码  | 示例代码         | 证券简称                      |
| ---------- | ----- | ------------ | ------------------------- |
| 上交所        | SHSE  | SHSE.600000  | 浦发银行                      |
| 深交所        | SZSE  | SZSE.000001  | 平安银行                      |
| 中金所        | CFFEX | CFFEX.IC2011 | 中证 500 指数 2020 年 11 月期货合约 |
| 上期所        | SHFE  | SHFE.rb2011  | 螺纹钢 2020 年 11 月期货合约       |
| 大商所        | DCE   | DCE.m2011    | 豆粕 2020 年 11 月期货合约        |
| 郑商所        | CZCE  | CZCE.FG101   | 玻璃 2021 年 1 月期货合约         |
| 上海国际能源交易中心 | INE   | INE.sc2011   | 原油 2020 年 11 月期货合约        |
| 广期所        | GFEX  | GFEX.lc2405  | 碳酸锂 2024 年 05 月期货合约       |
## 数据结构
### 数据类
#### Tick
#### Bar
#### L2Order
### 交易类
#### Account
#### Order
#### ExecRpt
#### Cash
#### Position
#### Indicator
#### algoOrder
## API 介绍
### 基本函数
#### init 初始化策略
- init(context)
- 设置全局变量 context
#### schedule 定时任务配置
- schedule(schedule_func, date_rule, time_rule)
	- schedule_func 策略定时执行算法
	- date_rule n + 时间单位， 可选'd/w/m' 表示 n 天/n 周/n 月
	- time_rule 执行算法的具体时间 (%H:%M:%S 格式)
- eg  schedule(schedule_func=algo_1, date_rule='1d', time_rule='19:06:20')
#### run 运行策略
- run(strategy_id='', filename='', mode=MODE_UNKNOWN, token='', backtest_start_time='', backtest_end_time='', backtest_initial_cash=1000000, backtest_transaction_ratio=1, backtest_commission_ratio=0, backtest_slippage_ratio=0, backtest_adjust=ADJUST_NONE, backtest_check_cache=1, serv_addr='', backtest_match_mode=0, backtest_intraday=0)
	- strategy_id='' , 策略 id
	- filename='', 策略文件名称
	- mode=MODE_UNKNOWN, 策略模式 MODE_LIVE(实时)=1 MODE_BACKTEST(回测) =2
	- token='', 用户标识
	- backtest_start_time='', 回测开始时间 (%Y-%m-%d %H:%M:%S 格式)
	- backtest_end_time='', 回测结束时间 (%Y-%m-%d %H:%M:%S 格式)
	- backtest_initial_cash=1000000, 回测初始资金, 默认 1000000
	- backtest_transaction_ratio=1, 回测成交比例, 默认 1.0, 即下单 100%成交
	- backtest_commission_ratio=0,  回测佣金比例, 默认 0
	- backtest_slippage_ratio=0,  回测滑点比例, 默认 0
	- backtest_adjust=ADJUST_NONE, 回测复权方式(默认不复权) ADJUST_NONE(不复权)=0 ADJUST_PREV(前复权)=1 ADJUST_POST(后复权)=2
	- backtest_check_cache=1, 回测是否使用缓存：1 - 使用， 0 - 不使用；默认使用
	- serv_addr='',  终端服务地址, 默认本地地址, 可不填，若需指定应输入 ip+端口号，如"127.0.0.1:7001"
	- backtest_match_mode=0,  回测市价撮合模式： 1-实时撮合：在当前 bar 的收盘价/当前 tick 的 price 撮合，0-延时撮合：在下个 bar 的开盘价/下个 tick 的 price 撮合，默认是模式 0
	- backtest_intraday=0 回测模式不订阅行情，current 和 current_price 行情数据查询函数返回的日线价格类型：  1 - 当日收盘价: 回测当前交易日的日线收盘价（T日盘中和盘后均为T日收盘价）;  0 - 历史收盘价：回测当前时刻的历史最新日线收盘价（T日盘中为T-1日收盘价，T日盘后为T日收盘价），默认是0
- stop 停止策略
	- eg if not context.symbols: stop()
- timer 设置定时器
	- timer(timer_func, period, start_delay)
	- timer_func 在 timer 设置的时间到达时触发的事件函数
	- period定时事件间隔毫秒数，设定每隔多少毫秒触发一次定时器，范围在 [1,43200000]
	- start_delay 等待秒数(毫秒)，设定多少毫秒后启动定时器，范围在[0,43200000]
	- output 
		- timer_status 定时器设置是否成功，成功=0，失败=非 0 错误码（timer_id 无效）
		- timer_id 设定好的定时器 id
- timer_stop 停止定时器
	- timer_stop(timer_id)
### 数据订阅
#### subscribe 行情订阅
- subscribe(symbols, frequency='1d', count=1, unsubscribe_previous=False)
	- symbols, 订阅标的代码, 注意大小写，支持字串格式，如有多个代码, 中间用 `,` (英文逗号) 隔开, 也支持 `['symbol1', 'symbol2']` 这种列表格式
	- frequency='1d',  频率, 支持 'tick', '60s', '300s', '900s' 等, 默认'1d'
	- count=1, context.data返回的订阅数据滑窗大小, 默认`1` ,
	- wait_group=False, 是否等待同一频率的bar同时到齐（只支持bar频率），默认`False`不取消, 输入`True`则同时等待同频率所有bar到齐再一次性返回
	- wait_group_timeout, 等待超时时间，只有wait_group=True时生效，默认'10s'
	- unsubscribe_previous=False, 是否取消过去订阅的 symbols, 默认`False`不取消, 输入`True`则取消所有原来的订阅
	- fields, context.data返回的对象字段, 如有多个字段, 中间用, 隔开, 默认所有
	- format, context.data返回的数据格式，默认"df", "df": 数据框格式，返回dataframe（默认）,"row": 原始行式组织格式，返回list[dict]（当用户对性能有要求时, 推荐使用此格式）, "col": 列式组织格式，返回dict 
#### unsubscribe 取消订阅
- unsubscribe(symbols='\*', frequency='60s')
	- symbol
	- frequency
### 数据事件
#### on_tick tick 数据推送事件
#### on_bar bar 数据推送事件
#### on_l2transaction 逐笔成交事件
#### on_l2order 逐笔委托事件


