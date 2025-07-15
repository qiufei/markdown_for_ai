# 行情、交易日及合约信息

## all_instruments - 获取所有合约基础信息

```python
all_instruments(type=None, market='cn', date=None)
```

获取某个国家市场的所有合约信息。使用者可以通过这一方法很快地对合约信息有一个快速了解，目前仅支持中国市场。

可传入date筛选指定日期的合约，返回的instrument 数据为合约的最新情况。

### 参数

| 参数     | 类型                              | 说明                                                       |
| :------- | :-------------------------------- | :--------------------------------------------------------- |
| `type`   | `str`                             | 需要查询合约类型，例如：`type='CS'`代表股票。默认是所有类型 |
| `market` | `str`                             | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |
| `date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 指定日期，筛选指定日期可交易的合约                         |

其中`type` 参数传入的合约类型和对应的解释如下：

| 合约类型    | 说明                                      |
| :---------- | :---------------------------------------- |
| `CS`        | Common Stock, 即股票                    |
| `ETF`       | Exchange Traded Fund, 即交易所交易基金  |
| `LOF`       | Listed Open-Ended Fund，即上市型开放式基金（以下分级基金已并入） |
| `INDX`      | Index, 即指数                           |
| `Future`    | Futures，即期货，包含股指、国债和商品期货 |
| `Spot`      | Spot，即现货，目前包括上海黄金交易所现货合约 |
| `Option`    | 期权，包括目前国内已上市的全部期权合约    |
| `Convertible` | 沪深两市场内有交易的可转债合约            |
| `Repo`      | 沪深两市交易所交易的回购合约              |

### 返回

`pandas DataFrame` - 所有合约的基本信息。

详细字段注释请参考instruments 返回字段说明

### 范例

获取中国内地市场所有合约的基础信息：

```python
[In]all_instruments()
[Out] abbrev_symbol order_book_id sector_code symbol
0 XJDQ 000400.XSHE Industrials 许继电气1 HXN 002582.XSHE ConsumerStaples 好想你2 NFGF 300004.XSHE Industrials 南风股份3 FLYY 002357.XSHE Industrials 富临运业...
```

获取中国内地市场所有LOF 基金的基础信息：

```python
[In]all_instruments(type='LOF')
[Out] abbrev_symbol order_book_id product sector_code symbol
0 CYGA 150303.XSHE null null 华安创业板50A 1 JY500A 150088.XSHE null null 金鹰500A 2 TD500A 150053.XSHE null null 泰达稳健3 HS500A 150110.XSHE null null 华商500A 4 QSAJ 150235.XSHE null null 鹏华证券A ...
```

获取中国内地市场所有期货的基础信息：

```python
[In]all_instruments(type='Future')
[Out] abbrev_symbol order_book_id product sector_code symbol
0 MH0610 CF0610 Commodity null 棉花0610 1 LD0209 GN0209 Commodity null 绿豆0209 ... 3615 HS1301 IF1301 Index null 沪深1301 ...
```

获取中国内地市场指定日期可交易的期货的基础信息：

```python
[In]all_instruments(type='Future', date='20160412')
[Out] abbrev_symbol order_book_id product symbol
0 HJ0809 AU0809 Commodity 黄金0809 1 MH1301 CF1301 Commodity 棉花1301 ... 4226 XC1103 WR1103 Commodity 线材1103 ...
```

获取中国内地市场场内交易的可转债的基础信息：

```python
[In]all_instruments(type='Convertible')
[Out] de_listed_date exchange listed_date market_tplus order_book_id round_lot status symbol type
0 2013-01-29 XSHG 2007-03-08 0 126003.XSHG 10 Delisted 07云化债 Convertible 1 2016-09-22 XSHG 2008-10-10 0 126018.XSHG 10 Delisted 08江铜债 Convertible 2 2015-06-02 XSHE 2013-08-19 0 128002.XSHE 10 Delisted 东华转债 Convertible 3 2015-02-26 XSHG 2010-09-10 0 113002.XSHG 10 Delisted 工行转债 Convertible 4 0000-00-00 XSHE 2019-01-21 0 128052.XSHE 10 Active 凯龙转债 Convertible ...
```

## instruments - 获取合约详细信息

```python
instruments(order_book_ids, market='cn')
```

获取某个国家市场内一个或多个合约最新的详细信息。目前仅支持中国市场。

### 参数

| 参数           | 类型             | 说明                                                       |
| :------------- | :--------------- | :--------------------------------------------------------- |
| `order_book_ids` | `str` OR `str list` | 合约代码，可传入`order_book_id`, `order_book_id list`。<br><br>中国市场的`order_book_id` 通常类似`'000001.XSHE'`。需要注意，国内股票、ETF、指数合约代码分别应当以`'.XSHG'`或`'.XSHE'`结尾，前者代表上证，后者代表深证。<br><br>比如查询平安银行这个股票合约，则键入`'000001.XSHE'`，前面的数字部分为交易所内这个股票的合约代码，后半部分为对应的交易所代码。<br><br>期货则无此要求 |
| `market`       | `str`            | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |

### 返回

一个instrument 对象，或一个instrument list。

目前系统并不支持跨国家市场的同时调用。传入的`order_book_id list` 必须属于同一国家市场，不能混合着中美两个国家市场的`order_book_id`。

**股票，ETF，指数Instrument 对象**

| 字段                     | 类型        | 说明                                                       |
| :----------------------- | :---------- | :--------------------------------------------------------- |
| `order_book_id`        | `str`       | 证券代码，证券的独特的标识符。应以`'.XSHG'`或`'.XSHE'`或`'.XHKG'`结尾。 `'.XSHG'` - 上证，`'.XSHE'` - 深证， `'.XHKG'` - 港股 |
| `symbol`                 | `str`       | 证券的简称，例如`'平安银行'`                               |
| `abbrev_symbol`          | `str`       | 证券的名称缩写，在中国A 股就是股票的拼音缩写。例如：`'PAYH'`就是平安银行股票的证券名缩写 |
| `round_lot`              | `int`       | 一手对应多少股，中国A 股一手是100 股                       |
| `sector_code`            | `str`       | 板块缩写代码，全球通用标准定义                             |
| `sector_code_name`       | `str`       | 以当地语言为标准的板块代码名                               |
| `industry_code`          | `str`       | 国民经济行业分类代码，具体可参考下方“Industry 列表”      |
| `industry_name`          | `str`       | 国民经济行业分类名称                                       |
| `listed_date`            | `str`       | 该证券上市日期                                             |
| `issue_price`            | `float`     | 该证券发行价（元）                                         |
| `de_listed_date`         | `str`       | 退市日期                                                   |
| `type`                   | `str`       | 合约类型，目前支持的类型有: `'CS'`, `'INDX'`, `'LOF'`, `'ETF'`, `'Future'` |
| `exchange`               | `str`       | 交易所，`'XSHE'` - 深交所, `'XSHG'` - 上交所           |
| `board_type`             | `str`       | 板块类别，`'MainBoard'` - 主板,`'GEM'` - 创业板,`'SME'` - 中小企业板,`'KSH'` - 科创板 |
| `status`                 | `str`       | 合约状态。`'Active'` - 正常上市, `'Delisted'` - 终止上市, `'TemporarySuspended'` - 暂停上市, `'PreIPO'` - 发行配售期间 , `'FailIPO'` - 发行失败 |
| `special_type`           | `str`       | 特别处理状态。`'Normal'` - 正常上市, `'ST'` - ST 处理, `'StarST'` - \*ST 代表该股票正在接受退市警告, `'PT'` - 代表该股票连续3 年收入为负，将被暂停交易, `'Other'` - 其他 |
| `trading_hours`          | `str`       | 合约最新的交易时间，如需历史数据请使用get_trading_hours    |
| `least_redeem`           | `str`       | 最低申赎份额，仅对ETF 基金展示                             |
| `cross_market`           | `str`       | 沪深港通标识。True-支持，False-不支持。仅对港股生效        |
| `least_redeem`           | `str`       | 最低申赎份额，仅对ETF 基金展示                             |
| `market_tplus`           | `str`       | 交易制度，0'表示T+0，'1'表示T+1，往后顺推                   |
| `purchasedate`           | `str`       | 申购日期                                                   |

**期货Instrument 对象**

| 字段                   | 类型        | 说明                                                       |
| :--------------------- | :---------- | :--------------------------------------------------------- |
| `order_book_id`      | `str`       | 期货代码，期货的独特的标识符（郑商所期货合约数字部分进行了补齐。例如原有代码`'ZC609'`补齐之后变为`'ZC1609'`）。主力连续合约UnderlyingSymbol+88，例如`'IF88'` ；指数连续合约命名规则为UnderlyingSymbol+99 |
| `symbol`               | `str`       | 期货的简称，例如`'沪深1005'`                             |
| `margin_rate`          | `float`     | 期货合约的最低保证金率                                     |
| `round_lot`            | `float`     | 期货全部为1.0                                              |
| `listed_date`          | `str`       | 期货的上市日期。主力连续合约与指数连续合约都为`'0000-00-00'` |
| `de_listed_date`       | `str`       | 期货的退市日期。                                           |
| `industry_name`        | `str`       | 行业分类名称                                               |
| `trading_code`         | `str`       | 交易代码                                                   |
| `market_tplus`         | `str`       | 交易制度。`'0'`表示T+0，`'1'`表示T+1，往后顺推             |
| `type`                 | `str`       | 合约类型，`'Future'`                                       |
| `contract_multiplier`  | `float`     | 合约乘数，例如沪深300 股指期货的乘数为300.0                |
| `underlying_order_book_id` | `str`       | 合约标的代码，目前除股指期货(IH, IF, IC)之外的期货合约，这一字段全部为`'null'` |
| `underlying_symbol`    | `str`       | 合约标的名称，例如IF1005 的合约标的名称为`'IF'`            |
| `maturity_date`        | `str`       | 期货到期日。主力连续合约与指数连续合约都为`'0000-00-00'`   |
| `exchange`             | `str`       | 交易所，`'DCE'` - 大连商品交易所, `'SHFE'` - 上海期货交易所，`'CFFEX'` - 中国金融期货交易所, `'CZCE'`- 郑州商品交易所, `'INE'` - 上海国际能源交易中心 |
| `trading_hours`        | `str`       | 合约最新的交易时间，如需历史数据请使用get_trading_hours    |
| `product`              | `str`       | 合约种类，`'Commodity'`-商品期货，`'Index'`-股指期货，`'Government'`-国债期货 |
| `start_delivery_date`  | `str`       | 开始交割日                                                 |
| `end_delivery_date`    | `str`       | 结束交割日                                                 |

**期权Instrument 对象**

| 字段                   | 类型        | 说明                                                       |
| :--------------------- | :---------- | :--------------------------------------------------------- |
| `order_book_id`      | `str`       | 合约代码，50ETF 期权为数字代码，例如10000615               |
| `symbol`               | `str`       | 合约简称                                                   |
| `round_lot`            | `float`     | 最小下单手数，期权全部为1.0                                |
| `listed_date`          | `str`       | 合约上市日期                                               |
| `type`                 | `str`       | 合约类型，`'Option'` 代表期权                                |
| `contract_multiplier`  | `float`     | 合约乘数，50ETF 期权只保存分红调整后的最新数据，变动历史请参考日线数据 |
| `underlying_order_book_id` | `str`       | 合约标的代码                                               |
| `underlying_symbol`    | `str`       | 合约所属品种                                               |
| `maturity_date`        | `str`       | 合约到期日                                                 |
| `exchange`             | `str`       | 交易所，`'DCE'` - 大连商品交易所, `'SHFE'` - 上海期货交易所，`'CFFEX'` - 中国金融期货交易所, `'CZCE'`- 郑州商品交易所, `'INE'` - 上海国际能源交易中心 |
| `strike_price`         | `float`     | 期权行权价，50ETF 期权只保存分红调整后的最新数据，变动历史请参考日线数据 |
| `option_type`          | `str`       | `'C'` 代表认购，`'P'`代表认沽                                |
| `exercise_type`        | `str`       | `'E'` 代表欧式期权，`'A'` 代表美式期权                       |
| `market_tplus`         | `str`       | 交易制度， `'0'`表示T+0，`'1'`表示T+1，往后顺推            |
| `product_name`         | `str`       | ETF 期权字母简称                                           |

**现货Instrument 对象**

| 字段            | 类型        | 说明                             |
| :-------------- | :---------- | :------------------------------- |
| `order_book_id` | `str`       | 合约代码                         |
| `symbol`        | `str`       | 合约简称                         |
| `exchange`      | `str`       | 交易所，`'SGEX'` - 上海黄金期货交易所 |
| `listed_date`   | `str`       | 合约上市日期                     |
| `de_listed_date` | `str`       | 退市日期                         |
| `type`          | `str`       | 合约类型，`'Spot'` 代表现货        |
| `trading_hours` | `str`       | 合约最新的交易时间，如需历史数据请使用get_trading_hours |
| `market_tplus`  | `str`       | 交易制度， `'0'`表示T+0，`'1'`表示T+1，往后顺推 |

**可转债Instrument 对象**

| 字段            | 类型        | 说明                             |
| :-------------- | :---------- | :------------------------------- |
| `order_book_id` | `str`       | 合约代码                         |
| `symbol`        | `str`       | 合约简称                         |
| `exchange`      | `str`       | 交易所，`'SXHE'` - 深交所，`'SXHG'` - 上交所 |
| `listed_date`   | `str`       | 合约上市日期                     |
| `de_listed_date` | `str`       | 退市日期                         |
| `type`          | `str`       | 合约类型，`'Spot'` 代表现货        |
| `market_tplus`  | `str`       | 交易制度， `'0'`表示T+0，`'1'`表示T+1，往后顺推 |

Instrument 对象也支持如下方法：

合约已上市天数。

```python
days_from_listed(date=None)
```

默认返回合约上市距离当前日期的天数。date 支持str, 如果合约首次上市交易，天数为0；如果合约尚未上市或已经退市，则天数值为-1

合约距离到期天数。

```python
days_to_expire(date=None)
```

如果策略已经退市，则天数值为-1

### 范例

获取单一股票合约的详细信息：

```python
In [5]: instruments('000001.XSHE')
Out[5]: Instrument(order_book_id='000001.XSHE', industry_code='J66', market_tplus=1, symbol='平安银行', special_type='Normal', exchange='XSHE', status='Active', type='CS', de_listed_date='0000-00-00', listed_date='1991-04-03', sector_code_name='金融', abbrev_symbol='PAYH', sector_code='Financials', round_lot=100, trading_hours='09:31-11:30,13:01-15:00', board_type='MainBoard', industry_name='货币金融服务', issue_price=40.0, citics_industry_code='40', citics_industry_name='银行')
```

获取多个股票合约的详细信息：

```python
[In]instruments(['000001.XSHE', '000024.XSHE'])
[Out] [Instrument(order_book_id='000001.XSHE', industry_code='J66', market_tplus=1, symbol='平安银行', special_type='Normal', exchange='XSHE', status='Active', type='CS', de_listed_date='0000-00-00', listed_date='1991-04-03', sector_code_name='金融', abbrev_symbol='PAYH', sector_code='Financials', round_lot=100, trading_hours='09:31-11:30,13:01-15:00', board_type='MainBoard', industry_name='货币金融服务',industry_name='银行'), Instrument(order_book_id='000024.XSHE', industry_code='K70', market_tplus=1, symbol='招商地产', special_type='Normal', exchange='XSHE', status='Delisted', type='CS', de_listed_date='2015-12-30', listed_date='1993-06-07', sector_code_name='房地产', abbrev_symbol='ZSDC', sector_code='RealEstate', round_lot=100, trading_hours='09:31-11:30,13:01-15:00', board_type='MainBoard', industry_name='房地产业')]
```

获取期权合约基础信息

```python
[In]: instruments('10000615')
[Out] Instrument(listed_date='2016-04-28', exchange='XSHG', underlying_symbol='510050.XSHG', symbol='510050C1612A02050', underlying_order_book_id='510050.XSHG', round_lot=1, de_listed_date='2016-12-28', maturity_date='2016-12-28', option_type='C', exercise_type='E', type='Option', contract_multiplier=10220, strike_price=2.006, order_book_id='10000615', market_tplus=0, trading_hours='09:31-11:30,13:01-15:00')
```

获取000001.XSHE 20160801 该天距离合约上市日天数：

```python
[In]instruments('000001.XSHE').days_from_listed('20160801')
[Out] 9252```

获取期权合约10000068 20150320 该天距离合约上市日天数：

```python
[In]instruments('10000068').days_from_listed('20150320')
[Out] 3
```

获取IF1608 20160801 该天距离合约到期日天数：

```python
[In]instruments('IF1608').days_to_expire('20160801')
[Out] 18
```

获取ZN2105C20000 20160801 该天距离合约到期日天数：

```python
[In] instruments('ZN2105C20000').days_to_expire('20201225')
[Out] 122
```

## id_convert - 交易所代码转换

```python
id_convert(order_book_ids,to='normal')
```

将交易所和其他平台的股票代码转换成米筐的标准合约代码，目前仅支持A 股、期货和期权代码转换。

例如, 支持转换类型包括000001.SZ, 000001SZ, SZ000001 转换为000001.XSHE

### 参数

| 参数           | 类型           | 说明                                                         |
| :------------- | :------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str` or `str list` | 合约代码(来自米筐或交易所或其他平台)                         |
| `to`           | `str`          | `'normal'`：由米筐代码转化为交易所和其他平台的代码不填：由交易所和其他平台的代码转化为米筐代码 |

### 返回

传入一个`order_book_ids`，函数会返回一个标准化合约代码字符串。 传入一个`order_book_ids` 列表，函数会返回一个标准化合约代码字符串list。

### 范例

获取其他平台标准合约代码:

```python
[In]id_convert('000001.XSHE', to='normal')
[Out] '000001.SZ'
```

获取单一股票的米筐标准合约代码:

```python
[In]id_convert('000935.SH')
[Out] '000935.XSHG'
```

获取多个股票的米筐标准合约代码:

```python
[In]id_convert(['000001.SZ', '000935.SH'])
[Out] ['000001.XSHE', '000935.XSHG']
```

获取单一期货的米筐标准合约代码:

```python
[In]id_convert('AP810')
[Out] 'AP1810'
```

获取单一期货的米筐标准合约代码_CTP 代码:

```python
[In]id_convert('ZC001.CZCE')
[Out] 'ZC2001'
```

获取单一期权的米筐标准合约代码_CTP 代码:

```python
[In]id_convert('m1901-C-2500')
[Out] 'M1901C2500'
```

获取单一期权的米筐标准合约代码_CTP 代码:

```python
[In]id_convert('SR901C4400')
[Out] 'SR1901C4400'
```

## get_price - 获取合约历史行情数据

```python
get_price(order_book_ids, start_date='2013-01-04', end_date='2014-01-04', frequency='1d', fields=None, adjust_type='pre', skip_suspended =False, market='cn', expect_df=True,time_slice=None)
```

获取指定合约或合约列表的历史数据（包含起止日期，周线、日线或分钟线）。目前仅支持中国市场的股票、期货、ETF、常见指数和上金所现货的行情数据，如黄金、铂金和白银产品。

注： 如需大量获取分钟或tick 数据，建议以单只合约为单位，并设置长时段获取以提高效率。

### 参数

| 参数             | 类型                                                       | 说明                                                         |
| :--------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str` OR `str list`                                        | 合约代码，可传入`order_book_id`, `order_book_id list`        |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                                                     |
| `frequency`      | `str`                                                      | 历史数据的频率。 现在支持周/日/分钟/tick 级别的历史数据，默认为`'1d'`。<br><br>`1m` - 分钟线<br><br>`1d` - 日线<br><br>`1w` - 周线，只支持`'1w'`<br><br>日线和分钟可选取不同频率，例如`'5m'`代表5 分钟线。 |
| `fields`         | `str` OR `str list`                                        | 字段名称                                                     |
| `adjust_type`    | `str`                                                      | 权息修复方案，默认为 `pre` 。<br><br>不复权- `none` ，<br><br>前复权- `pre` ，后复权- `post` ，<br><br>前复权- `pre_volume` , 后复权- `post_volume`<br><br>两组前后复权方式仅`volume` 字段处理不同，其他字段相同。其中`'pre'`、`'post'`中的`volume` 采用拆分因子调整；`'pre_volume'`、`'post_volume'`中的`volume` 采用复权因子调整。 |
| `skip_suspended` | `bool`                                                     | 是否跳过停牌数据。默认为`False`，不跳过，用停牌前数据进行补齐。`True` 则为跳过停牌期。 |
| `expect_df`      | `bool`                                                     | 默认返回`pandas dataframe`。如果调为`False`，则返回原有的数据结构,周线数据需设置`expect_df=True` |
| `market`         | `str`                                                      | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；    |
| `time_slice`     | `str`, `datetime.time`                                     | 开始、结束时间段。默认返回当天所有数据。<br><br>支持分钟/ tick 级别的切分，详见下方范例。 |

### 返回

注意: 周线数据目前只支持`'1w'`,依据日线数据进行合成，例如股票周线的前复权数据使用前复权日线数据进行合成，股票周线的不复权数据使用不复权的日线数据合成。

**bar 数据**

| 字段            | 类型        | 说明                                                       |
| :-------------- | :---------- | :--------------------------------------------------------- |
| `open`          | `float`     | 开盘价                                                     |
| `close`         | `float`     | 收盘价                                                     |
| `high`          | `float`     | 最高价                                                     |
| `low`           | `float`     | 最低价                                                     |
| `limit_up`      | `float`     | 涨停价                                                     |
| `limit_down`    | `float`     | 跌停价                                                     |
| `total_turnover` | `float`     | 成交额                                                     |
| `volume`        | `float`     | 成交量                                                     |
| `num_trades`    | `int`       | 成交笔数（仅支持股票、ETF、LOF、可转债；提供范围为2021-06-25 至今） |
| `prev_close`    | `float`     | 昨日收盘价（交易所披露的原始昨收价，复权方法对该字段无效） |
| `settlement`    | `float`     | 结算价（仅限期货期权日线数据）                             |
| `prev_settlement` | `float`     | 昨日结算价（仅限期货期权日线数据）                         |
| `open_interest` | `float`     | 累计持仓量（期货期权专用）                                 |
| `trading_date`  | `pandasTimeStamp` | 交易日期（仅限期货分钟线数据），对应期货夜盘的情况       |
| `dominant_id`   | `str`       | 实际合约的order_book_id，对应期货888 系主力连续合约的情况 |
| `strike_price`  | `float`     | 行权价，仅限期权日线数据                                   |
| `contract_multiplier` | `float`     | 合约乘数，仅限期权日线数据                                 |
| `iopv`          | `float`     | 场内基金实时估算净值                                       |
| `day_session_open` | `float`     | 日盘开盘价（仅限期货期权日线数据）                         |

**tick 数据**

| 字段             | 类型            | 说明                                                       |
| :--------------- | :-------------- | :--------------------------------------------------------- |
| `open`           | `float`         | 当日开盘价                                                 |
| `high`           | `float`         | 当日最高价                                                 |
| `low`            | `float`         | 当日最低价                                                 |
| `last`           | `float`         | 最新价                                                     |
| `prev_close`     | `float`         | 昨日收盘价                                                 |
| `total_turnover` | `float`         | 当天累计成交额                                             |
| `volume`         | `float`         | 当天累计成交量                                             |
| `num_trades`     | `int`           | 成交笔数（仅支持股票、ETF、LOF、可转债；提供范围为2021-06-25 至今） |
| `limit_up`       | `float`         | 涨停价                                                     |
| `limit_down`     | `float`         | 跌停价                                                     |
| `open_interest`  | `float`         | 累计持仓量                                                 |
| `datetime`       | `datetime.datetime` | 交易所时间戳                                               |
| `a1~a5`          | `float`         | 卖一至五档报盘价格                                         |
| `a1_v~a5_v`      | `float`         | 卖一至五档报盘量                                           |
| `b1~b5`          | `float`         | 买一至五档报盘价                                           |
| `b1_v~b5_v`      | `float`         | 买一至五档报盘量                                           |
| `change_rate`    | `float`         | 涨跌幅                                                     |
| `trading_date`   | `pandasTimeStamp` | 交易日期，对应期货夜盘的情况                               |
| `prev_settlement` | `float`         | 昨日结算价（仅期货有效）                                   |
| `iopv`           | `float`         | 场内基金实时估算净值                                       |

### 范例

获取单一期货20220512 - 20220513 每个交易日夜盘23:55 至日盘09:05 的历史分钟行情（返回pandas DataFrame）:

```python
[In] get_price('AG2209', start_date='20220512', end_date='20220513',frequency='1m',time_slice=('23:55', '09:05'),expect_df=False)
[Out] volume close trading_date open_interest low total_turnover open high datetime
2022-05-11 23:55:00 25.0 4795.0 2022-05-12 37996.0 4794.0 1797975.0 4796.0 4796.0
2022-05-11 23:56:00 2.0 4795.0 2022-05-12 37996.0 4795.0 143850.0 4795.0 4795.0
...
2022-05-13 09:04:00 81.0 4662.0 2022-05-13 40338.0 4662.0 5665485.0 4664.0 4665.0
2022-05-13 09:05:00 20.0 4661.0 2022-05-13 40333.0 4660.0 1398405.0 4662.0 4662.0```

获取单一股票20220512 - 20220513 每个交易日10:00 - 11:00 的历史分钟行情（返回pandas DataFrame）:

```python
[In] get_price('000001.XSHE', start_date='20220512', end_date='20220513',frequency='1m', time_slice=(datetime.time(hour=10, minute=0), datetime.time(hour=11, minute=0)),expect_df=False)
[Out] volume close num_trades low total_turnover open high datetime
2022-05-12 10:00:00 1108700.0 14.39 545.0 14.36 15928007.0 14.36 14.39
2022-05-12 10:01:00 351300.0 14.40 427.0 14.37 5052195.0 14.39 14.40
...
2022-05-13 10:58:00 481100.0 14.52 309.0 14.52 6990489.0 14.55 14.55
2022-05-13 10:59:00 230700.0 14.52 190.0 14.51 3348303.0 14.51 14.52
2022-05-13 11:00:00 227700.0 14.53 210.0 14.51 3306559.0 14.51 14.53
```

获取单一股票不复权的历史周线行情（返回pandas DataFrame）:

```python
[In] get_price('000001.XSHE',start_date='2015-04-01', end_date='2015-04-12',frequency='1w',adjust_type='none',expect_df=True)
[Out] total_turnover low open close volume num_trades high order_book_id date
000001.XSHE 2015-04-10 2.281686e+10 16.15 16.15 19.8 1.284539e+09 554132.0 19.8
```

获取单一股票历史日线行情（返回pandas DataFrame）:

```python
[In]get_price('000001.XSHE', start_date='2015-04-01', end_date='2015-04-12',expect_df=False)
[Out] close limit_up high num_trades total_turnover volume low limit_down open
2015-04-01 10.5038 11.4054 10.6222 72105.0 2.608977e+09 236637563.0 10.2339 9.3323 10.4116
2015-04-02 10.3985 11.5568 10.6222 72424.0 2.222671e+09 202440588.0 10.2800 9.4507 10.5893
2015-04-03 10.4314 11.4383 10.4906 61025.0 2.262844e+09 206631550.0 10.2734 9.3586 10.3326
2015-04-07 11.0632 11.4778 11.1619 131387.0 4.898119e+09 426308008.0 10.6288 9.3915 10.6288
2015-04-08 11.7937 12.1688 11.8990 135077.0 5.784459e+09 485517069.0 10.9579 9.9575 11.1421
2015-04-09 11.8463 12.9717 12.5374 148293.0 5.794632e+09 456921108.0 11.6686 10.6156 11.8134
2015-04-10 13.0310 13.0310 13.0310 139375.0 6.339649e+09 480990210.0 11.7476 10.6617 11.8463
```

获取单一股票历史日线行情（未经复权处理的原始数据，返回pandas DataFrame）:

```python
[In]get_price('000001.XSHE', start_date='2015-04-01', end_date='2015-04-03', adjust_type='none',expect_df=False)
[Out] open close high low total_turnover volume limit_up limit_down
2015-04-01 15.82 15.96 16.14 15.55 2.608977e+09 164331641.0 17.33 14.18
2015-04-02 16.09 15.80 16.14 15.62 2.222671e+09 140583742.0 17.56 14.36
2015-04-03 15.70 15.85 15.94 15.61 2.262844e+09 143494132.0 17.38 14.22
```

获取单一股票历史分钟线收盘价（返回pandas Series）:

```python
[In]get_price('000001.XSHE', start_date='2015-04-01', end_date='2015-04-12', fields='close',frequency='1m',expect_df=False)
[Out] 2015-04-01 09:31:00 10.5621
2015-04-01 09:32:00 10.5354
2015-04-01 09:33:00 10.5287
2015-04-01 09:34:00 10.5354
2015-04-01 09:35:00 10.5420
2015-04-01 09:36:00 10.5688
2015-04-01 09:37:00 10.5955
2015-04-01 09:38:00 10.5487
2015-04-01 09:39:00 10.5354
2015-04-01 09:40:00 10.5354
Name: close, dtype: float64
```

获取单一股票历史分钟线收盘价，指定返回dataframe 形式：

```python
[In]get_price('000001.XSHE', start_date='2015-04-01', end_date='2015-04-12', fields='close',frequency='1m',expect_df=True)
[Out] close order_book_id datetime
000001.XSHE 2015-04-01 09:31:00 10.3985
2015-04-01 09:32:00 10.3721
2015-04-01 09:33:00 10.3655
```

获取单一股票历史tick 行情（返回pandas DataFrame）

```python
[In]get_price('000001.XSHE', start_date='20180321', end_date='20180321', frequency='tick',expect_df=False)
[Out] trading_date open last high low prev_close volume total_turnover limit_up limit_down ... a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v change_rate datetime
2018-03-21 09:15:00 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.000000
2018-03-21 09:15:09 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 31440.0 960.0 0.0 0.0 0.0 0.000000
2018-03-21 09:15:18 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 75280.0 820.0 0.0 0.0 0.0 0.000000
2018-03-21 09:15:27 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 75280.0 3520.0 0.0 0.0 0.0 0.000000
2018-03-21 09:15:36 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 183780.0 33720.0 0.0 0.0 0.0 0.000000
2018-03-21 09:15:54 2018-03-21 11.95 11.82 0.0000 0.00 11.82 0.0 0.000000e+00 13.0 10.64 ... 0.0 0.0 0.0 0.0 183780.0 34720.0 0.0 0.0 0.0 0.000000 ...
```

获取股票列表历史tick 行情（返回pandas DataFrame）

```python
[In]get_price(['000001.XSHE', '000002.XSHE'], start_date='2019-04-01', end_date='2019-04-01',frequency='tick')
[Out] trading_date open last high low prev_close volume ... a5_v b1_v b2_v b3_v b4_v b5_v change_rate order_book_id datetime
... 000002.XSHE 2019-04-01 09:15:00 2019-04-01 0.00 30.72 0.00 0.00 30.72 0.0 ... 0.0 1100.0 0.0 0.0 0.0 0.0 0.000000
2019-04-01 09:15:09 2019-04-01 0.00 30.72 0.00 0.00 30.72 0.0 ... 0.0 158400.0 0.0 0.0 0.0 0.0 0.000000
2019-04-01 09:15:18 2019-04-01 0.00 30.72 0.00 0.00 30.72 0.0 ... 0.0 164300.0 0.0 0.0 0.0 0.0 0.000000
··· 000001.XSHE 2019-04-01 14:56:33 2019-04-01 12.83 13.19 13.55 12.83 12.82 192947985.0 ... 574800.0 201700.0 605367.0 107200.0 180800.0 254236.0 0.028861
2019-04-01 14:56:36 2019-04-01 12.83 13.19 13.55 12.83 12.82 192963385.0 ... 574800.0 219000.0 605367.0 106400.0 180800.0 254336.0 0.028861
2019-04-01 14:56:39 2019-04-01 12.83 13.19 13.55 12.83 12.82 192979885.0 ... 574800.0 236200.0 605367.0 106400.0 180800.0 254236.0 0.028861 ···
```

获取单一股票历史15 分钟线行情（返回pandas DataFrame）:

```python
[In]get_price('000001.XSHE', start_date='2015-04-01', end_date='2015-04-01', frequency='15m',expect_df=False)
[Out] total_turnover close low open high volume index
2015-04-01 09:45:00 348982890.0 10.6215 10.6215 10.7300 10.7843 31848628
2015-04-01 10:00:00 234445219.0 10.5808 10.5469 10.6283 10.6350 21618354
2015-04-01 10:15:00 125882985.0 10.6757 10.5808 10.5808 10.7368 11540023
2015-04-01 10:30:00 144396901.0 10.6622 10.6215 10.6757 10.7503 13190460
2015-04-01 10:45:00 169238918.0 10.7368 10.6554 10.6622 10.7775 15409772
2015-04-01 11:00:00 131464598.0 10.7232 10.6961 10.7436 10.7503 11977847
2015-04-01 11:15:00 83455348.0 10.7368 10.7164 10.7300 10.7639 7589260 ...
```

获取股票列表历史日线收盘价（返回pandas DataFrame）:

```python
[In]get_price(['000024.XSHE', '000001.XSHE', '000002.XSHE'], start_date='2015-04-01', end_date='2015-04-12', fields='close',expect_df=False)
[Out] 000024.XSHE 000001.XSHE 000002.XSHE
2015-04-01 32.1251 10.8249 12.7398
2015-04-02 31.6400 10.7164 12.6191
2015-04-03 31.6400 10.7503 12.4891
2015-04-07 31.6400 11.4015 12.7398
2015-04-08 31.6400 12.1543 12.8327
2015-04-09 31.6400 12.2086 13.5941
2015-04-10 31.6400 13.4294 13.2969
```

获取股票列表历史日线行情（返回pandas DataPanel）:

```python
[In]get_price(['000024.XSHE', '000001.XSHE', '000002.XSHE'], start_date='2015-04-01', end_date='2015-04-12',expect_df=False)
[Out] <class 'rqcommons.pandas_patch.HybridDataPanel'> Dimensions: 8 (items) x 7 (major_axis) x 3 (minor_axis) Items axis: open to limit_down Major_axis axis: 2015-04-01 00:00:00 to 2015-04-10 00:00:00 Minor_axis axis: 000024.XSHE to 000002.XSHE
```

获取单一期货合约历史日线行情（返回pandas DataFrame）:

```python
[In]get_price('A1601', start_date='2015-04-01', end_date='2015-04-12')
[Out] open close high low total_turnover volume settlement prev_settlement open_interest limit_up limit_down
2015-04-01 4188.0 4210.0 4231.0 4188.0 168002700.0 3988.0 4212 4204.0 42406.0 4372.0 4036.0
2015-04-02 4212.0 4192.0 4228.0 4192.0 191554400.0 4550.0 4209 4212.0 44986.0 4380.0 4044.0
2015-04-03 4191.0 4157.0 4191.0 4149.0 336611000.0 8092.0 4159 4209.0 47632.0 4377.0 4041.0
2015-04-07 4155.0 4122.0 4164.0 4121.0 213253200.0 5150.0 4140 4159.0 50678.0 4325.0 3993.0
2015-04-08 4102.0 4103.0 4123.0 4082.0 231593500.0 5646.0 4101 4140.0 50758.0 4305.0 3975.0
2015-04-09 4098.0 4069.0 4106.0 4062.0 217864900.0 5344.0 4076 4101.0 52028.0 4265.0 3937.0
2015-04-10 4068.0 4069.0 4105.0 4062.0 105630900.0 2592.0 4075 4076.0 52142.0 4239.0 3913.0
```

获取单一期货合约历史tick 行情（返回pandas DataFrame）:

```python
[In]get_price('IF1608', '20160801', '20160801', 'tick',expect_df=False).head()
[Out] trading_date open last high low
datetime 2016-08-01 09:29:00.000 2016-08-01 3174.0 3174.0 3174.0 3174.0
2016-08-01 09:30:00.000 2016-08-01 3174.0 3174.0 3180.8 3174.0
2016-08-01 09:30:00.500 2016-08-01 3174.0 3176.8 3180.8 3174.0
2016-08-01 09:30:01.000 2016-08-01 3174.0 3176.6 3180.8 3174.0
2016-08-01 09:30:01.500 2016-08-01 3174.0 3178.0 3180.8 3174.0
prev_settlement prev_close volume open_interest
datetime
2016-08-01 09:29:00.000 3184.6 3181.8 61.0 31847.0
2016-08-01 09:30:00.000 3184.6 3181.8 69.0 31843.0
2016-08-01 09:30:00.500 3184.6 3181.8 73.0 31840.0
2016-08-01 09:30:01.000 3184.6 3181.8 97.0 31839.0
2016-08-01 09:30:01.500 3184.6 3181.8 106.0 31836.0
total_turnover ... a2_v a3_v a4_v a5_v
datetime ...
2016-08-01 09:29:00.000 58084200.0 ... 0.0 0.0 0.0 0.0
2016-08-01 09:30:00.000 65709540.0 ... 0.0 0.0 0.0 0.0
2016-08-01 09:30:00.500 69523920.0 ... 0.0 0.0 0.0 0.0
2016-08-01 09:30:01.000 92391300.0 ... 0.0 0.0 0.0 0.0
2016-08-01 09:30:01.500 100971420.0 ... 0.0 0.0 0.0 0.0
b1_v b2_v b3_v b4_v b5_v change_rate
datetime
2016-08-01 09:29:00.000 1.0 0.0 0.0 0.0 0.0 -0.003329
2016-08-01 09:30:00.000 1.0 0.0 0.0 0.0 0.0 -0.003329
2016-08-01 09:30:00.500 1.0 0.0 0.0 0.0 0.0 -0.002449
2016-08-01 09:30:01.000 2.0 0.0 0.0 0.0 0.0 -0.002512
2016-08-01 09:30:01.500 1.0 0.0 0.0 0.0 0.0 -0.002072
```

50ETF 期权日线数据（可以看到行权价、合约乘数在50ETF 分红前后发生了变化）

```python
[In] get_price('10000615', start_date='20161125', end_date='20161130', frequency='1d',expect_df=False)
[Out] total_turnover open_interest open close volume low strike_price contract_multiplier high
2016-11-25 14745563.0 2711.0 0.3653 0.3904 4037.0 0.3500 2.050 10000.0 0.3904
2016-11-28 14495982.0 2377.0 0.4055 0.4062 3518.0 0.3995 2.050 10000.0 0.4261
2016-11-29 15053302.0 2016.0 0.3997 0.4333 3501.0 0.3843 2.006 10220.0 0.4510
2016-11-30 13510805.0 1934.0 0.4322 0.4171 3121.0 0.4118 2.006 10220.0 0.4409
```

获取回购合约日线

```python
[In] get_price("131801.XSHE",start_date=20190522,end_date=20190522,frequency='1d',expect_df=False)
[Out] close low num_trades high open total_turnover volume
2019-05-22 2.21 1.0 13544.0 2.78 2.65 3.906033e+09 3906033.0
```

## get_auction_info - 获取股票合约盘后数据

```python
get_auction_info(order_book_ids, start_date, end_date, frequency='1d', market='cn')
```

获取科创板、创业板等股票合约盘后固定价格交易信息，可获取历史和实时

### 参数

| 参数             | 类型                                                       | 说明                                                           |
| :--------------- | :--------------------------------------------------------- | :------------------------------------------------------------- |
| `order_book_ids` | `str` or `str list`                                        | 可输入`order_book_id`, `order_book_id list`,获取tick 数据时，只支持单个 |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期。                                                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期。                                                     |
| `frequency`      | `str`                                                      | 数据的频率。 现在支持日/分钟/tick 级别的数据，默认为`'1d'`。只支持`'1d'`,`'1m'`,`'tick'`,不支持`'5d'`等频率 |
| `fields`         | `str` OR `str list`                                        | 字段名称                                                       |
| `market`         | `str`                                                      | 默认是中国市场(`'cn'`)                                         |

### 返回

multi-index DataFrame

| 字段             | 类型    | 说明                 |
| :--------------- | :------ | :------------------- |
| `close`          | `float` | 收盘价               |
| `volume`         | `float` | 成交量               |
| `total_turnover` | `float` | 成交额               |
| `bid_vol`        | `int`   | 申买入量(tick 数据专用) |
| `ask_vol`        | `int`   | 申卖出量(tick 数据专用) |

### 范例

获取合约列表盘后日线数据

```python
[In] get_auction_info(['688012.XSHG','688011.XSHG'],'20190722','20190722','1d')
[Out] close volume total_turnover order_book_id date
688012.XSHG 2019-07-22 81.03 112858.0 9144883.74
688011.XSHG 2019-07-22 70.17 19350.0 1357789.50
```

获取单一合约盘后分钟数据

```python
[In] get_auction_info('688012.XSHG','20190722','20190722','1m')
[Out] close volume total_turnover order_book_id datetime
688012.XSHG 2019-07-22 15:06:00 81.03 1400.0 113442.00
2019-07-22 15:07:00 81.03 600.0 48618.00
...
2019-07-22 15:29:00 81.03 3241.0 262618.23
2019-07-22 15:30:00 81.03 1400.0 113442.00
```

获取单一合约盘后tick 数据

```python
[In] get_auction_info('688012.XSHG','20190722','20190722','tick')
[Out] close volume total_turnover bid_vol ask_vol datetime
2019-07-22 15:05:00.168 81.03 1000.0 81030.00 18292.0 0.0
2019-07-22 15:05:03.168 81.03 1000.0 81030.00 18292.0 0.0
...
2019-07-22 15:30:50.280 81.03 112858.0 9144883.74 0.0 69339.0
2019-07-22 15:30:56.720 81.03 112858.0 9144883.74 0.0 69339.0
```

## get_ticks - 获取日内tick 数据（试用版）

```python
get_ticks(order_book_id)
```

获取当日给定合约的level1 快照行情，无法获取历史。

说明：查询时间在交易日T 日7.30 pm 之前，返回T 日的tick 数据，查询时点在7.30pm 之后，返回交易日T+1 日的tick 数据。

### 参数

无

### 返回

pandas DataFrame

**tick 数据**

| 字段             | 类型            | 说明                               |
| :--------------- | :-------------- | :--------------------------------- |
| `open`           | `float`         | 当日开盘价                         |
| `high`           | `float`         | 当日最高价                         |
| `low`            | `float`         | 当日最低价                         |
| `last`           | `float`         | 最新价                             |
| `prev_close`     | `float`         | 昨日收盘价                         |
| `total_turnover` | `float`         | 当天累计成交额                     |
| `volume`         | `float`         | 当天累计成交量                     |
| `num_trades`     | `int`           | 成交笔数（仅支持股票、ETF、LOF、可转债） |
| `limit_up`       | `float`         | 涨停价                             |
| `limit_down`     | `float`         | 跌停价                             |
| `open_interest`  | `float`         | 累计持仓量                         |
| `datetime`       | `datetime.datetime` | 交易所时间戳                       |
| `a1~a5`          | `float`         | 卖一至五档报盘价格                 |
| `a1_v~a5_v`      | `float`         | 卖一至五档报盘量                   |
| `b1~b5`          | `float`         | 买一至五档报盘价                   |
| `b1_v~b5_v`      | `float`         | 买一至五档报盘量                   |
| `trading_date`   | `pandasTimeStamp` | 交易日期，对应期货夜盘的情况       |
| `prev_settlement` | `float`         | 昨日结算价（仅期货有效）           |
| `iopv`           | `float`         | 场内基金实时估算净值               |
| `prev_iopv`      | `float`         | 场内基金前估算净值                 |

### 范例

获取000001.XSHE 当日tick 数据

```python
[In] df=get_ticks('000001.XSHE')
df.head(1)
[Out] open last high low iopv prev_iopv limit_up limit_down prev_close volume ... a1_v a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v order_book_id datetime
000001.XSHE 2021-07-23 09:15:00 0.0 20.38 0.0 0.0 NaN NaN 22.42 18.34 20.38 0.0 ... 8700.0 11300.0 0.0 0.0 0.0 8700.0 0.0 0.0 0.0 0.0
```

获取ETF 期权10002725 当日tick 数据

```python
[In] get_ticks('10002725',expect_df=False)
[Out] update_time open last high low limit_up limit_down prev_settlement prev_close volume ... a1_v a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v datetime
2021-03-09 09:15:00.020 0 0.0000 0.6547 0.0000 0.0000 1.0124 0.3002 0.6563 0.6547 0.0 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0
2021-03-09 09:15:00.530 0 0.0000 0.6547 0.0000 0.0000 1.0124 0.3002 0.6563 0.6547 0.0 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0
2021-03-09 09:15:01.030 0 0.0000 0.6547 0.0000 0.0000 1.0124 0.3002 0.6563 0.6547 0.0 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0
```

## get_live_ticks - 获取日内tick 数据(支持日内时间切割）

```python
get_live_ticks(order_book_ids,start_dt,end_dt,fields,market='cn')
```

获取给定的股票、期货、期权、ETF、常见指数和上金所现货等合约的level1 快照行情，无法获取历史。

### 参数

| 参数             | 类型                                                       | 说明                                                         |
| :--------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str` or `str list`                                        | 可输入`order_book_id`, `order_book_id list`,                 |
| `start_dt`       | `str`, `datetime.datetime`, `pandasTimestamp`              | 开始时间，采用自然日时间戳，细化到秒                         |
| `end_dt`         | `str`, `datetime.datetime`, `pandasTimestamp`              | 结束时间，采用自然日时间戳，细化到秒                         |
| `fields`         | `str` or `str list`                                        | 字段名称                                                     |
| `market`         | `str`                                                      | 默认是中国市场(`'cn'`)                                       |

说明： `start_dt` 和`end_dt` 需同时传入或同时不传入，当不传入`start_dt,end_dt` 参数时，查询时间在交易日T 日7.30 pm 之前，返回T 日的tick 数据，查询时点在7.30pm 之后，返回交易日T+1 日的tick 数据。

### 返回

pandas DataFrame

**tick 数据**

| 字段             | 类型            | 说明                               |
| :--------------- | :-------------- | :--------------------------------- |
| `open`           | `float`         | 当日开盘价                         |
| `high`           | `float`         | 当日最高价                         |
| `low`            | `float`         | 当日最低价                         |
| `last`           | `float`         | 最新价                             |
| `prev_close`     | `float`         | 昨日收盘价                         |
| `total_turnover` | `float`         | 当天累计成交额                     |
| `volume`         | `float`         | 当天累计成交量                     |
| `num_trades`     | `int`           | 成交笔数（仅支持股票、ETF、LOF、可转债 |
| `limit_up`       | `float`         | 涨停价                             |
| `limit_down`     | `float`         | 跌停价                             |
| `open_interest`  | `float`         | 累计持仓量                         |
| `datetime`       | `datetime.datetime` | 交易所时间戳                       |
| `a1~a5`          | `float`         | 卖一至五档报盘价格                 |
| `a1_v~a5_v`      | `float`         | 卖一至五档报盘量                   |
| `b1~b5`          | `float`         | 买一至五档报盘价                   |
| `b1_v~b5_v`      | `float`         | 买一至五档报盘量                   |
| `trading_date`   | `pandasTimeStamp` | 交易日期，对应期货夜盘的情况       |
| `prev_settlement` | `float`         | 昨日结算价（仅期货有效）           |
| `iopv`           | `float`         | 场内基金实时估算净值               |
| `prev_iopv`      | `float`         | 场内基金前估算净值                 |

### 范例

获取期权合约2020 年3 月9 日9 时40 分00 秒-2020 年3 月9 日9 时40 分02 秒之间的tick 数据

```python
[In]get_live_ticks(order_book_ids=['10002726'],start_dt='20210309094000',end_dt='20210309094002')
[Out] trading_date update_time open last high low limit_up limit_down prev_settlement prev_close ... a1_v a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v order_book_id datetime
10002726 2021-03-09 09:40:00.020 NaT NaT 0.6173 0.6039 0.6173 0.6033 0.9624 0.2502 0.6063 0.6072 ... 10 2 30 10 10 30 10 10 10 10
2021-03-09 09:40:00.540 NaT NaT 0.6173 0.6039 0.6173 0.6033 0.9624 0.2502 0.6063 0.6072 ... 10 1 22 10 10 20 10 10 10 10
2021-03-09 09:40:01.030 NaT NaT 0.6173 0.6039 0.6173 0.6033 0.9624 0.2502 0.6063 0.6072 ... 8 20 2 10 10 20 10 10 10 10
2021-03-09 09:40:01.540 NaT NaT 0.6173 0.6039 0.6173 0.6033 0.9624 0.2502 0.6063 0.6072 ... 10 1 20 2 10 30 10 10 10 10
```

获取股票合约当日2020 年9 月18 日9 时15 分00 秒-2020 年9 月18 日9 时15 分30 秒之间的tick 数据

```python
[In] get_live_ticks(order_book_ids=['000001.XSHE','000006.XSHE'],start_dt='20200918091500',end_dt='20200918091530')
[Out] open last high low iopv prev_iopv limit_up limit_down prev_close volume ... a1_v a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v order_book_id datetime
000001.XSHE 2020-09-18 09:15:00 0 15.57 0 0 NaN NaN 17.13 14.01 15.57 0 ... 900 0 0 0 0 900 2500 0 0 0
2020-09-18 09:15:09 0 15.57 0 0 NaN NaN 17.13 14.01 15.57 0 ... 53500 2700 0 0 0 53500 0 0 0 0
2020-09-18 09:15:18 0 15.57 0 0 NaN NaN 17.13 14.01 15.57 0 ... 53600 2700 0 0 0 53600 0 0 0 0
2020-09-18 09:15:27 0 15.57 0 0 NaN NaN 17.13 14.01 15.57 0 ... 53500 2800 0 0 0 53500 0 0 0 0
000006.XSHE 2020-09-18 09:15:00 0 5.88 0 0 NaN NaN 6.47 5.29 5.88 0 ... 0 0 0 0 0 0 0 0 0 0
2020-09-18 09:15:09 0 5.88 0 0 NaN NaN 6.47 5.29 5.88 0 ... 2800 0 0 0 0 2800 9400 0 0 0
2020-09-18 09:15:18 0 5.88 0 0 NaN NaN 6.47 5.29 5.88 0 ... 2900 0 0 0 0 2900 9300 0 0 0
```

获取000001.XSHG 和RB2101 合约当日2020 年9 月18 日9 时31 分00 秒-2020 年9 月18 日9 时31 分06 秒之间的open,last ,high 等tick 字段

```python
[In] get_live_ticks(order_book_ids=['000001.XSHG','RB2101'],start_dt='20200918093100',end_dt='20200918093106',fields=['open','last','high'])
[Out] open last high order_book_id datetime
000001.XSHG 2020-09-18 09:31:00.790 3270.911 3272.8091 3275.0855
2020-09-18 09:31:05.060 3270.911 3272.8249 3275.0855
RB2101 2020-09-18 09:31:00.093 3580.000 3605.0000 3611.0000
2020-09-18 09:31:00.607 3580.000 3605.0000 3611.0000
2020-09-18 09:31:01.095 3580.000 3604.0000 3611.0000
2020-09-18 09:31:01.582 3580.000 3605.0000 3611.0000
2020-09-18 09:31:02.098 3580.000 3605.0000 3611.0000
2020-09-18 09:31:02.598 3580.000 3605.0000 3611.0000
2020-09-18 09:31:03.098 3580.000 3604.0000 3611.0000
2020-09-18 09:31:03.596 3580.000 3604.0000 3611.0000
2020-09-18 09:31:04.105 3580.000 3604.0000 3611.0000
2020-09-18 09:31:04.584 3580.000 3604.0000 3611.0000
2020-09-18 09:31:05.094 3580.000 3605.0000 3611.0000
2020-09-18 09:31:05.581 3580.000 3604.0000 3611.0000
```

## get_open_auction_info - 获取盘前集合竞价数据

```python
get_open_auction_info(order_book_ids,start_date, end_date,market='cn')
```

获取当日给定合约的盘前集合竞价结束的level1 快照行情。

### 参数

| 参数             | 类型                                                       | 说明                                         |
| :--------------- | :--------------------------------------------------------- | :------------------------------------------- |
| `order_book_ids` | `str` or `str list`                                        | 可输入`order_book_id`, `order_book_id list`  |
| `market`         | `str`                                                      | 默认是中国市场(`'cn'`)，目前仅支持中国市场   |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期。如不指定日期，则默认为取当天       |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期。如不指定日期，则默认为返回所填开始日期当天 |
| `fields`         | `str` OR `str list`                                        | 字段名称                                     |

### 返回

multi-index DataFrame

**tick 数据**

| 字段             | 类型            | 说明                               |
| :--------------- | :-------------- | :--------------------------------- |
| `open`           | `float`         | 当日开盘价                         |
| `high`           | `float`         | 当日最高价                         |
| `low`            | `float`         | 当日最低价                         |
| `last`           | `float`         | 最新价                             |
| `prev_settlement` | `float`         | 昨日结算价                         |
| `volume`         | `float`         | 成交量                             |
| `limit_up`       | `float`         | 涨停价                             |
| `limit_down`     | `float`         | 跌停价                             |
| `open_interest`  | `float`         | 累计持仓量                         |
| `datetime`       | `datetime.datetime` | 时间戳                             |
| `a1~a5`          | `float`         | 卖一至五档报盘价格                 |
| `a1_v~a5_v`      | `float`         | 卖一至五档报盘量                   |
| `b1~b5`          | `float`         | 买一至五档报盘价                   |
| `b1_v~b5_v`      | `float`         | 买一至五档报盘量                   |
| `change_rate`    | `float`         | 涨跌幅                             |
| `trading_date`   | `pandasTimeStamp` | 交易日期，对应期货夜盘的情况       |
| `iopv`           | `float`         | 场内基金实时估算净值               |
| `prev_iopv`      | `float`         | 场内基金前估算净值                 |

### 范例

获取单一合约集合竞价数据

```python
In []: get_open_auction_info('000001.XSHE','20190102','20190105')
Out[]: open last high low limit_up ... b1_v b2_v b3_v b4_v b5_v order_book_id datetime
... 000001.XSHE 2019-01-02 09:25:03 9.39 9.39 9.39 9.39 10.32 ... 183300.0 79600.0 65100.0 67700.0 43700.0
2019-01-03 09:25:03 9.18 9.18 9.18 9.18 10.11 ... 37230.0 76700.0 157700.0 73400.0 22000.0
2019-01-04 09:25:03 9.24 9.24 9.24 9.24 10.21 ... 56500.0 34200.0 41600.0 80200.0 42500.0
```

获取合约列表集合竞价数据

```python
[In] get_open_auction_info(['000001.XSHE','000002.XSHE'],'20190102','20190105')
[Out] open last high low limit_up limit_down ... a5_v b1_v b2_v b3_v b4_v b5_v order_book_id datetime
... 000001.XSHE 2019-01-02 09:25:03 9.39 9.39 9.39 9.39 10.32 8.44 ... 17800.0 183300.0 79600.0 65100.0 67700.0 43700.0
2019-01-03 09:25:03 9.18 9.18 9.18 9.18 10.11 8.27 ... 16200.0 37230.0 76700.0 157700.0 73400.0 22000.0
2019-01-04 09:25:03 9.24 9.24 9.24 9.24 10.21 8.35 ... 210900.0 56500.0 34200.0 41600.0 80200.0 42500.0
000002.XSHE 2019-01-02 09:25:03 23.83 23.83 23.83 23.83 26.20 21.44 ... 8593.0 35381.0 58700.0 1100.0 4800.0 100.0
2019-01-03 09:25:03 23.79 23.79 23.79 23.79 26.29 21.51 ... 4708.0 5400.0 2400.0 13100.0 17600.0 700.0
2019-01-04 09:25:03 23.91 23.91 23.91 23.91 26.48 21.66 ... 400.0 1800.0 200.0 5000.0 3000.0 100.0
```

## current_minute - 获取最近的分钟线数据

```python
current_minute(order_book_ids,skip_suspended=False,fields)```

获取当日给定合约的level1 最近合成的1 分钟行情，无法获取历史。

### 参数

| 参数             | 类型      | 说明                 |
| :--------------- | :-------- | :------------------- |
| `order_book_ids` | `str` or `str list` | 可输入`order_book_id`, `order_book_id list` |
| `skip_suspended` | `boolean` | 是否跳过停牌，默认不跳过   |
| `fields`         | `list`    | 可挑选返回的字段。默认返回所有 |

### 返回

pandas DataFrame

**分钟数据**

| 字段          | 类型      | 说明               |
| :------------ | :-------- | :----------------- |
| `open`        | `float`   | 此分钟开盘价       |
| `high`        | `float`   | 此分钟最高价       |
| `low`         | `float`   | 此分钟最低价       |
| `close`       | `float`   | 此分钟收盘价       |
| `volume`      | `integer` | 此分钟成交量       |
| `turnover`    | `float`   | 此分钟成交额       |
| `iopv`        | `float`   | 场内基金实时估算净值 |

### 范例

获取平安银行和浦发银行最近的分钟数据

```python
[In] current_minute(["000001.XSHE","600000.XSHG"])
[Out] close high low open total_turnover volume order_book_id datetime
000001.XSHE 2019-05-22 15:00:00 12.40 12.40 12.39 12.39 6502002.0 524355
600000.XSHG 2019-05-22 15:00:00 11.16 11.16 11.15 11.15 1987596.0 178100
```

获取交易所期权10002726 和90000339 最近的分钟数据

```python
[In]current_minute(order_book_ids=['10002726','90000339'])
[Out] close high low open open_interest total_turnover trading_date volume order_book_id datetime
10002726 2021-03-09 15:00:00 0.5514 0.5514 0.5514 0.5514 1488 0.0 20210309 0
90000339 2021-03-09 15:00:00 0.3286 0.3980 0.3286 0.3980 315 3394.0 20210309 1
```

## get_price_change_rate - 获取历史涨跌幅

```python
get_price_change_rate(id_or_symbols, start_date='20130104', end_date='20140104', expect_df=True)
```

获取股票或指数的历史涨跌幅（包含起止日期）。注意目前只支持股票、指数、可转债这三类合约，基金、期货等目前并不支持。历史涨跌幅基于后复权价格。

### 参数

| 参数            | 类型                                                       | 说明                                                           |
| :-------------- | :--------------------------------------------------------- | :------------------------------------------------------------- |
| `id_or_symbols` | `str` or `str list`                                        | 可输入`order_book_id`, `order_book_id list`                  |
| `start_date`    | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为`'2013-01-04'`                                   |
| `end_date`      | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为`'2014-01-04'`                                   |
| `expect_df`     | `boolean`                                                  | 默认返回`pandas dataframe`。如果调为`False`，则返回原有的数据结构 |

### 返回

（无特定表格，直接描述返回类型）

### 范例

获取平安银行以及沪深300 指数一段时间的涨跌幅情况。

```python
[In] get_price_change_rate(['000001.XSHE', '000300.XSHG'], '20150801', '20150807')
[Out] order_book_id 000001.XSHE 000300.XSHG
date
2015-08-03 0.037217 0.003285
2015-08-04 0.003120 0.031056
2015-08-05 -0.020995 -0.020581
2015-08-06 -0.004766 -0.009064
2015-08-07 0.006385 0.019597
```

## current_snapshot - 获取当前行情快照

```python
current_snapshot(order_book_ids, market='cn')
```

获取某一合约当前的LEVEL1 行情快照，支持集合竞价数据获取。

### 参数

| 参数             | 类型         | 说明                                       |
| :--------------- | :----------- | :----------------------------------------- |
| `order_book_ids` | `str` or `strlist` | 合约代码，可传入`order_book_id`, `order_book_id list`。 |
| `market`         | `str`        | 默认是中国市场(`'cn'`)，目前仅支持中国市场   |

### 返回

Tick 对象或者一个Tick list

| 属性              | 类型                 | 注释                                                         |
| :---------------- | :------------------- | :----------------------------------------------------------- |
| `datetime`        | `datetime.datetime`  | 时间戳                                                       |
| `order_book_id`   | `string`             | 合约代码                                                     |
| `open`            | `float`              | 当日开盘价                                                   |
| `high`            | `float`              | 当日最高价                                                   |
| `low`             | `float`              | 当日最低价                                                   |
| `last`            | `float`              | 最新价                                                       |
| `prev_settlement` | `float`              | 昨日结算价                                                   |
| `prev_close`      | `float`              | 昨日收盘价                                                   |
| `volume`          | `float`              | 成交量                                                       |
| `total_turnover`  | `float`              | 成交额                                                       |
| `limit_up`        | `float`              | 涨停价                                                       |
| `limit_down`      | `float`              | 跌停价                                                       |
| `open_interest`   | `float`              | 累计持仓量                                                   |
| `trading_phase_code` | `float`              | 停牌标识。T-正常交易、H-临时停牌、P-全天停牌。目前仅支持深交所股票 |
| `asks`            | `list`               | 卖出报盘价格，asks 代表盘口卖一档报盘价                   |
| `ask_vols`        | `list`               | 卖出报盘数量，ask_vols 代表盘口卖一档报盘数量             |
| `bids`            | `list`               | 买入报盘价格，bids 代表盘口买一档报盘价                   |
| `bid_vols`        | `list`               | 买入报盘数量，bid_vols 代表盘口买一档报盘数量             |
| `iopv`            | `float`              | 场内基金实时估算净值                                         |
| `prev_iopv`       | `float`              | 场内基金前估算净值                                           |
| `close`           | `float`              | 当日收盘价（现货专用，约15：30 可取，可以用是否大于0 判断是否已有值） |
| `settlement`      | `float`              | 当日结算价（现货专用）                                       |

### 范例

获取期权合约90000337 当前快照数据

```python
[In] current_snapshot('90000337')
[Out] Tick(ask_vols: [1, 1, 1, 10, 1], asks: [0.5119, 0.517, 0.5206, 0.5207, 0.522], bid_vols: [1, 1, 1, 1, 1], bids: [0.5007, 0.4967, 0.4926, 0.492, 0.4897], datetime: 2021-03-09 15:02:00, high: 0.6316, iopv: nan, last: 0.5118, limit_down: 0.1144, limit_up: 1.128, low: 0.5118, open: 0.6057, open_interest: 266, order_book_id: 90000337, prev_close: 0.6344, prev_iopv: nan, prev_settlement: 0.6212, total_turnover: 160569, trading_phase_code: T, volume: 27)
```

获取某一股票当前快照数据

```python
[In] current_snapshot('000001.XSHE')
[Out] Tick(ask_vols: [25400, 15500, 12300, 39985, 16200], asks: [13.7, 13.71, 13.72, 13.73, 13.74], bid_vols: [1050, 9300, 172301, 691800, 579400], bids: [13.69, 13.68, 13.67, 13.66, 13.65], datetime: 2020-07-24 11:30:00, high: 13.99, iopv: nan, last: 13.69, low: 13.66, open: 13.97, open_interest: None, order_book_id: 000001.XSHE, prev_close: 14.01, prev_iopv: nan, prev_settlement: None, total_turnover: 1199992014, trading_phase_code: T, volume: 86853387)
```

获取某一期货当前快照数据

```python
In [22]: current_snapshot('RB2010')
Out[22]: Tick(ask_vols: [158, 655, 954, 247, 373], asks: [3775.0, 3776.0, 3777.0, 3778.0, 3779.0], bid_vols: [25, 513, 90, 56, 2214], bids: [3774.0, 3773.0, 3772.0, 3771.0, 3770.0], datetime: 2020-07-24 11:30:00.143000, high: 3805.0, iopv: nan, last: 3774.0, low: 3766.0, open: 3804.0, open_interest: 1360251.0, order_book_id: RB2010, prev_close: 3806.0, prev_iopv: nan, prev_settlement: 3787.0, total_turnover: 29219994570.0, trading_phase_code: None, volume: 772145.0)
```

获取多个当前快照数据

```python
[In] current_snapshot(['000001.XSHE','600000.XSHG'])
[Out] [Tick(ask_vols: [25400, 15500, 12300, 39985, 16200], asks: [13.7, 13.71, 13.72, 13.73, 13.74], bid_vols: [1050, 9300, 172301, 691800, 579400], bids: [13.69, 13.68, 13.67, 13.66, 13.65], datetime: 2020-07-24 11:30:00, high: 13.99, iopv: nan, last: 13.69, low: 13.66, open: 13.97, open_interest: None, order_book_id: 000001.XSHE, prev_close: 14.01, prev_iopv: nan, prev_settlement: None, total_turnover: 1199992014, trading_phase_code: T, volume: 86853387), Tick(ask_vols: [251300, 4100, 13300, 344430, 13200], asks: [10.58, 10.59, 10.6, 10.61, 10.62], bid_vols: [1200, 124406, 294800, 136200, 170100], bids: [10.57, 10.56, 10.55, 10.54, 10.53], datetime: 2020-07-24 11:29:59.860000, high: 10.8, iopv: nan, last: 10.57, low: 10.55, open: 10.78, open_interest: None, order_book_id: 600000.XSHG, prev_close: 10.84, prev_iopv: nan, prev_settlement: None, total_turnover: 363418169.0, trading_phase_code: T, volume: 34090105)]
```

## get_trading_dates - 获取交易日列表

```python
get_trading_dates(start_date, end_date, market='cn')
```

获取某个国家市场的交易日列表（起止日期加入判断）。目前仅支持中国市场。

### 参数

| 参数         | 类型                                                       | 说明                                                       |
| :----------- | :--------------------------------------------------------- | :--------------------------------------------------------- |
| `start_date` | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                   |
| `end_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                                                   |
| `market`     | `str`                                                      | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |

### 返回

`datetime.date list` - 交易日期列表

### 范例

```python
[In] get_trading_dates(start_date='20160505', end_date='20160505')
[Out] [datetime.date(2016, 5, 5)]
```

## get_previous_trading_date - 获取历史某个交易日

```python
get_previous_trading_date(date,n,market='cn')
```

默认获取指定日期的上一交易日。

### 参数

| 参数     | 类型                                                       | 说明                                                       |
| :------- | :--------------------------------------------------------- | :--------------------------------------------------------- |
| `date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 指定日期                                                   |
| `n`      | `int`                                                      | `n` 代表往前第`n` 个交易日。默认为1，即前一个交易日        |
| `market` | `str`                                                      | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |

### 返回

`datetime.date` - 交易日期

### 范例

```python
[In] get_previous_trading_date('20160502',n=1)
[Out] [datetime.date(2016, 4, 29)]
```

## get_next_trading_date - 获取未来某个交易日

```python
get_next_trading_date(date, n, market='cn')
```

默认获取指定日期的国内市场的下一交易日。

### 参数

| 参数     | 类型                                                       | 说明                                                       |
| :------- | :--------------------------------------------------------- | :--------------------------------------------------------- |
| `date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 指定日期                                                   |
| `n`      | `int`                                                      | `n` 代表未来第`n` 个交易日。默认为1，即下一个交易日        |
| `market` | `str`                                                      | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |

### 返回

`datetime.date` - 交易日期

### 范例

```python
[In] get_next_trading_date(date='2016-05-01',n=1)
[Out] [datetime.date(2016, 5, 3)]
```

## get_latest_trading_date - 获取当前最近一个交易日

```python
get_latest_trading_date()
```

获取当天的最近一个交易日（若当天为交易日，则返回当天；若当天为节假日，则返回上一个交易日）

### 参数

| 参数     | 类型  | 说明                                                       |
| :------- | :---- | :--------------------------------------------------------- |
| `market` | `str` | 默认是中国内地市场(`'cn'`) 。可选`'cn'` - 中国内地市场；`'hk'` - 香港市场 |

### 返回

`datetime.date` - 交易日期

### 范例

```python
[In] get_latest_trading_date()
[Out]: datetime.date(2019, 11, 22)
```

## get_trading_hours - 获取合约连续竞价时间段

```python
get_trading_hours(order_book_id, frequency='1m', date=None, market='cn')
```

默认获取当前点国内市场合约字符串形式的连续竞价交易时间段。

### 参数

| 参数            | 类型                                                       | 说明                                                         |
| :-------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| `order_book_id` | `str`                                                      | 合约名称                                                     |
| `date`          | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 指定日期。使用场景，部分合约的当前和历史的连续竞价交易时间段会有不同 |
| `frequency`     | `str`                                                      | 频率,默认为`1m`, 对应米筐分钟线时间段的起始, 为tick 时返回交易所给出的交易时间 |
| `market`        | `str`                                                      | 目前只支持中国市场(`'cn'`)                                   |

### 返回

`string` - 交易时间

### 范例

```python
In [20]: get_trading_hours('000001.XSHE')
Out[20]: '09:31-11:30,13:01-15:00'
```

## get_exchange_rate - 获取汇率信息

```python
get_exchange_rate(start_date=None, end_date=None, fields=None)
```

获取指定时间段的汇率信息

### 参数

| 参数         | 类型                                                       | 说明     |
| :----------- | :--------------------------------------------------------- | :------- |
| `start_date` | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期 |
| `end_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期 |
| `fields`     | `str` OR `str list`                                        | 字段名称 |

### 返回

| 返回                   | 类型            | 说明                                                         |
| :--------------------- | :-------------- | :----------------------------------------------------------- |
| `date`                 | `str`           | 时间戳                                                       |
| `currency_pair`        | `str`           | 货币对。目前仅支持港币对人民币汇率，如`'HKDCNY'`表示1 港币对应的人民币 |
| `bid_referrence_rate`  | `str`           | 买入参考汇率，上交所和深交所披露值，日更新                   |
| `ask_referrence_rate`  | `str`           | 卖出参考汇率，上交所和深交所披露值，日更新                   |
| `middle_referrence_rate` | `str`           | 中间价，香港金融管理局披露值，月更新                         |
| `bid_settlement_rate_sh` | `str`           | 买入结算汇率-沪港通，盘后更新                                |
| `ask_settlement_rate_sh` | `str`           | 卖出结算汇率-沪港通，盘后更新                                |
| `bid_settlement_rate_sz` | `str`           | 买入结算汇率-深港通，盘后更新                                |
| `ask_settlement_rate_sz` | `str`           | 卖出结算汇率-深港通，盘后更新                                |

### 范例

```python
In [8]: rq.get_exchange_rate(20190101,20200101)
Out[8]: currency_pair bid_referrence_rate ask_referrence_rate ... ask_settlement_rate_sh bid_settlement_rate_sz ask_settlement_rate_sz
date
2019-01-02 HKDCNY 0.8509 0.9035 ... 0.87745 0.87683 0.87757
2019-01-03 HKDCNY 0.8497 0.9023 ... 0.87553 0.87649 0.87551
2019-01-04 HKDCNY 0.8523 0.9051 ... 0.87870 0.87866 0.87874
...
2020-08-14 HKDCNY 0.8687 0.9225 ... 0.89568 0.89534 0.89586
2020-08-17 HKDCNY 0.8700 0.9238 ... 0.89680 0.89708 0.89672
2020-08-18 HKDCNY 0.8682 0.9219 ... NaN NaN NaN
```

## get_yield_curve - 获取收益率曲线

```python
get_yield_curve(start_date='2013-01-04', end_date='2014-01-04', tenor=None, market='cn')
```

获取某个国家市场在一段时间内收益率曲线水平（包含起止日期）。目前仅支持中国市场。

数据为2002 年至今的中债国债收益率曲线，来源于中央国债登记结算有限责任公司。

### 参数

| 参数         | 类型                                                       | 说明                                         |
| :----------- | :--------------------------------------------------------- | :------------------------------------------- |
| `start_date` | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为`'2013-01-04'`                 |
| `end_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为`'2014-01-04'`                 |
| `tenor`      | `str`                                                      | 标准期限，`'0S'` - 隔夜，`'1M'` - 1 个月，`'1Y'` - 1 年 |
| `market`     | `str`                                                      | 默认是中国市场(`'cn'`)，目前支持中国市场。   |

### 返回

`pandas DataFrame` - 查询时间段内无风险收益率曲线

### 范例

```python
[In] get_yield_curve(start_date='20130104', end_date='20140104')
[Out] 0S 1M 2M 3M 6M 9M 1Y 2Y
2013-01-04 0.0196 0.0253 0.0288 0.0279 0.0280 0.0283 0.0292 0.0310
2013-01-05 0.0171 0.0243 0.0286 0.0275 0.0277 0.0281 0.0288 0.0305
2013-01-06 0.0160 0.0238 0.0285 0.0272 0.0273 0.0280 0.0287 0.0304
3Y 4Y ... 6Y 7Y 8Y 9Y 10Y
2013-01-04 0.0314 0.0318 ... 0.0342 0.0350 0.0353 0.0357 0.0361
2013-01-05 0.0309 0.0316 ... 0.0342 0.0350 0.0352 0.0356 0.0360
2013-01-06 0.0310 0.0315 ... 0.0340 0.0350 0.0352 0.0356 0.0360 ...
```

## get_live_minute_price_change_rate - 获取当日分钟涨跌幅（股票，指数）

```python
rqdatac.get_live_minute_price_change_rate(order_book_ids)
```

获取当日分钟涨跌幅

### 参数

| 参数             | 类型       | 说明                               |
| :--------------- | :--------- | :--------------------------------- |
| `order_book_ids` | `str` or `list` | 给出单个或多个`order_book_id`      |
| `adjust_type`    | `str`      | 昨收价格复权方式，默认为 `none`<br><br>不复权- `none`<br><br>前复权- `pre`<br><br>后复权- `post` |

### 返回

pandas Series

| 字段          | 类型    | 说明   |
| :------------ | :------ | :----- |
| `change_rate` | `float` | 涨跌幅 |

### 范例

获取多个合约当日分钟涨跌幅

```python
[In] rqdatac.get_live_minute_price_change_rate(['000001.XSHE','600000.XSHG'])
[Out] order_book_id 000001.XSHE 600000.XSHG
datetime
2022-09-23 09:31:00 -0.002441 -0.002809
2022-09-23 09:32:00 -0.001627 -0.001404
2022-09-23 09:33:00 0.000814 -0.002809
2022-09-23 09:34:00 0.000814 -0.002809
2022-09-23 09:35:00 0.000000 -0.001404
... ... ...
2022-09-23 14:56:00 -0.000814 0.004213
2022-09-23 14:57:00 0.000000 0.004213
2022-09-23 14:58:00 0.000814 0.004213
2022-09-23 14:59:00 0.000814 0.004213
2022-09-23 15:00:00 0.000000 0.005618
[240 rows x 2 columns]
```

## get_future_latest_trading_date - 获取当前最近一个期货交易日

```python
get_future_latest_trading_date()
```

获取当天的最近一个期货交易日（从夜盘的集合竞价开始算起，作为新的交易日；若当天为节假日，则返回下一个交易日）

### 参数

| 参数     | 类型  | 说明                         |
| :------- | :---- | :--------------------------- |
| `market` | `str` | 默认是中国内地市场(`'cn'`) |

### 返回

`datetime.date` - 交易日期

### 范例

```python
[In] get_future_latest_trading_date()
[Out]: datetime.date(2023, 6, 21)
```

## get_vwap - 获取日/分钟级别的vwap 历史数据

```python
rqdatac.get_vwap(order_book_ids, start_date=None, end_date=None, frequency='1d')
```

获取指定区间的vwap 数据

### 参数

| 参数             | 类型                                                       | 说明                                                         |
| :--------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str` OR `str list`                                        | 合约代码，可传入`order_book_id`, `order_book_id list`        |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                                                     |
| `frequency`      | `str`                                                      | 历史数据的频率。 默认为`'1d'`。<br><br>`1m` - 分钟级别<br><br>`1d` - 日级别分钟可选取不同频率，例如`'5m'`代表5 分线<br><br>支持品种：股票、期货、期权、ETF、可转债 |

### 返回

Series - vwap

### 范例

获取000001.XSHE 在2024-01-01~02-01 的日级别vwap 数据

```python
[In] rqdatac.get_vwap('000001.XSHE',20240101,20240201)
[Out]: order_book_id date
000001.XSHE 2024-01-02 9.286718
2024-01-03 9.182990
2024-01-04 9.112191
2024-01-05 9.302265
2024-01-08 9.178084
2024-01-09 9.140973
2024-01-10 9.128256
2024-01-11 9.135580
2024-01-12 9.203141
2024-01-15 9.205756
2024-01-16 9.277993
2024-01-17 9.314772
2024-01-18 9.103892
2024-01-19 9.148154
2024-01-22 9.174462
2024-01-23 9.078450
2024-01-24 9.209698
2024-01-25 9.422591
2024-01-26 9.562171
2024-01-29 9.730338
2024-01-30 9.594930
2024-01-31 9.481586
2024-02-01 9.414571
Name: 000001.XSHE, dtype: float64
```

获取IF2403 在2024-02-01 的1m 级别vwap 数据

```python
[In] rqdatac.get_vwap('IF2403',20240201,20240201,'1m')
[Out] order_book_id datetime
IF2403 2024-02-01 09:31:00 3201.224586
2024-02-01 09:32:00 3196.929688
2024-02-01 09:33:00 3200.511346
2024-02-01 09:34:00 3201.722967
2024-02-01 09:35:00 3203.796373
...
2024-02-01 14:56:00 3210.391429
2024-02-01 14:57:00 3209.169421
2024-02-01 14:58:00 3208.998773
2024-02-01 14:59:00 3208.252893
2024-02-01 15:00:00 3207.797403
Name: IF2403, Length: 240, dtype: float64
```

---

## 实时行情推送

考虑到实时行情中，用户不太方便通过主动轮询API 去获取合约最新不间断的实时行情，米筐开发提供了python sdk 和websocket 网络接口，用以支持用户获取实时行情推送数据，具体说明如下：

### Ricequant 实时数据的种类包括：

| 资产类别 | 说明               |
| :------- | :----------------- |
| 中国A 股 | 支持主板、创业板、科创板 |
| 场内基金 | 包括ETF、LOF         |
| 可转债   | 沪深市场合约       |
| 中国期货 | 包括股指、国债、商品期货 |
| 中国期权 | 包括ETF、股指、商品期权 |
| 国债逆回购 | 沪深市场合约       |
| 现货     | 包括黄金、铂金、白银等 |

### Ricequant 实时数据的频率包括：

*   提供Level1 tick 五档深度行情
*   提供实时的1 、3、5、15、30、60 等任意频率的分钟行情合成（注：30、60 分钟线是按照时间进行切片。例如合约在10:15-10:30 休市，则60 分钟线在11:00 只包含45 分钟的交易；而30 分钟线将出现10:15 的时间戳）

### A、实时行情推送的适用场景：

1.  驱动实盘交易或者模拟交易
2.  若客户已有实时行情，米筐可以作为备份

### B、相较于RQData 请求数据的优点：

1.  推送会比拉取型API 返回实时行情更及时，效率更高
2.  提供python sdk 和websocket 网络接口，用户可以使用任意语言接入，语言中性
3.  基于ricequant 的数据能力和rqdata 的基础设施，数据准确快速，可靠性高

### LiveMarketDataClient - websocket 实时行情推送方案

通过简单的一行代码从RQData 引入`LiveMarketDataClient` ，即可实现实时行情数据的推送。分别提供阻塞和不阻塞的调用方式，具体请参考范例。

### 范例

```python
[In] import rqdatac
from rqdatac import LiveMarketDataClient
rqdatac.init(username="license", password="邮件中一大串license的key")
client = LiveMarketDataClient()
# 订阅一支tick标的
client.subscribe('tick_000001.XSHE')
# 订阅1分钟行情
client.subscribe('bar_000001.XSHE')
# 订阅多支tick标的
client.subscribe(['tick_000001.XSHE', 'tick_000002.XSHE'])
# 订阅3分钟行情，其它分钟线的命名方法类似，修改后缀即可
# client.subscribe('bar_000001.XSHE_3m')
# 取消订阅tick标的
client.unsubscribe('tick_000002.XSHE')

# 检听行情
# 1. 阻塞的方式
for market in client.listen():
    print(market)
# 2. 不阻塞的方式
def handle_msg(tick_or_bar):
    # 可以自行定义处理
    # queue.push(tick_or_bar)
    print(tick_or_bar)
client.listen(handler=handle_msg) # handler不为None

# [Out]
# {'datetime': 20210913094009000, 'order_book_id': '000001.XSHE', 'prev_close': 20.57, 'num_trades': 12786, 'volume': 13134791.0, 'total_turnover': 267427634, 'trading_phase_code': 'T', 'last': 20.26, 'open': 20.36, 'high': 20.51, 'low': 20.25, 'limit_up': 22.63, 'limit_down': 18.51, 'ask': [20.28, 20.3, 20.31, 20.32, 20.33], 'bid': [20.26, 20.25, 20.24, 20.23, 20.22], 'ask_vol': [37100.0, 33500.0, 13200.0, 16100.0, 3600.0], 'bid_vol': [4400.0, 198000.0, 6800.0, 17700.0, 33600.0], 'trading_date': 20210913, 'channel': 'tick_000001.XSHE', 'action': 'feed'}
# {'datetime': 20210913094012000, 'order_book_id': '000001.XSHE', 'prev_close': 20.57, 'num_trades': 12882, 'volume': 13278591.0, 'total_turnover': 270342631, 'trading_phase_code': 'T', 'last': 20.3, 'open': 20.36, 'high': 20.51, 'low': 20.25, 'limit_up': 22.63, 'limit_down': 18.51, 'ask': [20.3, 20.31, 20.32, 20.33, 20.34], 'bid': [20.28, 20.25, 20.24, 20.23, 20.22], 'ask_vol': [15000.0, 13200.0, 16100.0, 3600.0, 4500.0], 'bid_vol': [4400.0, 150000.0, 6800.0, 18500.0, 33600.0], 'trading_date': 20210913, 'channel': 'tick_000001.XSHE', 'action': 'feed'}
# {'datetime': 20210913094015000, 'order_book_id': '000001.XSHE', 'prev_close': 20.57, 'num_trades': 13014, 'volume': 13402691.0, 'total_turnover': 272859932, 'trading_phase_code': 'T', 'last': 20.26, 'open': 20.36, 'high': 20.51, 'low': 20.25, 'limit_up': 22.63, 'limit_down': 18.51, 'ask': [20.27, 20.29, 20.3, 20.32, 20.33], 'bid': [20.26, 20.25, 20.24, 20.23, 20.22], 'ask_vol': [3200.0, 2300.0, 3700.0, 1500.0, 3600.0], 'bid_vol': [500.0, 112400.0, 8000.0, 17700.0, 33600.0], 'trading_date': 20210913, 'channel': 'tick_000001.XSHE', 'action': 'feed'}
# {'datetime': 20210913094018000, 'order_book_id': '000001.XSHE', 'prev_close': 20.57, 'num_trades': 13110, 'volume': 13499491.0, 'total_turnover': 274820911, 'trading_phase_code': 'T', 'last': 20.28, 'open': 20.36, 'high': 20.51, 'low': 20.25, 'limit_up': 22.63, 'limit_down': 18.51, 'ask': [20.28, 20.29, 20.3, 20.32, 20.33], 'bid': [20.25, 20.24, 20.23, 20.22, 20.21], 'ask_vol': [9100.0, 6100.0, 4800.0, 1500.0, 2600.0], 'bid_vol': [52800.0, 8000.0, 17700.0, 33600.0, 165600.0], 'trading_date': 20210913, 'channel': 'tick_000001.XSHE', 'action': 'feed'}
```


# 股票行情数据说明

可获取股票合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price.

# A 股财务数据

## get_pit_financials_ex - 查询季度财务信息(point-in-time 形式)

`get_pit_financials_ex(fields, start_quarter,end_quarter，order_book_ids=None, date=None, statements=None, market='cn')`

以给定一个报告期回溯的方式获取季度基础财务数据（三大表），即利润表（income_statement），资产负债表（balance_sheet），现金流量表（cash_flow_statement)。

### 参数

| 参数             | 类型         | 说明                                                         |
| :--------------- | :----------- | :----------------------------------------------------------- |
| `fields`         | `list`       | 需要返回的财务字段。支持的字段仅限[三大基础财报],具体可以参看财务数据页介绍。 |
| `start_quarter`  | `str`        | 财报回溯查询的起始报告期，例如'2015q2'代表2015 年半年报， 该参数必填。 |
| `end_quarter`    | `str`        | 财报回溯查询的截止报告期，例如'2015q4'代表2015 年年报，该参数必填。 |
| `order_book_ids` | `str or str list` | 合约代码，可传入order_book_id, order_book_id list ，该参数必填 |
| `date`           | `str`        | 查询日期，默认查询日期为当前最新日期                         |
| `statements`     | `str`        | 基于查询日期，返回某一个报告期的所有记录或最新一条记录，设置statements 为all 时返回所有记录，statements 等于latest 时返回最新的一条记录，默认为latest. |
| `market`         | `str`        | 市场，默认'cn'为中国内地市场                                 |

### 返回

`pandas DataFrame`

| 固定字段      | 类型  | 说明                                                         |
| :------------ | :---- | :----------------------------------------------------------- |
| `fields`      | `list` | 需要返回的财务字段。支持的字段仅限[三大基础财报],具体可以参看财务数据页介绍。 |
| `if_adjusted` | `int` | 是否为非当期财报数据, 0 代表当期，1 代表非当期（比如18 年的财报会披露本期和上年同期的数值，17 年年报的财务数值在18 年年报中披露的记录则为非当期， 17 年年报的财务数值在17 年年报中披露则为当期。 |
| `quarter`     | `str` | 报告期                                                       |
| `info_date`   | `str` | 公告发布日                                                   |

### 范例

获取股票2018q2-2018q3 各报告期最新一次记录

```
[In] get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q3',order_book_ids=['000001.XSHE','000048.XSHE'])
[Out] info_date revenue if_adjusted net_profit order_book_id quarter
000001.XSHE 2018q2 2019-08-08 5.724100e+10 1 1.337200e+10
2018q3 2019-10-22 8.666400e+10 1 2.045600e+10
000048.XSHE 2018q2 2019-08-31 7.362684e+08 1 -3.527276e+07
2018q3 2019-10-31 1.216331e+09 1 -4.566952e+07
```

获取股票2018q2 所有的记录

```
[In] get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q2',order_book_ids=['000001.XSHE','000048.XSHE'],statements='all')
[Out] info_date revenue if_adjusted net_profit order_book_id quarter
000001.XSHE 2018q2 2018-08-16 5.724100e+10 0 1.337200e+10
2018q2 2019-08-08 5.724100e+10 1 1.337200e+10
000048.XSHE 2018q2 2018-08-31 1.063670e+09 0 7.790205e+07
2018q2 2018-10-31 1.060487e+09 0 7.880372e+07
2018q2 2019-06-15 7.362684e+08 0 -3.527276e+07
2018q2 2019-08-31 7.362684e+08 1 -3.527276e+07
```

获取股票2018q2 查询日期为20190807 的记录

```
[In] get_pit_financials_ex(fields=['revenue','net_profit'], start_quarter='2018q2', end_quarter='2018q2',order_book_ids=['000001.XSHE','000048.XSHE'],statements='all',date='20190807')
[Out] info_date revenue if_adjusted net_profit order_book_id quarter
000001.XSHE 2018q2 2018-08-16 5.724100e+10 0 1.337200e+10
000048.XSHE 2018q2 2018-08-31 1.063670e+09 0 7.790205e+07
2018q2 2018-10-31 1.060487e+09 0 7.880372e+07
2018q2 2019-06-15 7.362684e+08 0 -3.527276e+07
```

## current_performance - 查询财务快报数据

`current_performance(order_book_id, info_date, quarter, interval, fields)`

默认返回给定的order_book_id 当前最近一期的快报数据。

### 参数

| 参数          | 类型        | 说明                                                         |
| :------------ | :---------- | :----------------------------------------------------------- |
| `order_book_id` | `str`       | 必填。由于快报报告日期的不规则，目前只支持单个查询，批量请考虑轮询。 |
| `info_date`   | `str`       | `yyyymmdd` 或者`yyyy-mm-dd`。如果不填(`info_date` 和`quarter` 都为空)，则返回当前日期的最新发布的快报。如果填写，则从`info_date` 当天或者之前最新的报告开始抓取。 |
| `quarter`     | `str`       | `info_date` 参数优先级高于`quarter`。如果`info_date` 填写了日期，则不查看`quarter` 这个字段。 如果`info_date` 没有填写而`quarter` 有填写，则财报回溯查询的起始报告期，例如'2015q2', '2015q4'分别代表2015 年半年报以及年报。默认只获取当前报告期财务信息 |
| `interval`    | `str`       | 查询财务数据的间隔。例如，填写'5y'，则代表从报告期开始回溯5 年，每年为相同报告期数据；'3q'则代表从报告期开始向前回溯3 个季度。不填写默认抓取一期。 |
| `fields`      | `str or list` | 抓取对应有效字段返回。默认返回所有字段。具体快报字段可参看线上财务文档。 |

### 返回

`pandas DataFrame`

### 范例

获取单只股票过去一个报告期的快报数据

```
[In] current_performance('000004.XSHE')
[Out] end_date info_date operating_revenue gross_profit operating_profit total_profit np_parent_owners net_profit_cut net_operate_cashflow...roe_cut_weighted_yoy net_operate_cash_flow_per_share_yoy net_asset_psto_opening
0 2017-12-31 2018-04-14 1.386058e+08 NaN 8796946.37 9716431.21 8566720.65 NaN NaN NaN NaN NaN
```

获取单只股票多个报告期的总利润

```
[In] current_performance('000004.XSHE',quarter='2017q4',fields='total_profit',interval='2q')
[Out] end_date info_date total_profit
0 2017-12-31 2018-04-14 9716431.21
1 2015-12-31 2016-04-15 10808606.48
```

```
[In] current_performance('000004.XSHE',info_date=20170331,fields='total_profit',interval='2q')
[Out] end_date info_date total_profit
0 2015-12-31 2016-04-15 10808606.48
1 2014-12-31 2015-04-16 20665807.64
```

## performance_forecast - 查询业绩预告数据

`performance_forecast(order_book_ids,info_date,end_date,fields)`

默认返回给定的order_book_ids 当前最近一期的业绩预告数据。

业绩预告主要用来调取公司对即将到来的财务季度的业绩预期的信息。有时同一个财务季度会有多条记录，分别是季度预期和累计预期（即本年至今）。

### 参数

| 参数             | 类型        | 说明                                                         |
| :--------------- | :---------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list` | 必填。                                                       |
| `info_date`      | `str`       | `yyyymmdd` 或者`yyyy-mm-dd`。如果不填(`info_date` 和`end_date` 都为空)，则返回当前日期的最新发布的业绩预告。如果填写，则从`info_date` 当天或者之前最新的报告开始抓取。注：`info_date` 优先级高于`end_date`。 |
| `end_date`       | `str`       | `yyyymmdd` 或者`yyyy-mm-dd`。对应财务预告期末日期，如'20150331'。 |
| `fields`         | `str or list` | 抓取对应有效字段返回。默认返回所有字段。具体业绩预告字段可参看线上财务文档。 |

### 返回

`pandas DataFrame`

### 范例

获取单只股票过去一个报告期的预告数据

```
[In] performance_forecast('000001.XSHE')
[Out] info_date end_date forecast_type forecast_description forecast_growth_rate_floor forecast_growth_rate_ceiling forecast_earning_floor forecast_earning_ceiling forecast_np_floor forecast_np_ceiling forecast_eps_floor forecast_eps_ceiling net_profit_yoy_const_forecast
0 2016-01-21 2015-12-31 预增 累计利润 5.0 15.0 NaN NaN 2.079206e+10 2.277225e+10 1.48 1.62 16.0
```

获取多只股票过去一个报告期指定字段的预告数据

```
[In] performance_forecast(['000001.XSHE','000006.XSHE'],fields=['forecast_description','forecast_earning_floor'])
[Out] info_date end_date forecast_description forecast_earning_floor order_book_id
000001.XSHE 2016-01-21 2015-12-31 累计利润 NaN
000006.XSHE 2020-04-09 2020-12-31 累计收入 NaN
```

获取单只股票指定报告期预告数据

```
[In] performance_forecast('000005.XSHE',end_date=20170331,fields=['forecast_description','forecast_earning_floor'])
[Out] info_date end_date forecast_description forecast_earning_floor
0 2017-04-15 2017-03-31 累计利润 NaN
```

# A 股因子数据

## get_factor - 获取因子

`get_factor(order_book_ids, factor, start_date, end_date, universe=None,expect_df=True)`

默认快速返回给定的order_book_id 当日的因子，包括财务因子、alpha101 因子、技术指标 等。

### 参数

| 参数             | 类型        | 说明                                                         |
| :--------------- | :---------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str list` | 合约代码，可传入order_book_id, order_book_id list          |
| `factor`         | `str or str list` | 因子名称，可查询get_all_factor_names() 得到所有有效因子字段 |
| `date`           | `str`       | 指定日期，默认为该日的前一个交易日                           |
| `start_date`     | `str`       | 开始日期。注：如使用开始日期，则必填结束日期                 |
| `end_date`       | `str`       | 结束日期。注：若使用结束日期，则开始日期必填                 |
| `universe`       | `str`       | 股票池，可选定某个指数的成分股，也可输入股票list，默认为None，全市场 |
| `expect_df`      | `boolean`   | 默认返回pandas dataframe。如果调为False，则返回原有的数据结构 |

### 返回

`pandas DataFrame`

### 范例

获取财务指标数据

```
[In] get_factor(['000001.XSHE','000002.XSHE'],'debt_to_equity_ratio',start_date='20180102',end_date='20180103')
[Out] debt_to_equity_ratio order_book_id date
000002.XSHE 2018-01-02 7.3097
2018-01-03 7.3097
000001.XSHE 2018-01-02 13.3848
2018-01-03 13.3848
```

获取alpha101 因子数据

```
[In] get_factor(['000001.XSHE', '600000.XSHG'],'WorldQuant_alpha010', '20190601', '20190604')
[Out] WorldQuant_alpha010 order_book_id date
000001.XSHE 2019-06-03 0.093489
2019-06-04 0.281502
600000.XSHG 2019-06-03 0.162771
2019-06-04 0.255633
```

获取技术指标数据

```
[In] get_factor(['000001.XSHE', '600000.XSHG'], 'MA30', '20190801','20190802')
[Out] MA30 order_book_id date
000001.XSHE 2019-08-01 13.850660
2019-08-02 13.858904
600000.XSHG 2019-08-01 11.632667
2019-08-02 11.612333
```

## get_all_factor_names - 获取因子字段列表

`get_all_factor_names(type=None)`

默认返回全部因子，可选择返回不同类型的所有因子字段名称列表。

### 参数

| 参数   | 类型  | 说明                                                         |
| :----- | :---- | :----------------------------------------------------------- |
| `type` | `str` | 默认返回所有因子'income_statement'：利润表( 91 个基础财务字段+ 其mrq_n、ttm_n 、lyr_n 因子，共2821 个)  'balance_sheet'：资产负债表( 156 个基础财务字段+ 其mrq_n、ttm_n 、lyr_n 因子，共4836 个)  'cash_flow_statement'：现金流量表( 70 个基础财务字段+ 其mrq_n、ttm_n 、lyr_n 因子，共2170 个)  'eod_indicator'：估值有关指标(41 个)  'operational_indicator'：经营衍生指标表(97 个)  'cash_flow_indicator'：现金流衍生指标(26 个)  'financial_indicator'：财务衍生指标(126 个)  'growth_indicator'：增长衍生指标(33 个)  'alpha101'：alpha101 因子(101 个)  'moving_average_indicator'：均线类指标(82 个)  'obos_indicator'：超买超卖指标(43 个)  'energy_indicator'：能量指标(36 个) |

### 返回

`list`

### 范例

获取超买超卖指标因子

```
[In] rqdatac.get_all_factor_names(type='obos_indicator')
[Out] ['OBOS', 'KDJ_K', 'KDJ_D', 'KDJ_J', 'RSI6', 'WR', 'LWR1', 'BIAS5', 'BIAS36', 'ACCER', 'CYF', 'SWL', 'ADTM', 'TR', 'DKX', 'TAPI', 'OSC', 'CCI', 'ROC', 'MFI', 'MTM', 'MARSI6', 'SKD_K', 'UDL', 'DI1', 'RSI10', 'LWR2', 'BIAS10', 'BIAS612', 'SWS', 'MAADTM', 'ATR', 'MADKX', 'MATAPI', 'MAMTM', 'MARSI10', 'SKD_D', 'MAUDL', 'DI2', 'BIAS20', 'MABIAS', 'ADX', 'ADXR']
```

# 基础日行情

## get_share_transformation - 获取股票转换股票代码信息

`get_share_transformation(predecessor=None, market='cn')`

查询股票因代码变更或并购等情况更换了股票代码的信息

### 参数

| 参数          | 类型  | 说明                                        |
| :------------ | :---- | :------------------------------------------ |
| `predecessor` | `str` | 合约代码(来自交易所或其他平台), 空值返回所有变更过股票代码的股票 |
| `market`      | `str` | 目前仅支持国内市场('cn')。                  |

### 返回

`pandas Dataframe`

| 字段                      | 类型       | 说明             |
| :------------------------ | :--------- | :--------------- |
| `predecessor`             | `str`      | 历史股票代码     |
| `successor`               | `str`      | 变更后股票代码   |
| `effective_date`          | `str`      | 变更生效日期     |
| `share_conversion_ratio`  | `float`    | 股票变更比例     |
| `predecessor_delisted`    | `boolean`  | 变更后旧代码是否退市 |
| `discretionary_execution` | `boolean`  | 是否有变更自主选择权 |
| `predecessor_delisted_date` | `datetime` | 历史股票代码退市日期 |
| `event`                   | `str`      | 股票代码变更原因 |

### 范例

```
[In]get_share_transformation(predecessor="000022.XSHE")
[Out] predecessor successor effective_date share_conversion_ratio predecessor_delisted discretionary_execution predecessor_delisted_date event
0 000022.XSHE 001872.XSHE 2018-12-26 1.0 True False 2018-12-26 code_change
```

## sector - 获取某板块股票列表

`sector(code, market='cn')`

获得属于某一板块的所有股票列表。

### 参数

| 参数   | 类型                 | 说明                                                         |
| :----- | :------------------- | :----------------------------------------------------------- |
| `code` | `str OR sector_code items` | 板块名称或板块代码。例如，能源板块可填写'Energy'、'能源'或sector_code.Energy |
| `market` | `str`                | 默认是中国市场('cn')，目前仅支持中国市场。                   |

### 返回

属于该板块的股票order_book_id 或order_book_id list.

### 范例

```
[In]sector('Energy')
[Out] ['300023.XSHE', '000571.XSHE', '600997.XSHG', '601798.XSHG', '603568.XSHG', .....]
[In]sector(sector_code.Energy)
[Out] ['300023.XSHE', '000571.XSHE', '600997.XSHG', '601798.XSHG', '603568.XSHG', .....]
```

### 板块分类列表

目前支持的板块分类如下，其取值参考自MSCI 发布的全球行业标准分类:

| 板块代码           | 中文板块名称 | 英文板块名称          |
| :----------------- | :----------- | :-------------------- |
| `Energy`           | 能源         | energy                |
| `Materials`        | 原材料       | materials             |
| `ConsumerDiscretionary` | 非必需消费品 | consumer discretionary |
| `ConsumerStaples`  | 必需消费品   | consumer staples      |
| `HealthCare`       | 医疗保健     | health care           |
| `Financials`       | 金融         | financials            |
| `RealEstate`       | 房地产       | real estate           |
| `InformationTechnology` | 信息技术     | information technology |
| `TelecommunicationServices` | 电信服务     | telecommunication services |
| `Utilities`        | 公共服务     | utilities             |
| `Industrials`      | 工业         | industrials           |

## industry - 获取某行业股票列表

`industry(code, market='cn')`

获得属于某一行业的所有股票列表。

### 参数

| 参数   | 类型                 | 说明                                                         |
| :----- | :------------------- | :----------------------------------------------------------- |
| `code` | `str OR industry_code items` | 行业名称或行业代码。例如，农业可填写industry_code.A01 或'A01' |
| `market` | `str`                | 默认是中国市场('cn')，目前仅支持中国市场                   |

### 返回

属于该行业的股票order_book_id 或order_book_id list.

### 范例

```
[In] industry('A01')
[Out] ['600540.XSHG', '600371.XSHG', '600359.XSHG', '600506.XSHG',...]
[In] industry(industry_code.A01)
[Out] ['600540.XSHG', '600371.XSHG', '600359.XSHG', '600506.XSHG',...]
```

### 行业分类列表

我们目前使用的行业分类来自于中国国家统计局的国民经济行业分类，可以使用这里的任何一个行业代码来调用行业的股票列表：

| 行业代码 | 行业名称       | 行业代码 | 行业名称           |
| :------- | :------------- | :------- | :----------------- |
| `A01`    | 农业           | `C30`    | 非金属矿物制品业   |
| `A02`    | 林业           | `C31`    | 黑色金属冶炼及压延加工业 |
| `A03`    | 畜牧业         | `C32`    | 有色金属冶炼和压延加工业 |
| `A04`    | 渔业           | `C33`    | 金属制品业         |
| `A05`    | 农、林、牧、渔服务业 | `C34`    | 通用设备制造业     |
| `B06`    | 煤炭开采和洗选业 | `C35`    | 专用设备制造业     |
| `B07`    | 石油和天然气开采业 | `C36`    | 汽车制造业         |
| `B08`    | 黑色金属矿采选业 | `C37`    | 铁路、船舶、航空航天和其它运输设备制造业 |
| `B09`    | 有色金属矿采选业 | `C38`    | 电气机械及器材制造业 |
| `B10`    | 非金属矿采选业 | `C39`    | 计算机、通信和其他电子设备制造业 |
| `B11`    | 开采辅助活动   | `C40`    | 仪器仪表制造业     |
| `B12`    | 其他采矿业     | `C41`    | 其他制造业         |
| `C13`    | 农副食品加工业 | `C42`    | 废弃资源综合利用业 |
| `C14`    | 食品制造业     | `C43`    | 金属制品、机械和设备修理业 |
| `C15`    | 酒、饮料和精制茶制造业 | `D44`    | 电力、热力生产和供应业 |
| `C16`    | 烟草制品业     | `D45`    | 燃气生产和供应业   |
| `C17`    | 纺织业         | `D46`    | 水的生产和供应业   |
| `C18`    | 纺织服装、服饰业 | `E47`    | 房屋建筑业         |
| `C19`    | 皮革、毛皮、羽毛及其制品和制鞋业 | `E48`    | 土木工程建筑业     |
| `C20`    | 木材加工及木、竹、藤、棕、草制品业 | `E49`    | 建筑安装业         |
| `C21`    | 家具制造业     | `E50`    | 建筑装饰和其他建筑业 |
| `C22`    | 造纸及纸制品业 | `F51`    | 批发业             |
| `C23`    | 印刷和记录媒介复制业 | `F52`    | 零售业             |
| `C24`    | 文教、工美、体育和娱乐用品制造业 | `G53`    | 铁路运输业         |
| `C25`    | 石油加工、炼焦及核燃料加工业 | `G54`    | 道路运输业         |
| `C26`    | 化学原料及化学制品制造业 | `G55`    | 水上运输业         |
| `C27`    | 医药制造业     | `G56`    | 航空运输业         |
| `C28`    | 化学纤维制造业 | `G57`    | 管道运输业         |
| `C29`    | 橡胶和塑料制品业 | `G58`    | 装卸搬运和运输代理业 |
| `G59`    | 仓储业         | `N77`    | 生态保护和环境治理业 |
| `G60`    | 邮政业         | `N78`    | 公共设施管理业     |
| `H61`    | 住宿业         | `O79`    | 居民服务业         |
| `H62`    | 餐饮业         | `O80`    | 机动车、电子产品和日用产品修理业 |
| `I63`    | 电信、广播电视和卫星传输服务 | `O81`    | 其他服务业         |
| `I64`    | 互联网和相关服务 | `P82`    | 教育               |
| `I65`    | 软件和信息技术服务业 | `Q83`    | 卫生               |
| `J66`    | 货币金融服务   | `Q84`    | 社会工作           |
| `J67`    | 资本市场服务   | `R85`    | 新闻和出版业       |
| `J68`    | 保险业         | `R86`    | 广播、电视、电影和影视录音制作业 |
| `J69`    | 其他金融业     | `R87`    | 文化艺术业         |
| `K70`    | 房地产业       | `R88`    | 体育               |
| `L71`    | 租赁业         | `R89`    | 娱乐业             |
| `L72`    | 商务服务业     | `S90`    | 综合               |
| `M73`    | 研究和试验发展 |          |                    |
| `M74`    | 专业技术服务业 |          |                    |
| `M75`    | 科技推广和应用服务业 |          |                    |
| `N76`    | 水利管理业     |          |                    |

## get_concept_list - 获取概念列表

`get_concept_list(start_date=None, end_date=None)`

获得股票概念列表。

### 参数

| 参数         | 类型  | 说明                                                       |
| :----------- | :---- | :--------------------------------------------------------- |
| `start_date` | `str` | 查询概念纳入日期开始时间，不传入默认返回所有时段数据       |
| `end_date`   | `str` | 查询概念纳入日期结束时间，不传入默认返回所有时段数据       |

### 返回

| 参数      | 类型   | 说明         |
| :-------- | :----- | :----------- |
| `date`    | `date` | 概念纳入日期   |
| `concept` | `str`  | 概念名称     |

### 范例

```
[In] get_concept_list(start_date='2019-01-01', end_date='2020-01-01')
[Out] date
2019-01-17 360私有化2019-01-17 油气改革2019-01-17 浦东国资改革2019-01-17 海南自贸区2019-01-17 海绵城市...
2019-07-15 ETC概念2019-07-17 光刻机概念2019-08-12 维生素2019-12-12 胎压监测2019-12-20 无线耳机
```

## get_concept - 获取所选概念对应股票列表

`get_concept(concepts, start_date=None, end_date=None)`

获得属于某个或某几个概念的股票列表。

### 参数

| 参数         | 类型             | 说明                                                         |
| :----------- | :--------------- | :----------------------------------------------------------- |
| `concepts`   | `str OR multiple str` | 概念名称。可以从概念列表中选择一个或多个概念填写             |
| `start_date` | `str`            | 查询股票纳入概念日期开始时间，不传入默认返回所有时段数据   |
| `end_date`   | `str`            | 查询股票纳入概念日期结束时间，不传入默认返回所有时段数据   |

### 返回

| 参数             | 类型   | 说明             |
| :--------------- | :----- | :--------------- |
| `order_book_id`  | `str`  | 合约代码         |
| `inclusion_date` | `date` | 股票纳入概念日期 |

### 范例

```
[In] get_concept(['ETC概念','维生素'],start_date='2019-01-01', end_date='2021-01-01')
[Out] order_book_id inclusion_date concept
ETC概念 000938.XSHE 2019-07-15 ETC概念
002104.XSHE 2019-10-16 ETC概念
002161.XSHE 2019-08-08 ETC概念
002373.XSHE 2019-07-15 ETC概念
002401.XSHE 2019-07-15 ETC概念
002512.XSHE 2019-07-15 ETC概念
002869.XSHE 2019-07-15 ETC概念
300014.XSHE 2019-07-15 ETC概念
300020.XSHE 2019-10-29 ETC概念
300075.XSHE 2019-07-15 ETC概念
300205.XSHE 2019-07-15 ETC概念
300376.XSHE 2019-07-23 ETC概念
300438.XSHE 2019-09-10 ETC概念
300448.XSHE 2019-07-15 ETC概念
300462.XSHE 2019-07-15 ETC概念
300552.XSHE 2019-07-15 ETC概念
300717.XSHE 2019-07-15 ETC概念
600035.XSHG 2019-07-15 ETC概念
603068.XSHG 2019-07-15 ETC概念
603458.XSHG 2019-07-31 ETC概念
603936.XSHG 2019-12-23 ETC概念
688208.XSHG 2020-02-24 维生素
000597.XSHE 2019-08-12 维生素
000952.XSHE 2019-08-12 维生素
002001.XSHE 2019-08-12 维生素
002019.XSHE 2019-08-12 维生素
002332.XSHE 2019-08-12 维生素
002562.XSHE 2019-08-12 维生素
002626.XSHE 2019-08-12 维生素
300267.XSHE 2019-08-12 维生素
300401.XSHE 2019-08-12 维生素
600216.XSHG 2019-08-12 维生素
600299.XSHG 2019-08-12 维生素
600812.XSHG 2019-08-12 维生素
603079.XSHG 2019-08-12
```

## get_stock_concept - 获取所选股票对应概念

`get_stock_concept(order_book_ids)`

获取单支或多支股票所对应的所有概念标签

### 参数

| 参数             | 类型     | 说明     |
| :--------------- | :------- | :------- |
| `order_book_id`  | `str or list` | 合约代码 |

### 返回

| 参数             | 类型   | 说明             |
| :--------------- | :----- | :--------------- |
| `order_book_id`  | `str`  | 合约代码         |
| `concept`        | `str`  | 所输入合约代码对应的概念 |
| `inclusion_date` | `date` | 股票纳入概念日期 |

### 范例

```
[In] get_stock_concept (['000002.XSHE','000504.XSHE'])
[Out] concept order_book_id inclusion_date
000002.XSHE 2019-01-17 房地产2019-01-17 深港通2019-01-17 租售同权2019-01-17 广东自贸区2019-01-22 举牌概念2019-03-04 粤港澳大湾区2019-03-18 雄安概念2019-03-18 特色小镇2019-05-07 互联网金融2019-08-19 体育2019-08-19 冬奥会2019-12-02 MSCI概念2020-01-15 超级品牌2020-11-16 长江三角洲概念2022-01-06 碳中和2024-01-17 智能家居2024-01-17 智慧城市
000504.XSHE 2019-01-17 美丽中国2019-01-22 举牌概念2019-05-07 文化传媒2021-12-01 医药2021-12-01 节能环保2022-11-28 国企混改2024-05-11 生物医药
```

## get_industry_mapping - 获取行业分类概览

`get_industry_mapping(source='citics', date=None, market='cn')`

通过传入机构名，获得该机构指定的行业划分概览。

### 参数

| 参数     | 类型  | 说明                                                         |
| :------- | :---- | :----------------------------------------------------------- |
| `source` | `str` | 分类依据。 `citics`: 中信, `gildata`: 聚源,`citics_2019`:中信2019 分类,默认`source='citics_2019'`.注意：`citics` 为中信2019 年新的行业分类未发布前的分类 |
| `date`   | `str` | 查询日期，默认为当前最新日期                                 |
| `market` | `str` | 目前仅有中国市场('cn')的行业分类                             |

### 返回

`DataFrame`

### 范例

得到当前行业分类的概览：

```
[In] get_industry_mapping()
[Out] first_industry_code first_industry_name second_industry_code second_industry_name third_industry_code third_industry_name
0 10 石油石化1010 石油开采101010 石油开采
1 10 石油石化1020 石油化工102010 炼油
2 10 石油石化1020 石油化工102040 油品销售及仓储
3 10 石油石化1020 石油化工102050 其他石化
4 10 石油石化1030 油田服务103010 油田服务
5 11 煤炭1110 煤炭开采洗选111010 动力煤...
```

## get_industry - 获取某行业的股票列表

`get_industry(industry, source='citics', date=None, market='cn')`

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票列表

### 参数

| 参数       | 类型  | 说明                                                         |
| :--------- | :---- | :----------------------------------------------------------- |
| `industry` | `str` | 可传入行业名称、行业指数代码或者行业代号                     |
| `source`   | `str` | 分类依据。 `citics`: 中信, `gildata`: 聚源, `citics_2019`:中信2019 分类, 默认`source='citics_2019'`. 注意：`citics` 为中信2019 年新的行业分类未发布前的分类 |
| `date`     | `str` | 查询日期，默认为当前最新日期                                 |
| `market`   | `str` | 目前仅有中国市场('cn')的行业分类                             |

### 返回

`list`

### 范例

得到当前某一级行业的股票列表：

```
[In] get_industry('银行')
[Out] ['000001.XSHE', '002142.XSHE', '002807.XSHE', '002839.XSHE', '002936.XSHE', '002948.XSHE', '002958.XSHE', '002966.XSHE', '600000.XSHG', '600015.XSHG', '600016.XSHG', '600036.XSHG', '600908.XSHG', '600919.XSHG', '600926.XSHG', '600928.XSHG', '601009.XSHG', '601128.XSHG', '601166.XSHG', '601169.XSHG', '601229.XSHG', '601288.XSHG', '601328.XSHG', '601398.XSHG', '601577.XSHG', '601818.XSHG', '601838.XSHG', '601860.XSHG', '601939.XSHG', '601988.XSHG', '601997.XSHG', '601998.XSHG', '603323.XSHG']
```

用中信行业代码获得股票列表：

```
[In] get_industry(industry='621020',source='citics')
[Out] ['000997.XSHE', '002152.XSHE', '002177.XSHE', '002268.XSHE', '002308.XSHE', '002312.XSHE', '002376.XSHE', '002383.XSHE', '002512.XSHE', '002518.XSHE', '002546.XSHE', '002635.XSHE', '002771.XSHE', '002829.XSHE', '002835.XSHE', '002906.XSHE', '300074.XSHE', '300076.XSHE', '300078.XSHE', '300098.XSHE', '300130.XSHE', '300167.XSHE', '300177.XSHE', '300270.XSHE', '300275.XSHE', '300311.XSHE', '300449.XSHE', '300455.XSHE', '300458.XSHE', '300479.XSHE', '300743.XSHE', '603106.XSHG', '603660.XSHG', '603890.XSHG']
```

## get_industry_change - 获取某行业的股票纳入剔除日期

`get_industry_change(industry, source='citics', level=None, market='cn')`

通过传入行业名称、行业指数代码或者行业代号，拿到指定行业的股票纳入剔除日期

### 参数

| 参数       | 类型    | 说明                                                         |
| :--------- | :------ | :----------------------------------------------------------- |
| `industry` | `str`   | 可传入行业名称、行业指数代码或者行业代号                     |
| `source`   | `str`   | 分类依据。 `citics_2019` - 中信新分类（2019 发布）, `citics` - 中信旧分类（退役中）, `gildata` -聚源。 默认`source='citics_2019'`. |
| `level`    | `integer` | 行业分类级别，共三级，默认一级分类。参数1,2,3 一一对应       |
| `market`   | `str`   | 目前仅有中国市场('cn')的行业分类                             |

### 返回

`pandas DataFrame`

| 字段          | 类型  | 说明             |
| :------------ | :---- | :--------------- |
| `start_date`  | `str` | 起始日期         |
| `cancel_date` | `str` | 取消日期，2200-12-31 表示未披露 |

### 范例

得到当前某一级行业的股票纳入剔除日期：

```
[In] get_industry_change(industry='银行', level=1,source='citics_2019')
[Out] start_date cancel_date order_book_id
601988.XSHG 2019-12-02 2200-12-31
601398.XSHG 2019-12-02 2200-12-31
601328.XSHG 2019-12-02 2200-12-31
601939.XSHG 2019-12-02 2200-12-31
601288.XSHG 2019-12-02 2200-12-31
... ... ...
601963.XSHG 2021-02-05 2200-12-31
601665.XSHG 2021-06-18 2200-12-31
601528.XSHG 2021-06-25 2200-12-31
601825.XSHG 2021-08-19 2200-12-31
001227.XSHE 2022-01-17 2200-12-31
```

## get_instrument_industry - 获取股票的指定行业分类

`get_instrument_industry(order_book_ids, source='citics_2019', level=1, date=None, market='cn')`

通过order_book_id 传入，拿到某个日期的该股票指定的行业分类

### 参数

| 参数             | 类型        | 说明                                                         |
| :--------------- | :---------- | :----------------------------------------------------------- |
| `order_book_id`  | `str or list` | 股票合约代码                                                 |
| `date`           | `str`       | 查询日期，默认为当前最新日期                                 |
| `source`         | `str`       | 分类依据。`citics_2019` - 中信新分类（2019 发布）, `citics` - 中信旧分类（退役中）, `gildata` -聚源。 默认`source='citics_2019'`. |
| `level`          | `integer`   | 行业分类级别，共三级，默认返回一级分类。参数0,1,2,3 一一对应，其中0 返回三级分类完整情况当`source='citics_2019'` 时，`level` 可传入'citics_sector' 获取该股票的衍生板块及风格归属 |

### 返回

所属合约的对应行业分类。

`pandas DataFrame`

### 范例

得到当前股票所对应的一级行业：

```
[In] get_instrument_industry('000001.XSHE')
[Out] first_industry_code first_industry_name order_book_id
000001.XSHE 40 银行
```

得到当前股票组所对应的中信行业的全部分类：

```
In [7]: get_instrument_industry(['000001.XSHE','000002.XSHE'],source='citics_2019',level=0)
Out[7]: first_industry_code first_industry_name second_industry_code second_industry_name third_industry_code third_industry_name order_book_id
000001.XSHE 40 银行4020 全国性股份制银行Ⅱ 402010 全国性股份制银行Ⅲ
000002.XSHE 42 房地产4210 房地产开发和运营421010 住宅物业开发
```

得到当前股票组所对应的中信2019 衍生板块及风格归属：

```
[In]: get_instrument_industry(['000001.XSHE','000002.XSHE'],source='citics_2019',level='citics_sector')
[Out]: industry_sector_name industry_chain_sector_name style_sector_name order_book_id
000001.XSHE 金融产业 大金融 金融风格
000002.XSHE 基础设施与地产产业大金融 金融风格
```

## get_turnover_rate - 获取历史换手率

`get_turnover_rate(order_book_ids, start_date, end_date, fields=None,expect_df=True)`

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str list`                        | 合约代码，可输入order_book_id, order_book_id list          |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，用户必须指定                                       |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，用户必须指定                                       |
| `fields`         | `str OR str list`                        | 默认为所有字段。当天换手率- `today` ，过去一周平均换手率- `week` ，过去一个月平均换手率- `month` ，过去一年平均换手率- `year` ，当年平均换手率- `current_year` |
| `expect_df`      | `boolean`                                | 默认返回pandas dataframe。如果调为False，则返回原有的数据结构 |

### 返回

`pandas DataFrame`

### 范例

获取平安银行历史换手率情况

```
In [17]: get_turnover_rate('000001.XSHE',20160801,20160806)
Out[17]: today week month year current_year order_book_id tradedate
000001.XSHE 2016-08-01 0.5190 0.4033 0.3175 0.5027 0.3585
2016-08-02 0.3070 0.4243 0.3206 0.5019 0.3581
2016-08-03 0.2902 0.4104 0.3193 0.5011 0.3576
2016-08-04 0.9189 0.4703 0.3443 0.5000 0.3615
2016-08-05 0.4962 0.4984 0.3476 0.4993 0.3624
```

获取平安银行与中信银行一段时间内的周平均换手率

```
[In] get_turnover_rate(['000001.XSHE', '601998.XSHG'], '20160801', '20160812', 'week')
[Out] week order_book_id tradedate
000001.XSHE 2016-08-01 0.4033
2016-08-02 0.4243
601998.XSHG 2016-08-01 0.1184
2016-08-02 0.1113
```

## get_dividend_info - 获取股票的分红信息

`get_dividend_info(order_book_ids, start_date=None, end_date=None, market='cn')`

获取某只股票在一段时间内的分红情况（包含起止日期）。如未指定日期，则默认所有。目前仅支持中国市场。

### 参数

| 参数             | 类型                                     | 说明                                         |
| :--------------- | :--------------------------------------- | :------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码                                     |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                     |
| `market`         | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场   |

### 返回

单只股票

`pandas single-index DataFrame` - 查询时间段内的某个股票的分红数据

一组股票

`pandas multi-index DataFrame` - 查询时间段内的一组股票的分红数据

| 字段             | 类型  | 说明                                                         |
| :--------------- | :---- | :----------------------------------------------------------- |
| `info_date`      | `str` | 公布日期                                                     |
| `effective_date` | `str` | 常规分红对应的有效财政季度；特殊分红则对应股权登记日         |
| `dividend_type`  | `str` | 是否分红及具体分红形式: `transferred share` 代表转增股份；`bonus share` 代表赠送股份；`cash` 为现金；`cash and share` 代表现金、转增股和送股都有涉及。 |
| `ex_dividend_date` | `str` | 除权除息日，该天股票的价格会因为分红而进行调整               |

### 范例

获取平安银行的历史分红信息：

```
[In] get_dividend_info('000001.XSHE')
[Out] dividend_type ex_dividend_date info_date order_book_id effective_date
1990-12-31 cash and bonus share 1991-04-03 1991-02-10 000001.XSHE
1991-12-31 cash and bonus share 1992-03-23 1992-03-14 000001.XSHE
1992-12-31 cash and share 1993-05-24 1993-05-07 000001.XSHE
1993-12-31 cash and share 1994-07-11 1994-07-02 000001.XSHE
1994-12-31 cash and bonus share 1995-09-25 1995-09-15 000001.XSHE
1995-12-31 bonus and transferred share 1996-05-27 1996-05-23 000001.XSHE
...
```

## get_dividend - 获取股票现金分红数据

`get_dividend(order_book_ids, start_date=None, end_date=None, market='cn')`

获取某只股票在一段时间内的现金分红情况（包含起止日期，以分红宣布日为查询基准）。如未指定日期，则默认所有。目前仅支持中国市场。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码                                                     |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `market`         | `str`                                    | 默认是中国内地市场('cn')。cn-中国内地市场，hk-中国香港市场 |

### 返回

单只股票

`pandas single-index DataFrame` - 查询时间段内的某个股票的现金分红数据

一组股票

`pandas multi-index DataFrame` - 查询时间段内的一组股票的现金分红数据

| 字段                          | 类型    | 说明                                                         |
| :---------------------------- | :------ | :----------------------------------------------------------- |
| `declaration_announcement_date` | `str`   | 分红宣布日，上市公司一般会提前一段时间公布未来的分红派息事件 |
| `book_closure_date`           | `str`   | 股权登记日                                                   |
| `dividend_cash_before_tax`    | `float` | 税前分红                                                     |
| `ex_dividend_date`            | `str`   | 除权除息日，该天股票的价格会因为分红而进行调整               |
| `payable_date`                | `str`   | 分红到帐日，这一天最终分红的现金会到账                       |
| `round_lot`                   | `float` | 分红最小单位，例如：10 代表每10 股派发`dividend_cash_before_tax` 单位的税前现金 |
| `advance_date`                | `str`   | 股东会日期                                                   |
| `quarter`                     | `str`   | 报告期                                                       |

### 范例

获取平安银行2013-01-04 到2014-01-06 的现金分红数据：

```
[In] get_dividend('000001.XSHE', start_date='20130104', end_date='20140106')
[Out] book_closure_date dividend_cash_before_tax \
declaration_announcement_date 2013-06-14 2013-06-19 1.7
ex_dividend_date payable_date round_lot \
declaration_announcement_date 2013-06-14 2013-06-20 2013-06-20 10.0
advance_date quarter
declaration_announcement_date 2013-06-14 2013-03-08 2012q4
```

## get_dividend_amount - 获取股票分红总额数据

`get_dividend_amount(order_book_ids, start_quarter = None, end_quarter = None, date = None, market = 'cn')`

获取股票历年分红总额数据。目前仅支持中国市场。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码                                                     |
| `start_quarter`  | `str`                                    | 起始报告期，默认返回全部。 传入样例'2023q4'期                |
| `end_quarter`    | `str`                                    | 截止报告期，默认返回全部。 传入样例'2023q4'期                |
| `date`           | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询日期，默认值为当前最新日期                               |
| `market`         | `str`                                    | 默认是中国内地市场('cn')。cn-中国内地市场，hk-中国香港市场 |

### 返回

单只股票

`pandas single-index DataFrame` - 查询时间段内的某个股票的分红总额数据

一组股票

`pandas multi-index DataFrame` - 查询时间段内的一组股票的分红总额数据

| 字段            | 类型    | 说明       |
| :-------------- | :------ | :--------- |
| `event_procedure` | `str`   | 事件进程。预案，决案，方案实施 |
| `info_date`     | `str`   | 公告日期   |
| `amount`        | `float` | 分红总额   |

### 范例

获取平安银行有史以来现金分红总额数据：

```
[In] rqdatac.get_dividend_amount('000001.XSHE')
[Out] event_procedure info_date amount order_book_id quarter
000001.XSHE 2018q4 预案 2019-03-07 2.489710e+09
2018q4 决案 2019-05-31 2.489710e+09
2018q4 方案实施 2019-06-20 2.489710e+09
2019q4 预案 2020-02-14 4.230000e+09
2019q4 决案 2020-05-15 4.230000e+09
2019q4 方案实施 2020-05-22 4.230490e+09
2020q4 预案 2021-02-02 3.493000e+09
2020q4 决案 2021-04-09 3.493000e+09
2020q4 方案实施 2021-05-07 3.493065e+09
2021q4 预案 2022-03-10 4.425000e+09
2021q4 决案 2022-06-29 4.425000e+09
2021q4 方案实施 2022-07-15 4.424549e+09
2022q4 预案 2023-03-09 5.530687e+09
2022q4 决案 2023-06-01 5.530687e+09
2022q4 方案实施 2023-06-07 5.530687e+09
2023q4 预案 2024-03-15 1.395286e+10
2023q4 决案 2024-05-25 1.395286e+10
2023q4 方案实施 2024-06-06 1.395286e+10
```

## get_split - 获取股票拆分数据

`get_split(order_book_ids, start_date=None, end_date=None, market='cn')`

获取某只股票在一段时间内的拆分情况（包含起止日期，以股权登记日为查询基准），如未指定日期，则默认所有。目前仅支持中国市场。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码                                                     |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `market`         | `str`                                    | 默认是中国内地市场('cn')。cn-中国内地市场，hk-中国香港市场 |

### 返回

单只股票

`pandas single-index DataFrame` - 查询时间段内的某个股票的拆分数据

一组股票

`pandas multi-index DataFrame` - 查询时间段内的一组股票的拆分数据

| 字段                      | 类型    | 说明                                                         |
| :------------------------ | :------ | :----------------------------------------------------------- |
| `ex_dividend_date`        | `str`   | 除权除息日，该天股票的价格会因为拆分而进行调整               |
| `book_closure_date`       | `str`   | 股权登记日                                                   |
| `split_coefficient_from`  | `float` | 拆分因子（拆分前）                                           |
| `split_coefficient_to`    | `float` | 拆分因子（拆分后）                                           |
| `payable_date`            | `str`   | 送转股上市日                                                 |
| `cum_factor`              | `float` | 累计复权因子（拆分） 例如：每10 股转增2 股，则`split_coefficient_from = 10`, `split_coefficient_to = 12`. |

### 范例

获取平安银行2010-01-04 到当天之间的拆分信息：

```
[In] get_split('000001.XSHE', start_date='20100104', end_date='20140104')
[Out] book_closure_date order_book_id payable_date split_coefficient_from split_coefficient_to cum_factor ex_dividend_date
2013-06-20 2013-06-19 000001.XSHE 2013-06-20 10 16.0 1.6
```

## get_ex_factor - 获取复权因子

`get_ex_factor(order_book_ids, start_date=None, end_date=None, market='cn')`

获取某只股票在一段时间内的复权因子（包含起止日期，以除权除息日为查询基准）。如未指定日期，则默认所有。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码                                                     |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `market`         | `str`                                    | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场；'hk' - 香港市场 |

### 返回

`pandas dataframe` - 包含了复权因子的日期和对应的各项数值

| 字段              | 类型    | 说明                                                         |
| :---------------- | :------ | :----------------------------------------------------------- |
| `ex_date`         | `str`   | 除权除息日                                                   |
| `ex_factor`       | `float` | 复权因子，考虑了分红派息与拆分的影响，为一段时间内的股价调整乘数。 举例来说，平安银行（'000001.XSHE'）在2016 年6 月15 日每10 股派发现金股利人民币1.53 元（含税），并以资本公积转增股本每10 股转增2 股。 6 月15 日的收盘价为10.44 元，其除权除息后的价格应当为(10.44-1.53/10) / 1.2 = 8.5725.本期复权因子为10.44 / 8.5725 = 1.217847 |
| `ex_cum_factor`   | `float` | 累计复权因子，X 日所在期复权因子= 当前最新累计复权因子/ 截至X 日最新累计复权因子。 举例来说，2016 年5 月05 日所在期复权因子= 122.424143 / 100.525060 = 1.217847 |
| `announcement_date` | `str`   | 股权登记日                                                   |
| `ex_end_date`     | `str`   | 复权因子所在期的截止日期                                     |

### 范例

```
[In] get_ex_factor('000001.XSHE', start_date='2013-01-04', end_date='2017-01-04')
[Out] order_book_id ex_factor ex_cum_factor announcement_date \
ex_date
2013-06-20 000001.XSHE 1.614263 68.255824 2013-06-19
2014-06-12 000001.XSHE 1.216523 83.034780 2014-06-11
2015-04-13 000001.XSHE 1.210638 100.525060 2015-04-10
2016-06-16 000001.XSHE 1.217847 122.424143 2016-06-15
ex_end_date
ex_date
2013-06-20 2014-06-11
2014-06-12 2015-04-12
2015-04-13 2016-06-15
2016-06-16 NaT
```

## is_suspended - 判断股票是否全天停牌

`is_suspended(order_book_ids, start_date=None, end_date=None, market='cn')`

判断某只股票在一段时间（包含起止日期）是否全天停牌。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码。传入单只或多支股票的order_book_id                |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为股票上市日期                                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期，如果股票已经退市，则为退市日期     |
| `market`         | `str`                                    | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场；'hk' - 香港市场 |

### 返回

`pandas DataFrame`

如果在查询期间内股票尚未上市，或已经退市，则函数返回None；如果开始日期早于股票上市日期，则以股票上市日期作为开始日期。

### 范例

获取武钢股份从2016 年6 月24 日至今（2016 年8 月31 日）的停牌情况：

```
[In] is_suspended('武钢股份', start_date='20160624')
[Out] 武钢股份
2016-06-24 False
2016-06-27 True
2016-06-28 True
2016-06-29 True
2016-06-30 True
2016-07-01 True
2016-07-04 True
2016-07-05 True
2016-07-06 True
...
2016-08-30 True
2016-08-31 True
[In] is_suspended(['武钢股份','000001.XSHE'], start_date='20160624')
[Out] 000001.XSHE 600005.XSHG
2016-06-24 False False
2016-06-27 False True
2016-06-28 False True
2016-06-29 False True
2016-06-30 False True
2016-07-01 False True
2016-07-04 False True
...
2016-09-22 False True
2016-09-23 False True
```

## is_st_stock - 查询股票是否为ST 股

`is_st_stock(order_book_ids, start_date, end_date)`

判断一只或多只股票在一段时间（包含起止日期）内是否为ST 股。

ST 股包括如下:

*   `S*ST` - 公司经营连续三年亏损，退市预警+还没有完成股改;
*   `*ST` - 公司经营连续三年亏损，退市预警;
*   `ST` - 公司经营连续二年亏损，特别处理;
*   `SST` - 公司经营连续二年亏损，特别处理+还没有完成股改;

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str list`                        | 合约代码，可传入order_book_id, order_book_id list          |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |

### 返回

`pandas DataFrame` - 查询时间段内是否为ST 股的查询结果

### 范例

```
[In] is_st_stock("002336.XSHE", "20160411", "20160510")
[Out] 002336.XSHE
2016-04-11 False
2016-04-12 False
...
2016-05-09 True
2016-05-10 True
[In] is_st_stock(["002336.XSHE", "000001.XSHE"], "2016-04-11", "2016-05-10")
[Out] 002336.XSHE 000001.XSHE
2016-04-11 False False
2016-04-12 False False
...
2016-05-09 True False
2016-05-10 True False
```

## get_shares - 获取流通股信息

`get_shares(order_book_ids, start_date='2013-01-04', end_date='2014-01-04', fields=None, market='cn', expect_df=True)`

获取某只股票在一段时间内的流通情况（包含起止日期）。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str OR str list`                        | 可输入order_book_id 或symbol                               |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为'2013-01-04'                                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为'2014-01-04'                                 |
| `fields`         | `str OR str list`                        | 默认为所有字段。见下方列表                                   |
| `market`         | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场                   |
| `expect_df`      | `boolean`                                | 默认返回pandas dataframe,如果调为False ,则返回原有的数据结构 |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `total` 总股本
*   `circulation_a` 流通A 股
*   `non_circulation_a` 非流通A 股
*   `total_a` A 股总股本
*   `free_circulation` 自由流通股本（提供范围为2005 年至今）
*   `preferred_shares` 优先股

### 范例

获取平安银行流通股概况

```
[In] get_shares('000001.XSHE', start_date='20160801', end_date='20160806',expect_df=False)
[Out] circulation_a non_circulation_a total_a free_circulation preferred_shares total date
2016-08-01 1.463118e+10 2.539231e+09 1.717041e+10 7.220546e+09 0.0 1.717041e+10
2016-08-02 1.463118e+10 2.539231e+09 1.717041e+10 7.220546e+09 0.0 1.717041e+10
2016-08-03 1.463118e+10 2.539231e+09 1.717041e+10 7.220546e+09 0.0 1.717041e+10
2016-08-04 1.463118e+10 2.539231e+09 1.717041e+10 7.220546e+09 0.0 1.717041e+10
2016-08-05 1.463118e+10 2.539231e+09 1.717041e+10 7.220546e+09 0.0 1.717041e+10
```

获取平安银行总股本数据

```
[In] get_shares('000001.XSHE', start_date='20160801', end_date='20160806', fields='total')
[Out] total order_book_id date
000001.XSHE 2016-08-01 1.717041e+10
2016-08-02 1.717041e+10
2016-08-03 1.717041e+10
2016-08-04 1.717041e+10
2016-08-05 1.717041e+10
```

## get_main_shareholder - 获取主要A 股股东信息

`get_main_shareholder(order_book_ids, start_date=None, end_date=None, start_rank=None,end_rank=None,is_total=False, market='cn')`

获取A 股主要股东构成及持流通A 股数量比例、持股性质等信息，通常为前十位。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str_list`                        | 合约代码                                                     |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为去年当日。                                   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为查询当日。                                   |
| `start_rank`     | `int`                                    | 排名开始值                                                   |
| `end_rank`       | `int`                                    | 排名结束值,`start_rank` ,`end_rank` 不传参时返回全部的十位股东名单 |
| `is_total`       | `bool`                                   | 默认为False, 即基于持有A 股流通股。若为True 则基于所有发行出的A 股。 |
| `market`         | `str`                                    | 市场，默认'cn'为中国内地市场。                               |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `info_date` 公告发布日
*   `end_date` 截止日期
*   `rank` 排名
*   `shareholder_name` 股东名称
*   `shareholder_attr` 股东属性
*   `shareholder_kind` 股东性质
*   `shareholder_type` 股东类别
*   `hold_percent_total` 占股比例（%） 当`fields='total'`时，持股数(股)/总股本*100。
*   `hold_percent_float` 占流通A 股比例（%）,无限售流通A 股/已上市流通A 股（不含高管股）*100
*   `share_pledge` 股权质押涉及股数（股）
*   `share_freeze` 股权冻结涉及股数（股）

### 范例

获取平安银行在2018 年三月上旬的主要的A 股股东名单

```
[In] get_main_shareholder('000001.XSHE', start_date='20180301', end_date='20180315', is_total=False)
[Out] end_date rank shareholder_name shareholder_attr shareholder_kind shareholder_type hold_percent_total hold_percent_float share_pledge share_freeze info_date
2018-03-15 2017-12-31 1 中国平安保险(集团)股份有限公司-集团本级-自有资金 企业 金融机构—保险公司 None 48.095791 48.813413 NaN NaN
2018-03-15 2017-12-31 2 中国平安人寿保险股份有限公司-自有资金 企业 金融机构—保险公司 None 6.112042 6.203238 NaN NaN
2018-03-15 2017-12-31 3 中国证券金融股份有限公司 企业 金融机构—证券、信托公司 None 2.854768 2.897363 NaN NaN
2018-03-15 2017-12-31 4 中国平安人寿保险股份有限公司-传统-普通保险产品 证券品种 保险投资组合 None 2.269811 2.303679 NaN NaN
2018-03-15 2017-12-31 5 香港中央结算有限公司 企业 外资独资企业 None 2.124405 2.156103 NaN NaN
2018-03-15 2017-12-31 6 中央汇金资产管理有限责任公司 企业 资产管理公司 None 1.259219 1.278007 NaN NaN
2018-03-15 2017-12-31 7 深圳中电投资股份有限公司 企业 投资、咨询公司 None 1.083561 1.099729 NaN NaN
2018-03-15 2017-12-31 8 河南鸿宝集团有限公司 企业 一般企业 None 0.459273 0.466125 NaN NaN
2018-03-15 2017-12-31 9 南方基金-农业银行-南方中证金融资产管理计划 证券品种 基金专户理财 None 0.336683 0.341707 NaN NaN
2018-03-15 2017-12-31 10 新华人寿保险股份有限公司-分红-个人分红-018L-FH002深 证券品种 保险投资组合 None 0.311545 0.316193 NaN NaN
```

## get_private_placement - 获取A 股股票定向增发信息

`get_private_placement(order_book_ids, start_date=None, end_date=None, progress='complete',issue_type='private', market='cn')`

获取A 股股票在一段时间内的定向增发信息（包含起止日期，以公告发布日为查询基准）。如未指定日期，则默认所有。

### 参数

| 参数           | 类型                                     | 说明                                                         |
| :------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_id` | `str`                                    | 给出单个order_book_id。                                      |
| `start_date`   | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `progress`     | `str`                                    | 是否已完成定增，默认为complete。可选参数["complete", "incomplete", "all"] |
| `issue_type`   | `str`                                    | 发行方式，默认为private。可选参数["private", "public", "all"] |
| `market`       | `str`                                    | 市场，默认'cn'为中国内地市场。                               |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `initial_info_date` 公告发布日
*   `csrc_approval_date` 证监会批准日
*   `issue_price` 定增发行价
*   `issue_type` 发行方式
*   `issued_shares` 发行股数
*   `listed_date` 上市日期
*   `progress` 目前进度

### 范例

获取平安银行非公开发行实施完成的定增数据

```
[In] get_private_placement("000001.XSHE")
[Out] csrc_approval_date issue_price issue_type issued_shares listed_date progress order_book_id initial_info_date
000001.XSHE 2009-06-13 2010-06-29 18.26 非公开发行3.795800e+08 2010-09-17 实施完成2010-09-02
2011-06-29 17.75 非公开发行1.638337e+09 2011-08-05 实施完成2013-09-09
2013-12-31 11.17 非公开发行1.323385e+09 2014-01-09 实施完成2014-07-16
2015-04-25 16.70 非公开发行5.988024e+08 2015-05-21 实施完成
```

## get_allotment - 获取A 股股票配股信息

`get_allotment(order_book_ids, start_date=None, end_date=None, market='cn')`

获取A 股股票在一段时间内的配股信息（包含起止日期，以首次信息发布日为查询基准）。如未指定日期，则默认所有。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `market`         | `str`                                    | 市场，默认'cn'为中国内地市场                                 |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `declaration_announcement_date` 首次信息发布日期
*   `proportion` 计划配股比例
*   `allotted_proportion` 实际配股比例
*   `allotted_shares` 实际配股数量(股)
*   `allotment_price` 每股配股价格(元)
*   `book_closure_date` 股权登记日
*   `ex_right_date` 除权除息日

### 范例

获取凯伦股份20180101 到20200101 的配股信息

```
[In] get_allotment('300715.XSHE','20180101','20200101')
[Out] proportion allotted_proportion allotted_shares \
order_book_id declaration_announcement_date
300715.XSHE 2019-04-19 0.3 0.29639 39074500.0
allotment_price book_closure_date ex_right_date
300715.XSHE 2019-04-19 12.64 2019-12-19 2019-12-30
```

## get_block_trade - 获取A 股大宗交易数据

`get_block_trade(order_book_ids, start_date, end_date)`

获取A 股大宗交易数据。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `price` 成交价
*   `volume` 成交量
*   `total_turnover` 成交额
*   `buyer` 买方营业部
*   `seller` 卖方营业部

### 范例

获取单个合约大宗交易数据

```
[In] rqdatac.get_block_trade('000001.XSHE','20190101','20191010')
[Out] price volume total_turnover buyer seller order_book_id trade_date
000001.XSHE 2019-02-28 11.16 289300 3.228588e+06 广发证券股份有限公司汕头珠池路证券营业部中信证券股份有限公司汕头海滨路证券营业部
2019-05-06 12.47 36000000 4.489200e+08 华泰证券股份有限公司河南分公司华泰证券股份有限公司河南分公司
2019-05-07 11.58 33400000 3.867720e+08 华泰证券股份有限公司河南分公司华泰证券股份有限公司河南分公司
2019-05-08 11.66 28314899 3.301517e+08 申万宏源证券有限公司河南分公司中国银河证券股份有限公司郑州经三路证券营业部
2019-05-20 12.38 7362200 9.114404e+07 机构专用机构专用
2019-07-10 13.56 610000 8.271600e+06 机构专用机构专用
2019-08-15 14.67 763800 1.120495e+07 申万宏源证券有限公司上海第二分公司中信证券股份有限公司广州花城大道证券营业部
2019-08-19 14.50 1581699 2.293464e+07 申万宏源证券有限公司上海第二分公司天风证券股份有限公司广州华夏路证券营业部
2019-09-24 13.84 216000 2.989440e+06 天风证券股份有限公司深圳卓越城证券营业部天风证券股份有限公司深圳卓越城证券营业部
2019-09-24 15.03 135000 2.029050e+06 东兴证券股份有限公司上海肇嘉浜路证券营业部机构专用
2019-09-25 13.66 240000 3.278400e+06 天风证券股份有限公司深圳卓越城证券营业部天风证券股份有限公司深圳卓越城证券营业部
```

获取多个合约大宗交易数据

```
[In] rqdatac.get_block_trade(['000001.XSHE','000046.XSHE'],'20190101','20191010')
[Out] price volume total_turnover buyer seller order_book_id trade_date
000001.XSHE 2019-02-28 11.16 289300 3.228588e+06 广发证券股份有限公司汕头珠池路证券营业部中信证券股份有限公司汕头海滨路证券营业部
2019-05-06 12.47 36000000 4.489200e+08 华泰证券股份有限公司河南分公司华泰证券股份有限公司河南分公司
2019-05-07 11.58 33400000 3.867720e+08 华泰证券股份有限公司河南分公司华泰证券股份有限公司河南分公司
2019-05-08 11.66 28314899 3.301517e+08 申万宏源证券有限公司河南分公司中国银河证券股份有限公司郑州经三路证券营业部
2019-05-20 12.38 7362200 9.114404e+07 机构专用机构专用
2019-07-10 13.56 610000 8.271600e+06 机构专用机构专用
2019-08-15 14.67 763800 1.120495e+07 申万宏源证券有限公司上海第二分公司中信证券股份有限公司广州花城大道证券营业部
2019-08-19 14.50 1581699 2.293464e+07 申万宏源证券有限公司上海第二分公司天风证券股份有限公司广州华夏路证券营业部
2019-09-24 13.84 216000 2.989440e+06 天风证券股份有限公司深圳卓越城证券营业部天风证券股份有限公司深圳卓越城证券营业部
2019-09-24 15.03 135000 2.029050e+06 东兴证券股份有限公司上海肇嘉浜路证券营业部机构专用
2019-09-25 13.66 240000 3.278400e+06 天风证券股份有限公司深圳卓越城证券营业部天风证券股份有限公司深圳卓越城证券营业部
000046.XSHE 2019-09-27 3.91 44139500 1.725854e+08 中信证券股份有限公司天津大港证券交易营业部申万宏源证券有限公司温州车站大道证券营业部
```

## get_symbol_change_info - 获取合约的历史简称信息

`get_symbol_change_info(order_book_id)`

获取A 股合约简称变更信息。

### 参数

| 参数             | 类型     | 说明         |
| :--------------- | :------- | :----------- |
| `order_book_ids` | `str or list` | 给出单个或多个order_book_id |

### 返回

`pandas DataFrame`

| 字段        | 说明         |
| :---------- | :----------- |
| `info_date` | 信息发布日期   |
| `change_date` | 简称变更日期   |
| `symbol`    | 证券简称     |

### 范例

获取单个合约简称变更数据

```
[In] rqdatac.get_symbol_change_info('000001.XSHE')
[Out] info_date symbol order_book_id change_date
000001.XSHE 1991-04-03 1991-04-03 深发展Ａ
2006-10-09 2006-09-28 S深发展A
2007-06-20 2007-06-14 深发展Ａ
2012-08-02 2012-01-20 平安银行
```

## get_special_treatment_info - 获取合约特殊处理状态信息

`get_special_treatment_info(order_book_id)`

获取A 股合约特殊处理状态信息。

### 参数

| 参数             | 类型     | 说明         |
| :--------------- | :------- | :----------- |
| `order_book_ids` | `str or list` | 给出单个或多个order_book_id |

### 返回

`pandas DataFrame`

| 字段          | 说明                       |
| :------------ | :------------------------- |
| `info_date`   | 信息发布日期               |
| `change_date` | 特别处理(或撤销)实施日期   |
| `symbol`      | 证券简称                   |
| `type`        | 特别处理(或撤销)类别       |
| `description` | 特别处理(或撤销)事项描述   |

### 范例

获取单个合约特殊处理状态数据

```
[In] rqdatac.get_special_treatment_info('000020.XSHE')
[Out] info_date symbol type description order_book_id change_date
000020.XSHE 1999-04-27 1999-04-24 ST华发Ａ ST
2000-03-29 2000-03-28 深华发Ａ 撤销ST
2004-04-27 2004-04-26 ST华发Ａ ST None
2005-04-29 2005-04-28 *ST华发Ａ 从ST变为*ST None
2006-11-22 2006-11-21 SST华发A 撤消*ST并实行ST None
2009-05-19 2009-05-18 深华发Ａ 撤销ST None
```

## current_freefloat_turnover - 获取当日累计自由流通换手率

`rqdatac.current_freefloat_turnover(order_book_ids)`

获取A 股合约当日累计自由流通换手率数据

### 参数

| 参数             | 类型     | 说明         |
| :--------------- | :------- | :----------- |
| `order_book_ids` | `str or list` | 给出单个或多个order_book_id |

### 返回

`pandas Series`

| 字段          | 类型    | 说明                                                         |
| :------------ | :------ | :----------------------------------------------------------- |
| `自由流通换手率` | `float` | 截至到调用时间的当日累计自由流通换手率自由流通换手率=当日累计成交金额/自由流通市值（盘中实时分钟级别） |

### 范例

获取多个合约当日累计自由流通换手率

```
[In] rqdatac.current_freefloat_turnover(['000001.XSHE','600000.XSHG'])
[Out] 000001.XSHE 0.007206
600000.XSHG 0.002283
dtype: float64
```

## get_holder_number - 获取股东户数

`rqdatac.get_holder_number(order_book_ids, start_date, end_date,market='cn')`

获取A 股股东户数数据

### 参数

| 参数             | 类型                                     | 说明                                         |
| :--------------- | :--------------------------------------- | :------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为去年当日                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为去年当日                     |
| `market`         | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场   |

### 返回

`pandas Series`

| 字段                      | 类型         | 说明               |
| :------------------------ | :----------- | :----------------- |
| `order_book_ids`          | `str`        | 合约代码           |
| `info_date`               | `datetime.date` | 发布日期           |
| `end_date`                | `datetime.date` | 截止日期           |
| `share_holders`           | `float`      | 股东总户数(户)     |
| `avg_share_holders`       | `float`      | 户均持股数(股/户)  |
| `a_share_holders`         | `float`      | A 股股东户数(户)   |
| `avg_a_share_holders`     | `float`      | A 股股东户均持股数(股/户) |
| `avg_circulation_share_holders` | `float`      | 无限售A 股股东户均持股数(股/户) |

### 范例

获取一个合约最近一年的股东户数数据

```
[In] rqdatac.get_holder_number('000001.XSHE')
[Out] end_date share_holders a_share_holders avg_circulation_share_holders avg_share_holders avg_a_share_holders order_book_id info_date
000001.XSHE 2023-03-09 2022-12-31 487200.0 487200.0 39830.0 39831.52 39831.52
2023-03-09 2023-02-28 477304.0 477304.0 40656.0 40657.36 40657.36
2023-04-25 2023-03-31 506867.0 506867.0 38285.0 38286.02 38286.02
2023-08-24 2023-06-30 536701.0 536701.0 36157.0 36157.78 36157.78
2023-10-25 2023-09-30 530229.0 530229.0 36598.0 36599.13 36599.13
```

## get_abnormal_stocks - 获取龙虎榜每日明细

`rqdatac.get_abnormal_stocks(start_date=None, end_date=None,types=None,market='cn')`

获取A 股龙虎榜每日明细数据

### 参数

| 参数         | 类型                                     | 说明                                                         |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为去年当日                                     |
| `end_date`   | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为去年当日                                     |
| `types`      | `str`                                    | 异动类型。具体类型及描述见异动类型代码及其对应原因 默认返回全部 |
| `market`     | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场                   |

### 返回

`pandas DataFrame`

| 字段              | 类型       | 说明           |
| :---------------- | :--------- | :------------- |
| `order_book_ids`  | `str`      | 合约代码       |
| `date`            | `datetime` | 日期           |
| `type`            | `str`      | 异动类型       |
| `abnormal_s_date` | `datetime.date` | 异动起始日期   |
| `abnormal_e_date` | `datetime.date` | 异动截至日期   |
| `volume`          | `float`    | 成交量         |
| `total_turnover`  | `float`    | 成交额         |
| `change_rate`     | `float`    | 涨跌幅         |
| `turnover_rate`   | `float`    | 换手率         |
| `amplitude`       | `float`    | 振幅           |
| `deviation`       | `float`    | 涨跌幅偏离值   |
| `reason`          | `str`      | 异动类型名称，即上榜原因 |

### 范例

获取某一天的龙虎榜数据

```
[In] rqdatac.get_abnormal_stocks(20240606,20240606)
[Out] type abnormal_s_date abnormal_e_date volume total_turnover change_rate turnover_rate amplitude deviation reason order_book_id date
000037.XSHE 2024-06-06 U01 2024-06-06 2024-06-06 60760000.00 6.371700e+08 NaN NaN NaN 0.1168 日涨幅偏离值达7%
002579.XSHE 2024-06-06 U01 2024-06-06 2024-06-06 42820000.00 3.247400e+08 NaN NaN NaN 0.1168 日涨幅偏离值达7%
003008.XSHE 2024-06-06 U01 2024-06-06 2024-06-06 7440000.00 1.334500e+08 NaN NaN NaN 0.1168 日涨幅偏离值达7%
002356.XSHE 2024-06-06 U01 2024-06-06 2024-06-06 20870000.00 7.025000e+07 NaN NaN NaN 0.1168 日涨幅偏离值达7%
003026.XSHE 2024-06-06 U01 2024-06-06 2024-06-06 1240000.00 3.906000e+07 NaN NaN NaN 0.1168 日涨幅偏离值达7%
... ... ... ... ... ... ... ... ... ... ...
600647.XSHG 2024-06-06 L01 2024-06-06 2024-06-06 9118026.00 1.309210e+07 NaN NaN NaN NaN 退市整理
600766.XSHG 2024-06-06 L01 2024-06-06 2024-06-06 27377520.00 9.662712e+06 NaN NaN NaN NaN 退市整理
603133.XSHG 2024-06-06 L01 2024-06-06 2024-06-06 25534776.00 8.026108e+06 NaN NaN NaN NaN 退市整理
600306.XSHG 2024-06-06 L01 2024-06-06 2024-06-06 4969300.00 1.642356e+06 NaN NaN NaN NaN 退市整理
600220.XSHG 2024-06-06 N03 2024-05-22 2024-06-06 2229.17 1.513860e+03 NaN NaN NaN NaN 连续10个交易日内4次出现负向异常波动情形[77 rows x 10 columns]
```

## get_abnormal_stocks_detail - 获取龙虎榜机构交易明细数据

`rqdatac.get_abnormal_stocks_detail(order_book_ids,start_date=None,end_date=None,sides=None,types=None,market='cn')`

获取A 股龙虎榜机构交易明细数据

### 参数

| 参数         | 类型                                     | 说明                                                         |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为去年当日                                     |
| `end_date`   | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为去年当日                                     |
| `sides`      | `str`                                    | 买卖方向， 'buy'：买； 'sell'：卖； 'cum'：严重异常期间的累计数据。注意这里并不是指买卖方向的数据总和。 默认返回全部 |
| `types`      | `str`                                    | 异动类型。具体类型及描述见异动类型代码及其对应原因 默认返回全部 |
| `market`     | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场                   |

### 返回

`pandas DataFrame`

| 字段             | 类型       | 说明           |
| :--------------- | :--------- | :------------- |
| `order_book_ids` | `str`      | 合约代码       |
| `date`           | `datetime` | 日期           |
| `rank`           | `int`      | 排名           |
| `side`           | `str`      | 买卖方向       |
| `agency`         | `str`      | 营业部名称     |
| `buy_value`      | `float`    | 买入金额       |
| `sell_value`     | `float`    | 卖出金额       |
| `reason`         | `str`      | 异动类型名称，即上榜原因 |

### 范例

获取某一天的龙虎榜机构交易明细数据

```
[In] rqdatac.get_abnormal_stocks_detail('000037.XSHE',20240606,20240606)
[Out] side rank agency buy_value sell_value type reason order_book_id date
000037.XSHE 2024-06-06 buy 1 国泰君安证券股份有限公司宜昌珍珠路证券营业部19984430.00 145680.0 U01 日涨幅偏离值达7%
2024-06-06 buy 2 中泰证券股份有限公司湖北分公司15909115.00 0.0 U01 日涨幅偏离值达7%
2024-06-06 buy 3 东亚前海证券有限责任公司广东分公司11229691.00 0.0 U01 日涨幅偏离值达7%
2024-06-06 buy 4 中泰证券股份有限公司莱芜鲁中东大街证券营业部10631391.00 150130.0 U01 日涨幅偏离值达7%
2024-06-06 buy 5 华鑫证券有限责任公司深圳益田路证券营业部9847864.00 0.0 U01 日涨幅偏离值达7%
2024-06-06 sell 1 海通证券股份有限公司泰安迎胜东路证券营业部3703098.00 20295550.0 U01 日涨幅偏离值达7%
2024-06-06 sell 2 机构专用5370201.99 12796800.0 U01 日涨幅偏离值达7%
2024-06-06 sell 3 招商证券股份有限公司上海长柳路证券营业部1229167.00 6726082.0 U01 日涨幅偏离值达7%
2024-06-06 sell 4 海通证券股份有限公司上海黄浦区福州路证券营业部140537.00 5134030.0 U01 日涨幅偏离值达7%
2024-06-06 sell 5 机构专用2354284.00 5016705.0 U01 日涨幅偏离值达7%
```

## get_buy_back - 获取回购数据

`rqdatac.get_buy_back(order_book_ids,start_date=None, end_date=None,fields=None)`

获取A 股回购数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str list`                        | 合约代码，可传入order_book_id, order_book_id list          |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 起始日期，默认返回最近三个月数据                             |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认返回最近三个月数据                             |
| `fields`         | `str OR str list`                        | 字段名称，默认返回全部                                       |

### 返回

`pandas DataFrame`

| 字段                | 类型       | 说明                     |
| :------------------ | :--------- | :----------------------- |
| `seller`            | `str`      | 股份被回购方             |
| `procedure`         | `str`      | 事件进程                 |
| `share_type`        | `str`      | 股份类别                 |
| `annoucement_dt`    | `datetime` | 公告发布当天的日期时间戳 |
| `buy_back_start_date` | `datetime` | 回购期限起始日           |
| `buy_back_end_date` | `datetime` | 回购期限截至日           |
| `write_off_date`    | `datetime` | 回购注销公告日（该字段为空的时候代表这行记录尚未完成注销，有日期的时候代表已完成注销） |
| `maturity_desc`     | `str`      | 股份回购期限说明         |
| `buy_back_volume`   | `float`    | 回购股数(股)(份)         |
| `volume_ceiling`    | `float`    | 回购数量上限(股)(份)     |
| `volume_floor`      | `float`    | 回购数量下限(股)(份)     |
| `buy_back_value`    | `float`    | 回购总金额(元)           |
| `buy_back_price`    | `float`    | 回购价格(元/股)(元/份)   |
| `price_ceiling`     | `float`    | 回购价格上限(元)         |
| `price_floor`       | `float`    | 回购价格下限(元)         |
| `currency`          | `str`      | 货币单位                 |
| `purpose`           | `str`      | 回购目的                 |
| `buy_back_percent`  | `str`      | 占总股本比例             |
| `volume_floor`      | `float`    | 拟回购资金总额下限(元)   |
| `value_ceiling`     | `float`    | 拟回购资金总额上限(元)   |
| `buy_back_mode`     | `str`      | 股份回购方式             |

### 范例

获取某一天的回购数据

```
[In] rqdatac.get_buy_back('000026.XSHE',20200707,20200707)
[Out] seller procedure share_type ... value_floor value_ceiling buy_back_mode order_book_id date
000004.XSHE 2021-04-28 彭瀛等对象 实施完成 流通A股 ... 1.0 1.0 协议回购
1 rows × 21 columns
```

# 融资融券和南北向数据

## get_capital_flow - 获取股票资金流入流出

`get_capital_flow(order_book_ids, start_date=None, end_date=None, frequency='1d', market='cn')`

获取某只股票的资金流入流出信息。目前仅支持中国市场。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 合约代码，注意快照级别仅支持单个合约传入。                   |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期                                                     |
| `frequency`      | `str`                                    | 默认为'1d' 即单日级别，另支持'1m'和'tick'。目前不支持resample，即5d,5m 等分时图无内置 |
| `market`         | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场                   |

### 返回

`pandas multi-index DataFrame`

日线及分钟线返回的字段：

| 字段            | 类型      | 说明       |
| :-------------- | :-------- | :--------- |
| `order_book_id` | `str`     | 合约代码，索引一 |
| `date`          | `pandasTimestamp` | 时间，索引二   |
| `buy_volume`    | `integer` | 主动买的股数 |
| `buy_value`     | `integer` | 主动买的合计金额 |
| `sell_volume`   | `integer` | 主动卖的股数 |
| `sell_value`    | `integer` | 主动卖的合计金额 |

快照级别返回的字段：

| 字段            | 类型      | 说明       |
| :-------------- | :-------- | :--------- |
| `order_book_id` | `str`     | 合约代码，索引一 |
| `datetime`      | `pandasTimestamp` | 时间，索引二   |
| `direction`     | `integer` | 1 为主动买，-1 为主动卖 |
| `volume`        | `integer` | 变动股数   |
| `value`         | `integer` | 变动金额   |

其中，关于买卖方向的判断：

*   对于涨停，即卖一询价为空，买一非空，则为主动买
*   对于跌停，即买一询价为空，卖一非空，则为主动卖
*   如果，最新价>=上一笔的卖一询价，则为主动买
*   如果，最新价<=上一笔的买一询价，则为主动卖
*   否则，取前一笔的买卖方向

另，连续竞价撮合成当天第一笔交易的，成交价>=上一笔卖询价，为主动买，否则为主动卖。

该API 基于level 1 数据合成，所以暂且不对资金量（大中小）作主观分类。

### 范例

获取平安银行某日快照级别资金流入流出：

```
[In] get_capital_flow('000001.XSHE',start_date=20190412,end_date=20190412,frequency='tick')
[Out] direction value volume datetime
2019-04-12 09:25:03 1 4311404 319600
2019-04-12 09:26:03 1 0 0
2019-04-12 09:27:03 1 0 0
2019-04-12 09:28:03 1 0 0
2019-04-12 09:29:03 1 0 0
2019-04-12 09:30:00 1 0 0
2019-04-12 09:30:03 1 3472850 256860
2019-04-12 09:30:06 1 836686 61936
2019-04-12 09:30:09 -1 994734 73600
2019-04-12 09:30:12 -1 550366 40700
2019-04-12 09:30:15 -1 1002377 74200
...
```

获取多只股票日级别资金流入流出：

```
[In] get_capital_flow(['000001.XSHE','000002.XSHE'],start_date=20190412,end_date=20190415,frequency='1d')
[Out] buy_volume buy_value sell_volume sell_value order_book_id date
000001.XSHE 2019-04-12 42805075 572261719 34627389 462877954
2019-04-15 72481761 1008887497 80907307 1125821484
000002.XSHE 2019-04-12 22722286 708667739 25521391 795822317
2019-04-15 25321496 799505139 30459357 959805142
...
```

## current_capital_flow_minute - 获取最近的分钟资金流数据

`current_capital_flow_minute(order_book_ids)`

获取当日某只股票的最近一分钟资金流入流出信息，无法获取历史。

### 参数

| 参数             | 类型     | 说明     |
| :--------------- | :------- | :------- |
| `order_book_ids` | `str or list` | 合约代码 |

### 返回

`pandas multi-index DataFrame`

| 字段            | 类型      | 说明       |
| :-------------- | :-------- | :--------- |
| `order_book_id` | `str`     | 合约代码，索引一 |
| `date`          | `pandasTimestamp` | 时间，索引二   |
| `buy_volume`    | `integer` | 主动买的股数 |
| `buy_value`     | `integer` | 主动买的合计金额 |
| `sell_volume`   | `integer` | 主动卖的股数 |
| `sell_value`    | `integer` | 主动卖的合计金额 |

该API 基于level 1 数据合成，所以暂且不对资金量（大中小）作主观分类。

### 范例

获取某股票最近一分钟的资金流入流出：

```
[In] current_capital_flow_minute(['000001.XSHE','600000.XSHG'])
[Out] buy_volume buy_value sell_volume sell_value order_book_id datetime
000001.XSHE 2024-09-19 11:30:00 55400.0 542977.0 51500.0 504757.0
600000.XSHG 2024-09-19 11:30:00 28200.0 238271.0 9600.0 81092.0
```

## get_securities_margin - 获取融资融券信息

`get_securities_margin(order_book_ids, start_date=None, end_date=None, fields=None, expect_df=True)`

获取融资融券信息。包括深证融资融券数据以及上证融资融券数据情况。既包括个股数据，也包括市场整体数据。需要注意，融资融券的开始日期为2010 年3 月31 日;根据交易所的原始数据，上交所个股跟整个市场的输出信息列表不一致，个股没有融券余量金额跟融资融券余额两项, 而深交所个股跟整个市场的输出信息列表一致。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or str list`                        | 可输入order_book_id, order_book_id list。另外，输入'XSHG'或'sh'代表整个上证整体情况；'XSHE'或'sz'代表深证整体情况 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为当前最近日期前一个月                         |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前有数据的最新日期                         |
| `fields`         | `str or str list`                        | 默认为所有字段。见下方列表                                   |
| `expect_df`      | `boolean`                                | 默认返回pandas dataframe。如果调为False，则返回原有的数据结构 |

`fields` 字段名
*   `margin_balance` 融资余额
*   `buy_on_margin_value` 融资买入额
*   `margin_repayment` 融资偿还额
*   `short_balance` 融券余额
*   `short_balance_quantity` 融券余量
*   `short_sell_quantity` 融券卖出量
*   `short_repayment_quantity` 融券偿还量
*   `total_balance` 融资融券余额

### 返回

`pandas DataFrame`

### 范例

获取沪深两个市场一段时间内的融资余额

```
[In] get_securities_margin('510050.XSHG', start_date='20160801', end_date='20160805',expect_df=False)
[Out] margin_balance buy_on_margin_value short_sell_quantity margin_repayment short_balance_quantity short_repayment_quantity short_balance total_balance
2016-08-01 7.811396e+09 50012306.0 3597600.0 41652042.0 15020600.0 1645576.0 NaN NaN
2016-08-02 7.826381e+09 34518238.0 2375700.0 19532586.0 14154000.0 3242300.0 NaN NaN
2016-08-03 7.733306e+09 17967333.0 4719700.0 111043009.0 16235600.0 2638100.0 NaN NaN
2016-08-04 7.741497e+09 30259359.0 6488600.0 22068637.0 17499000.0 5225200.0 NaN NaN
2016-08-05 7.726343e+09 25270756.0 2865863.0 40423859.0 14252363.0 6112500.0 NaN NaN
```

获取沪深两个市场一段时间内的融资余额

```
[In] get_securities_margin(['XSHE', 'XSHG'],start_date='20160801', end_date='20160802', fields='margin_balance')
[Out] margin_balance order_book_id date
XSHE 2016-08-01 383762696120
2016-08-02 382892321734
XSHG 2016-08-01 476355670754
2016-08-02 476393053057
```

获取上证个股以及整个上证市场融资融券情况

```
[In] get_securities_margin(['XSHG', '601988.XSHG', '510050.XSHG'],start_date='20160801', end_date='20160805',expect_df=False)
[Out] <class 'pandas.core.panel.Panel'>
Dimensions: 8 (items) x 5 (major_axis) x 3 (minor_axis)
Items axis: margin_balance to total_balance
Major_axis axis: 2016-08-01 00:00:00 to 2016-08-05 00:00:00
Minor_axis axis: XSHG to 510050.XSHG
```

获取50ETF 融资偿还额情况

```
[In] get_securities_margin('510050.XSHG', start_date='20160801', end_date='20160805', fields='margin_repayment')
[Out] margin_repayment order_book_id date
510050.XSHG 2016-08-01 41652042
2016-08-02 19532586
2016-08-03 111043009
2016-08-04 22068637
2016-08-05 40423859
```

## get_margin_stocks - 获取融资融券股票列表

`get_margin_stocks(date=None, exchange=None,margin_type='stock',market='cn')`

获取某个日期深证、上证融资融券股票列表。

### 参数

| 参数          | 类型                                     | 说明                                                         |
| :------------ | :--------------------------------------- | :----------------------------------------------------------- |
| `date`        | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询日期，默认为今天上一交易日                               |
| `exchange`    | `str`                                    | 交易所，默认为None，返回所有字段。可选字段包括：'XSHE', 'sz' 代表深交所；'XSHG', 'sh' 代表上交所 |
| `margin_type` | `str`                                    | 'stock' 代表融券卖出，'cash'，代表融资买入，默认为'stock'    |
| `market`      | `str`                                    | 默认是中国市场('cn')，目前仅支持中国市场                   |

### 返回

`list` 证券列表。如果所查询日期没有融资融券股票列表，则返回空list

### 范例

获取沪深市场的融券卖出列表

```
[In] get_margin_stocks(date='20190819',exchange=None,margin_type='stock')
[Out] ['000001.XSHE', '000002.XSHE', '000006.XSHE', ...]
```

获取沪深市场融资买入列表

```
[In] get_margin_stocks(date='20190819',exchange=None,margin_type='cash')
[Out] ['000001.XSHE', '000002.XSHE', '000006.XSHE', ...]
```

获取深证融券卖出列表

```
[In] get_margin_stocks(date='20190819',exchange='XSHE',margin_type='stock')
[Out] ['000001.XSHE', '000002.XSHE', '000006.XSHE', ...]
```

获取上证融资买入列表

```
[In] get_margin_stocks(date='20190819',exchange='XSHG',margin_type='cash')
[Out] ['510050.XSHG', '510160.XSHG', '510180.XSHG', ...]
```

## get_stock_connect - 获取沪深股通持股信息

`get_stock_connect(order_book_ids, start_date='2017-03-17', end_date='2018-03-16', fields=None, expect_df=True)`

获取A 股股票在一段时间内的在香港上市交易的持股情况。

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str`                                    | 可输入order_book_id 或symbol。另， 1、输入'shanghai_connect'可返回沪股通的全部股票数据。 2、输入'shenzhen_connect'可返回深股通的全部股票数据。 3、输入'all_connect'可返回沪股通、深股通的全部股票数据。 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为'2017-03-17'                                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为'2018-03-16'                                 |
| `fields`         | `str OR str list`                        | 默认为所有字段。见下方列表                                   |
| `expect_df`      | `boolean`                                | 默认返回pandas dataframe。如果调为False，则返回原有的数据结构 |

### 返回

`pandas DataFrame`

`fields` 字段名
*   `shares_holding` 持股量
*   `holding_ratio` 持股比例
*   `adjusted_holding_ratio` 调整后持股比例

### 范例

获取德赛电池持股概况

```
[In] get_stock_connect('000049.XSHE',start_date='2018-05-08',end_date='2018-05-10')
[Out] shares_holding holding_ratio adjusted_holding_ratio order_book_id trading_date
000049.XSHE 2018-05-08 194295.0 0.09 0.0947
2018-05-09 144228.0 0.07 0.0703
2018-05-10 136628.0 0.06 0.0666
```

获取沪股通持股概况

```
[In] df = get_stock_connect('shanghai_connect',start_date='20180508',end_date='20180510',expect_df=True)
df.head()
[Out] shares_holding holding_ratio adjusted_holding_ratio order_book_id trading_date
600000.XSHG 2018-05-08 156945807.0 0.55 0.5585
2018-05-09 157301679.0 0.55 0.5597
2018-05-10 160277136.0 0.57 0.5703
600004.XSHG 2018-05-08 259814825.0 12.55 12.5556
2018-05-09 261758055.0 12.64 12.6495
```

## current_stock_connect_quota - 获取沪深港通实时每日额度数据

`current_stock_connect_quota(connect,fields)`

获取沪深港通每日额度数据

### 参数

| 参数      | 类型        | 说明                                                         |
| :-------- | :---------- | :----------------------------------------------------------- |
| `connect` | `str or str list` | 默认返回全部connect 1、输入输入'hk_to_sh'返回沪股通的额度信息。 2、输入'hk_to_sz'返回深股通的额度信息。 3、输入'sh_to_hk'返回港股通（上海）的额度信息。 4、输入'sz_to_hk'返回港股通（深圳）的额度信息 |
| `fields`  | `str or str list` | 默认为所有字段。见下方列表                                   |

### 返回

`fields` 字段名
*   `quota_balance` 余额
*   `quota_balance_ratio` 占比
*   `buy_turnover` 买方金额 1、沪股通和深股通单位为RMB ， 2、 港股通（上海). 港股通（ 深圳）单位为HKD
*   `sell_turnover` 卖方金额 1、沪股通和深股通单位为RMB ， 2、 港股通（上海). 港股通（ 深圳）单位为HKD

### 范例

```
[In] current_stock_connect_quota()
[Out] buy_turnover sell_turnover quota_balance quota_balance_ratio datetime connect
2020-05-26 16:10:00 sh_to_hk 5.463000e+09 3.548000e+09 3.969274e+10 0.945065
2020-05-26 15:01:00 hk_to_sh 1.115100e+10 1.015700e+10 5.024400e+10 0.960000
2020-05-26 16:10:00 sz_to_hk 5.474000e+09 3.178000e+09 3.926800e+10 0.934952
2020-05-26 15:01:00 hk_to_sz 1.803100e+10 1.513800e+10 4.847800e+10 0.930000
```

## get_stock_connect_quota - 获取沪深港通历史每日额度数据

`get_stock_connect_quota(connect,start_date,end_date,fields)`

获取沪深港通历史每日额度数据

### 参数

| 参数      | 类型        | 说明                                                         |
| :-------- | :---------- | :----------------------------------------------------------- |
| `connect` | `str or str list` | 默认返回全部connect 1、输入输入'hk_to_sh'返回沪股通的额度信息。 2、输入'hk_to_sz'返回深股通的额度信息。 3、输入'sh_to_hk'返回港股通（上海）的额度信息。 4、输入'sz_to_hk'返回港股通（深圳）的额度信息 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 默认为全部历史数据                                           |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 默认为最新日期                                               |
| `fields`  | `str or str list` | 默认为所有字段。见下方列表                                   |

### 返回

`fields` 字段名
*   `quota_balance` 余额
*   `quota_balance_ratio` 占比
*   `buy_turnover` 买方金额 1、沪股通和深股通单位为RMB ， 2、 港股通（上海). 港股通（ 深圳）单位为HKD
*   `sell_turnover` 卖方金额 1、沪股通和深股通单位为RMB ， 2、 港股通（上海). 港股通（ 深圳）单位为HKD

### 范例

获取指定时间段的深股通的额度信息

```
In [20]: get_stock_connect_quota(connect='hk_to_sz',start_date=20200101,end_date=20200401)
Out[20]: buy_turnover quota_balance quota_balance_ratio sell_turnover datetime connect
2020-01-02 hk_to_sz 4.353300e+09 4.018800e+12 0.956857 2.846830e+09
2020-01-03 hk_to_sz 3.477980e+09 4.079900e+12 0.971405 2.572800e+09
2020-01-06 hk_to_sz 3.737750e+09 4.094900e+12 0.974976 3.033440e+09
2020-01-07 hk_to_sz 3.248760e+09 4.076700e+12 0.970643 2.357280e+09
2020-01-08 hk_to_sz 3.299240e+09 4.114200e+12 0.979571 2.790880e+09
···
```

# 公告相关

## get_incentive_plan - 获取合约股权激励数据

`rqdatac.get_incentive_plan(order_book_ids)`

获取A 股合约股权激励数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期。注：如使用开始日期，则必填结束日期                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期。注：若使用结束日期，则开始日期必填                 |

### 返回

`pandas DataFrame`

| 字段             | 说明               |
| :--------------- | :----------------- |
| `info_date`      | 信息发布日期       |
| `effective_date` | 生效日期           |
| `first_info_date` | 首次信息发布日期   |
| `incentive_price` | 激励股票数量(股)   |
| `incentive_mode` | 激励模式           |
| `info_type`      | 公告类型，草案或者调整 |

### 范例

获取单个合约股权激励数据

```
[In] rqdatac.get_incentive_plan('002074.XSHE')
[Out] first_info_date effective_date shares_num incentive_price incentive_mode info_type order_book_id info_date
002074.XSHE 2021-08-28 2021-08-28 2021-08-28 29980000.0 39.30 股票期权草案
2022-04-29 2022-04-29 2022-04-29 60000000.0 18.77 股票期权草案
2022-07-09 2021-08-28 2022-07-09 29980000.0 39.20 股票期权调整
2022-07-09 2022-04-29 2022-07-09 59687500.0 18.67 股票期权调整
```

## get_investor_ra - 获取投资者关系活动数据

`rqdatac.get_investor_ra(order_book_id,start_date=None,end_date=None)`

获取A 股合约投资者关系活动数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期。注：如使用开始日期，则必填结束日期                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期。注：若使用结束日期，则开始日期必填                 |

### 返回

`pandas DataFrame`

| 字段          | 说明       |
| :------------ | :--------- |
| `info_date`   | 信息发布日期 |
| `participant` | 参与人员   |
| `institution` | 调研机构   |
| `detail`      | 与会描述   |

### 范例

获取单个合约投资者关系活动数据

```
[In] rqdatac.get_investor_ra('002507.XSHE')
[Out] participant institute detail order_book_id info_date
002507.XSHE 2012-08-15 唐桦博时基金None
2012-08-15 张延鹏朱雀投资None
2012-08-15 黄仕川西南证券None
2012-08-15 解睿平安证券None
2012-09-17 None None 吕耀子...
... ... ... ...
2022-08-29 陈硕旸长江证券None
2022-09-09 王亦沁鹏扬基金None
2022-09-09 沈瑞浦银安盛None
2022-09-09 赵钦国海富兰克林基金None
2022-09-09 王明明嘉实基金None
[592 rows x 3 columns]
```

## get_announcement - 获取公司公告数据

`rqdatac.get_announcement(order_book_ids,start_date,end_date)`

获取A 股合约公司公告数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期。注：如使用开始日期，则必填结束日期                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期。注：若使用结束日期，则开始日期必填                 |

### 返回

`pandas Series`

| 字段              | 类型         | 说明         |
| :---------------- | :----------- | :----------- |
| `order_book_ids`  | `str`        | 合约代码     |
| `info_date`       | `datetime.date` | 发布日期     |
| `meida`           | `str`        | 媒体出处     |
| `category`        | `str`        | 内容类别     |
| `title`           | `str`        | 标题         |
| `language`        | `str`        | 语言         |
| `file_type`       | `str`        | 文件格式     |
| `info_type`       | `str`        | 信息类别     |
| `announcement_link` | `str`        | 公告链接     |
| `create_tm`       | `datetime.date` | 入库时间     |

### 范例

获取一个合约某个时间段内的公司公告数据

```
[In] rqdatac.get_announcement('000001.XSHE',20221001,20221010)
[Out] media category title language file_type info_type announcement_link create_tm order_book_id info_date
000001.XSHE 2022-10-09 中国货币网16 平安银行股份有限公司2022年第117期同业存单发行公告简体中文PDF 发行上市书https://www.chinamoney.com.cn/dqs/cm-s-notice-... 2022-10-09 16:43:03
2022-10-10 中国货币网99 平安银行股份有限公司2022年第117期同业存单发行情况公告简体中文PDF 临时公告https://www.chinamoney.com.cn/dqs/cm-s-notice-... 2022-10-10 16:41:28
2022-10-10 中国货币网16 平安银行股份有限公司2022年第118期同业存单发行公告简体中文PDF 发行上市书https://www.chinamoney.com.cn/dqs/cm-s-notice-... 2022-10-10 17:41:15
```

## get_audit_opinion - 获取财务报告审计意见

`get_audit_opinion(order_book_ids, start_quarter=None, end_quarter=None, date=None, opinion_types=None, market='cn')`

获取A 股季度基础财务报告的审计意见相关数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_quarter`  | `str`                                    | 财报回溯查询的起始报告期，例如'2015q2'代表2015 年半年报， 该参数必填 |
| `end_quarter`    | `str`                                    | 财报回溯查询的截止报告期，例如'2015q4'代表2015 年年报，该参数必填 |
| `date`           | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询日期，默认查询日期为当前最新日期                         |
| `type`           | `str or list`                            | 需要返回的审计报告类型: 'financial_statements'：财务报表审计报告 'internal_control'：内部控制审计报告 |
| `opinion_types`  | `str or list`                            | 需要返回的审计意见类型，详细类型见下方表格。默认返回所有     |
| `market`         | `str`                                    | 市场，默认'cn'为中国内地市场                                 |

### 审计意见类型

| `opinion_type`          | 说明               |
| :---------------------- | :----------------- |
| `unqualified`           | 无保留             |
| `unqualified_with_explanation` | 无保留带解释性说明 |
| `qualified`             | 保留意见           |
| `disclaimer`            | 拒绝/无法表示意见  |
| `adverse`               | 否定意见           |
| `unaudited`             | 未经审计           |
| `qualified_with_explanation` | 保留带解释性说明   |
| `uncertainty_audit`     | 经审计(不确定具体意见类型) |
| `material_uncertainty`  | 无保留带持续经营重大不确定性 |

### 返回

`pandas DataFrame`

| 字段          | 类型         | 说明       |
| :------------ | :----------- | :--------- |
| `info_date`   | `datetime.date` | 公告发布日   |
| `quarter`     | `str`        | 报告期     |
| `type`        | `str`        | 审计报告类型 |
| `audit_agency` | `str`        | 会计师事务所 |
| `opinion_type` | `str`        | 审计意见类型 |

### 范例

获取一个合约某个报告期的审计意见数据

```
[In] rqdatac.get_audit_opinion('000001.XSHE', start_quarter='2023q4', end_quarter='2023q4',opinion_types=None)
[Out] audit_agency type info_date opinion_type order_book_id quarter
000001.XSHE 2023q4 安永华明会计师事务所(特殊普通合伙) internal_control 2024-03-15 unqualified
2023q4 安永华明会计师事务所(特殊普通合伙) financial_statements 2024-03-15 unqualified
```

## get_restricted_shares - 获取股票限售解禁明细数据

`rqdatac.get_restricted_shares(order_book_ids, start_date=None, end_date=None,market='cn')`

获取A 股限售解禁明细数据

### 参数

| 参数             | 类型                                     | 说明                                                         |
| :--------------- | :--------------------------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str or list`                            | 给出单个或多个order_book_id                                  |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期。注：如使用开始日期，则必填结束日期                 |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期。注：若使用结束日期，则开始日期必填                 |
| `market`         | `str`                                    | 默认是中国内地市场('cn') 。                                 |

### 返回

`pandas Series`

| 字段                      | 类型         | 说明               |
| :------------------------ | :----------- | :----------------- |
| `order_book_ids`          | `str`        | 合约代码           |
| `info_date`               | `datetime.date` | 发布日期           |
| `relieve_date`            | `datetime.date` | 解禁日期           |
| `shareholder_attr`        | `str`        | 股东属性           |
| `relieve_shares`          | `float`      | 解除限售股份数量(股) |
| `auctual_relieve_shares`  | `float`      | 实际上市流通数量(股)(提供范围为2024-01-01 至今) |
| `reason`                  | `str`        | 解禁原因           |

### 范例

获取一个合约某个时间段内的解禁明细数据

```
[In] rqdatac.get_restricted_shares('000001.XSHE',20100101,20240101)
[Out] relieve_date shareholder_name shareholder_attr relieve_shares auctual_relieve_shares reason order_book_id info_date
000001.XSHE 2010-06-25 2010-06-28 中国平安保险（集团）股份有限公司企业1.812557e+08 None 股权分置限售流通
2010-09-16 2013-11-12 中国平安人寿保险股份有限公司企业6.073280e+08 None 增发A股法人配售上市
2011-07-29 2014-09-01 中国平安保险（集团）股份有限公司企业3.145606e+09 None 增发A股原股东配售上市
2014-01-08 2017-01-09 中国平安保险（集团）股份有限公司企业2.286809e+09 None 增发A股原股东配售上市
2015-05-20 2016-05-23 财通基金-兴业银行-鹏华资产管理(深圳)有限公司证券品种2.952090e+05 None 增发A股法人配售上市
```


# 期货行情数据说明

可获取期货合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price。

## 连续合约

需要注意，由于期货合约存续的特殊性，针对每一品种的期货合约，系统中都增加了主力、次主力连续合约以及指数连续合约两个人工合成的合约来满足使用需求。

*   **主力连续合约**：合约首次上市时，以当日收盘同品种持仓量最大者作为从第二个交易日开始的主力合约。当同品种其他合约持仓量在收盘后超过当前主力合约1.1 倍时，从第二个交易日开始进行主力合约的切换。日内不会进行主力合约的切换。主力连续合约是由该品种期货不同时期主力合约接续而成，代码以88、888、889 结尾。
    *   例如IF88、IF888、IF889：
        *   **IF88** 为合约量价数据的简单拼接，未做平滑处理
        *   **IF888** 为对价格进行了"前复权平滑"处理，处理规则如下：以主力合约切换前一天（T-1 日）新、旧两个主力合约收盘价做差，之后将T-1 日及以前的主力连续合约的所有价格水平整体加上或减去该价差，以"整体抬升"或"整体下降"主力合约的价格水平，成交量、持仓量均不作调整，成交额统一设置为0。
        *   **IF889** 为对价格进行“后复权平滑”处理，处理规则如下：以主力合约切换当天（T 日）旧、新两个主力合约开盘价做差，之后将T 日及以后的主力连续合约的所有价格水平整体加上或减去该价差，成交量、持仓量均不作调整，成交额统一设置为0
*   **次主力连续合约**：比当前主力合约远月，未做过主力合约，也未做过次主力的合约中以累计持仓量最大者作为次主力连续合约，代码以88A2 结尾，例如AU88A2。
*   **指数连续合约**：由当前品种全部可交易合约以累计持仓量为权重加权平均得到，代码以99 结尾，例如IF99。

## futures.get_dominant - 获取主力合约列表

`futures.get_dominant(underlying_symbol, start_date=None, end_date=None,rule=0)`



获取某一期货品种一段时间的主力合约列表。合约首次上市时，以当日收盘同品种持仓量最大者作为从第二个交易日开始的主力合约。当同品种其他合约持仓量在收盘后超过当前主力合约1.1 倍时，从第二个交易日开始进行主力合约的切换。日内不会进行主力合约的切换。

### 参数

| 参数                 | 类型                                                    | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :------------------- | :------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `underlying_symbol`  | `str`                                                   | 期货合约品种，例如沪深300 股指期货为'IF'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `start_date`         | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为期货品种最早上市日期后一交易日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `end_date`           | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为当前日期                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `rule`               | `int`                                                   | 主力合约选取规则。默认rule=0，当同品种其他合约持仓量在收盘后超过当前主力合约1.1 倍时，从第二个交易日开始进行主力合约的切换。每个合约只能做一次主力/次主力合约，不会重复出现。针对股指期货，只在当月和次月选择主力合约。当rule=1 时，主力/次主力合约的选取只考虑最大/第二大昨仓这个条件。当rule=2 时，采用昨日成交量与持仓量同为最大/第二大的合约为当日主力/次主力。当rule=3 时，在rule=0 选取规则上，考虑在最后一个交易日不能成为主力/次主力合约。                                                                                                                                                                                                                                                               |
| `rank`               | `int`                                                   | 默认rank=1。1-主力合约，2-次主力合约，3-次次主力合约。针对股指期货，'2','3'仅在rule=1 和2 时生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

### 返回

`Pandas.Series` - 主力合约代码列表

### 范例

**获取某一天的主力合约代码**

```python
[In] futures.get_dominant('IF', '20160801')
[Out] date
20160801 IF1608
```


**获取从上市到某天之间的主力合约代码**

```python
[In] futures.get_dominant('IC', end_date='20150501')
[Out] date
20150417 IC1505
20150420 IC1505
20150421 IC1505
20150422 IC1505
20150423 IC1505
20150424 IC1505
20150427 IC1505
20150428 IC1505
20150429 IC1505
20150430 IC1505
20150501 IC1505
```



## futures.get_contracts - 获取期货可交易合约列表

`futures.get_contracts(underlying_symbol, date=None)`


获取某一期货品种在策略当前日期的可交易合约`order_book_id` 列表。按照到期月份，下标从小到大排列，返回列表中第一个合约对应的就是该品种的近月合约。

### 参数

| 参数                | 类型                                                    | 说明           |
| :------------------ | :------------------------------------------------------ | :------------- |
| `underlying_symbol` | `str`                                                   | 期货合约品种，例如沪深300 股指期货为'IF' |
| `date`              | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 查询日期，默认为当日 |

### 返回

`str list` - 可交易的`order_book_id` list

### 范例

```python
[In] futures.get_contracts('IF', '20160801')
[Out] ['IF1608', 'IF1609', 'IF1612', 'IF1703']
```


## futures.get_dominant_price - 获取期货主力连续合约行情数据

`futures.get_dominant_price(underlying_symbols,start_date=None,end_date=None,frequency='1d',fields=None,adjust_type='pre', adjust_method='prev_close_spread')`


主力连续合约是由不同时期的主力合约拼接而成，在主力合约发生切换时，前后两个合约会存在价差，因而未经平滑处理的主力连续合约有着明显的价格跳空现象。为避免策略出现虚假信号，造成信号失真。米筐提供了价差和比例复权两种方法平滑处理后的主力连续合约的行情数据，避免虚假信号造成影响。

获取期货主力连续合约行情数据参数如下：

### 参数

| 参数                 | 类型             | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| :------------------- | :--------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `underlying_symbols` | `str` or `str list` | 期货合约品种，可传入 `underlying_symbol`, `underlying_symbol list`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `start_date`         | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `end_date`           | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期 不传入`start_date`, `end_date` 则默认返回最近三个月的数据                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `frequency`          | `str`            | 历史数据的频率。 支持/日/分钟/tick 级别的历史数据，默认为'1d'。1m- 分钟线，1d-日线，分钟可选取不同频率，例如'5m'代表5 分钟线                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `fields`             | `str` or `str list` | 字段名称                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `adjust_type`        | `str`            | 复权方式，'pre'，不复权- none，前复权- pre，后复权- post，                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `adjust_method`      | `str`            | 复权方法'prev_close_spread'：基于主力合约切换前一个交易日收盘价价差进行复权 'open_spread'：基于主力合约切换当日开盘价价差进行复权 'prev_close_ratio'：基于主力合约切换前一个交易日收盘价比例进行复权 'open_ratio'：基于主力合约切换当日开盘价比例进行复权'默认为'prev_close_spread'; `adjust_type` 为None 时，`adjust_method` 复权方法设置无效                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `rule`               | `int`            | 主力合约选取规则。默认rule=0，当同品种其他合约持仓量在收盘后超过当前主力合约1.1 倍时，从第二个交易日开始进行主力合约的切换。每个合约只能做一次主力/次主力合约，不会重复出现。针对股指期货，只在当月和次月选择主力合约。当rule=1 时，主力/次主力合约的选取只考虑最大/第二大昨仓这个条件。当rule=2 时，采用昨日成交量与持仓量同为最大/第二大的合约为当日主力/次主力。当rule=3 时，在rule=0 选取规则上，考虑在最后一个交易日不能成为主力/次主力合约。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `rank`               | `int`            | 默认rank=1。1-主力合约，2-次主力合约，3-次次主力合约。针对股指期货，'2','3'仅在rule=1 和2 时生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

### 返回

`bar` 数据

| 字段                | 类型           | 说明                 |
| :------------------ | :------------- | :------------------- |
| `open`              | `float`        | 开盘价               |
| `close`             | `float`        | 收盘价               |
| `high`              | `float`        | 最高价               |
| `low`               | `float`        | 最低价               |
| `limit_up`          | `float`        | 涨停价（仅限日线数据） |
| `limit_down`        | `float`        | 跌停价（仅限日线数据） |
| `total_turnover`    | `float`        | 成交额               |
| `volume`            | `float`        | 成交量               |
| `settlement`        | `float`        | 结算价（仅限日线数据） |
| `prev_settlement`   | `float`        | 昨日结算价（仅限日线数据） |
| `open_interest`     | `float`        | 累计持仓量             |
| `trading_date`      | `pandasTimestamp` | 交易日期（仅限分钟线数据），对应期货夜盘的情况 |
| `dominant_id`       | `str`          | 主力合约             |

`tick` 数据

| 字段              | 类型              | 说明                   |
| :---------------- | :---------------- | :--------------------- |
| `open`            | `float`           | 当日开盘价             |
| `high`            | `float`           | 当日最高价             |
| `low`             | `float`           | 当日最低价             |
| `last`            | `float`           | 最新价                 |
| `prev_close`      | `float`           | 昨日收盘价             |
| `total_turnover`  | `float`           | 成交额                 |
| `volume`          | `float`           | 成交量                 |
| `limit_up`        | `float`           | 涨停价                 |
| `limit_down`      | `float`           | 跌停价                 |
| `open_interest`   | `float`           | 累计持仓量             |
| `datetime`        | `datetime.datetime` | 交易所时间戳           |
| `a1`~`a5`         | `float`           | 卖一至五档报盘价格     |
| `a1_v`~`a5_v`     | `float`           | 卖一至五档报盘量       |
| `b1`~`b5`         | `float`           | 买一至五档报盘价       |
| `b1_v`~`b5_v`     | `float`           | 买一至五档报盘量       |
| `change_rate`     | `float`           | 涨跌幅                 |
| `trading_date`    | `pandasTimestamp` | 交易日期，对应期货夜盘的情况 |
| `prev_settlement` | `float`           | 昨日结算价             |
| `dominant_id`     | `str`             | 主力合约               |

### 范例

**获取期货主力连续合约前复权日线行情**

```python
[In] rqdatac.futures.get_dominant_price(underlying_symbols='IF',start_date=20210901,end_date=20210902,frequency='1d',fields=None,adjust_type='pre', adjust_method='prev_close_spread')
[Out] settlement volume limit_down open open_interest total_turnover limit_up close low prev_settlement high underlying_symbol date
IF 2021-09-01 4855.0 130017.0 4290.2 4767.2 143730.0 0 5243.4 4856.4 4737.2 4766.8 4898.6
2021-09-02 4854.0 73853.0 4369.6 4855.0 128436.0 0 5340.4 4854.2 4830.2 4855.0 4879.0
```


**获取期货主力连续合约不复权分钟线行情**

```python
[In] rqdatac.futures.get_dominant_price(underlying_symbols='IF',start_date=20210901,end_date=20210901,frequency='1m',fields=None,adjust_type='none', adjust_method='prev_close_spread')
[Out] trading_date volume open open_interest total_turnover close low high underlying_symbol datetime
IF 2021-09-01 09:31:00 2021-09-01 2087.0 4767.2 140180.0 2.990109e+09 4779.0 4767.0 4781.8
2021-09-01 09:32:00 2021-09-01 1044.0 4779.6 139408.0 1.496143e+09 4773.2 4772.0 4780.6
2021-09-01 09:33:00 2021-09-01 964.0 4773.2 138709.0 1.379020e+09 4763.4 4763.0 4773.2
2021-09-01 09:34:00 2021-09-01 1239.0 4763.4 137894.0 1.768014e+09 4751.4 4750.8 4763.4
2021-09-01 09:35:00 2021-09-01 1126.0 4750.8 137099.0 1.605260e+09 4755.2 4748.0 4755.6
···
2021-09-01 14:58:00 2021-09-01 589.0 4854.4 143113.0 8.574656e+08 4855.2 4851.0 4855.4
2021-09-01 14:59:00 2021-09-01 445.0 4855.0 143410.0 6.483443e+08 4856.8 4853.8 4858.2
2021-09-01 15:00:00 2021-09-01 611.0 4857.4 143730.0 8.907135e+08 4856.4 4855.8 4861.0
```



## futures.get_ex_factor - 获取期货主力连续合约复权因子

`futures.get_ex_factor(underlying_symbols, start_date=None, end_date=None,adjust_method=None, market='cn')`


获取期货主力连续合约复权因子数据的参数如下：

### 参数

| 参数                 | 类型           | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :------------------- | :------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `underlying_symbols` | `str` or `list` | 品种代码                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `start_date`         | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `end_date`           | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，不传入是返回全部                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `adjust_method`      | `str`          | 复权方法 'prev_close_spread' ，'open_spread' , 'prev_close_ratio'，'open_ratio，默认为'prev_close_spread'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `rule`               | `int`          | 主力合约选取规则。默认rule=0，当同品种其他合约持仓量在收盘后超过当前主力合约1.1 倍时，从第二个交易日开始进行主力合约的切换。每个合约只能做一次主力/次主力合约，不会重复出现。针对股指期货，只在当月和次月选择主力合约。当rule=1 时，主力/次主力合约的选取只考虑最大/第二大昨仓这个条件。当rule=2 时，采用昨日成交量与持仓量同为最大/第二大的合约为当日主力/次主力。当rule=3 时，在rule=0 选取规则上，考虑在最后一个交易日不能成为主力/次主力合约。                                                                                                                                                                                                                                                               |
| `rank`               | `int`          | 默认rank=1。1-主力合约，2-次主力合约，3-次次主力合约。针对股指期货，'2','3'仅在rule=1 和2 时生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `market`             | `str`          | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

### 返回

返回 `pandas dataframe` - 包含了复权因子的日期和对应的各项数值

| 参数            | 类型  | 说明                         |
| :-------------- | :---- | :--------------------------- |
| `ex_date`       | `str` | 除权除息日（ 主力合约切换日） |
| `underlying_symbol` | `str` | 品种代码                     |
| `ex_factor`     | `float` | 复权因子                     |
| `ex_cum_factor` | `float` | 累计复权因子                 |
| `ex_end_date`   | `str` | 复权因子所在期的截止日期       |

### 范例

```python
[In] rqdatac.futures.get_ex_factor(underlying_symbols='IF', start_date=20210601, end_date=20210902,adjust_method='prev_close_spread', market='cn')
[Out] underlying_symbol ex_factor ex_end_date ex_cum_factor ex_date
2021-06-18 IF 32.8 2021-07-15 1165.8
2021-07-16 IF 16.8 2021-08-12 1182.6
2021-08-13 IF 29.0 NaT 1211.6
```


## futures.get_contract_multiplier - 获取期货品种合约乘数

`futures.get_contract_multiplier(underlying_symbols, start_date=None, end_date=None, market='cn')`


获取期货品种的合约乘数

### 参数

| 参数                 | 类型           | 说明               |
| :------------------- | :------------- | :----------------- |
| `underlying_symbols` | `str` or `str list` | 期货合约品种       |
| `start_date`         | `str`          | 开始日期           |
| `end_date`           | `str`          | 结束日期           |
| `market`             | `str`          | 目前只支持中国市场('cn') |

### 返回

- `pandas DataFrame`

| 字段                | 类型  | 说明     |
| :------------------ | :---- | :------- |
| `exchange`          | `str` | 期货品种对应交易所 |
| `contract_multiplier` | `str` | 合约乘数   |

### 范例

```python
In: futures.get_contract_multiplier(['FB','I'], start_date='20191128', end_date='20191203', market='cn')
Out: exchange contract_multiplier underlying_symbol date
FB 2019-11-28 DCE 500.0
2019-11-29 DCE 500.0
2019-12-02 DCE 10.0
2019-12-03 DCE 10.0
I 2019-11-28 DCE 100.0
2019-11-29 DCE 100.0
2019-12-02 DCE 100.0
2019-12-03 DCE 100.0
```


## futures.get_exchange_daily - 获取期货交易所日线数据

`futures.get_exchange_daily(order_book_ids, start_date=None, end_date=None, fields=None, market='cn')`


获取期货交易所日线数据

### 参数

| 参数           | 类型           | 说明               |
| :------------- | :------------- | :----------------- |
| `order_book_ids` | `str` or `str list` | 可输入`order_book_id`, `order_book_id list` |
| `start_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期           |
| `end_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期           |
| `fields`       | `str` OR `str list` | 字段名称           |
| `market`       | `str`          | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场 |

### 返回

`pandas DataFrame`

| 字段              | 类型    | 说明         |
| :---------------- | :------ | :----------- |
| `open`            | `float` | 开盘价       |
| `close`           | `float` | 收盘价       |
| `high`            | `float` | 最高价       |
| `low`             | `float` | 最低价       |
| `total_turnover`  | `float` | 成交额       |
| `volume`          | `float` | 成交量       |
| `settlement`      | `float` | 结算价       |
| `prev_settlement` | `float` | 昨日结算价     |
| `open_interest`   | `float` | 累计持仓量     |

### 范例

```python
In: futures.get_exchange_daily('A2409', start_date='20240801', end_date='20240816', market='cn')
Out: open close high low total_turnover volume settlement prev_settlement open_interest order_book_id date
A2409 2024-08-01 4549.0 4576.0 4581.0 4530.0 5.480035e+09 120316.0 4554.0 4543.0 102030.0
2024-08-02 4580.0 4614.0 4634.0 4573.0 6.643340e+09 144204.0 4606.0 4554.0 104363.0
2024-08-05 4617.0 4590.0 4628.0 4582.0 4.818868e+09 104612.0 4606.0 4606.0 100831.0
2024-08-06 4584.0 4586.0 4602.0 4560.0 4.223373e+09 92163.0 4582.0 4606.0 90101.0
2024-08-07 4586.0 4582.0 4601.0 4569.0 3.611244e+09 78789.0 4583.0 4582.0 85198.0
2024-08-08 4572.0 4585.0 4593.0 4569.0 2.368785e+09 51666.0 4584.0 4583.0 79301.0
2024-08-09 4584.0 4575.0 4598.0 4553.0 4.023708e+09 87879.0 4578.0 4584.0 73742.0
2024-08-12 4575.0 4559.0 4578.0 4553.0 2.552710e+09 55909.0 4565.0 4578.0 67254.0
2024-08-13 4559.0 4540.0 4565.0 4536.0 2.781623e+09 61125.0 4550.0 4565.0 62705.0
2024-08-14 4538.0 4513.0 4551.0 4500.0 3.146009e+09 69587.0 4520.0 4550.0 54827.0
2024-08-15 4520.0 4487.0 4536.0 4482.0 2.088194e+05 46333.0 4506.0 4520.0 49281.0
2024-08-16 4488.0 4464.0 4507.0 4449.0 1.966178e+05 43952.0 4473.0 4506.0 38939.0
```


## futures.get_continuous_contracts - 获取股指期货当月等连续合约

`futures.get_continuous_contracts(underlying_symbol, start_date=None, end_date=None,type='front_month')`


获取股指期货当月等连续合约

### 参数

| 参数                | 类型                                                    | 说明                                   |
| :------------------ | :------------------------------------------------------ | :------------------------------------- |
| `underlying_symbol` | `str`                                                   | 期货合约品种，目前仅支持股指品种         |
| `start_date`        | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                               |
| `end_date`          | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                               |
| `type`              | `str`                                                   | 类型，`front_month` - 近月，`next_month` - 次月，`current_quarter` - 季月，`next_quarter` - 远季，默认`front_month` |

### 返回

`Pandas Series` - 连续合约`order_book_id`

### 范例

```python
In: futures.get_continuous_contracts('IF', 20250616, 20250701,'front_month')
Out: 20250616 IF2506
20250617 IF2506
20250618 IF2506
20250619 IF2506
20250620 IF2506
20250623 IF2507
20250624 IF2507
20250625 IF2507
20250626 IF2507
20250627 IF2507
20250630 IF2507
20250701 IF2507
```


## 期货其他数据

## futures.get_member_rank - 获取期货会员持仓等排名情况

`futures.get_member_rank(obj, trading_date, rank_by)`


获取期货某合约或品种的会员排名数据

### 参数

| 参数         | 类型 | 说明                                                                                                           |
| :----------- | :--- | :------------------------------------------------------------------------------------------------------------- |
| `obj`        | `str` | 可以是期货的具体合约或者品种                                                                                     |
| `trading_date` | `str` | 查询日期，默认为当日                                                                                           |
| `start_date` | `str` | 开始日期。需要传入该参数时，必须打上'start_date='字样                                                          |
| `end_date`   | `str` | 结束日期。需要传入该参数时，必须打上'end_date='字样                                                            |
| `rank_by`    |      | 排名依据，默认为volume 即根据交易量统计排名，另外可选'long'和'short'，分别对应持买仓量统计和持卖仓量统计。 |

### 返回

- `pandas DataFrame`

| 字段            | 类型  | 说明                               |
| :-------------- | :---- | :--------------------------------- |
| `commodity_id`  | `str` | 期货品种代码或期货合约代码           |
| `member_name`   | `str` | 期货商名称                           |
| `rank`          | `int` | 排名                               |
| `volume`        | `int` | 交易量或持仓量视乎参数`rank_by` 的设定 |
| `volume_change` | `int` | 交易量或持仓量较之前的变动           |

上期所、中金所的品种排名是米筐通过交易所的合约层级数据加总计算得到的。

由于交易所的合约数据并不涵盖交易不活跃合约，因而品种层级的排名数据仅供参考。

### 范例

**获取期货合约为标的的会员排名：**

```python
[In] futures.get_member_rank('A1901',trading_date=20180910,rank_by='short')
[Out] commodity_id member_name rank volume volume_change trading_date
2018-09-10 A1901 国投安信 1 20143 5065
2018-09-10 A1901 五矿经易 2 14909 4465
2018-09-10 A1901 华安期货 3 9360 3464
2018-09-10 A1901 国泰君安 4 7915 -26
2018-09-10 A1901 永安期货 5 6683 998
2018-09-10 A1901 中信期货 6 6587 -583
2018-09-10 A1901 华泰期货 7 5918 -430
2018-09-10 A1901 东证期货 8 5075 1837
2018-09-10 A1901 中国国际 9 4792 2169
2018-09-10 A1901 国富期货 10 4632 -213
2018-09-10 A1901 浙商期货 11 4160 -513
2018-09-10 A1901 新湖期货 12 3960 119
2018-09-10 A1901 中金期货 13 3868 -25
2018-09-10 A1901 光大期货 14 3694 2566
2018-09-10 A1901 摩根大通 15 3644 0
2018-09-10 A1901 银河期货 16 3173 559
2018-09-10 A1901 兴证期货 17 3151 -251
2018-09-10 A1901 方正中期 18 2206 146
2018-09-10 A1901 一德期货 19 2017 838
2018-09-10 A1901 南华期货 20 1949 -190
```


**获取期货品种为标的的会员排名：**

```python
[In] futures.get_member_rank('CU',trading_date=20180910,rank_by='short')
[Out] commodity_id member_name rank volume volume_change trading_date
2018-09-10 CU 五矿经易 1 29160 302
2018-09-10 CU 中信期货 2 27136 -535
2018-09-10 CU 永安期货 3 16521 753
2018-09-10 CU 海通期货 4 15994 -161
2018-09-10 CU 中粮期货 5 14572 -614
2018-09-10 CU 国泰君安 6 8852 245
2018-09-10 CU 金瑞期货 7 8668 -703
2018-09-10 CU 迈科期货 8 8320 94
2018-09-10 CU 建信期货 9 6688 -57
2018-09-10 CU 广发期货 10 5847 -34
2018-09-10 CU 华安期货 11 5451 -289
2018-09-10 CU 格林大华 12 5330 -217
2018-09-10 CU 中银国际 13 5190 487
2018-09-10 CU 铜冠金源 14 4896 139
2018-09-10 CU 兴证期货 15 4636 -459
2018-09-10 CU 方正中期 16 4587 -92
2018-09-10 CU 国投安信 17 4567 79
2018-09-10 CU 东方财富 18 4551 127
2018-09-10 CU 新湖期货 19 4269 -60
2018-09-10 CU 国贸期货 20 3396 -522
```


**获取期货品种为标的的指定时间段会员排名**

单个合约的指定时间段会员排名获取方式雷同

```python
[In] : futures.get_member_rank('RB',start_date='20180910',end_date='20180915').tail(5)
[Out]: commodity_id member_name rank volume volume_change trading_date
2018-09-14 RB 中辉期货16 53761 -5682
2018-09-14 RB 宏源期货17 51777 -9398
2018-09-14 RB 广发期货18 51279 -2157
2018-09-14 RB 国贸期货19 51145 -23497
2018-09-14 RB 上海中期20 47731 -4862
```


## futures.get_warehouse_stocks - 获取期货仓单数据

`futures.get_warehouse_stocks(underlying_symbols, start_date=None, end_date=None, market='cn')`


获取期货某品种的注册仓单数据

### 参数

| 参数                 | 类型 | 说明                 |
| :------------------- | :--- | :------------------- |
| `underlying_symbols` | `str` | 期货合约品种         |
| `start_date`         | `str` | 开始日期，用户必须指定 |
| `end_date`           | `str` | 结束日期，默认为策略当天日期的前一天 |
| `market`             | `str` | 目前只支持中国市场('cn') |

### 返回

- `pandas DataFrame`

| 字段            | 类型  | 说明                         |
| :-------------- | :---- | :--------------------------- |
| `on_warrant`    | `int` | 注册仓单量                   |
| `exchange`      | `str` | 期货品种对应交易所             |
| `effective_forecast` | `str` | 有效预报。仅支持郑商所（CZCE）合约 |
| `warrant_units` | `str` | 仓单单位。仅支持郑商所（CZCE）合约 |
| `deliverable`   | `str` | 符合交割品质的货物数量。仅支持上期所（SHFE）合约 |

### 范例

```python
In: rq.futures.get_warehouse_stocks('CF',start_date=20191201,end_date=20191205)
Out: on_warrant exchange effective_forecast warrant_units deliverable date underlying_symbol
20191202 CF 19425 CZCE 4753 8 NaN
20191203 CF 19921 CZCE 4696 8 NaN
20191204 CF 19997 CZCE 5005 8 NaN
20191205 CF 20603 CZCE 4752 8 NaN
```



## futures.get_basis - 获取股指期货每日升贴水数据

`futures.get_basis(order_book_ids, start_date=None, end_date=None,fields=None,frequency='1d')`


股指期货贴水指的是股指期货比股指现货低的情况，而当股指期货高于现货时，则称之升水。在股票中性策略中，股指期货一直以来都是最重要的对冲伙伴,而对冲存在对冲成本，因此在开发股票市场中性策略过程中，研究对冲成本——股指期货贴水，就显得尤为必要了。

如下图通过获取股指期货升贴水数据，可以发现自从2015 年中证500 期货（IC）推出以来，大部分时间都处于贴水状态（指数期货-指数为负）,通过做多贴水，以期获得超额收益。

ic-basis

获取股指期货每日升贴水数据的参数如下：

### 参数

| 参数             | 类型                  | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :--------------- | :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `order_book_ids` | `str_or_str_list`     | 合约代码，可传入`order_book_id`, `order_book_id list`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，`start_date`, `end_date` 不传参数时默认返回最近三个月的数据                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `fields`         | `str_or_str_list`     | 字段名称                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `frequency`      | `str`                 | 频率，支持/日/分钟/tick 级别的历史数据，默认为'1d'。`1d` - 日线 `1m` - 分钟线 `tick`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

### 返回

`pandas multi-index DataFrame`

| 字段                | 类型           | 说明                                                                                                                                                                        |
| :------------------ | :------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `open`              | `float`        | 开盘价                                                                                                                                                                      |
| `high`              | `float`        | 最高价                                                                                                                                                                      |
| `low`               | `float`        | 最低价                                                                                                                                                                      |
| `close`             | `float`        | 收盘价                                                                                                                                                                      |
| `index`             | `str`          | 指数合约                                                                                                                                                                    |
| `close_index`       | `float`        | 指数收盘价                                                                                                                                                                  |
| `basis`             | `float`        | 升贴水，等于期货合约收盘价- 对应指数收盘价                                                                                                                                  |
| `basis_rate`        | `float`        | 升贴水率(%)，（期货合约收盘价- 对应指数收盘价）\*100/对应指数收盘价                                                                                                            |
| `basis_annual_rate` | `float`        | 年化升贴水率（%), `basis_rate` \*(`250`/合约到期剩余交易日）                                                                                                                 |
| `settlement`        | `float`        | 结算价                                                                                                                                                                      |
| `settle_basis`      | `float`        | 升贴水，等于期货合约结算价- 对应指数收盘价                                                                                                                                  |
| `settle_basis_rate` | `float`        | 升贴水率(%)，（期货合约结算价- 对应指数收盘价）\*100/对应指数收盘价                                                                                                            |
| `settle_basis_annual_rate` | `float`        | 年化升贴水率（%), `settle_basis_rate` \*(`250`/合约到期剩余交易日）                                                                                                        |

注意: 接近到期日的年化升贴水率仅供参考，原因是在离到期日只有几天，分母特别小的情况下，计算出的年化升贴水率数值会失真，这时候看绝对升贴水更好。

### 范例

**获取IF2106 和IH2106 的升贴水数据**

```python
[In] futures.get_basis(['IF2106','IH2106'],'20210412','20210413')
[Out] low open high basis basis_rate index close basis_annual_rate close_index order_book_id date
IH2106 2021-04-12 3395.0 3438.0 3446.4 -64.2654 -1.848265 000016.XSHG 3412.8 -10.268142 3477.0654
2021-04-13 3384.2 3430.2 3432.6 -61.4592 -1.775529 000016.XSHG 3400.0 -10.088231 3461.4592
IF2106 2021-04-12 4854.0 4938.2 4956.0 -81.7459 -1.652185 000300.XSHG 4866.0 -9.178804 4947.7459
2021-04-13 4844.0 4891.2 4905.2 -74.2438 -1.503019 000300.XSHG 4865.4 -8.539882 4939.6438
```



## futures.get_current_basis - 获取股指期货实时升贴水数据

`rqdatac.futures.get_current_basis(order_book_ids,market='cn')`


实时升贴水基于`current_snapshot` 计算，计算逻辑同`get_basis`。

备注：每日15 点30 分过后股指期货结算价更新之后，实时升贴水用结算价计算

获取股指期货每日升贴水数据的参数如下：

### 参数

| 参数           | 类型              | 说明                       |
| :------------- | :---------------- | :------------------------- |
| `order_book_ids` | `str_or_str_list` | 合约代码，可传入`order_book_id`, `order_book_id list` |
| `market`       | `str`             | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场； |

### 返回

`pandas multi-index DataFrame`

| 字段              | 类型              | 说明                               |
| :---------------- | :---------------- | :--------------------------------- |
| `order_book_id`   | `str`             | 合约代码                           |
| `datetime`        | `datetime.datetime` | 最新一行tick 的时间戳              |
| `index`           | `str`             | 指数合约                           |
| `index_px`        | `float`           | 指数最新价格                       |
| `future_px`       | `float`           | 期货最新价格                       |
| `basis`           | `float`           | 升贴水，等于期货合约收盘价- 对应指数收盘价 |
| `basis_rate`      | `float`           | 升贴水率(%)，（期货合约收盘价- 对应指数收盘价）\*100/对应指数收盘价 |
| `basis_annual_rate` | `float`           | 年化升贴水率（%), `basis_rate` \*(`250`/合约到期剩余交易日） |

### 范例

**获取IF2403 的实时升贴水数据**

```python
[In] rqdatac.futures.get_current_basis('IF2403')
[Out] index datetime index_px future_px basis basis_rate basis_annual_rate order_book_id
IF2403 000300.XSHG 2024-02-26 15:23:08.200 3453.3585 3445.4 -7.9585 -0.230457 -4.1153
```



## futures.get_trading_parameters - 获取期货交易参数信息

`rqdatac.futures.get_trading_parameters(order_book_ids, start_date=None, end_date=None, fields=None, market='cn')`



获取期货保证金、手续费等交易参数信息

### 参数

| 参数             | 类型           | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :--------------- | :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `order_book_ids` | `str` or `list` | 合约代码                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，若不指定日期，则默认为当前交易日期                                                                                                                                                                                                                                                                                                                                                                                         |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，若不指定日期，则默认为当前交易日期                                                                                                                                                                                                                                                                                                                                                                                         |
| `fields`         | `str` OR `str list` | 可选字段见下方返回，若不指定，则默认获取所有字段                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `market`         | `str`          | 默认是中国内地市场('cn') 。可选'cn' - 中国内地市场                                                                                                                                                                                                                                                                                                                                                                                                           |

说明：

1、`start_date` 和`end_date` 需同时传入或同时不传入。当不传入`start_date`, `end_date` 参数时，查询时间在交易日T 日8.40 pm 之前，返回T 日的数据；查询时点在8.40pm 之后，返回交易日T+1 日的数据。

2、保证金、手续费数据提供范围为2010.04 月至今；限仓数据各交易所提供范围见下方表格

| 交易所 | 时间范围         |
| :----- | :--------------- |
| 中金所 | 2010-04-16 至今  |
| 上期所 | 2013-10-08 至今  |
| 大商所 | 2018-12-21 至今  |
| 郑商所 | 2021-04-12 至今  |
| 上能源 | 2021-06-11 至今  |
| 广期所 | 2022-12-23 至今  |

### 返回

返回 `multi-index DataFrame`

| 参数                   | 类型              | 说明                       |
| :--------------------- | :---------------- | :------------------------- |
| `order_book_ids`       | `str`             | 合约代码                   |
| `trading_date`         | `pandasTimestamp` | 交易日期                   |
| `long_margin_ratio`    | `float`           | 多头保证金率               |
| `short_margin_ratio`   | `float`           | 空头保证金率               |
| `commission_type`      | `str`             | 手续费类型（按成交量/按成交额） |
| `open_commission`      | `float`           | 开仓手续费                 |
| `close_commission`     | `float`           | 平仓手续费                 |
| `discount_rate`        | `float`           | 平今折扣率                 |
| `close_commission_today` | `float`           | 平今仓手续费/率            |
| `non_member_limit_rate` | `float`           | 非期货会员持仓限额比例     |
| `client_limit_rate`    | `float`           | 客户持仓限额比例           |
| `non_member_limit`     | `float`           | 非期货会员持仓限额(手)     |
| `client_limit`         | `float`           | 客户持仓限额(手)           |
| `min_order_quantity`   | `float`           | 最小开仓下单量(手)         |
| `max_order_quantity`   | `float`           | 最大开仓下单量(手)         |
| `min_margin_ratio`     | `float`           | 最低交易保证金             |

### 范例

```python
[In] rqdatac.futures.get_trading_parameters('IF2312')
[Out] long_margin_ratio short_margin_ratio commission_type open_commission close_commission discount_rate close_commission_today non_member_limit_rate client_limit_rate non_member_limit client_limit
order_book_id trading_date
IF2312 2023-12-05 0.12 0.12 by_money 0.000023 0.000023 10.0 0.00023 NaN NaN NaN 5000.0
```


# 期权行情数据说明

## 支持的期权品种

国内期权全品种上市以来的日线、分钟线数据，18年10月以来的tick数据；国内期权全品种实时tick数据和实时分钟数据。具体如下：

### 1、上海证券交易所

| 品种               | 行权方式 | 上市日期   |
| :----------------- | :------- | :--------- |
| 华夏上证50ETF期权  | 欧式     | 2015-02-09 |
| 华泰沪深300ETF期权 | 欧式     | 2019-12-23 |
| 南方中证500ETF期权 | 欧式     | 2022-09-19 |
| 华夏上证科创板50ETF期权 | 欧式     | 2023-06-05 |
| 易方达上证科创板50ETF期权 | 欧式     | 2023-06-05 |

### 2、深圳证券交易所

| 品种               | 行权方式 | 上市日期   |
| :----------------- | :------- | :--------- |
| 嘉实沪深300ETF期权 | 欧式     | 2019-12-23 |
| 易方达创业板ETF期权 | 欧式     | 2022-09-19 |
| 嘉实中证500ETF期权 | 欧式     | 2022-09-19 |
| 易方达深证100ETF期权 | 欧式     | 2022-12-12 |

### 3、上海期货交易所

| 品种     | 行权方式 | 上市日期   |
| :------- | :------- | :--------- |
| 铜期权   | 美式     | 2018-09-21 |
| 天然橡胶期权 | 美式     | 2019-01-28 |
| 黄金期权 | 美式     | 2019-12-20 |
| 铝期权   | 美式     | 2020-08-10 |
| 锌期权   | 美式     | 2020-08-10 |
| 原油期权 | 美式     | 2021-06-21 |
| 螺纹钢期权 | 美式     | 2022-12-26 |
| 白银期权 | 美式     | 2022-12-26 |
| 合成橡胶期权 | 美式     | 2023-07-31 |
| 镍期权   | 美式     | 2024-09-02 |
| 锡期权   | 美式     | 2024-09-02 |
| 氧化铝期权 | 美式     | 2024-09-02 |
| 铅期权   | 美式     | 2024-09-02 |

### 4、郑州商品交易所

| 品种     | 行权方式 | 上市日期   |
| :------- | :------- | :--------- |
| 白糖期权 | 美式     | 2017-04-19 |
| 棉花期权 | 美式     | 2019-01-28 |
| 甲醇期权 | 美式     | 2019-12-16 |
| PTA期权  | 美式     | 2019-12-16 |
| 动力煤期权 | 美式     | 2020-06-30 |
| 菜籽粕期权 | 美式     | 2021-01-16 |
| 菜籽油期权 | 美式     | 2022-08-26 |
| 花生期权 | 美式     | 2022-08-26 |
| 对二甲苯期权 | 美式     | 2023-09-18 |
| 烧碱期权 | 美式     | 2023-09-18 |
| 纯碱期权 | 美式     | 2023-10-20 |
| 短纤期权 | 美式     | 2023-10-20 |
| 锰硅期权 | 美式     | 2023-10-20 |
| 硅铁期权 | 美式     | 2023-10-20 |
| 尿素期权 | 美式     | 2023-10-20 |
| 苹果期权 | 美式     | 2023-10-20 |
| 红枣期权 | 美式     | 2024-06-21 |
| 玻璃期权 | 美式     | 2024-06-21 |

### 5、大连商品交易所

| 品种     | 行权方式 | 上市日期   |
| :------- | :------- | :--------- |
| 豆粕期权 | 美式     | 2017-03-31 |
| 玉米期权 | 美式     | 2019-01-28 |
| 铁矿石期权 | 美式     | 2019-12-09 |
| 液化石油气期权 | 美式     | 2020-03-31 |
| 聚乙烯期权 | 美式     | 2020-07-06 |
| 聚氯乙烯期权 | 美式     | 2020-07-06 |
| 聚丙烯期权 | 美式     | 2020-07-06 |
| 棕榈油期权 | 美式     | 2021-06-18 |
| 黄大豆1号期权 | 美式     | 2022-08-08 |
| 黄大豆2号期权 | 美式     | 2022-08-08 |
| 豆油期权 | 美式     | 2022-08-08 |
| 乙二醇期权 | 美式     | 2023-05-15 |
| 苯乙烯期权 | 美式     | 2023-05-15 |
| 鸡蛋期权 | 美式     | 2024-08-23 |
| 玉米淀粉期权 | 美式     | 2024-08-23 |
| 生猪期权 | 美式     | 2024-08-23 |

### 6、中国金融期货交易所

| 品种       | 行权方式 | 上市日期   |
| :--------- | :------- | :--------- |
| 沪深300股指期权 | 欧式     | 2019-12-23 |
| 中证1000股指期权 | 欧式     | 2022-07-22 |
| 上证50股指期权 | 欧式     | 2022-12-19 |

### 7、上海国际能源交易中心

| 品种   | 行权方式 | 上市日期   |
| :----- | :------- | :--------- |
| 原油期权 | 美式     | 2021-06-21 |

### 8、广州期货交易所

| 品种   | 行权方式 | 上市日期   |
| :----- | :------- | :--------- |
| 工业硅期权 | 美式     | 2022-12-23 |
| 碳酸锂期权 | 美式     | 2023-07-24 |

## 支持的行情数据频率说明

| 频率   | 时间范围     | 更新频率   | 支持范围     |
| :----- | :----------- | :--------- | :----------- |
| 日行情 | 2015-至今    | 交易日盘后更新 | 国内期权全品种 |
| 分钟行情 | 2015-至今    | 交易日实时更新 | 国内期权全品种 |
| tick行情 | 2018.10-至今 | 交易日实时更新 | 国内期权全品种 |

## 范例

### 1、获取期权日行情

历史日线数据可通过API-get_price获取，get_price 参数设置请参考该API

查询PTA期权TA2005C5200和300ETF期权在2019年12月30日的日度行情数据

```python
get_price(['TA2005C5200','10002117'],'20191230','20191230',expect_df=True)
```

```
Out[1] strike_price limit_down total_turnover volume contract_multiplier low settlement limit_up open_interest close high open prev_settlement order_book_id date 10002117 2019-12-30 3.6 0.037 8613235.0 1834.0 10000.0 0.4187 0.4929 0.8404 1598.0 0.4929 0.505 0.4282 0.4387 TA2005C5200 2019-12-30 NaN 0.500 939550.0 1634.0 NaN 107.5000 115.0000 349.0000 4926.0 113.5000 124.500 114.0000 97.0000
```

### 2、获取期权分钟行情

历史分钟数据可通过API-get_price获取，get_price 参数设置请参考该API

查询50ETF期权10000001在2015年12月12日分钟行情数据

```python
get_price(['10000001'],'20150212','20150212','1m')
```

```
Out[2] total_turnover volume trading_date low open_interest close high open datetime 2015-02-12 09:31:00 34268.0 16.0 2015-02-12 0.2141 1639.0 0.2143 0.2143 0.2141 2015-02-12 09:32:00 4262.0 2.0 2015-02-12 0.2131 1640.0 0.2131 0.2131 0.2131 2015-02-12 09:33:00 0.0 0.0 2015-02-12 0.2131 1640.0 0.2131 0.2131 0.2131 ...
```

### 3、获取期权tick行情

期权合约历史tick数据可通过API-get_price获取，get_price 参数设置请参考该API

查询CU期权CU1906C49000在2018年11月30日tick行情数据

```python
get_price('CU1906C49000','20181130','20181130','tick')
```

```
Out[3] trading_date open last high low prev_settlement prev_close volume open_interest total_turnover ... a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v change_rate datetime 2018-11-29 20:59:00.500 2018-11-30 0.0 2520.0 0.0 0.0 2520.0 2520.0 0.0 42.0 0.0 ... 0.0 0.0 0.0 0.0 1.0 0.0 0.0 0.0 0.0 0.0 2018-11-29 21:00:48.000 2018-11-30 0.0 2520.0 0.0 0.0 2520.0 2520.0 0.0 42.0 0.0 ... 0.0 0.0 0.0 0.0 1.0 0.0 0.0 0.0 0.0 0.0 2018-11-29 21:00:52.000 2018-11-30 0.0 2520.0 0.0 0.0 2520.0 2520.0 0.0 42.0 0.0 ... 0.0 0.0 0.0 0.0 2.0 0.0 0.0 0.0 0.0 0.0 ...
```

### 4、获取期权实时分钟线

期权实时分钟线可通过API-get_price与API-current_minute获取，get_price与current_minute参数设置请参考上述API

查询300ETF期权10002886与10002871最近的实时分钟线

```python
current_minute(['10002886','10002871'])
```

```
Out[4] close high low open open_interest total_turnover trading_date volume order_book_id datetime 10002886 2021-04-15 11:30:00 0.0255 0.0255 0.0255 0.0255 35599 0.00 20210415 0 10002871 2021-04-15 11:30:00 0.0661 0.0668 0.0661 0.0668 5616 1341.17 20210415 2
```

查询股指期权IO2106C3600实时分钟线

```python
get_price('IO2106C3600','20210415','20210415','1m')
```

```
Out[5] total_turnover volume trading_date low open_interest close high open datetime 2021-04-15 09:31:00 0.0 0.0 2021-04-15 1306.8 41.0 1306.8 1306.8 1306.8 2021-04-15 09:32:00 0.0 0.0 2021-04-15 1306.8 41.0 1306.8 1306.8 1306.8 2015-04-15 09:33:00 0.0 0.0 2021-04-15 1306.8 41.0 1306.8 1306.8 1306.8 2021-04-15 09:34:00 0.0 0.0 2021-04-15 1306.8 41.0 1306.8 1306.8 1306.8 2021-04-15 09:35:00 128720.0 1.0 2021-04-15 1287.2 40.0 1287.2 1287.2 1287.2 ...
```

### 5、获取期权实时tick数据

期权实时tick数据可通过API-get_price、API-current_snapshot、API-get_live_ticks获取，具体参数设置请参考上述API

查询300ETF期权10002886当前快照数据

```python
current_snapshot(['10002886'])
```

```
Out[6] Tick(ask_vols: [70, 42, 3, 10, 10], asks: [0.0224, 0.0225, 0.0227, 0.0228, 0.023], bid_vols: [50, 10, 32, 10, 20], bids: [0.0221, 0.022, 0.0219, 0.0218, 0.0217], datetime: 2021-04-15 14:10:13.350000, high: 0.0265, iopv: nan, last: 0.0224, limit_down: 0.0001, limit_up: 0.3891, low: 0.0195, num_trades: 0, open: 0.0195, open_interest: 36830, order_book_id: 10002886, prev_close: 0.0182, prev_iopv: nan, prev_settlement: 0.018, total_turnover: 1196785.76, trading_phase_code: T 01, volume: 5109)
```

查询300ETF期权10002875当天实时tick数据

```python
get_price('10002875','20210415','20210415','tick')
```

```
Out[7] trading_date open last high low prev_settlement prev_close volume open_interest total_turnover ... a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v change_rate datetime 2021-04-15 09:15:00.510 2021-04-15 0.0000 0.2465 0.000 0.0000 0.2465 0.2465 0.0 2418.0 0.00 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.000000 2021-04-15 09:15:01.520 2021-04-15 0.0000 0.2465 0.000 0.0000 0.2465 0.2465 0.0 2418.0 0.00 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.000000 2021-04-15 09:15:02.020 2021-04-15 0.0000 0.2465 0.000 0.0000 0.2465 0.2465 0.0 2418.0 0.00 ... 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.0 0.000000 ...
```

查询股指期权IO2106C3600在20210415100101-20210415100102之间的实时tick数据

```python
get_live_ticks('IO2106C3600',start_dt='20210415100101',end_dt='20210415100103')
```

```
Out[8] trading_date update_time open last high low limit_up limit_down prev_settlement prev_close ... a1_v a2_v a3_v a4_v a5_v b1_v b2_v b3_v b4_v b5_v order_book_id datetime IO2106C3600 2021-04-15 10:01:01.503 2021-04-15 10:01:01.500000 1287.2 1287.2 1287.2 1287.2 1801.6 805.6 1303.6 1306.8 ... 7 0 0 0 0 6 0 0 0 0 2021-04-15 10:01:02.012 2021-04-15 10:01:02 1287.2 1287.2 1287.2 1287.2 1801.6 805.6 1303.6 1306.8 ... 7 0 0 0 0 3 0 0 0 0 2021-04-15 10:01:02.506 2021-04-15 10:01:02.500000 1287.2 1287.2 1287.2 1287.2 1801.6 805.6 1303.6 1306.8 ... 7 0 0 0 0 3 0 0 0 0
```

### options.get_contracts - 筛选期权合约

`options.get_contracts(underlying, option_type=None, maturity=None, strike=None, trading_date=None)`


#### 参数

| 参数         | 类型          | 说明                                                         |
| :----------- | :------------ | :----------------------------------------------------------- |
| `underlying` | `str`         | 期权标的。可以填写`'M'`代表期货品种的字母；也可填写`'M1901'`这种具体合约代码。必须填写，只支持单一品种或合约代码输入 |
| `option_type`| `str`         | `'C'`代表认购期权；`'P'`代表认沽期权合约。默认返回全部类型  |
| `maturity`   | `str` OR `int`| 到期月份。例如1811代表期权18年11月到期（而不是标的期货的到期时间）。默认返回全部到期月份 |
| `strike`     | `float`       | 行权价。查询时向左靠档匹配（例如，当前最高行权价是1000，则输入大于1000的行权价都会向左靠档至1000）。默认返回全部行权价 |
| `trading_date`| `str` OR `int`| 查询日期。默认返回全部数据                                   |

#### 返回

返回符合条件的期权order_book_id list；如果无符合条件期权则返回`[]`空list

#### 范例

查询铜期权2019年2月到期行权价是52000的期权

```python
options.get_contracts(underlying='CU', maturity='1902', strike=52000)
```

```
Out[9] ['CU1903P52000', 'CU1903C52000']
```

查询50ETF期权2016-11-29这一天行权价是2.006的期权合约

```python
options.get_contracts(underlying='510050.XSHG', strike=2.006, trading_date='20161129')
```

```
Out[10] ['10000615', '10000620']
```

### options.get_greeks - 获取期权风险指标

`options.get_greeks(order_book_ids, start_date, end_date,fields=None,model='implied_forward',price_type='close',frequency='1d')`


#### 参数

| 参数           | 类型                                 | 注释                                                         |
| :------------- | :----------------------------------- | :----------------------------------------------------------- |
| `order_book_ids`| `str`                                | 合约代码                                                     |
| `start_date`   | `str` `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 查询的开始日期，必须填写                                     |
| `end_date`     | `str` `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 查询的结束日期                                               |
| `model`        | `str`                                | 计算方法,默认为`'implied_forward'`。针对BS模型中的标的物远期利率，提供两种计算方法：（1）`'last'`-以国债同期限收益率作为无风险利率，标的物分红定为0计算远期利率；（2）`'implied_forward'`-基于期权的风险平价关系（put-call parity），推算市场数据隐含的标的物远期利率 |
| `fields`       | `str` OR `str list`                  | 查询字段，有效值见下方                                       |
| `price_type`   | `str`                                | 计算使用价格，默认为`'close'`，使用期权收盘价计算衍生指标。可选`'settlement'`， 使用期权结算价计算衍生指标 |
| `frequency`    | `str`                                | 数据的频率。默认为`'1d'`。`1m` - 分钟级别(仅支持股指期货,`price_type`必须为`'close'`) `1d` - 日级别 |

#### 返回

`pandas DataFrame`

| 字段    | 说明                                                         |
| :------ | :----------------------------------------------------------- |
| `iv`    | 从场内期权价格反推对应的标的物收益波动率（基于BS模型计算）   |
| `Delta` | 期权价格对于标的物价格变化的一阶敏感度（基于BS模型计算）     |
| `Gamma` | 期权价格对于标的物价格变化的二阶敏感度（基于BS模型计算）     |
| `Vega`  | 期权价格对于隐含波动率的一阶敏感度（基于BS模型计算）         |
| `Theta` | 期权价格对于合约待偿期变化的一阶敏感度，为每日历年的Theta（基于BS模型计算） |
| `Rho`   | 期权价格对于无风险利率变化的一阶敏感度（基于BS模型计算）     |

补充说明1

米筐希腊字母均为小数形式。例如，期权vega=12，当波动率增加1%时，期权价格会相应增加大约0.01 * 12 = 0.12。

补充说明2

在使用BS模型计算隐含波动率和希腊字母时，需要给定5个参数：

行权价（K）、标的物价格（S）、待偿期（t）、期权价格（C），这4个参数可从场内期权的合约基础信息或行情数据中获得；

无风险利率和标的物预期分红率的差值（r-q），该参数无法从合约基础信息或行情数据获得上述r和q的取值决定了隐含波动率和希腊字母的计算结果。目前米筐提供两种计算方案：

（1）r取值为同期限国债收益率，q取值为0。该方案为业界常见方案，计算结果也和交易所计算结果相近。

（2) r-q基于风险平价等式和远期价格定义算得。该方案保证参数取值符合期权风险平价等式，具有更好的理论意义和模型稳定性。具体计算如下：

认购-认沽期权平价公式如下：

$$
( C - P ) / ( F - K ) = e^{ - ( r - q ) * t }
$$

其中C和P分别为认购期权和认沽期权价格；F和K分别为标的物远期价格和行权价。再考虑远期价格定义：

$$
S / F = e^{ - ( r - q ) * t }
$$

其中S为标的物价格。联立上述两式可得隐含远期利率：

$$
r - q = \ln( K / ( S - ( C - P ) ) ) / t
$$

#### 范例

查询一个期权的风险指标

```python
In [13]: rq.options.get_greeks('10001739','20190601','20190605',fields=None,model='implied_forward')
Out[13]: iv delta gamma vega theta rho order_book_id trading_date 10001739 2019-06-03 0.220510 -0.754164 2.076502 0.216619 -0.387216 -0.138543 2019-06-04 0.231736 -0.778680 1.918539 0.198550 -0.357089 -0.136513 2019-06-05 0.228281 -0.792963 1.920322 0.186324 -0.321590 -0.132373
```

查询一组期权的风险指标

```python
In [12]: rq.options.get_greeks(['10001961','10001979'],'20191101','20191105',fields=None,model='implied_forward')
Out[12]: iv delta gamma vega theta rho order_book_id trading_date 10001961 2019-11-01 1.660567e-01 0.976153 0.415296 0.045758 -0.076153 0.194061 2019-11-04 1.190082e-01 0.998967 0.037848 0.002673 0.004237 0.176264 2019-11-05 4.612581e-08 1.000000 0.000000 0.000000 -0.014498 0.168714 10001979 2019-11-01 1.905027e-01 0.981533 0.291578 0.036856 -0.071821 0.191683 2019-11-04 1.716458e-01 0.994844 0.112385 0.011449 -0.008984 0.172303 2019-11-05 1.435756e-09 1.000000 0.000000 0.000000 -0.014239 0.165702
```

查询一个期权的分钟级别风险指标

```python
In [12]: rqdatac.options.get_greeks('IO2412C2800',20240401,20240506,frequency='1m')
Out[12]: iv delta gamma vega theta rho order_book_id datetime IO2412C2800 2024-04-01 09:31:00 1.304169e-01 0.980268 0.000121 144.678177 58.190402 -2002.412297 2024-04-01 09:32:00 1.248750e-01 0.984059 0.000106 120.727735 65.303168 -2014.764372 2024-04-01 09:33:00 1.206514e-01 0.986700 0.000093 103.369229 70.040171 -2023.243489 2024-04-01 09:34:00 1.197785e-01 0.987217 0.000091 99.896613 70.949117 -2024.893811 2024-04-01 09:35:00 7.200824e-08 1.000000 0.000000 0.000000 79.922641 -2058.613823 ... ... ... ... ... ... ... 2024-05-06 14:56:00 2.859633e-08 1.000000 0.000000 0.000000 30.778390 -1761.009874 2024-05-06 14:57:00 1.491064e-08 1.000000 0.000000 0.000000 31.170894 -1761.161984 2024-05-06 14:58:00 6.724471e-08 1.000000 0.000000 0.000000 31.407937 -1761.253841 2024-05-06 14:59:00 6.724471e-08 1.000000 0.000000 0.000000 31.407937 -1761.253841 2024-05-06 15:00:00 2.208110e-08 1.000000 0.000000 0.000000 29.153061 -1760.379859 [5040 rows x 6 columns]
```

### options.get_contract_property - 获取期权合约属性（时间序列）

`options.get_contract_property(order_book_ids, start_date=None, end_date=None, fields=None, market='cn')`



获取期权每日合约属性数据，仅支持交易所ETF期权

注：和商品期权不同，ETF期权执行价、合约乘数等数据存在因标的分进行调整的情况，详情请参考ETF期权合约条款，通过上述API可以追踪到它们的变动

#### 参数

| 参数             | 类型                  | 说明                                                         |
| :--------------- | :-------------------- | :----------------------------------------------------------- |
| `order_book_ids` | `str` or `str_list`   | 合约代码                                                     |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                                                     |
| `fields`         | `str` OR `str list`   | 字段名称                                                     |
| `market`         | `str`                 | 默认是中国内地市场(`'cn'`)。可选`'cn'` - 中国内地市场；    |

#### 返回

返回`DataFrame`

| 字段             | 说明     |
| :--------------- | :------- |
| `order_book_id`  | 合约代码 |
| `trading_date`   | 交易日   |
| `product_name`   | 期权字母简称 |
| `symbol`         | 合约简称 |
| `contract_multiplier` | 合约乘数 |
| `strike_price`   | 期权行权价 |

#### 范例

查询ETF期权10002752 20210115到20210118之间的合约属性数据

```python
options.get_contract_property(order_book_ids='10002752', start_date='20210115',end_date='20210118')
```

```
Out[11] contract_multiplier product_name strike_price symbol order_book_id trading_date 10002752 2021-01-15 10000.0 510300P2103M04400 4.400 300ETF沽3月4400 2021-01-18 10132.0 510300P2103A04400 4.343 XD300ETF沽3月4343A
```

查询ETF期权90000493和10002752在20210115 -20210118之间的执行价格数据

```python
options.get_contract_property(order_book_ids=['90000493','10002752'], start_date='20210115',end_date='20210118',fields=['strike_price'])
```

```
Out[12] strike_price order_book_id trading_date 10002752 2021-01-15 4.400 2021-01-18 4.343 90000493 2021-01-15 4.300 2021-01-18 4.300
```

### options.get_dominant_month - 获取期权主力月份

`options.get_dominant_month(underlying_symbol, start_date=None, end_date=None, rule=0)`



获取商品期权一段时间的主力月份列表。(目前仅支持商品期权)

当同品种其他月份的持仓量与成交量在收盘后超过当前主力月份时，从第二个交易日开始进行主力月份的切换。日内不会进行主力月份的切换。

#### 参数

| 参数             | 类型                  | 说明                                                         |
| :--------------- | :-------------------- | :----------------------------------------------------------- |
| `underlying_symbol`| `str` or `str_list`   | 期权标的代码，例`'CU'`                                     |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期                                                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期                                                     |
| `rule`           | `int`                 | 默认`rule=0`，每个月份只能做一次主力月份，不会重复出现。当`rule=1`时，主力月份的选取只考虑持仓量与成交量条件。 |
| `rank`           | `int`                 | 默认`rank=1`。1-主力月份，2-次主力月份                         |

#### 返回

返回`Pandas.Series` - 主力月份列表

#### 范例

查询CU期权2023-07-01到2023-07-26之间的主力月份数据

```python
rqdatac.options.get_dominant_month('CU',20230701,20230726)
```

```
Out[13] date 20230703 CU2308 20230704 CU2308 20230705 CU2308 20230706 CU2308 20230707 CU2308 20230710 CU2308 20230711 CU2308 20230712 CU2308 20230713 CU2308 20230714 CU2308 20230717 CU2308 20230718 CU2308 20230719 CU2308 20230720 CU2308 20230721 CU2308 20230724 CU2308 20230725 CU2308 20230726 CU2308 Name: dominant, dtype: object
```


# 指数、场内基金行情数据说明

可获取指数、场内基金合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price。

## 指数列表

目前所支持的指数列表可以参考[指数数据表](https://www.ricequant.com/doc/rqdata/api/index-data-table.html)。

个别API 支持范围有所区别，以API 标注为准。

## index_indicator - 获取指数每日估值指标

`index_indicator(order_book_ids,start_date,end_date,fields)`



获取指数每日估值指标。目前仅支持部分市值加权指数的市盈率和市净率，支持的市值指数列表可见 https://assets.ricequant.com/vendor/rqdata/market-index-list_20230522.xlsx 。

**计算逻辑：**

*   市盈率ttm：对应成分股的市值之和/ 成分股最新一期的滚动归属母公司净利润之和。
*   市盈率lyr：对应成分股的市值之和/ 成分股最新年报归属母公司净利润之和。
*   市净率ttm：对应成分股的市值之和/ 成分股最新一期的滚动归属母公司权益之和。
*   市净率lyr：对应成分股的市值之和/ 成分股最新年报归属母公司权益之和。
*   市净率lf：对应成分股的市值之和/ 成分股最新一期的归属母公司权益之和。

### 参数

| 参数             | 类型                                        | 说明                                    |
| :--------------- | :------------------------------------------ | :-------------------------------------- |
| `order_book_ids` | str or str list                             | 可输入order_book_id, order_book_id list。 |
| `start_date`     | str, datetime.date, datetime.datetime, pandasTimestamp | 开始日期，默认2016-01-04                  |
| `end_date`       | str, datetime.date, datetime.datetime, pandasTimestamp | 结束日期，默认2016-12-31                  |
| `fields`         | str or str list                             | 对应估值字段。默认为所有字段。            |

### 返回

单个order_book_id，多个fields 的时候返回DataFrame 多个order_book_id，多个fields 的时候返回Multi-index DataFrame

### 范例

```python
[In] index_indicator(['000016.XSHG','000300.XSHG'],start_date=20170402,end_date=20170408)
[Out] pb_lf pb_lyr pb_ttm pe_lyr pe_ttm
order_book_id trade_date
000016.XSHG 2017-04-05 1.183987 1.196851 1.225406 10.854710 10.778091
2017-04-06 1.183772 1.195776 1.225508 10.842157 10.779690
2017-04-07 1.185690 1.197714 1.227494 10.859727 10.797159
000300.XSHG 2017-04-05 1.503460 1.533702 1.563953 13.899681 13.773018
2017-04-06 1.505061 1.534583 1.565892 13.904718 13.790629
2017-04-07 1.508388 1.537986 1.569359 13.935358 13.820774
```


## index_components - 获取指数成分股列表

`index_components(order_book_id, date=None, start_date, end_date, market='cn',return_create_tm=False)`


获取某一指数的股票构成列表，也支持指数的历史构成查询。

### 参数

| 参数                 | 类型                                        | 说明                                                                                                                                                                                                                                                                                  |
| :------------------- | :------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `order_book_id`      | str                                         | 指数代码，传入order_book_id，例如'000001.XSHG'。                                                                                                                                                                                                                                      |
| `date`               | str, datetime.date, datetime.datetime, pandas Timestamp | 查询日期，默认为最新记录日期                                                                                                                                                                                                                                          |
| `start_date`         | str, datetime.date, datetime.datetime, pandas Timestamp | 指定开始日期，不能和date 同时指定                                                                                                                                                                                                                                     |
| `end_date`           | str, datetime.date, datetime.datetime, pandas Timestamp | 指定结束日期, 需和start_date 同时指定并且应当不小于开始日期                                                                                                                                                                                                               |
| `market`             | str                                         | 默认是中国市场('cn')，目前仅支持中国市场                                                                                                                                                                                                                                      |
| `return_create_tm` | bool                                        | 设置为True 的时候，传入date, 返回tuple, 第一个元素是列表, 第二个元素是入库时间; 传入start_date, end_date, 返回dict, 其中key 是日期, value 的话是tuple, 其中第一个元素是列表, 第二个元素是入库时间 |

### 返回

构成该指数股票的order_book_id list

### 范例

```python
[In] index_components('000001.XSHG')
[Out] ['600000.XSHG', '600004.XSHG', ...]
[In] index_components('000001.XSHG', date='2015-01-01')
[Out] ['600613.XSHG', '600239.XSHG', ...]
[In] index_components('000300.XSHG',start_date = '20190701',end_date ='20190706')
[Out] {datetime.datetime(2019, 7, 1, 0, 0): ['300433.XSHE', '601901.XSHG', '300498.XSHE', ... '300070.XSHE'], datetime.datetime(2019, 7, 5, 0, 0): ['300433.XSHE', '601901.XSHG', ... '300070.XSHE']}
[In] index_components('000300.XSHG',date=20220701,return_create_tm=True)
[Out] (['601009.XSHG', '002821.XSHE', '600030.XSHG', '601919.XSHG', '601225.XSHG' ... '300661.XSHE', '688169.XSHG'], Timestamp('2022-06-13 06:00:13'))
```



## index_weights - 获取指数历史权重

`index_weights(order_book_id, date=None)`


获取某一指数的历史构成以及权重。注意，该数据为月度更新。

### 参数

| 参数            | 类型                                        | 说明                                                                                              |
| :-------------- | :------------------------------------------ | :------------------------------------------------------------------------------------------------ |
| `order_book_id` | str                                         | 指数代码，可传入order_book_id，例如'000001.XSHG'或'沪深300'。目前所支持的指数列表可以参考[指数数据表](https://www.ricequant.com/doc/rqdata/api/index-data-table.html) |
| `date`          | str, datetime.date, datetime.datetime, pandas Timestamp | 查询日期，默认为最新记录日期                                                                      |

### 返回

pandas Series，每只股票在指数中的构成权重。

### 范例

获取上证50 指数在距离20160801 最近的一次指数构成结果：

```python
[In] index_weights('000016.XSHG', '20160801')
[Out] Order_book_id
600000.XSHG    0.03750
600010.XSHG    0.00761
600016.XSHG    0.05981
600028.XSHG    0.01391
600029.XSHG    0.00822
600030.XSHG    0.03526
600036.XSHG    0.04889
600050.XSHG    0.00998
600104.XSHG    0.02122
```



## index_weights_ex - 获取指数历史日度权重

`rqdatac.index_weights_ex(order_book_id, date=None, start_date=None, end_date=None, market='cn')`



获取某一指数的历史构成以及日度权重。

注意：该数据为自算权重。目前仅支持上证50、沪深300、 中证500、 中证800、 中证1000、中证2000 指数、科创50、中证红利。

### 参数

| 参数            | 类型                                        | 说明                                                                                                                                                                                  |
| :-------------- | :------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `order_book_id` | str                                         | 指数代码，可传入order_book_id，目前仅支持上证50、沪深300、 中证500、 中证800、 中证1000、中证2000 指数、科创50、中证红利。                                                                  |
| `date`          | str, datetime.date, datetime.datetime, pandas Timestamp | 查询日期，默认为最新记录日期                                                                                                                                                          |
| `start_date`    | str, datetime.date, datetime.datetime, pandas Timestamp | 指定开始日期，不能和date 同时指定                                                                                                                                                     |
| `end_date`      | str, datetime.date, datetime.datetime, pandas Timestamp | 指定结束日期, 需和start_date 同时指定并且应当不小于开始日期                                                                                                                           |

### 返回

pandas Series，每只股票在指数中的构成权重。

### 范例

获取上证50 指数在20160801 的指数权重：

```python
[In] index_weights_ex('000016.XSHG', '20160801')
[Out] order_book_id
600000.XSHG    0.03787
600010.XSHG    0.00761
600016.XSHG    0.06042
600028.XSHG    0.01399
600029.XSHG    0.00799
600030.XSHG    0.03523
600036.XSHG    0.04912
600050.XSHG    0.00985
600104.XSHG    0.02123
600109.XSHG    0.00644
600111.XSHG    0.00795
600518.XSHG    0.01369
600519.XSHG    0.04275
600637.XSHG    0.00838
600795.XSHG    0.00974
600837.XSHG    0.03405
600887.XSHG    0.03028
600893.XSHG    0.00750
601988.XSHG    0.01957
600048.XSHG    0.01743
601006.XSHG    0.01014
601398.XSHG    0.02913
601628.XSHG    0.00957
601166.XSHG    0.05771
601318.XSHG    0.09693
601998.XSHG    0.00511
601328.XSHG    0.04303
601919.XSHG    0.00541
601169.XSHG    0.02835
601088.XSHG    0.00820
601857.XSHG    0.00972
601390.XSHG    0.01057
601601.XSHG    0.02326
601186.XSHG    0.00840
601668.XSHG    0.02394
601766.XSHG    0.02271
601788.XSHG    0.00540
601727.XSHG    0.00632
600999.XSHG    0.01333
601989.XSHG    0.01651
601688.XSHG    0.01698
601288.XSHG    0.03306
601377.XSHG    0.00990
601818.XSHG    0.01704
601669.XSHG    0.00661
601800.XSHG    0.00450
601336.XSHG    0.00917
601211.XSHG    0.00739
600958.XSHG    0.01188
601985.XSHG    0.00862 dtype: float64
```

# 基金行情数据

仅有场内基金提供五档行情，日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price。

*场外基金无该数据

# 基金基础数据，净值数据，以及衍生数据

以下基金API 中，涉及order_book_ids 参数无需填后缀，比如000001、159001。

## fund.instruments - 获取基金基础信息

```python
fund.instruments(order_book_ids)
```

**参数**

| 参数           | 类型           | 注释     |
| -------------- | -------------- | -------- |
| order_book_ids | str OR str list | 基金代码 |

**返回**

基金instrument 对象或instrument list

| 字段                     | 类型    | 说明                                                     |
| ------------------------ | ------- | -------------------------------------------------------- |
| order_book_id            | str     | 合约代码                                                 |
| transition_time          | integer | 合约代码复用次数，代码从来都属于唯一个基金，则transition_time 为零 |
| symbol                   | str     | 证券的名称                                               |
| fund_manager             | str     | 基金经理                                                 |
| establishement_date      | str     | 基金成立日                                               |
| listed_date              | str     | 基金上市日                                               |
| stop_date                | str     | 基金终止日                                               |
| de_listed_date           | str     | 基金退市日                                               |
| benchmark                | str     | 业绩比较基准                                             |
| latest_size              | float   | 最新基金规模（单位：元）                                 |
| abbrev_symbol            | str     | 基金简称                                                 |
| object                   | str     | 投资目标                                                 |
| investment_scope         | str     | 投资范围                                                 |
| min_investment           | str     | 基金最小投资额                                           |
| type                     | str     | 合约的资产类型                                           |
| fund_type                | str     | 基金类型。债券型- Bond , 股票型- Stock , 混合型- Hybrid , 货币型- Money , 短期理财型- ShortBond , 股票指数- StockIndex , 债券指数- BondIndex , 联接基金- Related , QDII - QDII |
| least_redeem             | str     | 最小申赎份额，仅对ETF 展示                                 |
| amc                      | str     | 基金公司                                                 |
| amc_id                   | str     | 基金公司ID                                               |
| accrued_daily            | bool    | 货币基金收益分配方式(份额结转方式) 按日结转还是其他结转  |
| exchange                 | str     | 交易所，'XSHE' - 深交所, 'XSHG' - 上交所                     |
| round_lot                | int     | 一手对应多少份，通常公募基金一手是1 份                     |
| trustee                  | int     | 基金托管人代码                                           |
| redeem_amount_days       | int     | 赎回款到账天数                                           |
| confirmation_days        | int     | 申赎份额确认天数                                         |

**范例**

查询某基金合约信息

```python
In [20]: fund.instruments('000014')
Out[20]: Instrument(order_book_id='000014', benchmark='100.0％×一年定期存款收益率(税后)加1.2%', issuer='华夏基金管理有限公司', establishment_date='2013-03-19', listed_date='2013-03-19', de_listed_date='0000-00-00', stop_date='0000-00-00', symbol='华夏聚利债券', fund_manager='何家琪', fund_type='Bond', accrued_daily=False, latest_size=667046240.1, type='PublicFund', transition_time=1, exchange='', amc_id='41511', amc='华夏基金管理有限公司', abbrev_symbol='华夏聚利',..., min_investment=1.0, object='在控制风险的前提下，追求较高的当期收入和总回报。', trustee=3037, redeem_amount_days=7, confirmation_days=1, round_lot=1.0)
```

当某个旧基金的合约代码被重复使用，如何查找它的历史合约信息

```python
In [10]: fund.instruments('000014_CH0')
Out[7]: Instrument(order_book_id='000014_CH0', benchmark='100.0％×一年定期存款收益率(税后)加1.2%', issuer='华夏基金管理有限公司', establishment_date='2013-03-19', listed_date='2013-03-19', symbol='华夏一年债', accrued_daily=False, fund_type='Bond', transition_time=0, de_listed_date='2014-03-19', stop_date='2014-03-19', latest_size=4016611053.94, type='PublicFund', exchange='', amc_id='41511', amc='华夏基金管理有限公司', round_lot=1.0)
```

## fund.all_instruments - 获取所有公募基金信息

```python
fund.all_instruments(date=None)
```

**参数**

| 参数 | 类型                                     | 注释                                   |
| ---- | ---------------------------------------- | -------------------------------------- |
| date | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，默认为当前日期上一交易日。过滤掉在该日期尚未上市交易的基金合约 |

**返回**

pandas DataFrame

| 字段          | 类型    | 说明                                                     |
| ------------- | ------- | -------------------------------------------------------- |
| order_book_id | str     | 合约代码                                                 |
| symbol        | str     | 证券的简称                                               |
| issuer        | str     | 基金公司                                                 |
| fund_manager  | str     | 基金经理                                                 |
| listed_date   | str     | 发行日期                                                 |
| benchmark     | str     | 业绩比较基准                                             |
| latest_size   | float   | 最新基金规模（单位：元）                                 |
| fund_type     | str     | 基金类型。债券型- Bond , 股票型- Stock , 混合型- Hybrid , 货币型- Money , 短期理财型- ShortBond , 股票指数- StockIndex , 债券指数- BondIndex , 联接基金- Related , QDII - QDII |

**范例**

```python
In [20]: fund.all_instruments().head()
Out[20]: order_book_id listed_date issuer symbol fund_type \
0 233001 2004-03-26 摩根士丹利华鑫基金大摩基础行业混合Hybrid
1 165519 2013-08-16 信诚基金信诚中证800医药指数分级StockIndex
2 004234 2017-01-19 中欧基金中欧数据挖掘混合C Hybrid
3 370026 2013-02-04 上投摩根基金上投轮动添利债券C Bond
4 519320 2016-05-04 浦银安盛基金浦银安盛幸福聚利定开债A Other fund_manager latest_size benchmark
0 孙海波1.318854e+08 沪深300指数×55%+ 中证综合债券指数×45%
1 杨旭2.371657e+08 95%×中证800制药与生物科技指数收益率+5%×金融同业存款利率2 曲径NaN 沪深300指数收益率×60%+中债综合指数收益率×40%
3 唐瑭8.183768e+06 中证综合债券指数4 刘大巍3.018930e+09 一年期定期存款利率(税后)+1.4%
```

## fund.get_transition_info - 获取基金转型信息

```python
fund.get_transition_info('order_book_ids')
```

**参数**

| 参数           | 类型       | 注释     |
| -------------- | ---------- | -------- |
| order_book_ids | str or list | 合约代码 |

**返回**

pandas DataFrame

| 字段            | 类型 | 说明                                           |
| --------------- | ---- | ---------------------------------------------- |
| order_book_id   | str  | 基金合约号                                     |
| transition_time | str  | 转型次数。'0'-原始基金，'1'-第一次转型后基金，'2'-第二次转型后基金，以此类推 |
| abbrev_symbol   | str  | 基金简称                                       |
| symbol          | str  | 基金全称                                       |
| amc             | str  | 基金公司名称                                   |
| establishment_date | str  | 成立日                                         |
| listed_date     | str  | 上市日                                         |
| de_listed_date  | str  | 退市日                                         |
| stop_date       | str  | 终止日                                         |
| accrued_daily   | str  | 货币基金收益分配方式(份额结转方式) 按日结转还是其他结转 |

**范例**

```python
In [4]: fund.get_transition_info('000014')
Out[4]: abbrev_symbol accrued_daily amc ... listed_date stop_date symbol order_book_id transition_time ...
000014 0 华夏一年债False 华夏基金管理有限公司... 2013-03-19 2014-03-19 华夏一年定期开放债券1 华夏聚利False 华夏基金管理有限公司... 2014-03-19 0000-00-00 华夏聚利债券```

## fund.get_nav - 获取基金净值信息

```python
fund.get_nav(order_book_ids, start_date=None, end_date=None, fields=None, expect_df=False)
```

**参数**

| 参数           | 类型                                     | 注释             |
| -------------- | ---------------------------------------- | ---------------- |
| order_book_ids | str or list                              | 基金代码         |
| start_date     | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的开始日期，默认为所有净值数据 |
| end_date       | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的结束日期   |
| fields         | str OR str list                          | 查询字段，有效值见下方 |
| expect_df      | boolean                                  | 默认返回原有的数据结构。如果调为真，则返回pandas dataframe |

**返回**

pandas DataFrame

| 字段                | 类型  | 说明             |
| ------------------- | ----- | ---------------- |
| unit_net_value      | float | 单位净值         |
| acc_net_value       | float | 累计单位净值     |
| adjusted_net_value  | float | 复权净值         |
| change_rate         | float | 涨跌幅           |
| daily_profit        | float | 每万元收益       |
| weekly_yield        | float | 7 日年化收益率   |

**范例**

```python
In [11]: fund.get_nav(['000003','519505'],expect_df=True,start_date=20200910)
Out[11]: unit_net_value acc_net_value change_rate adjusted_net_value daily_profit weekly_yield order_book_id datetime
000003 2020-09-10 0.912 1.122 -0.009771 1.072268 NaN NaN
2020-09-11 0.915 1.125 0.003289 1.075795 NaN NaN
2020-09-14 0.915 1.125 0.000000 1.075795 NaN NaN
2020-09-15 0.915 1.125 0.000000 1.075795 NaN NaN
2020-09-16 0.914 1.124 -0.001093 1.074619 NaN NaN
2020-09-17 0.911 1.121 -0.003282 1.071092 NaN NaN
519505 2020-09-10 1.000 1.000 0.000046 1.498024 0.4591 0.01971
2020-09-11 1.000 1.000 0.000046 1.498093 0.4607 0.01921
2020-09-13 1.000 1.000 0.000046 1.498231 0.9221 0.01934
2020-09-14 1.000 1.000 0.000047 1.498302 0.4698 0.01832
2020-09-15 1.000 1.000 0.000047 1.498373 0.4719 0.01713
2020-09-16 1.000 1.000 0.000048 1.498445 0.4837 0.01718
2020-09-17 1.000 1.000 0.000048 1.498517 0.4790 0.01729
```

## fund.get_transaction_status - 获取基金申购赎回相关信息

```python
fund.get_transaction_status(order_book_ids, fields=None, start_date=None, end_date=None, investor=None)
```

**参数**

| 参数           | 类型                                     | 注释                                     |
| -------------- | ---------------------------------------- | ---------------------------------------- |
| order_book_ids | str or list                              | 基金代码                                 |
| start_date     | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的开始日期，默认为所有净值数据       |
| end_date       | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的结束日期                           |
| fields         | str OR str list                          | 查询字段，有效值见下方                   |
| investor       | str                                      | 可选机构-'institution', 个人-'retail', 不输入默认返回机构 |
| market         | str                                      | 指定市场，目前仅有中国市场('cn')的基金数据 |

**返回**

pandas DataFrame

| 字段                    | 类型  | 说明                                                     |
| ----------------------- | ----- | -------------------------------------------------------- |
| subscribe_status        | str   | 订阅状态。开放- Open , 暂停- Suspended , 限制大额申购- Limited , 封闭期- Close |
| redeem_status           | str   | 赎回状态。开放- Open , 暂停- Suspended , 限制大额赎回- Limited , 封闭期- Close |
| issue_status            | str   | 募集状态。募集中- Open , 非募集期- Close                 |
| subscribe_upper_limit   | float | 申购上限（金额）                                         |
| subscribe_lower_limit   | float | 申购下限（金额）                                         |
| redeem_lower_limit      | float | 赎回下限（份额）                                         |
| redeem_upper_limit      | float | 赎回上限（份额），仅支持ETF                              |

**范例**

获取个人申赎状态及相关信息

```python
In [14]: fund.get_transaction_status('040001',start_date='2020-11-01',end_date='2020-11-10',investor='retail')
Out[14]: subscribe_status redeem_status issue_status subscribe_upper_limit subscribe_lower_limit redeem_lower_limit order_book_id datetime
040001 2020-11-01 Open Open Close None 1 1
2020-11-02 Open Open Close None 1 1
2020-11-03 Open Open Close None 1 1
2020-11-04 Open Open Close None 1 1
2020-11-05 Open Open Close None 1 1
2020-11-06 Open Open Close None 1 1
2020-11-07 Open Open Close None 1 1
2020-11-08 Open Open Close None 1 1
2020-11-09 Open Open Close None 1 1
2020-11-10 Open Open Close None 1 1
```

获取机构申赎状态及相关信息

```python
In [18]: fund.get_transaction_status('040001',start_date='2020-01-15',end_date='2020-01-25',investor='institution')
Out[18]: subscribe_status redeem_status issue_status subscribe_upper_limit subscribe_lower_limit redeem_lower_limit order_book_id datetime
040001 2020-01-15 Open Open Close None 1 1
2020-01-16 Limited Open Close 1e+06 1 1
2020-01-17 Limited Open Close 1e+06 1 1
2020-01-18 Limited Open Close 1e+06 1 1
2020-01-19 Limited Open Close 1e+06 1 1
2020-01-20 Limited Open Close 1e+06 1 1
2020-01-21 Limited Open Close 1e+06 1 1
2020-01-22 Open Open Close None 1 1
2020-01-23 Open Open Close None 1 1
2020-01-24 Open Open Close None 1 1
2020-01-25 Open Open Close None 1 1
```

## fund.get_credit_quality - 获取基金债券持仓投资信用评级信息

```python
fund.get_credit_quality(order_book_ids, date=None, market='cn')
```

从指定日期回溯，获取最近的基金债券投资信用评级信息。

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的债券投资信用评级数据。如不指定日期，则获取所有日期的债券投资评级数据 |
| market         | str                                      | 指定市场，目前仅有中国市场('cn')的基金数据                   |

**返回**

pandas DataFrame

| 字段                    | 说明             |
| ----------------------- | ---------------- |
| order_book_id           | 基金合约代码     |
| date                    | 持仓披露日期     |
| credit_rating           | 债券信用等级     |
| bond_sector_rating_type | 债券持仓评级类别 |
| market_value            | 持仓市值（单位：元） |

**范例**

```python
In [8]: fund.get_credit_quality(['000003','000033'],20200620)
Out[8]: credit_rating bond_sector_rating_type market_value order_book_id date
000003 2019-12-31 未评级债券短期信用评级6.721030e+06
2019-12-31 AAA 债券长期信用评级1.083061e+08
2019-12-31 AAA以下债券长期信用评级4.014485e+07
000033 2019-12-31 A-1 债券短期信用评级8.182628e+07
2019-12-31 未评级债券短期信用评级3.466683e+08
2019-12-31 AAA 债券长期信用评级4.052186e+09
2019-12-31 AAA以下债券长期信用评级1.284435e+09
2019-12-31 AAA 资产支持证券将长期信用评级2.036309e+07
```

## fund.get_etf_components - 获取ETF 成分股持有情况

```python
fund.get_etf_components(order_book_ids, trading_date=None, market='cn')
```

获取ETF 成分股持有情况

**参数**

| 参数           | 类型                                     | 注释                             |
| -------------- | ---------------------------------------- | -------------------------------- |
| order_book_ids | str or list                              | 基金代码                         |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，如不指定日期，则获取当天数据（注意仅交易日有效）。 |

**返回**

pandas DataFrame

| 字段                           | 说明                                                |
| ------------------------------ | --------------------------------------------------- |
| order_book_id                  | 成分股代码                                          |
| trading_date                   | 持仓日期                                            |
| stock_amount                   | 股票数量                                            |
| cash_substitute                | 现金替代规则                                        |
| cash_substitute_proportion     | 现金替代比例                                        |
| fixed_cash_substitute          | 固定现金金额（上交所字段，深交所是用申购替换金额填充该字段） |
| redeem_cash_substitute         | 赎回替代金额(元)（深交所）                          |

**范例**

```python
In [10]: fund.get_etf_components('510050.XSHG',trading_date=20190117)
Out[10]: trading_date order_book_id stock_code stock_amount cash_substitute cash_substitute_proportion fixed_cash_substitute
0 2019-01-17 510050.XSHG 600000 5600.0 允许 0.1 NaN
1 2019-01-17 510050.XSHG 600016 11900.0 允许 0.1 NaN
2 2019-01-17 510050.XSHG 600019 4300.0 允许 0.1 NaN
3 2019-01-17 510050.XSHG 600028 5800.0 允许 0.1 NaN
4 2019-01-17 510050.XSHG 600029 1600.0 允许0.1 NaN
5 2019-01-17 510050.XSHG 600030 3800.0 允许 0.1 NaN
6 2019-01-17 510050.XSHG 600036 4900.0 允许 0.1 NaN
7 2019-01-17 510050.XSHG 600048 3400.0 允许 0.1 NaN
8 2019-01-17 510050.XSHG 600050 4500.0 允许 0.1 NaN
...
```

## fund.get_etf_cash_components - 获取ETF 现金差额数据

```python
fund.get_etf_cash_components(order_book_ids,start_date,end_date)
```

获取ETF 基金现金差额数据

**参数**

| 参数          | 类型 | 注释                 |
| ------------- | ---- | -------------------- |
| order_book_id | str or list | 基金代码             |
| start_date    | date | 开始日期，默认为历史全部 |
| end_date      | date | 结束日期，默认为当日   |

**返回**

pandas DataFrame

| 字段                 | 类型  | 说明                               |
| -------------------- | ----- | ---------------------------------- |
| date                 | str   | 预估日期                           |
| pre_date             | str   | 交易日期                           |
| cash_component       | float | 现金差额（单位:元）                  |
| nav_per_basket       | float | 最小申购赎回单位资产净值（单位:元）  |
| est_cash_component   | float | 预估现金差额（单位:元）              |
| max_cash_ratio       | float | 现金替代上限                       |

**范例**

获取单个ETF 现金差额数据

```python
In[]:fund.get_etf_cash_components('510050.XSHG','20191201','20191205')
Out[]: order_book_id date cash_component est_cash_component max_cash_ratio nav_per_basket pre_date
510050.XSHG 2019-12-02 55959.24 31237.24 0.5 2646969.24 2019-11-29
2019-12-03 31488.64 35832.64 0.5 2608899.64 2019-12-02
2019-12-04 34927.55 33264.55 0.5 2617784.55 2019-12-03
2019-12-05 33230.56 35727.56 0.5 2610131.56 2019-12-04
```

获取多个ETF 现金差额数据

```python
In[]:fund.get_etf_cash_components(['510050.XSHG','510300.XSHG'],'20191201','20191205')
Out[]: order_book_id date cash_component est_cash_component max_cash_ratio nav_per_basket pre_date
510050.XSHG 2019-12-02 55959.24 31237.24 0.5 2646969.24 2019-11-29
2019-12-03 31488.64 35832.64 0.5 2608899.64 2019-12-02
2019-12-04 34927.55 33264.55 0.5 2617784.55 2019-12-03
2019-12-05 33230.56 35727.56 0.5 2610131.56 2019-12-04
510300.XSHG 2019-12-02 -34311.25 -29329.25 0.5 3501800.75 2019-11-29
2019-12-03 -28545.40 -30993.40 0.5 3508327.60 2019-12-02
2019-12-04 -30828.28 -29276.28 0.5 3522019.72 2019-12-03
2019-12-05 -29934.94 -32790.94 0.5 3520765.06 2019-12-04
```

## fund.get_split - 获取基金拆分信息

```python
fund.get_split(order_book_ids)
```

**参数**

| 参数           | 类型       | 注释     |
| -------------- | ---------- | -------- |
| order_book_ids | str or list | 基金代码 |

**返回**

pandas DataFrame

| 字段        | 说明             |
| ----------- | ---------------- |
| index       | 除权除息日       |
| split_ratio | 拆分折算比例，1 拆几 |

**范例**

```python
In [13]: fund.get_split('000246').head()
Out[13]: split_ratio
2013-11-01 1.00499349
2013-12-02 1.00453123
2014-01-02 1.00455316
2014-02-07 1.00456182
2014-03-03 1.00452639
```

## fund.get_dividend - 获取基金分红信息

```python
fund.get_dividend(order_book_ids)
```

**参数**

| 参数           | 类型       | 注释     |
| -------------- | ---------- | -------- |
| order_book_ids | str or list | 基金代码 |

**返回**

pandas DataFrame

| 字段                | 说明         |
| ------------------- | ------------ |
| index               | 除权除息日   |
| book_closure_date   | 权益登记日   |
| dividend_before_tax | 每份税前分红 |
| payable_date        | 分红发放日   |

**范例**

```python
In [11]: fund.get_dividend('050116')
Out[11]: book_closure_date payable_date dividend_before_tax
2012-01-17 2012-01-17 2012-01-19 0.002
2013-01-16 2013-01-16 2013-01-18 0.013
2015-01-14 2015-01-14 2015-01-16 0.028
```

## fund.get_ratings - 获取基金评级信息

```python
fund.get_ratings(order_book_ids, date=None)
```

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的数据。如不指定日期，则获取所有日期的数据 |

**返回**

pandas DataFrame

| 字段 | 说明       |
| ---- | ---------- |
| index | 评级日期   |
| zs   | 招商评级s  |
| sh3  | 上海证券评级三年期 |
| sh5  | 上海证券评级五年期 |
| jajx | 济安金信评级 |

**范例**

```python
In [16]: fund.get_ratings('202101')
Out[16]: zs sh3 sh5 jajx
2009-12-31 NaN NaN NaN 3.0
2010-03-31 NaN NaN NaN 3.0
2010-04-30 2.0 NaN NaN NaN
2010-06-30 NaN 3.0 4.0 1.0
2010-09-30 NaN 3.0 4.0 1.0
2010-12-31 NaN 2.0 4.0 1.0
```

## fund.get_units_change - 获取基金份额变动信息

```python
fund.get_units_change(order_book_ids, date=None)
```

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的数据。如不指定日期，则获取所有日期的数据 |

**返回**

pandas DataFrame

| 字段          | 说明               |
| ------------- | ------------------ |
| index         | 参考日期           |
| subscribe_units | 期间申购（单位：份） |
| redeem_units  | 期间赎回（单位：份） |
| units         | 期末总份额（单位：份） |
| net_asset     | 期末总净资产值(单位：元) |
| info_date     | 公告日期           |

**范例**

```python
In [20]: fund.get_units_change('001554')
Out[20]: subscribe_units redeem_units info_date units net_asset order_book_id datetime
001554 2015-06-30 NaN NaN NaT NaN 5000049.32
2015-09-30 71408891.69 37755554.39 2015-10-24 38653337.30 27630465.58
2015-12-31 19756969.98 20692807.21 2016-01-22 37717500.07 29573475.62
2016-03-31 17467356.40 16372818.76 2016-04-21 38812037.71 25200577.87
2016-06-30 21264325.34 15937884.63 2016-07-19 44138478.42 26526043.52
2016-09-30 37842604.31 32218403.07 2016-10-26 49762679.66 30466565.39
2016-12-31 19158060.76 25157817.68 2017-01-20 43762922.74 27451195.12
2017-03-31 12145314.55 18072618.82 2017-04-22 37835618.47 25060465.04
2017-06-30 27133401.79 21380926.25 2017-07-21 43588094.01 28659171.47
2017-09-30 12997778.42 19264758.68 2017-10-26 37321113.75 25039774.80
2017-12-31 10697714.94 12001467.35 2018-01-19 36017361.34 24185742.78
2018-03-31 11924561.78 12505966.08 2018-04-23 35435957.04 22390195.84
2018-06-30 4840103.56 16326724.31 2018-07-19 23949336.29 13476927.82
```

## fund.get_holder_structure - 获取基金持有人结构

```python
fund.get_holder_structure(order_book_ids, start_date=None, end_date=None)
```

**参数**

| 参数           | 类型 | 注释                   |
| -------------- | ---- | ---------------------- |
| order_book_ids | str or list | 基金代码               |
| start_date     | str  | 开始日期, 格式为'YYYY-mm-dd' |
| end_date       | str  | 结束日期, 格式为'YYYY-mm-dd' |

**返回**

pandas DataFrame

| 字段            | 说明                 |
| --------------- | -------------------- |
| order_book_id   | 基金合约             |
| date            | 报告期               |
| instl           | 机构投资者持有份额(份) |
| instl_weight    | 机构投资者持有份额占比(%) |
| retail          | 个人投资者持有份额(份) |
| retail_weight   | 个人投资者持有份额占比(%) |

**范例**

```python
In [10]: fund.get_holder_structure('000001','20190101','20200101')
Out[10]: instl instl_weight retail retail_weight order_book_id date
000001 2019-06-30 16995587.39 0.40 4.277759e+09 99.60
2019-12-31 18827745.40 0.45 4.142996e+09 99.55
```

## fund.get_benchmark - 获取基金基准

```python
fund.get_benchmark(order_book_ids)
```

**参数**

| 参数           | 类型       | 注释     |
| -------------- | ---------- | -------- |
| order_book_ids | str or list | 基金代码 |

**返回**

pandas DataFrame

| 字段         | 说明   |
| ------------ | ------ |
| order_book_id | 基金合约 |
| start_date   | 起始日   |
| end_date     | 截止日   |
| index_code   | 指数代码 |
| index_code   | 指数名称 |
| index_weight | 指数权重 |

**范例**

```python
In [10]:fund.get_benchmark('000006')
Out[10]: end_date index_code index_name index_weight order_book_id start_date
000006 2019-02-15 2019-12-25 000905.XSHG 中证小盘500指数 0.75
2019-02-15 2019-12-25 B00009 活期存款利率(税后) 0.25
2019-12-25 NaT 000905.XSHG 中证小盘500指数 0.75
2019-12-25 NaT B00009 活期存款利率(税后) 0.25
```

## fund.get_financials - 获取基金财务信息

```python
fund.get_financials(order_book_ids,start_date=None, end_date=None，fields=None)
```

**参数**

| 参数           | 类型                                     | 注释             |
| -------------- | ---------------------------------------- | ---------------- |
| order_book_ids | str or list                              | 基金代码         |
| start_date     | str, datetime.date, datetime.datetime, pandasTimestamp | 开始日期         |
| end_date       | str, datetime.date, datetime.datetime, pandasTimestamp | 结束日期         |
| fields         | str or list                              | 查询字段,默认为全部 |

**返回**

pandas DataFrame

| 字段          | 说明   |
| ------------- | ------ |
| order_book_id | 基金合约 |
| date          | 报告期   |
| fields        | 字段名称 |

**字段说明**

| 字段                          | 说明                 |
| ----------------------------- | -------------------- |
| cash_equivalent               | 现金及现金等价物     |
| financial_asset_held_for_trading | 交易性金融资产       |
| dividend_receivable           | 应收股利             |
| interest_receivable           | 应收利息             |
| deferred_income_tax_assets    | 递延所得税资产       |
| other_accts_receivable        | 其他应收账款         |
| accts_receivable              | 应收账款             |
| other_assets                  | 其他资产             |
| deferred_expense              | 待摊费用             |
| total_asset                   | 总资产               |
| financial_liabilities         | 交易性金融负债       |
| redemption_money_payable      | 应付赎回款           |
| redemption_fee_payable        | 应付赎回费           |
| management_fee_payable        | 应付管理人报酬       |
| trust_fee_payable             | 应付托管费           |
| sales_fee_payable             | 应付销售服务费       |
| transaction_fee_payable       | 应付交易费用         |
| tax_payable                   | 应交税费             |
| interest_payable              | 应付利息             |
| profit_payable                | 应付利润             |
| deferred_income_tax_liabilities | 递延所得税负债       |
| accts_payable                 | 应付帐款             |
| other_accts_payable           | 其他应付款           |
| other_liabilities             | 其他负债             |
| total_liabilities             | 负债合计             |
| paid_in_capital               | 实收基金             |
| undistributed_profit          | 未分配利润           |
| other_equity                  | 其他权益             |
| total_equity                  | 总权益               |
| total_equity_and_liabilities  | 负债和所有者权益合计 |
| leverage                      | 杠杆率               |
| stock_cost                    | 股票买入成本         |
| stock_income                  | 股票买入收入         |

**范例**

```python
In [10]:fund.get_financials('000001','20190101','20191231',fields=['total_asset','total_equity','leverage','stock_cost','stock_income'])
Out[10]: leverage stock_cost stock_income total_asset total_equity order_book_id date
000001 2019-06-30 1.007034 6.082403e+09 6.246101e+09 4.747522e+09 4.714361e+09
2019-12-31 1.010894 1.364662e+10 1.378563e+10 4.648447e+09 4.598352e+09
```

## fund.get_fee - 获取基金费率信息

```python
fund.get_fee(order_book_ids,fee_type=None,charge_type='front',date=None,market_type='otc',market='cn')
```

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| fee_type       | str or list                              | 费率类型，默认为None 返回全部字段                            |
| charge_type    | str                                      | 'front'-前端费率（默认值），'back'-后端费率                   |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，获取距离指定日期最近的数据。如不指定日期，则默认为当前最新时点 |
| market_type    | str                                      | 费率适用渠道，'otc'-场外费率（默认值），'exchange'-场内费率     |

**返回**

pandas DataFrame

| 字段                 | 说明                                     |
| -------------------- | ---------------------------------------- |
| order_book_id        | 基金合约                                 |
| fee_type             | 费率类型                                 |
| fee_value            | 费率金额（元）                             |
| fee_ratio            | 费率                                     |
| inv_floor            | 金额下限（万元）                           |
| inv_cap              | 金额上限（万元）                           |
| share_floor          | 份额下限（万份）                           |
| share_cap            | 份额上限（万份）                           |
| holding_period_floor | 持有期下限（天）                           |
| holding_period_cap   | 持有期上限（天）                           |
| return_floor         | 基金年化收益率下限（%）                    |
| return_cap           | 基金年化收益率上限（%）                    |

**费率类型说明**

| fee_type         | 说明     |
| ---------------- | -------- |
| subscription_fee | 申购费率 |
| redemption_fee   | 赎回费率 |
| management_fee   | 管理费率 |
| custodian_fee    | 托管费率 |
| sales_service_fee | 营销费率 |
| purchase_fee     | 认购费率 |

**范例**

获取后端场外费率

```python
In [23]: fund.get_fee("000001", charge_type='back')
Out[23]: fee_ratio fee_value inv_floor ... holding_period_cap return_floor return_cap order_book_id fee_type ...
000001 custodian_fee 0.0025 NaN NaN ... NaN NaN NaN
management_fee 0.0150 NaN NaN ... NaN NaN NaN
redemption_fee 0.0050 NaN NaN ... NaN NaN NaN
redemption_fee 0.0150 NaN NaN ... 7.0 NaN NaN
subscription_fee 0.0150 NaN NaN ... 730.0 NaN NaN
subscription_fee 0.0180 NaN NaN ... 365.0 NaN NaN
subscription_fee 0.0120 NaN NaN ... 1095.0 NaN NaN
subscription_fee 0.0100 NaN NaN ... 1460.0 NaN NaN
subscription_fee 0.0000 NaN NaN ... NaN NaN NaN
subscription_fee 0.0050 NaN NaN ... 2920.0 NaN NaN
```

获取前端场外费率

```python
In [24]: fund.get_fee("000001", charge_type='front')
Out[24]: fee_ratio fee_value inv_floor ... holding_period_cap return_floor return_cap order_book_id fee_type ...
000001 custodian_fee 0.0025 NaN NaN ... NaN NaN NaN
management_fee 0.0150 NaN NaN ... NaN NaN NaN
purchase_fee 0.0100 NaN NaN ... NaN NaN NaN
redemption_fee 0.0150 NaN NaN ... 7.0 NaN NaN
redemption_fee 0.0050 NaN NaN ... NaN NaN NaN
subscription_fee NaN 1000.0 1000.0 ... NaN NaN NaN
subscription_fee 0.0120 NaN 100.0 ... NaN NaN NaN
subscription_fee 0.0080 NaN 500.0 ... NaN NaN NaN
subscription_fee 0.0150 NaN 0.0 ... NaN NaN NaN
```

## fund.get_benchmark_price - 获取特殊的基金基准行情

目前仅支持fund.get_category_mapping中指数

```python
fund.get_benchmark_price(order_book_ids,start_date=None,end_date=None)
```

**参数**

| 参数           | 说明                               |
| -------------- | ---------------------------------- |
| order_book_ids | 基准代码or list                      |
| start_date     | 开始日期，不指定则不限制开始日期   |
| end_date       | 结束日期，不指定则不限制结束日期   |

**返回**

```python
In [10]: fund.get_benchmark_price('1015003',start_date=20200520,end_date=20200526)
Out[10]: close
order_book_id date
1015003 2020-05-20 1650.3720
2020-05-21 1631.6981
2020-05-22 1598.8213
2020-05-25 1597.9072
```

## fund.get_snapshot - 获取基金最新的衍生数据

```python
fund.get_snapshot(order_book_ids, fields=None, rule=None, market='cn')
```

**参数**

| 参数           | 类型       | 注释                                         |
| -------------- | ---------- | -------------------------------------------- |
| order_book_ids | str or list | 基金代码                                     |
| fields         | str or list | 返回字段，默认返回所有衍生字段               |
| rule           | str        | 指定算法，目前仅支持返回算法'ricequant'      |
| indicator_type | str        | 指标类别，衍生指标值-value, 衍生指标排名-rank，默认返回衍生指标值 |
| market         | str        | 指定市场，目前仅有中国市场('cn')的基金衍生数据 |

**返回**

pandas DataFrame

index: order_book_id

标准版涵盖的衍生指标及频率如下，字段的组成方式为“支持的频率*字段”, 如“日度累计收益” 字段名为'daily_return'，货币基金仅支持展示下面部分衍生指标数据。

| 字段            | 说明                               | 支持的频率                                                                        |
| --------------- | ---------------------------------- | --------------------------------------------------------------------------------- |
| return          | 累计收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| return_a        | 累计收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| benchmark_return | 累计收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess          | 超额收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_a        | 超额收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_win      | 超额胜率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| stdev_a         | 波动率（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_avg_a | 下行波动率- 均值（年化）            | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_rf_a | 下行波动率- 无风险利率（年化）      | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd             | 期间最大回撤                       | m3、m6、y2、y1、y3、y5、total                                                     |
| excess_mdd      | 期间超额收益最大回撤               | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd_days        | 最大回撤持续期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| recovery_days   | 最大回撤恢复期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop        | 最大单日跌幅                       | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop_period | 最大连跌期数                       | m3、m6、y2、y1、y3、y5、total                                                     |
| neg_return_ratio | 亏损期占比                       | m3、m6、y2、y1、y3、y5、total                                                     |
| kurtosis        | 峰度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| skewness        | 偏度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| tracking_error  | 跟踪误差                           | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_downside   | 下行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_upside     | 上行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| var             | VaR                                | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_a         | Alpha（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_tstats    | Alpha Tstat                      | m3、m6、y2、y1、y3、y5、total                                                     |
| beta            | Beta                               | m3、m6、y2、y1、y3、y5、total                                                     |
| sharpe_a        | Sharpe Ratio（年化）               | m3、m6、y2、y1、y3、y5、total                                                     |
| inf_a           | Information Ratio（年化）          | m3、m6、y2、y1、y3、y5、total                                                     |
| sortino_a       | Sortino Ratio（年化）              | m3、m6、y2、y1、y3、y5、total                                                     |
| calmar_a        | Calmar Ratio                       | m3、m6、y2、y1、y3、y5、total                                                     |
| timing_ratio    | 择时比率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| benchmark       | 指标计算基准/排名范围              | 无                                                                                |

**范例**

返回基金最新衍生指标值与排名数据

```python
In [95]: fund.get_snapshot('000001')
Out[95]: benchmark daily_benchmark_return daily_excess daily_excess_a ... y5_tracking_error y5_var year_return year_return_a
order_book_id datetime ...
000001 2021-01-27 偏股型基金指数0.0 -0.009129 -2.300567 ... 0.062154 -0.016652 0.038263 0.691631
In [96]: fund.get_snapshot('000001',indicator_type='rank')
Out[96]: benchmark daily_benchmark_return daily_excess daily_excess_a ... y5_tracking_error y5_var year_return year_return_a
order_book_id datetime ...
000001 2021-01-27 偏股型1/1528 1382/1528 1382/1528 ... 508/521 47/521 1266/1507 1266/1507
```

## fund.get_indicators - 获取基金的衍生数据

```python
fund.get_indicators(order_book_ids, start_date=None, end_date=None, fields=None, rule=None, market='cn')
```

**参数**

| 参数           | 类型       | 注释                                                         |
| -------------- | ---------- | ------------------------------------------------------------ |
| order_book_ids | str or list | 基金代码                                                     |
| start_date     | str        | 开始日期, 格式为'YYYY-mm-dd', 默认为基金衍生数据最早有效日期 |
| end_date       | str        | 结束日期, 格式为'YYYY-mm-dd', 默认为查询当日                 |
| fields         | str or list | 返回字段，默认返回所有衍生字段                               |
| indicator_type | str        | 指标类别，衍生指标值-value, 衍生指标排名-rank，默认返回衍生指标值 |
| rule           | str        | 指定算法，目前仅支持返回'ricequant'                          |
| market         | str        | 指定市场，目前仅有中国市场('cn')的基金衍生数据               |

**返回**

pandas DataFrame

index: order_book_id

标准版涵盖的衍生指标及频率如下，字段的组成方式为“支持的频率*字段”, 如“日度累计收益” 字段名为'daily_return'，货币基金仅支持展示下面部分衍生指标数据。

| 字段            | 说明                               | 支持的频率                                                                        |
| --------------- | ---------------------------------- | --------------------------------------------------------------------------------- |
| return          | 累计收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| return_a        | 累计收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| benchmark_return | 基准收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total                                 |
| excess          | 超额收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_a        | 超额收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_win      | 超额胜率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| stdev_a         | 波动率（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_avg_a | 下行波动率- 均值（年化）            | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_rf_a | 下行波动率- 无风险利率（年化）      | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd             | 期间最大回撤                       | m3、m6、y2、y1、y3、y5、total                                                     |
| excess_mdd      | 期间超额收益最大回撤               | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd_days        | 最大回撤持续期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| recovery_days   | 最大回撤恢复期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop        | 最大单日跌幅                       | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop_period | 最大连跌期数                       | m3、m6、y2、y1、y3、y5、total                                                     |
| neg_return_ratio | 亏损期占比                       | m3、m6、y2、y1、y3、y5、total                                                     |
| kurtosis        | 峰度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| skewness        | 偏度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| tracking_error  | 跟踪误差                           | m3、m6、y2、y1、y3、y5、total                                                     |
| var             | VaR                                | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_a         | Alpha（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_tstats    | Alpha Tstat                      | m3、m6、y2、y1、y3、y5、total                                                     |
| beta            | Beta                               | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_downside   | 下行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_upside     | 上行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| sharpe_a        | Sharpe Ratio（年化）               | m3、m6、y2、y1、y3、y5、total                                                     |
| inf_a           | Information Ratio（年化）          | m3、m6、y2、y1、y3、y5、total                                                     |
| sortino_a       | Sortino Ratio（年化）              | m3、m6、y2、y1、y3、y5、total                                                     |
| calmar_a        | Calmar Ratio                       | m3、m6、y2、y1、y3、y5、total                                                     |
| timing_ratio    | 择时比率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| benchmark       | 指标计算基准                       | 无                                                                                |

**范例**

返回基金衍生指标值与排名数据

```python
In [98]: fund.get_indicators('000001',start_date=20200601,rule='ricequant',indicator_type='value',fields=['m3_alpha_a','m6_beta','benchmark'])
Out[98]: m3_alpha_a m6_beta benchmark
order_book_id datetime
000001 2020-06-01 -0.017309 0.897002 偏股型基金指数
2020-06-02 -0.032750 0.897575 偏股型基金指数
2020-06-03 -0.036943 0.897945 偏股型基金指数
2020-06-04 -0.061339 0.897735 偏股型基金指数
2020-06-05 -0.072694 0.895500 偏股型基金指数
... ... ... ...
2021-01-21 -0.544216 0.905062 偏股型基金指数
2021-01-22 -0.492470 0.914805 偏股型基金指数
2021-01-25 -0.419185 0.924407 偏股型基金指数
2021-01-26 -0.431358 0.922676 偏股型基金指数
2021-01-27 -0.455787 0.923730 偏股型基金指数
In [99]: fund.get_indicators('000001',start_date=20200601,rule='ricequant',indicator_type='rank',fields=['m3_alpha_a','m6_beta','benchmark'])
Out[99]: m3_alpha_a m6_beta benchmark
order_book_id datetime
000001 2020-06-01 555/1116 746/1015 偏股型
2020-06-02 590/1116 749/1015 偏股型
2020-06-03 601/1117 749/1015 偏股型
2020-06-04 648/1117 749/1018 偏股型
2020-06-05 691/1120 753/1018 偏股型
... ... ... ...
2021-01-21 1446/1475 921/1273 偏股型
2021-01-22 1425/1471 898/1277 偏股型
2021-01-25 1403/1475 897/1308 偏股型
2021-01-26 1351/1423 897/1259 偏股型
2021-01-27 1359/1415 893/1251 偏股型
```

## fund.get_related_code - 获取分级基金的分级关系

```python
fund.get_related_code(order_book_ids, market='cn')
```

**参数**

| 参数           | 类型       | 注释                                     |
| -------------- | ---------- | ---------------------------------------- |
| order_book_ids | str or list | 基金代码                                 |
| market         | str        | 指定市场，目前仅有中国市场('cn')的分级基金数据 |

**返回**

pandas DataFrame

| 字段           | 说明                                             |
| -------------- | ------------------------------------------------ |
| main_code      | 平级关系或母子关系的主代码                       |
| related_code   | 平级关系或母子关系的次代码                       |
| type           | 分级基金关系：同一基金分级关系- multi_share， 母子基金分级关系- parent_and_child， 同一基金不同货币关系（QDII）- multi_currency |
| effective_date | 该条记录的有效起始日                             |
| cancel_date    | 该条记录的失效日                                 |

**范例**

```python
In [23]: fund.get_related_code(['000003','000004','005929','160513'])
Out[23]: main_code related_code type effective_date cancel_date
0 000003 000004 multi_share 2013-02-20 NaT
1 005929 005930 multi_share 2018-10-12 2019-01-16
2 160513 160514 multi_share 2014-06-10 NaT
3 160513 160514 parent_and_child 2011-05-20 2014-06-10
4 160513 150043 parent_and_child 2011-05-20 2014-06-10
```

## fund.get_daily_units - 获取基金的日度份额数据

```python
fund.get_daily_units(order_book_ids, start_date=None, end_date=None)
```

**参数**

| 参数           | 类型 | 注释                                       |
| -------------- | ---- | ------------------------------------------ |
| order_book_ids | str or list | 基金代码                                   |
| start_date     | str  | 开始日期, 格式为'YYYY-mm-dd', 默认返回最近3 个月 |
| end_date       | str  | 结束日期, 格式为'YYYY-mm-dd', 默认返回最近3 个月日 |

**返回**

pandas DataFrame

| 字段  | 说明               |
| ----- | ------------------ |
| units | 期末总份额（单位：份） |

**范例**

```python
In [23]: rqdatac.fund.get_daily_units('159621',20221101,20221130)
Out[23]: units
order_book_id datetime
159621 2022-11-01 203434717.0
2022-11-02 197434717.0
2022-11-03 194434717.0
2022-11-04 194434717.0
2022-11-07 194434717.0
2022-11-08 191434717.0
2022-11-09 191434717.0
2022-11-10 191434717.0
2022-11-11 191434717.0
2022-11-14 185434717.0
2022-11-15 185434717.0
2022-11-16 182434717.0
2022-11-17 179434717.0
2022-11-18 179434717.0
2022-11-21 179434717.0
2022-11-22 179434717.0
2022-11-23 179434717.0
2022-11-24 176434717.0
2022-11-25 176434717.0
2022-11-28 176434717.0
2022-11-29 173434717.0
2022-11-30 170434717.0
```

# 基金持仓与配置信息数据

## fund.get_holdings - 获取基金持仓信息

```python
fund.get_holdings(order_book_ids, date=None)
```

从指定日期回溯，获取最近的基金持仓信息。

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金合约代码                                                 |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的持仓数据。如不指定日期，则获取所有日期的持仓数据 |

**返回**

pandas DataFrame

| 字段          | 类型  | 说明                                                     |
| ------------- | ----- | -------------------------------------------------------- |
| order_book_id | str   | 持仓合约代码，如股票持仓、债券持仓等合约代码             |
| weight        | float | 持仓百分比                                               |
| date          | str   | 报告期                                                   |
| release_date  | str   | 公告发布日                                               |
| shares        | float | 持仓股数（如股票单位：1 股，债券为NaN）                    |
| market_value  | float | 持仓市值（单位：元，债券为NaN）                            |
| symbol        | str   | 持仓简称                                                 |
| type          | str   | 持仓资产类别大类，股票-Stock，债券-Bond，基金-Fund，权证-Warrant, 期权-Futures，其他-Other |
| category      | str   | 持仓资产类别细类（如：category='Hshare'港股，category='Ashare'A 股均属于type='Stock' ） |

**范例**

```python
In [171]: fund.get_holdings('000001',20190930)
Out[171]: order_book_id weight shares ... type category symbol fund_id date
...
000001 2019-09-30 101564021.IB 0.0221 1000000.0 ... Bond CorporateBond 15华能集MTN002 2019-09-30
128016.XSHE 0.0001 4172.0 ... Bond ConvertibleBond 雨虹转债2019-09-30
128022.XSHE 0.0001 6248.0 ... Bond ConvertibleBond 众信转债2019-09-30
128046.XSHE 0.0013 59048.0 ... Bond ConvertibleBond 利尔转债2019-09-30
180208.IB 0.0333 1500000.0 ... Bond FinancialBond 18国开08 2019-09-30
180409.IB 0.0290 1300000.0 ... Bond FinancialBond 18农发09 2019-09-30
190201.IB 0.0219 1000000.0 ... Bond FinancialBond 19国开01 2019-09-30
190303.IB 0.0218 1000000.0 ... Bond FinancialBond 19进出03 2019-09-30
000858.XSHE 0.0428 1509443.0 ... Stock AShare 五粮液2019-09-30
002127.XSHE 0.0309 13733457.0 ... Stock AShare 南极电商2019-09-30
002384.XSHE 0.0369 8571900.0 ... Stock AShare 东山精密
```

## fund.get_stock_change - 获取基金报告期内重大股票持仓变动情况

```python
fund.get_stock_change(order_book_ids,start_date,end_date)
```

获取基金报告期内重大股票持仓变动情况

**参数**

| 参数           | 类型                                     | 注释                                     |
| -------------- | ---------------------------------------- | ---------------------------------------- |
| order_book_ids | str or list                              | 基金代码                                 |
| start_date     | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的开始日期，默认为最新一期数据       |
| end_date       | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的结束日期，默认为最新一期数据       |

**返回**

pandas DataFrame

| 字段          | 类型  | 说明             |
| ------------- | ----- | ---------------- |
| order_book_id | str   | 股票合约代码     |
| date          | str   | 持仓披露日期     |
| weight        | float | 持仓百分比       |
| market_value  | float | 持仓市值         |
| change_type   | str   | 变动类型。1-买入，2-卖出 |

**范例**

```python
In [19]: fund.get_stock_change('519933','20190101','20191001')
Out[19]: order_book_id market_value weight change_type date
2019-06-30 000921.XSHE 361296.00 0.0497 2
2019-06-30 601288.XSHG 744548.00 0.1025 2
2019-06-30 600660.XSHG 194344.00 0.0267 2
2019-06-30 601398.XSHG 601000.00 0.0827 1
2019-06-30 600519.XSHG 852090.00 0.1173 1
2019-06-30 600004.XSHG 1005822.00 0.1384 2
···
2019-06-30 002025.XSHE 493102.00 0.0679 1
2019-06-30 601398.XSHG 575489.00 0.0792 2
2019-06-30 600519.XSHG 853209.00 0.1174 2
2019-06-30 603589.XSHG 465176.00 0.0640 2
```

## fund.get_term_to_maturity - 获取货币型基金持仓期限数据

```python
fund.get_term_to_maturity(order_book_ids,start_date,end_date)
```

**参数**

| 参数           | 类型                                     | 注释                       |
| -------------- | ---------------------------------------- | -------------------------- |
| order_book_ids | str or list                              | 基金代码                   |
| start_date     | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的开始日               |
| end_date       | str, datetime.date, datetime.datetime, pandasTimestamp | 查询的结束日期，默认为最新一期数据 |

**返回**

pandas DataFrame

| 字段   | 类型  | 说明           |
| ------ | ----- | -------------- |
| date   | str   | 报告期         |
| term   | str   | 剩余期限范围   |
| weight | float | 剩余期限占资产净值比例 |

**范例**

```python
In [18]: fund.get_term_to_maturity('050003','20190101','20191120')
Out[18]: term weight
date
2019-03-31 0_30 0.5013
2019-03-31 30_60 0.1077
2019-03-31 60_90 0.1419
2019-03-31 90_120 0.0624
2019-03-31 120_397 0.2090
2019-06-30 0_30 0.4116
2019-06-30 30_60 0.0749
2019-06-30 60_90 0.2211
2019-06-30 90_120 0.0781
2019-06-30 120_397 0.2123
2019-09-30 0_30 0.3682
2019-09-30 30_60 0.1786
2019-09-30 60_90 0.1537
2019-09-30 90_120 0.0647
2019-09-30 120_397 0.2454
```

## fund.get_bond_structure - 获取基金持仓中债券组合结构信息

```python
fund.get_bond_structure(order_book_ids, date=None， market='cn')
```

从指定日期回溯，获取最近的基金债券组合结构信息。

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的债券组合结构数据。如不指定日期，则获取所有日期的债券组合结构数据 |
| market         | str                                      | 指定市场，目前仅有中国市场('cn')的基金数据                   |

**返回**

pandas DataFrame

| 字段             | 类型  | 说明                                                     |
| ---------------- | ----- | -------------------------------------------------------- |
| order_book_id    | str   | 基金合约代码                                             |
| date             | str   | 持仓披露日期                                             |
| bond_type        | str   | 债券种类：国债- government ，金融债券- financial ，企业债券- corporate ，可转换债券- convertible ，央行票据- bank_notes ，短期融资券- short_financing ，中期票据- medium_notes ，同业存单- ncd ，中小企业私募债- s_m_private ，地方政府债券- local_government ，其他债券- other_bond |
| weight_nv        | float | 持仓占资产净值百分比                                     |
| weight_bond_mv   | float | 持仓占债券组合市值百分比                                 |
| market_value     | float | 持仓市值（单位：元）                                     |

**范例**

```python
In [28]: fund.get_bond_structure(['000014','000005'],20200630)
Out[28]: bond_type weight_nv weight_bond_mv market_value
order_book_id date
000005 2020-06-30 financial 0.2370 0.183999 13469400.00
2020-06-30 convertible 0.0347 0.026921 1970729.00
2020-06-30 corporate 0.3668 0.284768 20846100.00
2020-06-30 medium_notes 0.6495 0.504312 36917600.00
000014 2020-06-30 government 0.1423 0.127257 14705500.00
2020-06-30 convertible 0.7407 0.662522 76559552.12
2020-06-30 corporate 0.2350 0.210221 24292667.20
```

## fund.get_asset_allocation - 获取基金资产配置

```python
fund.get_asset_allocation(order_book_ids, date=None)
```

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，查询日期和报告期去比较,回溯获取距离指定日期最近的报告期数据。如不指定日期，则获取所有日期的数据 |

**返回**

pandas DataFrame

| 字段          | 说明                                         |
| ------------- | -------------------------------------------- |
| index         | 报告期                                       |
| info_date     | 公告发布日                                   |
| stock         | 股票占净资产比例                             |
| bond          | 债券占净资产比例（由于债券通过质押式回购进行融资杠杆交易的存在，债券占比数值可能超过100%） |
| cash          | 现金占净资产比例                             |
| other         | 其他资产占净资产比例                         |
| net_asset     | 基金净资产（单位：元）                       |
| total_asset   | 基金总资产（单位：元）                       |

**范例**

```python
In [12]: fund.get_asset_allocation('000058',date='20201231')
Out [12] info_date stock bond fund cash other nav net_asset total_asset
order_book_id datetime
000058 2020-12-31 2021-01-22 0.311344 0.6614 NaN 0.013306 0.015539 6.928471e+08 6.928471e+08 693971161.4
```

## fund.get_industry_allocation - 获取基金权益类持仓行业配置

```python
fund.get_industry_allocation(order_book_ids, date=None)
```

**参数**

| 参数           | 类型                                     | 注释                                                         |
| -------------- | ---------------------------------------- | ------------------------------------------------------------ |
| order_book_ids | str or list                              | 基金代码                                                     |
| date           | str, datetime.date, datetime.datetime, pandasTimestamp | 查询日期，回溯获取距离指定日期最近的数据。如不指定日期，则获取所有日期的数据 |

**返回**

pandas DataFrame

| 字段         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| index        | 报告期                                                       |
| standard     | 行业划分标准: CSRC-CSRC 行业分类, GICS-GICS 行业分类， MSCI-MSCI 行业分类， Bloomberg-Bloomberg 行业分类， CSRC_2012-证监会行业分类2012 版， Gildata-聚合行业分类（QDII 基金专用） |
| industry     | 行业名称                                                     |
| weight       | 行业占比                                                     |
| market_value | 持仓市值（单位：元）                                         |

**范例**

```python
In [55]: fund.get_industry_allocation('000001',date='20200630')
Out[55]: standard industry weight market_value
order_book_id datetime
000001 2020-06-30 CSRC_2012 金融业0.0003 1.702350e+06
2020-06-30 CSRC_2012 采矿业0.0622 3.145813e+08
2020-06-30 CSRC_2012 综合0.0009 4.671139e+06
2020-06-30 CSRC_2012 租赁和商务服务业0.0954 4.821860e+08
2020-06-30 CSRC_2012 科学研究和技术服务业0.0244 1.231741e+08
2020-06-30 CSRC_2012 电力、热力、燃气及水生产和供应业0.0218 1.103732e+08
2020-06-30 CSRC_2012 水利、环境和公共设施管理业0.0002 1.167722e+06
2020-06-30 CSRC_2012 文化、体育和娱乐业0.0017 8.767187e+06
2020-06-30 CSRC_2012 批发和零售业0.0020 9.917138e+06
2020-06-30 CSRC_2012 房地产业0.0039 1.968217e+07
2020-06-30 CSRC_2012 建筑业0.0000 6.025800e+02
2020-06-30 CSRC_2012 制造业0.4653 2.352107e+09
2020-06-30 CSRC_2012 信息传输、软件和信息技术服务业0.0979 4.949599e+08
```

## fund.get_qdii_scope - 获取QDII 地区配置

```python
rq.fund.get_qdii_scope(order_book_ids, start_date=None, end_date=None)
```

**参数**

| 参数           | 类型 | 注释                   |
| -------------- | ---- | ---------------------- |
| order_book_ids | str or list | 基金代码               |
| start_date     | str  | 开始日期, 格式为'YYYY-mm-dd' |
| end_date       | str  | 结束日期, 格式为'YYYY-mm-dd' |

**返回**

pandas DataFrame

| 字段          | 说明       |
| ------------- | ---------- |
| order_book_id | 基金合约   |
| date          | 报告期       |
| region        | 地区         |
| market_value  | 市值(元)   |
| weight        | 占净资产比列 |

**范例**

```python
In [10]: fund.get_qdii_scope('183001','20190101','20200101')
Out[10]: region market_value weight
order_book_id date
183001 2019-03-31 中国香港 12540825.98 20.00
2019-06-30 中国香港 13108299.05 20.50
2019-09-30 中国香港 12177202.64 18.99
2019-12-31 中国香港 9065869.58 13.94
```

## fund.get_instrument_category - 获取基金的风格分类数据

```python
fund.get_instrument_category(order_book_ids,date=None,category_type=None,source='gildata',market='cn')
```

**参数**

| 参数           | 说明                                             |
| -------------- | ------------------------------------------------ |
| order_book_ids | 基金合约号。支持传入列表                         |
| source         | 分类来源。'gildata'                              |
| date           | 日期，默认为最新                                 |
| category_type  | 分类类型，可传入list                              |

默认返回以下几种：

-   价值风格-value
-   规模风格-size
-   操作风格-operating_style
-   久期分布-duration
-   券种配置-bond_type
    *其他枚举值请见下方完整表格

**category_type 枚举值**

| category_type | 中文释义   | 备注     |
| ------------- | ---------- | -------- |
| value         | 价值风格   |          |
| size          | 规模风格   |          |
| operating_style | 操作风格   |          |
| duration      | 久期分布   |          |
| bond_type     | 券种配置   |          |
| fund_type     | 基金分类（聚源） | 该分类数据返回结构与其他不同，故只能单独调取 |
| concept       | 概念板块   |          |
| industry_citics | 行业分类（中信一级） |          |
| investment_style | 投资风格分类 |          |
| structured_fund | 分级基金标识 |          |
| universe      | 基金属性   | 返回值参照：close_end - 封闭型基金，open_end - 开放型基金，fund_of_etf - ETF 联接基金，lof - LOF，etf - ETF |

**返回**

pandas DataFrame

| 字段              | 说明                               |
| ----------------- | ---------------------------------- |
| order_book_id     | 基金合约                           |
| category_type     | 分类类型                           |
| category          | 基金细分分类名称                   |
| category_index    | 基金细分分类指数代码               |

仅对category_type='fund_type'返回的字段：

| 字段              | 说明   |
| ----------------- | ------ |
| first_type_code   | 一级分类代码 |
| first_type        | 一级分类名称 |
| second_type_code  | 二级分类代码 |
| second_type       | 二级分类名称 |
| third_type_code   | 三级分类代码 |
| third_type        | 三级分类名称 |

**范例**

不指定category_type，获取默认分类类型数据

```python
In [8]: fund.get_instrument_category('000001')
Out[8]: category category_index order_book_id category_type
000001 value blend 1014002
operating_style flexible 1015003
size mid_cap 1013002
```

指定获取基金的基金属性、概念板块

```python
In [12]: fund.get_instrument_category('000001',category_type=['universe','concept'])
Out[12]: category category_index order_book_id category_type
000001 concept 新材料1010018
concept MSCI概念1010054
concept 新三板1010052
concept 5G概念1010024
universe open_end None
```

获取多个基金的基金分类数据

```python
In [14]: fund.get_instrument_category(['000001','000014'],category_type='fund_type')
Out[14]: first_type_code first_type second_type_code second_type third_type_code third_type
order_book_id category_type
000001 fund_type 12 混合型1201 偏股型120101 偏股型
000014 fund_type 13 债券型1302 普通债券型130201 普通债券型(一级)
```

## fund.get_category - 获取风格分类所属基金列表

```python
fund.get_category(category={"category_type":["category"]}, date=None, source='gildata', market='cn')
```

**参数**

| 参数     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| category | 以dict 的形式输入：支持输入多个category_type、category。结构为{"category_type":["category"],"category_type":["category"]} ,可参考范例帮助理解。 |
| date     | 默认最新日期                                                 |
| source   | 分类来源。'gildata' - 聚源（默认值）                         |

**返回**

order_book_id list

**范例**

传入类别名称和名称查询列表

```python
In [8]: rq.fund.get_category(category={"concept": ["人工智能","MSCI概念"], "size": "large","operating_style":"flexible"})
Out[8]: ['040001', '202005', '270021', ··· '006573', '006574', '005029']
```

## fund.get_category_mapping - 获取风格分类清单

```python
fund.get_category_mapping(source='gildata', market='cn')
```

**参数**

| 参数   | 说明                           |
| ------ | ------------------------------ |
| source | 分类来源。'gildata' - 聚源（默认值） |

**返回**

| 字段            | 说明           |
| --------------- | -------------- |
| category_type   | 分类维度       |
| category        | 基金细分分类名称 |
| category_index  | 基金细分分类指数代码 |

**范例**

```python
In [17]: fund.get_category_mapping()
Out[17]: category category_index category_type
structured_fund structured_fund None
universe fund_of_etf None
universe lof None
universe close_end None
concept 一带一路1010005
... ... ...
duration 1_year 1111001
industry_citics 基础化工1012022
industry_citics 建材1012024
investment_style 其他型None
concept 北斗导航1010025
[155 rows x 2 columns]
```

**中文映射表**

| category_type 中文 | category_type 英文 | category 中文 | category 英文   |
| ------------------ | ------------------ | ------------- | --------------- |
| 股债偏重           | 价值风格           | value         | 成长            |
| 偏股型基金         | 价值风格           | value         | 价值            |
| 偏股型基金         | 价值风格           | value         | 平衡            |
| 偏股型基金         | 规模风格           | size          | 大盘            |
| 偏股型基金         | 规模风格           | size          | 小盘            |
| 偏股型基金         | 规模风格           | size          | 中盘            |
| 偏股型基金         | 操作风格           | operating_style | 激进操作        |
| 偏股型基金         | 操作风格           | operating_style | 灵活操作        |
| 偏股型基金         | 操作风格           | operating_style | 平均操作        |
| 偏股型基金         | 操作风格           | operating_style | 稳健操作        |
| 偏股型基金         | 操作风格           | operating_style | 积极操作        |
| 偏股型基金         | 久期分布           | duration      | 1 年(含)以下    |
| 偏债型基金         | 久期分布           | duration      | 1-3 年(含)      |
| 偏债型基金         | 久期分布           | duration      | 3-5 年(含)      |
| 偏债型基金         | 久期分布           | duration      | 5 年以上        |
| 偏债型基金         | 券种配置           | bond_type     | 可转债          |
| 偏债型基金         | 券种配置           | bond_type     | 利率债          |
| 偏债型基金         | 券种配置           | bond_type     | 信用债          |
| ...                | ...                | ...           | ...             |

# 基金管理人信息，以及衍生数据

## fund.get_manager - 获取指定基金的基金经理管理信息

```python
fund.get_manager(order_book_ids,expect_df=True)
```

**参数**

| 参数           | 类型          | 注释                               |
| -------------- | ------------- | ---------------------------------- |
| order_book_ids | str or str list | 基金代码                           |
| expect_df      | boolean       | 默认返回pandas dataframe。如果调为假，则返回原有的数据结构 |

**返回**

pandas DataFrame

| 字段       | 说明                   |
| ---------- | ---------------------- |
| name       | 基金经理名称           |
| id         | 基金经理代码           |
| days       | 基金经理管理当前基金累计天数 |
| start_date | 基金经理开始管理当前基金的日期 |
| end_date   | 基金经理结束管理当前基金的日期（NaT 代表任职至今） |
| return     | 基金经理任职回报       |
| title      | 职位                   |

**范例**

获取单只基金的基金经理管理信息

```python
In [47]: fund.get_manager(['000001'],expect_df=False)
Out[47]: name days start_date end_date return title id
101000229 王亚伟 1211 2001-12-18 2005-04-12 0.133084 基金经理
101000228 田擎 605 2002-07-01 2004-02-26 0.110716 基金经理助理
101002472 乔巍 730 2002-07-01 2004-06-30 0.007694 基金经理助理
101000228 田擎 610 2004-02-27 2005-10-29 -0.151132 基金经理
101000595 巩怀志 1540 2005-10-29 2010-01-16 3.922946 基金经理
101000348 童汀 1616 2010-01-16 2014-06-20 -0.077224 基金经理
101001854 孙振峰 449 2012-04-05 2013-06-28 0.119477 基金经理
101000866 倪邈 612 2014-03-17 2015-11-19 0.469314 基金经理
101001125 李铧汶 1033 2014-03-17 2017-01-13 0.130557 基金经理
101000925 崔同魁 201 2014-06-20 2015-01-07 0.244463 基金经理
101001090 董阳阳 2238 2015-01-07 2021-02-22 0.501579 基金经理
101002061 许利明 574 2015-09-01 2017-03-28 -0.098604 基金经理
101001669 孙萌 463 2015-11-19 2017-02-24 -0.173107 基金经理
101001757 阳琨 149 2021-02-22 NaT -0.039810 基金经理
```

获取基金列表的基金经理管理信息

```python
In [50]: fund.get_manager(['160224', '217019'],expect_df=True)
Out[50]: days end_date name return start_date title
order_book_id id
160224 101002093 1879 NaT 艾小军-0.119662 2015-03-26 基金经理
217019 101001928 2027 2017-01-13 王平0.345410 2011-06-27 基金经理
101001014 1624 2017-07-01 罗毅0.833497 2013-01-19 基金经理
101004652 1220 NaT 苏燕青0.215929 2017-01-13 基金经理
101012888 607 2020-01-02 刘重杰0.073923 2018-05-05 基金经理
```

## fund.get_manager_info - 获取基金经理背景信息

```python
fund.get_manager_info(manager_id,fields=None)
```

**参数**

| 参数       | 类型       | 注释                         |
| ---------- | ---------- | ---------------------------- |
| manager_id | str or list | 可传入基金经理id 或名字。名字与id 不能同时传入 |
| fields     | str or list | 对应返回字段，默认为所有字段   |

**返回**

pandas DataFrame

| 字段          | 说明       |
| ------------- | ---------- |
| id            | 基金经理代码 |
| name          | 基金经理名称 |
| gender        | 性别       |
| region        | 出生地     |
| birthdate     | 生日       |
| education     | 学历       |
| practice_date | 执业开始时间 |
| experience_time | 执业年限   |
| background    | 个人简介   |

**范例**

获取单个基金经理背景信息

```python
In [11]: fund.get_manager_info('101002094',fields=None)
Out[11]: chinesename gender region birthdate education practice_date experience_time background
id
101002094 胡剑男中国None 硕士2006-01-01 12.8 胡剑先生，经济学硕士。曾任易方达基金管理有限公司固定收益部债券研究员、基金经理助理兼...
```

获取多个基金经理背景信息

```python
In [10]: fund.get_manager_info(['101002094','101010264'],fields=None)
Out [10]: chinesename gender region birthdate education practice_date experience_time background
id
101002094 胡剑男中国None 硕士2006-01-01 12.8 胡剑先生，经济学硕士。曾任易方达基金管理有限公司固定收益部债券研究员、基金经理助理兼...
101010264 刘杰None None None 硕士2010-01-01 8.8 核心人员
```

## fund.get_manager_indicators - 获取基金经理人的衍生数据

```python
fund.get_manager_indicators(manager_ids, start_date=None, end_date=None, fields=None, asset_type=None, manage_type=None,rule='ricequant', market='cn')
```

**参数**

| 参数          | 类型       | 注释                                                         |
| ------------- | ---------- | ------------------------------------------------------------ |
| manager_ids   | str or list | 基金经理代码，建议结合fund.get_manager_info 一起使用         |
| start_date    | datetime   | 开始日期, 格式为'YYYY-mm-dd', 默认为基金经理人衍生数据最早有效日期 |
| end_date      | datetime   | 结束日期, 格式为'YYYY-mm-dd', 默认为查询当日                 |
| fields        | str or list | 返回字段，默认返回所有衍生字段                               |
| asset_type    | str        | 在管基金类型，股票型-'stock', 债券型-'bond'，默认返回'stock' |
| manage_type   | str        | 管理方式，独立管理-'independent'，所有参与管理-'all'，默认返回'all' |
| rule          | str        | 指定算法，目前仅支持算法'ricequant'                          |
| market        | str        | 指定市场，目前仅有中国市场('cn')的基金经理人衍生数据         |

**返回**

pandas DataFrame

multi-index: manager_ids,datetime

标准版涵盖的衍生指标及频率如下，字段的组成方式为“支持的频率*字段”, 如“日度累计收益” 字段名为'daily_return'

| 字段            | 说明                               | 支持的频率                                                                        |
| --------------- | ---------------------------------- | --------------------------------------------------------------------------------- |
| return          | 累计收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| return_a        | 累计收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| benchmark_return | 基准收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total                                 |
| excess          | 超额收益率                          | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_a        | 超额收益率（年化）                  | daily、w1、m1、m3、m6、y2、y1、y3、y5、total、year                                 |
| excess_win      | 超额胜率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| stdev_a         | 波动率（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_avg_a | 下行波动率- 均值（年化）            | m3、m6、y2、y1、y3、y5、total                                                     |
| dev_downside_rf_a | 下行波动率- 无风险利率（年化）      | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd             | 期间最大回撤                       | m3、m6、y2、y1、y3、y5、total                                                     |
| excess_mdd      | 期间超额收益最大回撤               | m3、m6、y2、y1、y3、y5、total                                                     |
| mdd_days        | 最大回撤持续期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| recovery_days   | 最大回撤恢复期                     | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop        | 最大单日跌幅                       | m3、m6、y2、y1、y3、y5、total                                                     |
| max_drop_period | 最大连跌期数                       | m3、m6、y2、y1、y3、y5、total                                                     |
| neg_return_ratio | 亏损期占比                       | m3、m6、y2、y1、y3、y5、total                                                     |
| kurtosis        | 峰度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| skewness        | 偏度                               | m3、m6、y2、y1、y3、y5、total                                                     |
| tracking_error  | 跟踪误差                           | m3、m6、y2、y1、y3、y5、total                                                     |
| var             | VaR                                | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_a         | Alpha（年化）                      | m3、m6、y2、y1、y3、y5、total                                                     |
| alpha_tstats    | Alpha Tstat                      | m3、m6、y2、y1、y3、y5、total                                                     |
| beta            | Beta                               | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_downside   | 下行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| beta_upside     | 上行Beta                          | m3、m6、y2、y1、y3、y5、total                                                     |
| sharpe_a        | Sharpe Ratio（年化）               | m3、m6、y2、y1、y3、y5、total                                                     |
| inf_a           | Information Ratio（年化）          | m3、m6、y2、y1、y3、y5、total                                                     |
| sortino_a       | Sortino Ratio（年化）              | m3、m6、y2、y1、y3、y5、total                                                     |
| calmar_a        | Calmar Ratio                       | m3、m6、y2、y1、y3、y5、total                                                     |
| timing_ratio    | 择时比率                           | m3、m6、y2、y1、y3、y5、total                                                     |
| benchmark       | 指标计算基准                       | 无                                                                                |

**范例**

```python
In [25]: fund.get_manager_indicators('101000932',fields=['daily_return','total_calmar_a'],start_date='2018-01-01',manage_type='independent',asset_type='stock')
Out[25]: daily_return total_calmar_a
manager_id datetime
101000932 2018-02-06 -0.031451 0.006801
2018-02-07 -0.021206 0.006622
2018-02-08 0.006771 0.006667
2018-02-09 -0.028918 0.006426
2018-02-12 0.027701 0.006639
... ... ...
2020-12-09 -0.002541 0.003929
2020-12-10 0.011998 0.003956
2020-12-11 0.002877 0.003961
2020-12-14 0.025610 0.004022
2020-12-15 0.006463 0.004035
```

## fund.get_manager_weight_info - 获取基金经理人在管产品权重信息

```python
fund.get_manager_weight_info(manager_ids,start_date=None,end_date=None,asset_type=None,manage_type=None,market='cn' )
```

**参数**

| 参数        | 类型       | 注释                                                 |
| ----------- | ---------- | ---------------------------------------------------- |
| manager_ids | str or list | 可传入基金经理id 或名字。名字与id 不能同时传入         |
| start_date  | datetime   | 开始日期, 格式为'YYYY-mm-dd', 默认为基金经理人管理最早有效日期 |
| end_date    | datetime   | 结束日期, 格式为'YYYY-mm-dd', 默认为查询当日         |
| asset_type  | str        | 在管基金类型，股票型-'stock', 债券型-'bond'，默认返回'stock' |
| manage_type | str        | 管理方式，独立管理-'independent'，所有参与管理- 'all'，默认返回'all' |
| market      | str        | 指定市场，目前仅有中国市场('cn')的基金经理人衍生数据 |

**返回**

pandas DataFrame

multi-index: manager_ids, datetime

| 字段         | 说明                           |
| ------------ | ------------------------------ |
| datetime     | 在管时间                       |
| order_book_id | 在管基金代码                   |
| weight       | 基金占经理人当期管理所有基金的规模比例 |
| manager_name | 经理人名字                     |

**范例**

```python
In [27]: fund.get_manager_weight_info('101002315',asset_type='bond',manage_type='independent',start_date=20200101)
Out[27]: order_book_id weight manager_name
manager_id datetime
101002315 2020-03-31 007834 0.297317 蔡宾
2020-03-31 007833 0.297317 蔡宾
2020-03-31 007745 0.202683 蔡宾
2020-03-31 007744 0.202683 蔡宾
2020-06-30 007834 0.310725 蔡宾
2020-06-30 007833 0.310725 蔡宾
2020-06-30 007745 0.189275 蔡宾
2020-06-30 007744 0.189275 蔡宾
In [30]: fund.get_manager_weight_info('李博',asset_type='stock',manage_type='independent',start_date=20200101)
Out[30]: order_book_id weight manager_name
manager_id datetime
101001503 2020-03-31 001144 0.389673 李博
2020-03-31 090004 0.610327 李博
2020-06-30 001144 0.351493 李博
2020-06-30 090004 0.648507 李博
2020-09-30 001144 0.279292 李博
2020-09-30 090004 0.720708 李博
101001538 2020-03-31 000457 0.623594 李博
2020-03-31 005983 0.008162 李博
2020-03-31 377010 0.368245 李博
2020-06-30 000457 0.602569 李博
2020-06-30 005983 0.007005 李博
2020-06-30 377010 0.390426 李博
2020-09-30 005983 0.009946 李博
2020-09-30 377010 0.467850 李博
2020-09-30 000457 0.522204 李博
```


# 可转债行情数据说明

可获取可转债合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price.

## 可转债基础信息，历史日行情、分钟线

### `convertible.instruments` - 获取可转债合约基础信息

```python
convertible.instruments(order_book_ids)
```

获取债券合约基础信息。

**参数**

| 参数           | 类型            | 说明                               |
| :------------- | :-------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |

**返回**

传入一个order_book_id，函数会返回一个instrument object
传入多个order_book_id，函数会返回一个instrument object list

| 字段                     | 类型        | 说明                                     |
| :----------------------- | :---------- | :--------------------------------------- |
| `order_book_id`          | `str`       | 可转债合约代码                                 |
| `full_name`              | `str`       | 债券全称                                     |
| `symbol`                 | `str`       | 债券简称                                     |
| `call_protection`        | `integer`   | 强赎保护期（月计），即此段时间不可强制赎回       |
| `issue_price`            | `float`     | 发行价格                                     |
| `total_issue_size`       | `float`     | 发行总规模                                   |
| `listed_date`            | `datetime`  | 上市日                                       |
| `de_listed_date`         | `datetime`  | 债券摘牌日                                   |
| `stop_trading_date`      | `datetime`  | 停止交易日                                   |
| `value_date`             | `datetime`  | 起息日                                       |
| `maturity_date`          | `datetime`  | 到期日(初期公告披露的日期)                   |
| `early_maturity_date`    | `datetime`  | 实际到期日                                   |
| `par_value`              | `float`     | 面值                                         |
| `coupon_rate`            | `float`     | 发行票面利率                                 |
| `coupon_frequency`       | `float`     | 付息频率                                     |
| `compensation_rate`      | `float`     | 到期补偿利率                                 |
| `conversion_start_date`  | `datetime`  | 转换期起始日                                 |
| `conversion_end_date`    | `datetime`  | 转换期截止日                                 |
| `redemption_price`       | `float`     | 到期赎回价格                                 |
| `stock_code`             | `str`       | 对应股票的order_book_id                    |
| `exchange`               | `str`       | 交易所                                       |
| `coupon_method`          | `str`       | 债券计息方式                                 |
| `trade_type`             | `str`       | 交易方式                                     |
| `bond_type`              | `str`       | 债券分类                                     |
| `issue_method`           | `str`       | 发行方式                                     |
| `list_announcement_date` | `datetime`  | 上市公告书发布日                             |
| `pref_allocation_registration_date` | `datetime`  | 老股东优先配售股权登记日                     |
| `pref_allocation_payment_end_date`  | `datetime`  | 老股东优先配售缴款日                         |

*   `eb` 可交换债券
*   `cb` 可转换债券
*   `separately_traded` 就是上交所和深交所公布的可分离债

转债Instrument 对象支持`coupon_rate_table()`方法来获取不同付息期的票息率：

```python
coupon_rate_table()
```

转债Instrument 对象支持`option()`方法来获取赎回和回售条款：

```python
convertible.instruments().option(option_type=None)
```

其中参数`option_type` 可以支持输入`int` 类型1~7 ，代表含义参考下表。默认返回全部类型

| `option_type` | 值 | 说明           |
| :------------ | :- | :------------- |
| 1             |    | 到期赎回权     |
| 2             |    | 发行人赎回权   |
| 3             |    | 有条件回售权   |
| 4             |    | 附加回售权     |
| 5             |    | 无条件回售权   |
| 6             |    | 时点回售权     |
| 7             |    | 价格修正权     |

`option()`方法返回结构为pandas DataFrame，返回字段如下：

| 字段             | 类型       | 说明                                     |
| :--------------- | :--------- | :--------------------------------------- |
| `option_type`    | `int`      | 权利类型                                 |
| `start_date`     | `datetime` | 起止日期                                 |
| `end_date`       | `datetime` | 结束日期                                 |
| `level`          | `float`    | 触发比例                                 |
| `window_days`    | `int`      | 触发天数                                 |
| `reach_days`     | `int`      | 满足天数                                 |
| `frequency`      | `int`      | 类型                                     |
| `payment_year`   | `int`      | 计息年度序列                             |
| `if_include_interest` | `bool`     | 是否包含利息                             |
| `price`          | `float`    | 价格                                     |
| `remark`         | `str`      | 备注                                     |

**范例**

获取110074.XSHE 的基础信息：

```python
[In]convertible.instruments("110074.XSHE")
[Out] Instrument( order_book_id='110074.XSHG', symbol='精达转债', full_name='铜陵精达特种电磁线股份有限公司公开发行可转换公司债券', exchange='XSHG', bond_type='cb', trade_type='dirty_price', value_date=datetime.datetime(2020, 8, 19, 0, 0), listed_date=datetime.datetime(2020, 9, 21, 0, 0), maturity_date=datetime.datetime(2026, 8, 19, 0, 0), early_maturity_date=None, par_value=100.0, coupon_rate=0.004, coupon_frequency=1, coupon_method='stepup_rate', compensation_rate=0.1, total_issue_size=787000000.0, de_listed_date=datetime.datetime(2026, 8, 19, 0, 0), stock_code='600577.XSHG', conversion_start_date=datetime.datetime(2021, 2, 25, 0, 0), conversion_end_date=datetime.datetime(2026, 8, 18, 0, 0), redemption_price=112.0, stop_trading_date=None, issue_price=100.0, issue_method='上网定价,老股东优先配售', list_announcement_date=datetime.datetime(2020, 9, 17, 0, 0), pref_allocation_registration_date=datetime.datetime(2020, 8, 18, 0, 0), pref_allocation_payment_end_date=datetime.datetime(2020, 8, 19, 0, 0), call_protection=6.0 )
```

获取110030.XSHG 格力转债的票息率

```python
[In]convertible.instruments("110030.XSHG").coupon_rate_table()
[Out] coupon_rate start_date end_date
2014-12-25 2015-12-24 0.6
2015-12-25 2016-12-24 0.8
2016-12-25 2017-12-24 1.0
2017-12-25 2018-12-24 1.5
2018-12-25 2019-12-24 6.0
```

获取110058.XSHG 赎回和回售条款

```python
[In]convertible.instruments('110058.XSHG').option()
[Out] option_type start_date end_date payment_year level window_days reach_days frequency price if_include_interest remark
0 1 NaT NaT NaN NaN NaN NaN NaN 110.0 True (1)到期赎回条款\r\n 在本次发行的可转换公司债券期满后5个交易日内,公司...
1 2 2019-10-22 2025-04-15 1.0 1.30 30.0 15.0 随时 100.4 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
2 2 2019-10-22 2025-04-15 2.0 1.30 30.0 15.0 随时 100.6 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
3 2 2019-10-22 2025-04-15 3.0 1.30 30.0 15.0 随时 101.0 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
4 2 2019-10-22 2025-04-15 4.0 1.30 30.0 15.0 随时 101.5 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
5 2 2019-10-22 2025-04-15 5.0 1.30 30.0 15.0 随时 101.8 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
6 2 2019-10-22 2025-04-15 6.0 1.30 30.0 15.0 随时 102.0 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
7 3 2023-04-16 2025-04-16 5.0 0.70 30.0 30.0 每计息年度1次 101.8 True (1)有条件回售条款\r\n 在本次发行的可转换公司债券最后两个计息年度,如果...
8 3 2023-04-16 2025-04-16 6.0 0.70 30.0 30.0 每计息年度1次 102.0 True (1)有条件回售条款\r\n 在本次发行的可转换公司债券最后两个计息年度,如果...
9 4 NaT NaT NaN NaN NaN NaN NaN NaN NaN (2)附加回售条款\r\n 若公司本次发行的可转换公司债券募集资金投资项目的实...
10 7 2019-04-16 2025-04-16 NaN 0.85 30.0 15.0 随时 NaN NaN NaN
```

### `convertible.all_instruments` - 获取所有可转债合约

```python
convertible.all_instruments(date=None)
```

获取所有可转债基础信息,传入日期可筛选该日上市状态合约列表

**返回**

传入一个DataFrame

| 字段                     | 类型        | 说明                                     |
| :----------------------- | :---------- | :--------------------------------------- |
| `order_book_id`          | `str`       | 可转债合约代码                                 |
| `full_name`              | `str`       | 债券全称                                     |
| `symbol`                 | `str`       | 债券简称                                     |
| `call_protection`        | `integer`   | 强赎保护期（月计），即此段时间不可强制赎回       |
| `issue_price`            | `float`     | 发行价格                                     |
| `total_issue_size`       | `float`     | 发行总规模                                   |
| `listed_date`            | `datetime`  | 上市日                                       |
| `de_listed_date`         | `datetime`  | 债券摘牌日                                   |
| `stop_trading_date`      | `datetime`  | 停止交易日                                   |
| `value_date`             | `datetime`  | 起息日                                       |
| `maturity_date`          | `datetime`  | 到期日(初期公告披露的日期)                   |
| `early_maturity_date`    | `datetime`  | 实际到期日                                   |
| `par_value`              | `float`     | 面值                                         |
| `coupon_rate`            | `float`     | 发行票面利率                                 |
| `coupon_frequency`       | `float`     | 付息频率                                     |
| `compensation_rate`      | `float`     | 到期补偿利率                                 |
| `conversion_start_date`  | `datetime`  | 转换期起始日                                 |
| `conversion_end_date`    | `datetime`  | 转换期截止日                                 |
| `redemption_price`       | `float`     | 到期赎回价格                                 |
| `stock_code`             | `str`       | 对应股票的order_book_id                    |
| `exchange`               | `str`       | 交易所                                       |
| `coupon_method`          | `str`       | 债券计息方式                                 |
| `trade_type`             | `str`       | 交易方式                                     |
| `bond_type`              | `str`       | 债券分类                                     |
| `issue_method`           | `str`       | 发行方式                                     |
| `list_announcement_date` | `datetime`  | 上市公告书发布日                             |
| `pref_allocation_registration_date` | `datetime`  | 老股东优先配售股权登记日                     |
| `pref_allocation_payment_end_date`  | `datetime`  | 老股东优先配售缴款日                         |

*   `eb` 可交换债券
*   `cb` 可转换债券
*   `separately_traded` 就是上交所和深交所公布的可分离债

**范例**

获取所有可转债基础信息：

```python
[In]convertible.all_instruments()
[Out] order_book_id symbol full_name exchange bond_type trade_type value_date maturity_date par_value coupon_rate ... coupon_method total_issue_size
0 100001.XSHG 南化转债 南宁化工股份有限公司可转换公司债券 XSHG convertible clean_price 1998-08-03 2003-08-03 100.0 1.00 ... stepup_rate 1.500000e+08
1 100009.XSHG 机场转债 上海国际机场股份有限公司可转换公司债券 XSHG convertible clean_price 2000-02-25 2005-02-25 100.0 0.80 ... fixed_rate 1.350000e+09
2 100016.XSHG 民生转债 中国民生银行股份有限公司可转换公司债券 XSHG convertible clean_price 2003-02-27 2008-02-27 100.0 1.50 ... fixed_rate 4.000000e+09
...
```

### `convertible.get_conversion_price` - 获取可转债转股价信息

```python
convertible.get_conversion_price(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的转股价变动。信息来源为交易所的可转债转股统计公告。

**参数**

| 参数           | 类型                                   | 说明                               |
| :------------- | :------------------------------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期           |

**返回**

返回DataFrame

| 字段               | 类型       | 说明             |
| :----------------- | :--------- | :--------------- |
| `info_date`        | `datetime` | 交易所信息发布日期 |
| `conversion_price` | `float`    | 本次转股价       |
| `effective_date`   | `datetime` | 转股价截止日期   |

**范例**

获取110013.XSHG 的截止某日的转股价变动情况：

```python
[In]convertible.get_conversion_price('110013.XSHG',end_date=20110704)
[Out] conversion_price effective_date order_book_id info_date
110013.XSHG 2011-01-21 7.29 2011-01-25
2011-07-04 7.27 2011-07-04
```

### `convertible.get_conversion_info` - 获取可转债转股导致的规模变动情况

```python
convertible.get_conversion_info(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的转股规模变动。

**参数**

| 参数           | 类型                                   | 说明                               |
| :------------- | :------------------------------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期           |

**返回**

返回DataFrame

| 字段                   | 类型        | 说明                                 |
| :--------------------- | :---------- | :----------------------------------- |
| `info_date`            | `datetime`  | 信息发布日                           |
| `total_amount_converted` | `integer`   | 累计转债已经转为股票的金额（元），累计每次转股金额 |
| `total_shares_converted` | `float`     | 累计转股数                           |
| `remaining_amount`     | `integer`   | 尚未转股的转债金额（元）             |
| `amount_converted`     | `integer`   | 本期转债已转为股票的金额（元）, 近似本期转股价与转股数乘积取值 |
| `shares_converted`     | `integer`   | 本期转股股数                         |
| `end_date`             | `datetime`  | 截止日期                             |
| `conversion_price`     | `float`     | 本次转股价                           |

**范例**

获取110044.XSHG 的转股规模变动情况：

```python
[In]convertible.get_conversion_info('110044.XSHG')
[Out] amount_converted conversion_price end_date remaining_amount shares_converted total_amount_converted total_shares_converted order_book_id info_date
110044.XSHG 2019-01-04 455562.48 6.91 2019-01-03 7.995444e+08 65928.0 4.555625e+05 65928.0
2019-01-07 683792.87 6.91 2019-01-04 7.988606e+08 98957.0 1.139355e+06 164885.0
2019-01-08 86068043.98 6.91 2019-01-07 7.127926e+08 12455578.0 8.720740e+07 12620463.0
2019-01-09 38735269.53 6.91 2019-01-08 6.740573e+08 5605683.0 1.259427e+08 18226146.0
2019-01-10 70718653.68 6.91 2019-01-09 6.033387e+08 10234248.0 1.966613e+08 28460394.0
...
```

### `convertible.get_call_info` - 获取可转债强赎信息

```python
convertible.get_call_info(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的强制赎回信息。

**参数**

| 参数           | 类型                                   | 说明                               |
| :------------- | :------------------------------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期           |

**返回**

返回DataFrame

| 字段               | 类型        | 说明                               |
| :----------------- | :---------- | :--------------------------------- |
| `info_date`        | `datetime`  | 信息发布日                         |
| `exercise_price`   | `float`     | 行权价格                           |
| `interest_included` | `integer`   | 0 对应不包含，1 对应包含。Null 对应不明确 |
| `interest_amount`  | `float`     | 应计利息                           |
| `exercise_date`    | `datetime`  | 行权日                             |
| `call_amount`      | `integer`   | 赎回债券票面金额                   |
| `record_date`      | `datetime`  | 理论登记日，不跳过假日             |

**范例**

获取110020.XSHG 的强赎情况：

```python
[In]convertible.get_call_info('110020.XSHG')
[Out] call_amount exercise_date exercise_price interest_amount interest_included record_date order_book_id info_date
110020.XSHG 2015-01-22 8111000.0 2015-03-11 104.0 1.6 1 2015-03-10
```

### `convertible.get_put_info` - 获取可转债回售信息

```python
convertible.get_put_info(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的持有人回售信息。

**参数**

| 参数           | 类型                                   | 说明                               |
| :------------- | :------------------------------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期           |

**返回**

返回DataFrame

| 字段                  | 类型        | 说明                               |
| :-------------------- | :---------- | :--------------------------------- |
| `info_date`           | `datetime`  | 信息发布日                         |
| `exercise_price`      | `float`     | 行权价格                           |
| `interest_included`   | `integer`   | 0 对应不包含，1 对应包含。Null 对应不明确 |
| `interest_amount`     | `float`     | 应计利息                           |
| `enrollment_start_date` | `datetime`  | 回售登记开始日期                   |
| `enrollment_end_date` | `datetime`  | 回售登记结束日期                   |
| `payment_date`        | `datetime`  | 资金到账日                         |
| `put_amount`          | `integer`   | 回售债券票面金额                   |
| `put_code`            | `str`       | 回售代码                           |

**范例**

获取132002.XSHG 的回售情况：

```python
[In]convertible.get_put_info('132002.XSHG')
[Out] enrollment_end_date enrollment_start_date exercise_price interest_amount interest_included payment_date put_amount put_code order_book_id info_date
132002.XSHG 2018-06-25 2018-07-06 2018-07-02 107.0 0.08 1 2018-07-11 1.154681e+09 182187
2018-07-16 2018-07-20 2018-07-16 107.0 0.12 1 2018-07-25 5.166000e+06 182152
2018-07-24 2018-08-03 2018-07-30 107.0 0.16 1 2018-08-08 6.792000e+06 182153
...
```

### `convertible.get_cash_flow` - 获取可转债的现金流

```python
convertible.get_cash_flow(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约的现金流数据。

**参数**

| 参数           | 类型                                   | 说明                           |
| :------------- | :------------------------------------- | :----------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债流通场所代码             |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的兑付日   |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认则返回开始日期后续所有兑付日 |

**返回**

返回DataFrame

| 字段                  | 类型        | 说明                 |
| :-------------------- | :---------- | :------------------- |
| `payment_date`        | `datetime`  | 理论兑付日           |
| `payment_date_act`    | `datetime`  | 实际兑付日           |
| `record_date`         | `datetime`  | 债券登记日           |
| `interest_payment_pretax` | `float`     | 每百元面额付息(税前) |
| `interest_payment`    | `float`     | 每百元面额付息       |
| `principal_payment`   | `float`     | 每百元面额兑付现金   |
| `cash_flow_pretax`    | `float`     | 税前现金流           |
| `cash_flow`           | `float`     | 税后现金流           |

**范例**

获取110032.XSHG 的现金流情况：

```python
[In]convertible.get_cash_flow('110032.XSHG')
[Out] record_date cash_flow_pretax principal_payment interest_payment_pretax payment_date_act cash_flow interest_payment order_book_id payment_date
110032.XSHG 2017-01-04 2017-01-03 0.200 0.0 0.200 2017-01-10 0.1600 0.1600
2018-01-04 2018-01-03 0.500 0.0 0.500 2018-01-10 0.4000 0.4000
2019-01-04 2019-01-03 1.000 0.0 1.000 2019-01-10 0.8000 0.8000
2019-03-20 2019-03-19 100.304 100.0 0.304 2019-03-26 100.2432 0.2432
...
```

### `convertible.is_suspended` - 判断可转债是否全天停牌

```python
convertible.is_suspended(order_book_ids,start_date=None,end_date=None)
```

判断某只可转债或列表在一段时间内是否全天停牌

**参数**

| 参数           | 类型                                   | 说明                               |
| :------------- | :------------------------------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债合约代码                       |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为转债上市日期         |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期，如果转债已经退市，则为退市日期 |

**返回**

pandas DataFrame
如果在查询期间内转债尚未上市，或已退市，函数则报错提示；如果开始日期早于转债上市日期，则以转债上市日期作为开始日期

**范例**

获取国轩转债从2020 年5 月1 日至2020 年5 月30 日的停牌情况：

```python
[In]convertible.is_suspended('128086.XSHE',20200501,20200530)
[Out] 128086.XSHE
2020-05-06 False
2020-05-07 False
2020-05-08 False
2020-05-11 False
2020-05-12 False
2020-05-13 False
2020-05-14 False
2020-05-15 False
2020-05-18 False
2020-05-19 False
2020-05-20 True
2020-05-21 True
2020-05-22 True
2020-05-25 True
2020-05-26 True
2020-05-27 True
2020-05-28 True
2020-05-29 False
```

### `convertible.get_instrument_industry` - 获取转债所属行业分类信息

```python
convertible.get_instrument_industry('order_book_ids',source=None,level=1,date=None)
```

转债行业分类即为对应正股上市公司行业分类
日期默认为当前最新，获取转债对应正股指定日期行业分类

**参数**

| 参数           | 类型            | 说明                               |
| :------------- | :-------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码                 |
| `date`         | `Str`           | 行业分类指定查询日期               |
| `source`       | `str`           | 指定行业分标准，包含citics 以及citics_2019: 中信, gildata: 聚源 |
| `level`        | `int`           | 行业分类级别，共三级，默认返回一级分类。参数0,1,2,3 一一对应，其中0 返回三级分类完整情况 |

**返回**

| 字段                  | 类型   | 说明           |
| :-------------------- | :----- | :------------- |
| `first_industry_code` | `int`  | 一级行业分类代码 |
| `first_industry_name` | `str`  | 一级行业分类名称 |
| `second_industry_code` | `int`  | 二级行业分类代码 |
| `second_industry_name` | `str`  | 二级行业分类名称 |
| `third_industry_code` | `int`  | 三级行业分类代码 |
| `third_industry_name` | `str`  | 三级行业分类名称 |

**范例**

获取当前转债所对应的一级行业分类

```python
[In] rqdatac.convertible.get_instrument_industry('113029.XSHG')
[Out] first_industry_code first_industry_name order_book_id
113029.XSHG 27 电力设备
```

获取当前转债组所对应的中信行业的全部分类：

```python
[In] rqdatac.convertible.get_instrument_industry(['125069.XSHE','113029.XSHG'],source='citics',level=0)
[Out] first_industry_code first_industry_name second_industry_code second_industry_name third_industry_code third_industry_name order_book_id
125069.XSHE 42 房地产4210 房地产开发管理421020 商业地产
113029.XSHG 27 电力设备2730 新能源设备273010 风电
```

### `convertible.get_industry` - 获取指定行业分类下的转债列表

```python
convertible.get_industry(industry,source='citics',date=None)
```

指定日期，返回指定行业分类下该日期仍在上市状态下的转债列表；默认日期, 即返回所有转债列表

**参数**

| 参数       | 类型   | 说明                               |
| :--------- | :----- | :--------------------------------- |
| `industry` | `str`  | 对应行业分类名称                   |
| `date`     | `Str`  | 行业分类指定查询日期               |
| `source`   | `str`  | 指定行业分标准，包含citics 以及citics_2019: 中信, gildata: 聚源 |

**返回**

可转债id 列表

**范例**

获取指定行业分类、日期上市状态可转债id 列表

```python
[In] rqdatac.convertible.get_industry(industry='电气设备',source='citics_2019',date='2020-01-26')
[Out] ['113505.XSHG', '113546.XSHG', '113549.XSHG', '123014.XSHE', '123030.XSHE', '123034.XSHE', '128018.XSHE', '128042.XSHE', '128089.XSHE']
```

### `convertible.get_accrued_interest_eod` - 获取可转债日终应计利息

```python
convertible.get_accrued_interest_eod(order_book_ids,start_date=None,end_date=None)
```

不指定日期, 返回近3 个月日应计利息数据；应计利息从转债起息日起算

**参数**

| 参数           | 类型            | 说明           |
| :------------- | :-------------- | :------------- |
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date`     | `Str`           | 查询起始日期   |
| `end_date`       | `str`           | 查询截止日期   |

**返回**

可转债id 对应日期区间应计利息pd.DataFrame

**范例**

获取指定可转债id 应计利息数据

```python
[In] rqdatac.convertible.get_accrued_interest_eod('110072.XSHG','20200805','20201101')
[Out] 110072.XSHG
date
2020-08-18 0.000000
2020-08-19 0.000548
2020-08-20 0.001096
2020-08-21 0.001644
2020-08-22 0.002192
...
...
2020-10-28 0.038904
2020-10-29 0.039452
2020-10-30 0.040000
2020-10-31 0.040548
2020-11-01 0.041096
```

### `convertible.get_call_announcement` - 获取可转债赎回提示性公告数据

```python
convertible.get_call_announcement(order_book_ids,start_date=None,end_date=None)
```

查询可转债赎回提示性公告数据，包含赎回和不赎回的信息

**参数**

| 参数           | 类型            | 说明           |
| :------------- | :-------------- | :------------- |
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date`     | `Str`           | 查询起始日期   |
| `end_date`       | `str`           | 查询截止日期   |

**返回**

| 字段                      | 类型       | 说明                               |
| :------------------------ | :--------- | :--------------------------------- |
| `info_date`               | `datetime` | 公告日                             |
| `first_info_date`         | `datetime` | 首次发布赎回公告日                 |
| `if_call`                 | `bool`     | 是否赎回                           |
| `if_issuer call`          | `bool`     | 是否发行人赎回(True-发行人赎回，False-到期赎回) |
| `call_price`              | `float`    | 赎回价格(扣税,元/张)               |
| `call_price_before_tax`   | `float`    | 赎回价格(含税,元/张)               |
| `stop_exe_start_date`     | `datetime` | 触发不行权区间起始日               |
| `stop_exe_end_date`       | `datetime` | 触发不行权区间截止日               |
| `update_time`             | `datetime` | 数据入库时间                       |

**范例**

获取指定可转债id 赎回提示性公告数据

```python
[In] rqdatac.convertible.get_call_announcement('113541.XSHG')
[Out] update_time call_price if_call first_info_date stop_exe_start_date call_price_before_tax stop_exe_end_date order_book_id info_date
113541.XSHG 2021-11-23 2021-11-22 17:09:26 NaN False NaT 2021-11-23 NaN 2022-05-22 2022-06-14 2022-06-14 00:00:00 100.746 True 2022-05-24 NaT 100.932 NaT
```

### `convertible.get_close_price` - 获取可转债全价净价数据

```python
convertible.get_close_price(order_book_ids,start_date=None,end_date=None,fields=None)
```

查询可转债当日收盘价的全价和净价数据

**参数**

| 参数           | 类型            | 说明                               |
| :------------- | :-------------- | :--------------------------------- |
| `order_book_ids` | `str` OR `str list` | 可转债合约代码                       |
| `start_date`     | `Str`           | 查询起始日期                       |
| `end_date`       | `str`           | 查询截止日期                       |
| `fields`         | `str` OR `str list` | 可输入clean_price 或dirty_price，默认返回全部 |

**返回**

| 字段            | 类型       | 说明             |
| :-------------- | :--------- | :--------------- |
| `datetime`      | `datetime` | 收盘价日期       |
| `order_book_id` | `str`      | 可转债合约代码   |
| `clean_price`   | `float`    | 可转债当日收盘价净价 |
| `dirty_price`   | `float`    | 可转债当日收盘价全价 |

**范例**

获取指定可转债id 赎回提示性公告数据

```python
[In] rqdatac.convertible.get_close_price(['132020.XSHG','132026.XSHG'],start_date='2024-04-30', end_date='2024-04-30')
[Out] clean_price dirty_price order_book_id date
132020.XSHG 2024-04-30 110.673 111.207247
132026.XSHG 2024-04-30 125.525 125.616507
```

## 可转债衍生数据

### `convertible.get_indicators` - 获取可转债衍生指标

```python
convertible.get_indicators(order_book_ids,start_date=None, end_date=None,fields=None)
```

**参数**

| 参数           | 类型                                   | 说明             |
| :------------- | :------------------------------------- | :--------------- |
| `order_book_ids` | `str` OR `str list`                    | 可转债合约代码   |
| `start_date`     | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询开始日期     |
| `end_date`       | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询结束日期     |
| `fields`         | `str` OR `str list`                    | 选定返回列，默认返回全部 |

**返回**

| 字段                               | 类型        | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
This response contains some lengthy python code and its explanation. If the user expects to see the entire content, please remind me. Otherwise, I will truncate the code.可转债行情数据说明| Ricequant Docs

# 可转债行情数据说明

可获取可转债合约的日行情、分钟行情、tick 行情数据，具体调用方式请参考API-get_price.

## 可转债基础信息，历史日行情、分钟线

### `convertible.instruments` - 获取可转债合约基础信息

```python
convertible.instruments(order_book_ids)
```

获取债券合约基础信息。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |

**返回**

传入一个order_book_id，函数会返回一个instrument object 传入多个order_book_id，函数会返回一个instrument object list

| 字段 | 类型 | 说明 |
|---|---|---|
| `order_book_id` | `str` | 可转债合约代码 |
| `full_name` | `str` | 债券全称 |
| `symbol` | `str` | 债券简称 |
| `call_protection` | `integer` | 强赎保护期（月计），即此段时间不可强制赎回 |
| `issue_price` | `float` | 发行价格 |
| `total_issue_size` | `float` | 发行总规模 |
| `listed_date` | `datetime` | 上市日 |
| `de_listed_date` | `datetime` | 债券摘牌日 |
| `stop_trading_date` | `datetime` | 停止交易日 |
| `value_date` | `datetime` | 起息日 |
| `maturity_date` | `datetime` | 到期日(初期公告披露的日期) |
| `early_maturity_date` | `datetime` | 实际到期日 |
| `par_value` | `float` | 面值 |
| `coupon_rate` | `float` | 发行票面利率 |
| `coupon_frequency` | `float` | 付息频率 |
| `compensation_rate` | `float` | 到期补偿利率 |
| `conversion_start_date` | `datetime` | 转换期起始日 |
| `conversion_end_date` | `datetime` | 转换期截止日 |
| `redemption_price` | `float` | 到期赎回价格 |
| `stock_code` | `str` | 对应股票的order_book_id |
| `exchange` | `str` | 交易所 |
| `coupon_method` | `str` | 债券计息方式 |
| `trade_type` | `str` | 交易方式 |
| `bond_type` | `str` | 债券分类 |
| `issue_method` | `str` | 发行方式 |
| `list_announcement_date` | `datetime` | 上市公告书发布日 |
| `pref_allocation_registration_date` | `datetime` | 老股东优先配售股权登记日 |
| `pref_allocation_payment_end_date` | `datetime` | 老股东优先配售缴款日 |

- `eb` 可交换债券
- `cb` 可转换债券
- `separately_traded` 就是上交所和深交所公布的可分离债

转债Instrument 对象支持`coupon_rate_table()`方法来获取不同付息期的票息率：

```python
coupon_rate_table()
```

转债Instrument 对象支持`option()`方法来获取赎回和回售条款：

```python
convertible.instruments().option(option_type=None)
```

其中参数`option_type` 可以支持输入`int` 类型1~7 ，代表含义参考下表。默认返回全部类型

| `option_type` | 值 | 说明 |
|---|---|---|
| 1 | | 到期赎回权 |
| 2 | | 发行人赎回权 |
| 3 | | 有条件回售权 |
| 4 | | 附加回售权 |
| 5 | | 无条件回售权 |
| 6 | | 时点回售权 |
| 7 | | 价格修正权 |

`option()`方法返回结构为pandas DataFrame，返回字段如下：

| 字段 | 类型 | 说明 |
|---|---|---|
| `option_type` | `int` | 权利类型 |
| `start_date` | `datetime` | 起止日期 |
| `end_date` | `datetime` | 结束日期 |
| `level` | `float` | 触发比例 |
| `window_days` | `int` | 触发天数 |
| `reach_days` | `int` | 满足天数 |
| `frequency` | `int` | 类型 |
| `payment_year` | `int` | 计息年度序列 |
| `if_include_interest` | `bool` | 是否包含利息 |
| `price` | `float` | 价格 |
| `remark` | `str` | 备注 |

**范例**

获取110074.XSHE 的基础信息：

```python
[In]convertible.instruments("110074.XSHE")
[Out] Instrument( order_book_id='110074.XSHG', symbol='精达转债', full_name='铜陵精达特种电磁线股份有限公司公开发行可转换公司债券', exchange='XSHG', bond_type='cb', trade_type='dirty_price', value_date=datetime.datetime(2020, 8, 19, 0, 0), listed_date=datetime.datetime(2020, 9, 21, 0, 0), maturity_date=datetime.datetime(2026, 8, 19, 0, 0), early_maturity_date=None, par_value=100.0, coupon_rate=0.004, coupon_frequency=1, coupon_method='stepup_rate', compensation_rate=0.1, total_issue_size=787000000.0, de_listed_date=datetime.datetime(2026, 8, 19, 0, 0), stock_code='600577.XSHG', conversion_start_date=datetime.datetime(2021, 2, 25, 0, 0), conversion_end_date=datetime.datetime(2026, 8, 18, 0, 0), redemption_price=112.0, stop_trading_date=None, issue_price=100.0, issue_method='上网定价,老股东优先配售', list_announcement_date=datetime.datetime(2020, 9, 17, 0, 0), pref_allocation_registration_date=datetime.datetime(2020, 8, 18, 0, 0), pref_allocation_payment_end_date=datetime.datetime(2020, 8, 19, 0, 0), call_protection=6.0 )
```

获取110030.XSHG 格力转债的票息率

```python
[In]convertible.instruments("110030.XSHG").coupon_rate_table()
[Out] coupon_rate start_date end_date
2014-12-25 2015-12-24 0.6
2015-12-25 2016-12-24 0.8
2016-12-25 2017-12-24 1.0
2017-12-25 2018-12-24 1.5
2018-12-25 2019-12-24 6.0
```

获取110058.XSHG 赎回和回售条款

```python
[In]convertible.instruments('110058.XSHG').option()
[Out] option_type start_date end_date payment_year level window_days reach_days frequency price if_include_interest remark
0 1 NaT NaT NaN NaN NaN NaN NaN 110.0 True (1)到期赎回条款\r\n 在本次发行的可转换公司债券期满后5个交易日内,公司...
1 2 2019-10-22 2025-04-15 1.0 1.30 30.0 15.0 随时 100.4 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
2 2 2019-10-22 2025-04-15 2.0 1.30 30.0 15.0 随时 100.6 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
3 2 2019-10-22 2025-04-15 3.0 1.30 30.0 15.0 随时 101.0 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
4 2 2019-10-22 2025-04-15 4.0 1.30 30.0 15.0 随时 101.5 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
5 2 2019-10-22 2025-04-15 5.0 1.30 30.0 15.0 随时 101.8 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
6 2 2019-10-22 2025-04-15 6.0 1.30 30.0 15.0 随时 102.0 True (2)有条件赎回条款\r\n 在本次发行的可转换公司债券转股期内,当下述两种情...
7 3 2023-04-16 2025-04-16 5.0 0.70 30.0 30.0 每计息年度1次 101.8 True (1)有条件回售条款\r\n 在本次发行的可转换公司债券最后两个计息年度,如果...
8 3 2023-04-16 2025-04-16 6.0 0.70 30.0 30.0 每计息年度1次 102.0 True (1)有条件回售条款\r\n 在本次发行的可转换公司债券最后两个计息年度,如果...
9 4 NaT NaT NaN NaN NaN NaN NaN NaN NaN (2)附加回售条款\r\n 若公司本次发行的可转换公司债券募集资金投资项目的实...
10 7 2019-04-16 2025-04-16 NaN 0.85 30.0 15.0 随时 NaN NaN NaN
```

### `convertible.all_instruments` - 获取所有可转债合约

```python
convertible.all_instruments(date=None)
```

获取所有可转债基础信息,传入日期可筛选该日上市状态合约列表

**返回**

传入一个DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `order_book_id` | `str` | 可转债合约代码 |
| `full_name` | `str` | 债券全称 |
| `symbol` | `str` | 债券简称 |
| `call_protection` | `integer` | 强赎保护期（月计），即此段时间不可强制赎回 |
| `issue_price` | `float` | 发行价格 |
| `total_issue_size` | `float` | 发行总规模 |
| `listed_date` | `datetime` | 上市日 |
| `de_listed_date` | `datetime` | 债券摘牌日 |
| `stop_trading_date` | `datetime` | 停止交易日 |
| `value_date` | `datetime` | 起息日 |
| `maturity_date` | `datetime` | 到期日(初期公告披露的日期) |
| `early_maturity_date` | `datetime` | 实际到期日 |
| `par_value` | `float` | 面值 |
| `coupon_rate` | `float` | 发行票面利率 |
| `coupon_frequency` | `float` | 付息频率 |
| `compensation_rate` | `float` | 到期补偿利率 |
| `conversion_start_date` | `datetime` | 转换期起始日 |
| `conversion_end_date` | `datetime` | 转换期截止日 |
| `redemption_price` | `float` | 到期赎回价格 |
| `stock_code` | `str` | 对应股票的order_book_id |
| `exchange` | `str` | 交易所 |
| `coupon_method` | `str` | 债券计息方式 |
| `trade_type` | `str` | 交易方式 |
| `bond_type` | `str` | 债券分类 |
| `issue_method` | `str` | 发行方式 |
| `list_announcement_date` | `datetime` | 上市公告书发布日 |
| `pref_allocation_registration_date` | `datetime` | 老股东优先配售股权登记日 |
| `pref_allocation_payment_end_date` | `datetime` | 老股东优先配售缴款日 |

- `eb` 可交换债券
- `cb` 可转换债券
- `separately_traded` 就是上交所和深交所公布的可分离债

**范例**

获取所有可转债基础信息：

```python
[In]convertible.all_instruments()
[Out] order_book_id symbol full_name exchange bond_type trade_type value_date maturity_date par_value coupon_rate ... coupon_method total_issue_size
0 100001.XSHG 南化转债 南宁化工股份有限公司可转换公司债券 XSHG convertible clean_price 1998-08-03 2003-08-03 100.0 1.00 ... stepup_rate 1.500000e+08
1 100009.XSHG 机场转债 上海国际机场股份有限公司可转换公司债券 XSHG convertible clean_price 2000-02-25 2005-02-25 100.0 0.80 ... fixed_rate 1.350000e+09
2 100016.XSHG 民生转债 中国民生银行股份有限公司可转换公司债券 XSHG convertible clean_price 2003-02-27 2008-02-27 100.0 1.50 ... fixed_rate 4.000000e+09
...
```

### `convertible.get_conversion_price` - 获取可转债转股价信息

```python
convertible.get_conversion_price(order_book_ids, start_date=None, end_date=None)```

获取可转债合约一段时期的转股价变动。信息来源为交易所的可转债转股统计公告。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期 |

**返回**

返回DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `info_date` | `datetime` | 交易所信息发布日期 |
| `conversion_price` | `float` | 本次转股价 |
| `effective_date` | `datetime` | 转股价截止日期 |

**范例**

获取110013.XSHG 的截止某日的转股价变动情况：

```python
[In]convertible.get_conversion_price('110013.XSHG',end_date=20110704)
[Out] conversion_price effective_date order_book_id info_date
110013.XSHG 2011-01-21 7.29 2011-01-25
2011-07-04 7.27 2011-07-04
```

### `convertible.get_conversion_info` - 获取可转债转股导致的规模变动情况

```python
convertible.get_conversion_info(order_book_ids, start_date=None, end_date=None)```

获取可转债合约一段时期的转股规模变动。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期 |

**返回**

返回DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `info_date` | `datetime` | 信息发布日 |
| `total_amount_converted` | `integer` | 累计转债已经转为股票的金额（元），累计每次转股金额 |
| `total_shares_converted` | `float` | 累计转股数 |
| `remaining_amount` | `integer` | 尚未转股的转债金额（元） |
| `amount_converted` | `integer` | 本期转债已转为股票的金额（元）, 近似本期转股价与转股数乘积取值 |
| `shares_converted` | `integer` | 本期转股股数 |
| `end_date` | `datetime` | 截止日期 |
| `conversion_price` | `float` | 本次转股价 |

**范例**

获取110044.XSHG 的转股规模变动情况：

```python
[In]convertible.get_conversion_info('110044.XSHG')
[Out] amount_converted conversion_price end_date remaining_amount shares_converted total_amount_converted total_shares_converted order_book_id info_date
110044.XSHG 2019-01-04 455562.48 6.91 2019-01-03 7.995444e+08 65928.0 4.555625e+05 65928.0
2019-01-07 683792.87 6.91 2019-01-04 7.988606e+08 98957.0 1.139355e+06 164885.0
2019-01-08 86068043.98 6.91 2019-01-07 7.127926e+08 12455578.0 8.720740e+07 12620463.0
2019-01-09 38735269.53 6.91 2019-01-08 6.740573e+08 5605683.0 1.259427e+08 18226146.0
2019-01-10 70718653.68 6.91 2019-01-09 6.033387e+08 10234248.0 1.966613e+08 28460394.0
...
```

### `convertible.get_call_info` - 获取可转债强赎信息

```python
convertible.get_call_info(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的强制赎回信息。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期 |

**返回**

返回DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `info_date` | `datetime` | 信息发布日 |
| `exercise_price` | `float` | 行权价格 |
| `interest_included` | `integer` | 0 对应不包含，1 对应包含。Null 对应不明确 |
| `interest_amount` | `float` | 应计利息 |
| `exercise_date` | `datetime` | 行权日 |
| `call_amount` | `integer` | 赎回债券票面金额 |
| `record_date` | `datetime` | 理论登记日，不跳过假日 |

**范例**

获取110020.XSHG 的强赎情况：

```python
[In]convertible.get_call_info('110020.XSHG')
[Out] call_amount exercise_date exercise_price interest_amount interest_included record_date order_book_id info_date
110020.XSHG 2015-01-22 8111000.0 2015-03-11 104.0 1.6 1 2015-03-10
```

### `convertible.get_put_info` - 获取可转债回售信息

```python
convertible.get_put_info(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约一段时期的持有人回售信息。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码，可传入order_book_id, order_book_id list。 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的信息发布日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期 |

**返回**

返回DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `info_date` | `datetime` | 信息发布日 |
| `exercise_price` | `float` | 行权价格 |
| `interest_included` | `integer` | 0 对应不包含，1 对应包含。Null 对应不明确 |
| `interest_amount` | `float` | 应计利息 |
| `enrollment_start_date` | `datetime` | 回售登记开始日期 |
| `enrollment_end_date` | `datetime` | 回售登记结束日期 |
| `payment_date` | `datetime` | 资金到账日 |
| `put_amount` | `integer` | 回售债券票面金额 |
| `put_code` | `str` | 回售代码 |

**范例**

获取132002.XSHG 的回售情况：

```python
[In]convertible.get_put_info('132002.XSHG')
[Out] enrollment_end_date enrollment_start_date exercise_price interest_amount interest_included payment_date put_amount put_code order_book_id info_date
132002.XSHG 2018-06-25 2018-07-06 2018-07-02 107.0 0.08 1 2018-07-11 1.154681e+09 182187
2018-07-16 2018-07-20 2018-07-16 107.0 0.12 1 2018-07-25 5.166000e+06 182152
2018-07-24 2018-08-03 2018-07-30 107.0 0.16 1 2018-08-08 6.792000e+06 182153
...
```

### `convertible.get_cash_flow` - 获取可转债的现金流

```python
convertible.get_cash_flow(order_book_ids, start_date=None, end_date=None)
```

获取可转债合约的现金流数据。

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为初始的兑付日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认则返回开始日期后续所有兑付日 |

**返回**

返回DataFrame

| 字段 | 类型 | 说明 |
|---|---|---|
| `payment_date` | `datetime` | 理论兑付日 |
| `payment_date_act` | `datetime` | 实际兑付日 |
| `record_date` | `datetime` | 债券登记日 |
| `interest_payment_pretax` | `float` | 每百元面额付息(税前) |
| `interest_payment` | `float` | 每百元面额付息 |
| `principal_payment` | `float` | 每百元面额兑付现金 |
| `cash_flow_pretax` | `float` | 税前现金流 |
| `cash_flow` | `float` | 税后现金流 |

**范例**

获取110032.XSHG 的现金流情况：

```python
[In]convertible.get_cash_flow('110032.XSHG')
[Out] record_date cash_flow_pretax principal_payment interest_payment_pretax payment_date_act cash_flow interest_payment order_book_id payment_date
110032.XSHG 2017-01-04 2017-01-03 0.200 0.0 0.200 2017-01-10 0.1600 0.1600
2018-01-04 2018-01-03 0.500 0.0 0.500 2018-01-10 0.4000 0.4000
2019-01-04 2019-01-03 1.000 0.0 1.000 2019-01-10 0.8000 0.8000
2019-03-20 2019-03-19 100.304 100.0 0.304 2019-03-26 100.2432 0.2432
...
```

### `convertible.is_suspended` - 判断可转债是否全天停牌

```python
convertible.is_suspended(order_book_ids,start_date=None,end_date=None)
```

判断某只可转债或列表在一段时间内是否全天停牌

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为转债上市日期 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前日期，如果转债已经退市，则为退市日期 |

**返回**

pandas DataFrame 如果在查询期间内转债尚未上市，或已退市，函数则报错提示；如果开始日期早于转债上市日期，则以转债上市日期作为开始日期

**范例**

获取国轩转债从2020 年5 月1 日至2020 年5 月30 日的停牌情况：

```python
[In]convertible.is_suspended('128086.XSHE',20200501,20200530)
[Out] 128086.XSHE
2020-05-06 False
2020-05-07 False
2020-05-08 False
2020-05-11 False
2020-05-12 False
2020-05-13 False
2020-05-14 False
2020-05-15 False
2020-05-18 False
2020-05-19 False
2020-05-20 True
2020-05-21 True
2020-05-22 True
2020-05-25 True
2020-05-26 True
2020-05-27 True
2020-05-28 True
2020-05-29 False
```

### `convertible.get_instrument_industry` - 获取转债所属行业分类信息

```python
convertible.get_instrument_industry('order_book_ids',source=None,level=1,date=None)
```

转债行业分类即为对应正股上市公司行业分类
日期默认为当前最新，获取转债对应正股指定日期行业分类

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债流通场所代码 |
| `date` | `Str` | 行业分类指定查询日期 |
| `source` | `str` | 指定行业分标准，包含citics 以及citics_2019: 中信, gildata: 聚源 |
| `level` | `int` | 行业分类级别，共三级，默认返回一级分类。参数0,1,2,3 一一对应，其中0 返回三级分类完整情况 |

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `first_industry_code` | `int` | 一级行业分类代码 |
| `first_industry_name` | `str` | 一级行业分类名称 |
| `second_industry_code` | `int` | 二级行业分类代码 |
| `second_industry_name` | `str` | 二级行业分类名称 |
| `third_industry_code` | `int` | 三级行业分类代码 |
| `third_industry_name` | `str` | 三级行业分类名称 |

**范例**

获取当前转债所对应的一级行业分类

```python
[In] rqdatac.convertible.get_instrument_industry('113029.XSHG')
[Out] first_industry_code first_industry_name order_book_id
113029.XSHG 27 电力设备
```

获取当前转债组所对应的中信行业的全部分类：

```python
[In] rqdatac.convertible.get_instrument_industry(['125069.XSHE','113029.XSHG'],source='citics',level=0)
[Out] first_industry_code first_industry_name second_industry_code second_industry_name third_industry_code third_industry_name order_book_id
125069.XSHE 42 房地产4210 房地产开发管理421020 商业地产
113029.XSHG 27 电力设备2730 新能源设备273010 风电
```

### `convertible.get_industry` - 获取指定行业分类下的转债列表

```python
convertible.get_industry(industry,source='citics',date=None)
```

指定日期，返回指定行业分类下该日期仍在上市状态下的转债列表；默认日期, 即返回所有转债列表

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `industry` | `str` | 对应行业分类名称 |
| `date` | `Str` | 行业分类指定查询日期 |
| `source` | `str` | 指定行业分标准，包含citics 以及citics_2019: 中信, gildata: 聚源 |

**返回**

可转债id 列表

**范例**

获取指定行业分类、日期上市状态可转债id 列表

```python
[In] rqdatac.convertible.get_industry(industry='电气设备',source='citics_2019',date='2020-01-26')
[Out] ['113505.XSHG', '113546.XSHG', '113549.XSHG', '123014.XSHE', '123030.XSHE', '123034.XSHE', '128018.XSHE', '128042.XSHE', '128089.XSHE']
```

### `convertible.get_accrued_interest_eod` - 获取可转债日终应计利息

```python
convertible.get_accrued_interest_eod(order_book_ids,start_date=None,end_date=None)
```

不指定日期, 返回近3 个月日应计利息数据；应计利息从转债起息日起算

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `Str` | 查询起始日期 |
| `end_date` | `str` | 查询截止日期 |

**返回**

可转债id 对应日期区间应计利息pd.DataFrame

**范例**

获取指定可转债id 应计利息数据

```python
[In] rqdatac.convertible.get_accrued_interest_eod('110072.XSHG','20200805','20201101')
[Out] 110072.XSHG
date
2020-08-18 0.000000
2020-08-19 0.000548
2020-08-20 0.001096
2020-08-21 0.001644
2020-08-22 0.002192
...
...
2020-10-28 0.038904
2020-10-29 0.039452
2020-10-30 0.040000
2020-10-31 0.040548
2020-11-01 0.041096
```

### `convertible.get_call_announcement` - 获取可转债赎回提示性公告数据

```python
convertible.get_call_announcement(order_book_ids,start_date=None,end_date=None)
```

查询可转债赎回提示性公告数据，包含赎回和不赎回的信息

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `Str` | 查询起始日期 |
| `end_date` | `str` | 查询截止日期 |

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `info_date` | `datetime` | 公告日 |
| `first_info_date` | `datetime` | 首次发布赎回公告日 |
| `if_call` | `bool` | 是否赎回 |
| `if_issuer call` | `bool` | 是否发行人赎回(True-发行人赎回，False-到期赎回) |
| `call_price` | `float` | 赎回价格(扣税,元/张) |
| `call_price_before_tax` | `float` | 赎回价格(含税,元/张) |
| `stop_exe_start_date` | `datetime` | 触发不行权区间起始日 |
| `stop_exe_end_date` | `datetime` | 触发不行权区间截止日 |
| `update_time` | `datetime` | 数据入库时间 |

**范例**

获取指定可转债id 赎回提示性公告数据

```python
[In] rqdatac.convertible.get_call_announcement('113541.XSHG')
[Out] update_time call_price if_call first_info_date stop_exe_start_date call_price_before_tax stop_exe_end_date order_book_id info_date
113541.XSHG 2021-11-23 2021-11-22 17:09:26 NaN False NaT 2021-11-23 NaN 2022-05-22 2022-06-14 2022-06-14 00:00:00 100.746 True 2022-05-24 NaT 100.932 NaT
```

### `convertible.get_close_price` - 获取可转债全价净价数据

```python
convertible.get_close_price(order_book_ids,start_date=None,end_date=None,fields=None)
```

查询可转债当日收盘价的全价和净价数据

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `Str` | 查询起始日期 |
| `end_date` | `str` | 查询截止日期 |
| `fields` | `str` OR `str list` | 可输入clean_price 或dirty_price，默认返回全部 |

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `datetime` | `datetime` | 收盘价日期 |
| `order_book_id` | `str` | 可转债合约代码 |
| `clean_price` | `float` | 可转债当日收盘价净价 |
| `dirty_price` | `float` | 可转债当日收盘价全价 |

**范例**

获取指定可转债id 赎回提示性公告数据

```python
[In] rqdatac.convertible.get_close_price(['132020.XSHG','132026.XSHG'],start_date='2024-04-30', end_date='2024-04-30')
[Out] clean_price dirty_price order_book_id date
132020.XSHG 2024-04-30 110.673 111.207247
132026.XSHG 2024-04-30 125.525 125.616507
```

## 可转债衍生数据

### `convertible.get_indicators` - 获取可转债衍生指标

```python
convertible.get_indicators(order_book_ids,start_date=None, end_date=None,fields=None)
```

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询开始日期 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 查询结束日期 |
| `fields` | `str` OR `str list` | 选定返回列，默认返回全部 |

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `conversion_coefficient` | `float` | 转股系数=100/转股价 |
| `conversion_value` | `float` | 转股价值=正股收盘价\*转股系数 |
| `conversion_premium` | `float` | 转股溢价率=(转债收盘全价/转股价值-1)\*100% |
| `yield_to_maturity` | `float` | 税后到期收益率 当日至到期日之间产生的税后未来现金流（到期本金为到期赎回价）贴现为转债当前收盘全价的贴现收益率（若该转债在当日已发布强赎公告，则该指标取值为空） |
| `yield_to_maturity_pretax` | `float` | 税前到期收益率 当日至到期日之间产生的税前未来现金流（到期本金为到期赎回价）贴现为转债当前收盘全价的贴现收益率（若该转债在当日已发布强赎公告，则该指标取值为空） |
| `yield_to_put` | `float` | 税后回售收益率 当日至预测回售日之间产生的税后未来现金流贴现为转债当前收盘全价的贴现收益率（若该转债没有回售条款，则该指标取值为空） |
| `yield_to_put_pretax` | `float` | 税前回售收益率 当日至预测回售日之间产生的税前未来现金流贴现为转债当前收盘全价的贴现收益率（若该转债没有回售条款，则该指标取值为空） |
| `double_low_factor` | `float` | 双低指标 双低指标=转债收盘全价+转股溢价率\*100 |
| `call_trigger_price` | `float` | 赎回触发价 赎回触发价=当期转股价\*130%（无数据代表没有强赎条款） |
| `put_trigger_price` | `float` | 回售触发价 回售触发价=当期转股价\*70%（无数据代表没有回售条款） |
| `conversion_price_reset_trigger_price` | `float` | 下修触发价 下修转股价触发价=当期转股价\*下修触发水平（无数据代表没有下修条款） |
| `turnover_rate` | `float` | 换手率=转债当日成交额/转债剩余市值 |
| `remaining_size` | `float` | 剩余规模（元） 剩余规模=未转股的转债数量\*转债面值 |
| `convertible_market_cap_ratio` | `float` | 转债市值占比 转债剩余市值=转债剩余规模/面值\*现价转债占比=转债剩余市值/正股总市值 |
| `pb_ratio` | `float` | 市净率 当前正股总市值/ 归属母公司股东权益合计mrq |
| `put_qualified_days` | `float` | 回售已满足天数 根据回售条款指定时间区间，统计收盘价低于回售触发价的交易日数量 |
| `call_qualified_days` | `float` | 赎回已满足天数 根据赎回条款指定时间区间，统计正股收盘价高于赎回触发价的交易日数量 |
| `conversion_price_reset_qualified_days` | `float` | 转股价下修已满足天数 根据下修条款指定时间区间，统计正股收盘价低于下修触发价的交易日数量 |
| `put_status` | `float` | 回售条款满足状态 根据回售条款，查看收盘价低于回售触发价的交易日数量是否达到回售条件，如还未进入回售期或没有回售条款状态标为0，如进入回售期但还没有满足回售条件的状态为1，如满足回售条件的状态为2 |
| `call_status` | `float` | 强赎条款满足状态 根据强赎条款，查看收盘价高于赎回触发价的交易日数量是否达到强赎条件，如还未进入转股期或没有强赎条款状态为0，如进入转股期但还没有满足赎回条件的状态为1，如满足赎回条件但还未发强赎公告的状态标为2，如发布了强赎公告的状态为3 |
| `conversion_price_reset_status` | `float` | 下修条款满足状态 根据转股价下修条款，查看收盘价低于下修触发价的交易日数量是否达到下修条件，如还未满足下修条件或没有下修条款状态标为0，满足下修条件状态为1 |
| `pure_bond_value_1` | `float` | 纯债价值 $P = \sum_{t=1}^{n} \frac{I}{(1+R)^{t}} + \frac{P_0}{(1+R)^{t}}$ 式中P：债券的价格； P 0 P_0 P0​：债券面值；I：每年利息；R：市场利率或投资者要求的必要报酬率；n:付息总期数; 其中R 选取相同评级的中债企业债即期收益率作为折现率 |
| `pure_bond_value_premium_1` | `float` | 纯债溢价率 （转债收盘全价/ 纯债价值-1）\*100% |
| `iv` | `float` | 隐含波动率 基于BS 模型使用布伦特方法计算隐含波动率，涉及如下参数： 行权价：以可转债最新转股价为行权价 标的价格：以当日正股收盘价（不复权）为标的价格 待偿期：以当日至转债到期日为待偿期 期权价：以可转债收盘价减去纯债价值再除以转股比例为期权价 无风险利率：以一年期国债利率为无风险利率 股息率：以滚动四季度股息率估计 |
| `delta` | `float` | delta 转债价格对正股股价的敏感度 |
| `theta` | `float` | theta 转债价格对时间的偏导 |
| `gamma` | `float` | gamma 转债价格对正股股价的二阶导 |
| `vega` | `float` | vega 转债价格对隐含波动率的偏导 |
| `pure_bond_value_premium` | `float` | deprecate |
| `pure_bond_value_premium_pretax` | `float` | deprecate |
| `pure_bond_value` | `float` | deprecate |
| `pure_bond_value_pretax` | `float` | deprecate |

到期赎回价：对于没有到期赎回价的转债，将其到期赎回价填为本金加最后一期利息
预计回售日：对于还没有进入回售期的转债，预测回售日期= 回售期开始日期+ 回售触发自然日天数（基于window_days 推算）+ 回售资金到账所需时间（假设为30 自然日）
对于已经进入回售期的转债，预测回售日期=计算日+ 回售触发自然日天数（基于window_days 推算）+ 回售资金到账所需时间（假设为30 自然日）

**范例**

获取指定日期可转债列表衍生指标数据

```python
[In] rqdatac.convertible.get_indicators(['110031.XSHG','110033.XSHG'],start_date=20200803, end_date=20200803)
[Out] call_qualified_days call_status call_trigger_price conversion_coefficient conversion_premium conversion_price_reset_qualified_days conversion_price_reset_status conversion_price_reset_trigger_price conversion_value convertible_market_cap_ratio ... pure_bond_value_pretax put_qualified_days put_status put_trigger_price remaining_size turnover_rate yield_to_maturity yield_to_maturity_pretax yield_to_put yield_to_put_pretax order_book_id date
110031.XSHG 2020-08-03 0 1 28.028 4.638219 0.323528 20 1 19.404 85.204082 0.070301 ... 104.444003 0 1 15.092 2.398829e+09 0.007542 -0.074144 -0.059667 -0.127928 -0.126791
110033.XSHG 2020-08-03 0 1 9.347 13.908206 0.153650 7 0 6.471 98.470097 0.092509 ... 105.106132 0 1 5.033 1.211731e+09 0.005133 -0.036681 -0.024483 -0.447469 -0.440123
```

### `convertible.get_credit_rating` - 获取可转债债项评级数据

```python
convertible.get_credit_rating(order_book_ids, start_date=None, end_date=None, institutions=None)
```

查询可转债债项评级数据（信用评级中：SPC 标识为标普信用评级，pi 为主动评级，u 为主动评级，sf 为结构融资产品的评级）

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `Str` | 查询起始日期 |
| `end_date` | `str` | 查询截止日期 |
| `institutions` | `str` | 不传默认输出所有评级机构。 `institutions` 可选项见下表： |

**institutions 可选项：**

- 上海新世纪资信评估投资服务有限公司
- 上海资信有限公司
- 东方金诚国际信用评估有限公司
- 中债资信评估有限责任公司
- 中国诚信信用管理股份有限公司
- 中证鹏元资信评估股份有限公司
- 中诚信国际信用评级有限责任公司
- 中诚信证评数据科技有限公司
- 云南省资信评估事务所
- 大公国际资信评估有限公司
- 大普信用评级股份有限公司
- 安融信用评级有限公司
- 惠誉博华信用评级有限公司
- 惠誉国际信用评级有限公司
- 标准普尔评级公司
- 标普信用评级(中国)有限公司
- 福建省资信评级委员会
- 穆迪评级公司
- 联合信用评级有限公司
- 联合资信评估股份有限公司
- 远东资信评估有限公司

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` | 可转债合约代码 |
| `credit_date` | `datetime` | 债项评级日期 |
| `info_date` | `datetime` | 公告发布日期 |
| `info_source` | `varchar(200)` | 信息来源 |
| `institution` | `str` | 债项评级机构 |
| `credit` | `str` | 债项评级 |
| `rice_create_tm` | `datetime` | 米筐入库时间 |

**范例**

获取指定可转债id 债项评级数据

```python
[In] rqdatac.convertible.get_credit_rating('110031.XSHE')
[Out] info_date info_source institution credit rice_create_tm order_book_id credit_date
110031.XSHG 2014-08-21 2015-06-10 6-10 资信评级机构为本次发行可转换公司债券出具的资信评级报告 联合信用评级有限公司 AAA 2023-12-11 07:00:18
2015-07-31 2015-08-04 航天信息2014年可转换公司债券跟踪评级分析报告 联合信用评级有限公司AAA 2023-12-11 07:00:18
2016-05-17 2016-05-18 航天信息：可转换公司债券2016年跟踪评级报告 联合信用评级有限公司AAA 2023-12-11 07:00:18
2017-05-17 2017-05-19 航天信息：关于“航信转债”跟踪信用评级结果的公告 联合信用评级有限公司 AAA 2023-12-11 07:00:18
2018-04-26 2018-04-27 航天信息可转换公司债券2018年跟踪评级报告 联合信用评级有限公司 AAA 2023-12-11 07:00:18
2019-05-24 2019-05-28 航天信息可转换公司债券2019年跟踪评级报告 联合信用评级有限公司 AAA 2023-12-11 07:00:18
2020-06-05 2020-06-10 航天信息：可转换公司债券2020年跟踪评级报告 联合信用评级有限公司 AAA 2023-12-11 07:00:18
2021-05-25 2022-02-10 航天信息股份有限公司公开发行可转换公司债券2021年跟踪评级报告联合资信评估股份有限公司AAA 2023-12-11 07:00:18
```

### `convertible.get_std_discount` - 获取可转债标准劵折算率

```python
convertible.get_std_discount(order_book_ids, start_date=None, end_date=None)
```

查询可转债标准劵折算率

**参数**

| 参数 | 类型 | 说明 |
|---|---|---|
| `order_book_ids` | `str` OR `str list` | 可转债合约代码 |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为当前交易日 |
| `end_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为当前交易日 |

**返回**

| 字段 | 类型 | 说明 |
|---|---|---|
| `discount_factor` | `float` | 标准券折算率(每百元面值折算成标准券所乘的系数) |

**范例**

获取指定可转债id 的标准劵折算率

```python
[In] rqdatac.convertible.get_std_discount('110059.XSHG', start_date=20240615, end_date=20240621)
[Out] discount_factor order_book_id date
110059.XSHG 2024-06-17 0.73
2024-06-18 0.73
2024-06-19 0.73
2024-06-20 0.73
2024-06-21 0.73
```



# 风险因子数据

*   **风格因子暴露度**：个股对于特定风格因子的风险暴露。暴露度绝对值越大，则投资组合表现对因子表现变化越敏感。可用于投资组合风格评估、风险敞口管理等。部分风格因子由多个细分风格因子组合而成。
*   **细分风格因子暴露度**：个股对细分风格因子的风险暴露。细分风格因子表示某一类风格中的细分风险维度，用户可根据实际投资研究需求，使用细分风格因子代替风格因子。
*   **个股对指数贝塔**：个股对指数（上证50、沪深300、中证500 等）的原始贝塔值（未进行标准化，因此区别于贝塔因子），可用于指数跟踪或贝塔中性处理。
*   **因子收益率**：因子在给定股票池产生的超额收益。目前提供全市场、沪深300、中证500、中证800 四个股票池的因子收益。用户可根据实际投资研究中所使用的股票池，选择相应的因子收益进行风格追踪和构建指数增强策略。
*   **特异收益率**：个股收益中无法被因子解释的部分。例如上市公司出现高管人员变动，可能引起股价剧烈波动，产生较大的特异收益。此时上市公司股价主要由消息面驱动，而与其所处行业、基本面、及市场行情相关性较低。
*   **因子协方差**：投资组合风险中能被因子解释的系统性风险部分。目前提供日度、月度、季度三套不同预测期限的风险模型，适用于不同调仓频率的交易策略。
*   **特异风险**：投资组合风险中不能被因子解释的，与个股自身特殊因素相关的部分（见上述特异收益率解释）。目前提供日度、月度、季度三个不同预测期限的风险模型，适用于不同调仓频率的交易策略。

## get_factor_exposure - 获取一组股票的因子暴露度

```python
get_factor_exposure(order_book_ids, start_date, end_date, factors = None,industry_mapping='sws_2021', model = 'v1')
```

### 参数

| 参数             | 类型             | 解释                                                         |
| :--------------- | :--------------- | :----------------------------------------------------------- |
| order_book_ids   | str or list of str | 证券代码（例如：'600705.XSHG'）                             |
| start_date       | str              | 开始日期（例如：'2017-03-03'）                               |
| end_date         | str              | 结束日期（例如：'2017-03-20'）                               |
| factors          | None or list of str | 因子。默认获取全部风格因子暴露度（None）。风格因子、行业因子和市场联动字段名称见表1和表2 |
| industry_mapping | str              | 'sws_2021' - 申万2021 行业分类 'citics_2019' - 中信2019 行业分类默认：industry_mapping = 'sws_2021'。 |
| model            | str              | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |

### 返回

MultiIndex 的pandas.DataFrame

index 第一个level 为order_book_id，第二个level 为date， columns 为因子字段名称。

### 范例

**获取单一股票的因子暴露度**

```python
In [24]: rqdatac.get_factor_exposure('600705.XSHG','20170302','20170307',factors=None,industry_mapping='sws_2021' )
Out[24]: momentum beta book_to_price earnings_yield liquidity size residual_volatility non_linear_size comovement leverage growth 银行计算机环保商贸零售电力设备建筑装饰建筑材料农林牧渔电子交通运输汽车纺织服饰医药生物房地产通信公用事业综合机械设备石油石化有色金属传媒家用电器基础化工非银金融社会服务轻工制造国防军工美容护理煤炭食品饮料钢铁date order_book_id
2017-03-02 600705.XSHG -0.765684 -0.085280 0.368789 0.285730 -0.145475 1.423206 -0.643894 -0.779187 1 1.519396 -1.712015 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
2017-03-03 600705.XSHG -0.707014 -0.088001 0.373052 0.290378 -0.171835 1.428292 -0.801934 -0.773588 1 1.521861 -1.711590 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
2017-03-06 600705.XSHG -0.733606 -0.142462 0.392585 0.310572 -0.199674 1.423293 -0.796411 -0.792342 1 1.523406 -1.711635 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
2017-03-07 600705.XSHG -0.755761 -0.108790 0.403361 0.076728 -0.159671 1.450870 -0.727523 -0.743816 1 1.593371 -1.300758 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
```

**获取多股票的因子暴露度**

```python
In [21]: rqdatac.get_factor_exposure(['600705.XSHG','601600.XSHG'],'20170302','20170307',factors=None,industry_mapping='sws_2021' )
Out[21]: momentum beta book_to_price earnings_yield liquidity size residual_volatility non_linear_size comovement leverage growth 银行计算机环保商贸零售电力设备建筑装饰建筑材料农林牧渔电子交通运输汽车纺织服饰医药生物房地产通信公用事业综合机械设备石油石化有色金属传媒家用电器基础化工非银金融社会服务轻工制造国防军工美容护理煤炭食品饮料钢铁date order_book_id
2017-03-02 600705.XSHG -0.765684 -0.085280 0.368789 0.285730 -0.145475 1.423206 -0.643894 -0.779187 1 1.519396 -1.712015 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
601600.XSHG 0.632379 -0.577844 2.116718 0.256038 0.471672 1.394988 0.846457 -0.826099 1 1.431694 0.067756 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0
2017-03-03 600705.XSHG -0.707014 -0.088001 0.373052 0.290378 -0.171835 1.428292 -0.801934 -0.773588 1 1.521861 -1.711590 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
601600.XSHG 0.515550 -0.589616 2.126162 0.289633 0.445027 1.378327 0.863395 -0.855620 1 1.433824 0.067364 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0
2017-03-06 600705.XSHG -0.733606 -0.142462 0.392585 0.310572 -0.199674 1.423293 -0.796411 -0.792342 1 1.523406 -1.711635 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
601600.XSHG 0.360484 -0.741722 2.126348 0.324218 0.421461 1.352377 0.936466 -0.905581 1 1.435153 0.067866 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0
2017-03-07 600705.XSHG -0.755761 -0.108790 0.403361 0.076728 -0.159671 1.450870 -0.727523 -0.743816 1 1.593371 -1.300758 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0
601600.XSHG 0.388384 -0.764634 2.132366 0.344829 0.409443 1.334277 0.914831 -0.933042 1 1.437516 0.068503 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0
```

## get_descriptor_exposure - 获取一组股票的细分风格因子暴露度

```python
get_descriptor_exposure(order_book_ids, start_date, end_date, descriptors = None, model = 'v1')
```

### 参数

| 参数           | 类型             | 解释                                                         |
| :------------- | :--------------- | :----------------------------------------------------------- |
| order_book_ids | str or list of str | 证券代码（例如：'600705.XSHG'）                             |
| start_date     | str              | 开始日期（例如：'2017-03-03'）                               |
| end_date       | str              | 结束日期（例如：'2017-03-20'）                               |
| descriptors    | None or list of str | 细分风格因子。默认获取全部细分风格因子的暴露度（None）。细分风格因子字段名称rqdata-API-fields-1) |
| model          | str              | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |

### 返回

MultiIndex 的pandas.DataFrame

index 第一个level 为order_book_id，第二个level 为date， column 为细分风格因子字段名称。

### 范例

**获取一只股票的全部细分因子暴露度**

```python
In [11]: get_descriptor_exposure('600705.XSHG','20170302','20170307',descriptors=None)
Out[11]: book_leverage cash_earnings_to_price_ratio ... three_months_share_turnover twelve_months_share_turnover order_book_id date ...
600705.XSHG 2017-03-02 1.716070 -2.174376 ... -0.572241 0.073076
2017-03-03 1.718035 -2.161017 ... -0.628732 0.065085
2017-03-06 1.719792 -2.166595 ... -0.681997 0.063982
2017-03-07 1.716341 -2.167661 ... -0.685791 0.071536
```

**获取一只股票一组细分风格因子暴露度**

```python
In [26]: get_descriptor_exposure('600705.XSHG','20170302','20170307',descriptors=['earnings_growth','cash_earnings_to_price_ratio','sales_growth'])
Out[26]: cash_earnings_to_price_ratio earnings_growth sales_growth order_book_id date
600705.XSHG 2017-03-02 -2.174376 -0.412443 -0.434541
2017-03-03 -2.161017 -0.412443 -0.434541
2017-03-06 -2.166595 -0.412443 -0.434541
2017-03-07 -2.167661 -0.438942 -0.454265
```

## get_stock_beta - 获取一组股票的基准贝塔值

```python
get_stock_beta(order_book_ids, start_date, end_date, benchmark='000300.XSHG', model = 'v1')
```

### 参数

| 参数           | 类型             | 解释                                                         |
| :------------- | :--------------- | :----------------------------------------------------------- |
| order_book_ids | str or list of str | 证券代码（例如：'600705.XSHG'）                             |
| start_date     | str              | 开始日期（例如：'2017-03-03'）                               |
| end_date       | str              | 结束日期（例如：'2017-03-20'）                               |
| benchmark      | str              | 基准指数。默认为沪深300（'000300.XSHG'），可选上证50（'000016.XSHG'）、中证500（'000905.XSHG'）、中证800（'000906.XSHG'）以及中证全指（'000985.XSHG'） |
| model          | str              | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |

### 返回

返回pandas.DataFrame

index 为日期， column 为个股的order_book_id

### 范例

**获取一只股票的基准贝塔值**

```python
In [12]: get_stock_beta('600705.XSHG','20170302','20170307' )
Out[12]: order_book_id
date
2017-03-02 1.396796
2017-03-03 1.395106
2017-03-06 1.378063
2017-03-07 1.399051
```

## get_factor_return - 获取因子收益率

```python
get_factor_return(start_date, end_date, factors= None, universe='whole_market',method='implicit',industry_mapping='sws_2021', model = 'v1')
```

### 参数

| 参数             | 类型         | 解释                                                         |
| :--------------- | :----------- | :----------------------------------------------------------- |
| start_date       | str          | 开始日期（例如：'2017-03-03'）                               |
| end_date         | str          | 结束日期（例如：'2017-03-20'）                               |
| factors          | None or list of str | 因子。默认获取全部因子的因子收益率(None)。风格因子、行业因子和市场联动字段名称表1 和表2 |
| universe         | str or list  | 基准指数。默认为全市场('whole_market')，可选沪深300 ('000300.XSHG'),中证500 ('000905.XSHG')、中证800（'000906.XSHG'） |
| method           | _str _       | 计算方法（1）。默认为'implicit'（隐式因子收益率），可选'explicit'（显式风格因子收益率） |
| industry_mapping | str          | 'sws_2021' - 申万2021 行业分类 'citics_2019' - 中信2019 行业分类默认：industry_mapping = 'sws_2021'。 |
| model            | str          | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |

备注：（1） 由于隐式和显式收益率的计算方法差异，当method 参数取值为'implicit' ， 可返回三类因子（风格、行业、市场联动）的隐式因子收益率； 而当method 参数取值为'explicit' ，只可返回风格因子的显式因子收益率。

### 返回

pandas.DataFrame

index 为日期， column 为因子字段名称

### 范例

**获取全部因子的因子收益率**

```python
In [14]: rqdatac.get_factor_return('20170302','20170307',factors=None,universe='whole_market',industry_mapping='sws_2021')
Out[14]: factor beta book_to_price comovement earnings_yield growth leverage liquidity momentum non_linear_size residual_volatility size 交通运输传媒公用事业农林牧渔医药生物商贸零售... 汽车煤炭环保电力设备电子石油石化社会服务纺织服饰综合美容护理计算机轻工制造通信钢铁银行非银金融食品饮料date ...
2017-03-02 -0.001044 -0.003007 -0.005161 0.000895 -0.000558 0.000269 0.000050 0.000680 -0.000147 -0.003483 0.000171 -0.006161 -0.002081 0.000525 -0.004602 -0.002468 -0.003860 ... 0.000504 0.000077 -0.002917 0.004257 0.000370 -0.000766 -0.001737 0.002724 0.000486 0.004157 -0.002981 -0.000922 0.000394 0.005574 0.006018 0.001711 0.000042
2017-03-03 0.001259 -0.001100 -0.001086 0.001748 0.000521 -0.000775 -0.001693 0.000446 0.000820 -0.001096 -0.001652 -0.007279 -0.000296 -0.000124 0.000697 0.000994 -0.001389 ... 0.005918 -0.008764 0.002470 0.009808 0.011413 -0.006552 0.006445 -0.000879 -0.000159 0.000600 0.005042 0.002710 0.002237 -0.008201 -0.006301 -0.001433 0.001814
2017-03-06 0.002535 -0.001496 0.009265 0.000960 0.000021 -0.000851 -0.000870 -0.000549 -0.000989 0.000002 -0.001650 -0.000152 -0.001880 -0.000092 -0.004033 -0.002653 -0.003367 ... -0.001338 0.008903 -0.001816 -0.001218 0.006256 -0.000935 -0.001956 -0.003656 -0.004730 0.000664 0.003920 0.000532 0.005297 0.001956 -0.000836 -0.001430 -0.000331
2017-03-07 -0.000222 -0.001369 0.002965 0.000156 -0.000555 -0.001759 -0.001441 -0.000478 -0.000535 -0.000521 -0.001305 -0.000647 -0.001396 0.000425 -0.000495 -0.000400 0.001695 ... -0.001178 -0.007857 -0.001802 -0.002089 -0.000649 -0.001059 -0.002213 -0.003938 -0.000919 0.000182 0.004406 0.001884 0.001422 -0.005280 0.006863 -0.000516 0.009694
```

## get_specific_return - 获取一组股票的特异收益率

```python
get_specific_return(order_book_ids, start_date, end_date, model = 'v1',industry_mapping='sws_2021')
```

### 参数

| 参数             | 类型         | 解释                                                         |
| :--------------- | :----------- | :----------------------------------------------------------- |
| order_book_ids   | str or list  | 证券代码（例如：'600705.XSHG'）                             |
| start_date       | str          | 开始日期（例如：'2017-03-03'）                               |
| end_date         | str          | 结束日期（例如：'2017-03-20'）                               |
| model            | str          | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |
| industry_mapping | str          | 'sws_2021' - 申万2021 行业分类 'citics_2019' - 中信2019 行业分类默认：industry_mapping = 'sws_2021'。 |

### 返回

pandas.DataFrame

index 为日期， column 为个股的order_book_id

### 范例

**获取一只股票的特异收益率**

```python
In [16]: get_specific_return('600705.XSHG','20170302','20170307' )
Out[16]: order_book_id
date
2017-03-02 -0.012205
2017-03-03 0.004288
2017-03-06 -0.005601
2017-03-07 0.022702
```

**获取一组股票的特异收益率**

```python
In [29]: get_specific_return(['600705.XSHG','600100.XSHG'],'20170302','20170307' )
Out[29]: order_book_id
date
2017-03-02 0.000004 -0.012205
2017-03-03 -0.004658 0.004288
2017-03-06 -0.003140 -0.005601
2017-03-07 0.013050 0.022702
```

**获取一个股票中信2019 分类下的特异收益率**

```python
In [29]: rqdatac.get_specific_return('600705.XSHG','20170302','20170307',industry_mapping='citics_2019' )
Out[29]: order_book_id
date
2017-03-02 -0.015470
2017-03-03 0.005021
2017-03-06 -0.003329
2017-03-07 0.022044
```

## get_factor_covariance - 获取因子协方差

```python
get_factor_covariance(date, horizon= 'daily', model = 'v1',industry_mapping='sws_2021')
```

### 参数

| 参数             | 类型 | 解释                                                         |
| :--------------- | :--- | :----------------------------------------------------------- |
| date             | str  | 开始日期（例如：'2017-03-03'）                               |
| horizon          | str  | 预测期限。默认为日度（'daily'），可选月度（'monthly'）或季度（'quarterly'） |
| model            | str  | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |
| industry_mapping | str  | 'sws_2021' - 申万2021 行业分类 'citics_2019' - 中信2019 行业分类默认：industry_mapping = 'sws_2021'。 |

### 返回

pandas.DataFrame

index 和column 均为因子名称

### 范例

```python
In [17]: get_factor_covariance('20170303',horizon='daily')
Out[17]: beta book_to_price comovement earnings_yield growth ... 采掘钢铁银行非银金融食品饮料beta 0.001163 -2.385022e-04 0.002044 -6.211431e-05 -0.000042 ... -0.000155 -0.000455 -0.000377 0.000040 -0.000311
book_to_price -0.000239 2.517092e-03 -0.000964 -1.118821e-04 0.000054 ... 0.000697 0.001682 0.000427 0.000162 0.000362
comovement 0.002044 -9.643928e-04 0.016008 -1.260676e-04 -0.000253 ... 0.000226 0.000192 -0.004963 0.001152 -0.001320
earnings_yield -0.000062 -1.118821e-04 -0.000126 5.460846e-04 -0.000049 ... -0.000208 -0.000219 0.000070 0.000150 0.000187
growth -0.000042 5.362409e-05 -0.000253 -4.887124e-05 0.000171 ... -0.000020 -0.000155 0.000185 -0.000051 0.000077
leverage -0.000050 2.557107e-04 -0.000145 -1.153423e-05 -0.000002 ... 0.000203 0.000691 -0.000176 0.000078 0.000115
liquidity 0.000118 -5.388243e-07 0.001384 -5.870778e-05 0.000011 ... 0.000140 0.000314 -0.000606 -0.000068 0.000047
momentum -0.000260 1.096554e-04 -0.000522 -1.625567e-05 0.000022 ... -0.000072 -0.000302 0.000247 -0.000053 0.000041
non_linear_size 0.000087 -1.806789e-04 0.000144 -3.357935e-05 -0.000007 ... -0.000071 0.000102 -0.000159 -0.000084 -0.000039
residual_volatility 0.000632 -3.166259e-04 0.001609 -8.017871e-05 -0.000019 ... -0.000173 -0.000612 -0.000197 -0.000120 -0.000466
size -0.000226 3.508242e-04 -0.001208 3.006616e-04 0.000034 ... 0.000252 0.000720 0.000300 0.000206 0.000330
交通运输0.000014 2.175025e-04 0.000782 -2.044910e-04 0.000015 ... -0.000289 0.001544 -0.002213 -0.001391 0.000661
休闲服务-0.000056 7.833654e-05 -0.001222 5.397482e-05 0.000014 ... -0.000589 0.000032 -0.001537 -0.000974 0.001098
··· 银行-0.000377 4.270598e-04 -0.004963 6.968579e-05 0.000185 ... -0.000726 -0.005122 0.010001 0.000824 -0.001567
非银金融0.000040 1.620042e-04 0.001152 1.504377e-04 -0.000051 ... -0.000602 -0.001897 0.000824 0.006821 -0.001082
食品饮料-0.000311 3.616767e-04 -0.001320 1.873396e-04 0.000077 ... -0.000083 0.001159 -0.001567 -0.001082 0.005584
```

```python
In [17]: rqdatac.get_factor_covariance('20170303',horizon='daily',industry_mapping='citics_2019')
Out[17]: beta book_to_price comovement earnings_yield growth leverage liquidity momentum non_linear_size residual_volatility ... 纺织服装综合综合金融计算机轻工制造通信钢铁银行非银行金融食品饮料beta 0.001399 -0.000007 0.004359 0.000088 -6.770298e-05 -0.000095 -6.687593e-05 -4.446151e-04 1.121304e-05 0.000915 ... -0.000035 -0.000162 -0.000015 0.000496 0.000119 0.000188 -0.000318 -0.000891 0.000075 0.000007
book_to_price -0.000007 0.001188 0.000354 0.000074 -5.944671e-05 0.000229 7.424089e-05 3.840258e-05 -1.089268e-04 -0.000048 ... -0.000080 0.000009 0.000479 -0.000683 -0.000270 -0.000372 0.001352 -0.000476 0.000317 0.000462
comovement 0.004359 0.000354 0.016260 0.000564 -2.733087e-04 -0.000328 -1.883315e-04 -1.597542e-03 -1.618321e-05 0.003254 ... -0.000432 -0.001010 -0.000307 0.000750 0.000249 0.000031 -0.001076 -0.002098 0.000910 0.000374
earnings_yield 0.000088 0.000074 0.000564 0.000341 -1.657873e-05 -0.000039 -3.718981e-05 -2.519291e-05 -2.958569e-05 0.000008 ... -0.000067 -0.000126 -0.000046 -0.000028 0.000020 -0.000026 -0.000140 -0.000226 0.000198 0.000475
growth -0.000068 -0.000059 -0.000273 -0.000017 1.651912e-04 -0.000053 -1.308102e-07 3.171995e-05 1.839054e-05 -0.000027 ... -0.000046 -0.000021 -0.000031 0.000156 -0.000065 0.000111 -0.000296 0.000303 -0.000100 -0.000135
leverage -0.000095 0.000229 -0.000328 -0.000039 -5.293993e-05 0.000275 5.422277e-05 2.748743e-05 -2.591696e-05 -0.000135 ... 0.000103 0.000075 0.000117 -0.000439 -0.000009 -0.000245 0.000739 -0.000294 0.000129 0.000184
liquidity -0.000067 0.000074 -0.000188 -0.000037 -1.308102e-07 0.000054 6.349408e-04 -4.340426e-05 -7.344311e-06 -0.000027 ... -0.000097 -0.000064 -0.000191 -0.000284 -0.000115 -0.000235 0.000048 -0.000024 -0.000099 0.000152
momentum -0.000445 0.000038 -0.001598 -0.000025 3.171995e-05 0.000027 -4.340426e-05 4.285512e-04 4.915310e-07 -0.000385 ... -0.000101 0.000003 -0.000009 -0.000101 -0.000080 -0.000068 -0.000223 0.000514 -0.000009 -0.000104
non_linear_size 0.000011 -0.000109 -0.000016 -0.000030 1.839054e-05 -0.000026 -7.344311e-06 4.915310e-07 1.000601e-04 0.000031 ... 0.000037 0.000022 -0.000059 0.000138 0.000052 0.000045 0.000066 -0.000041 -0.000093 -0.000071
residual_volatility 0.000915 -0.000048 0.003254 0.000008 -2.668991e-05 -0.000135 -2.728752e-05 -3.853061e-04 3.093620e-05 0.001066 ... -0.000113 -0.000150 0.000166 0.000526 0.000054 0.000263 -0.000454 -0.000448 -0.000161 -0.000217
size -0.000358 0.000295 -0.000832 0.000153 -1.484045e-05 0.000090 3.278297e-04 8.068121e-05 -5.206873e-05 -0.000388 ... -0.000076 -0.000223 -0.000136 -0.000574 -0.000193 -0.000463 0.000606 -0.000018 0.000377 0.000378
交通运输-0.000102 0.000150 -0.000332 -0.000145 -1.001581e-04 0.000303 3.600846e-004 -1.730589e-04 -6.712213e-05 -0.000025 ... 0.000545 0.000771 0.000963 -0.000309 0.000773 0.000176 0.001407 -0.002519 -0.001657 0.000813
传媒0.000158 -0.000449 -0.000262 -0.000062 1.034563e-04 -0.000243 -2.328359e-04 3.048113e-05 1.974499e-05 0.000205 ... 0.000318 0.000759 0.000956 0.002107 0.000762 0.001588 -0.001282 -0.001594 -0.001332 -0.000295
农林牧渔-0.000076 0.000258 -0.000249 0.000116 -1.219678e-04 0.000257 1.056987e-05 -9.805772e-05 1.223279e-05 -0.000130 ... 0.000612 0.000502 0.000347 -0.000671 0.000589 -0.000249 0.001948 -0.002567 -0.000891 0.001917
医药0.000372 -0.000185 0.000947 0.000234 8.351098e-06 -0.000152 -1.239801e-04 -1.238810e-04 1.905877e-05 0.000207 ... 0.000468 0.000296 0.000403 0.000851 0.000640 0.000573 -0.000599 -0.002098 -0.001150 0.001156
商贸零售0.000142 -0.000080 0.000310 -0.000004 -1.377363e-04 0.000045 1.288081e-04 -2.074744e-04 1.592768e-05 0.000146 ... 0.001178 0.001221 0.001280 -0.000059 0.001023 0.000149 0.000564 -0.003222 -0.000770 0.001014
国防军工0.000776 0.000140 0.001911 -0.000085 -1.402052e-05 0.000291 6.471573e-05 -3.466389e-04 -5.263616e-05 0.000593 ... 0.000030 -0.000172 0.000424 0.000298 -0.000199 0.001194 0.000685 -0.003790 -0.001595 0.000298
基础化工0.000216 -0.000329 0.000400 -0.000026 -8.059124e-05 -0.000083 1.655167e-05 -1.808071e-04 8.051281e-05 0.000195 ... 0.000746 0.000668 0.000151 0.000264 0.000666 0.000487 0.000896 -0.002623 -0.001467 0.000510
家电0.000075 -0.000127 0.000135 0.000208 -7.764720e-05 -0.000063 9.480138e-05 -2.370653e-04 -1.065073e-05 -0.000067 ... 0.000686 0.000414 0.000285 0.000088 0.000720 0.000468 0.000529 -0.002337 -0.000867 0.001731
建材0.000034 0.000537 -0.000116 -0.000109 -3.763579e-04 0.000442 3.322789e-04 -1.453769e-04 8.369679e-05 -0.000035 ... 0.001229 0.001458 0.000911 -0.001123 0.001111 -0.000456 0.007036 -0.005746 -0.002363 0.001917
建筑-0.000136 0.000651 -0.000116 0.000018 -1.237513e-04 0.000283 4.542388e-05 3.644480e-05 -9.319595e-06 -0.000142 ... 0.000215 0.000434 0.000528 -0.000403 0.000449 0.000140 0.001039 -0.002048 -0.000734 0.000155
房地产-0.000056 0.000024 -0.000311 -0.000004 -2.817610e-05 0.000035 9.880655e-05 -6.599190e-05 -1.578748e-05 -0.000114 ... 0.000375 0.000825 0.000483 -0.000321 0.000235 -0.000123 -0.000119 -0.000917 -0.000233 0.000211
有色金属-0.000212 0.000909 -0.000932 -0.000127 -2.694873e-04 0.000558 -7.352625e-05 -6.875829e-09 -4.801330e-05 -0.000262 ... 0.000547 0.000297 -0.000338 -0.002009 0.000105 -0.001164 0.008366 -0.003554 -0.001110 0.000531
机械0.000194 -0.000093 0.000479 0.000009 -3.434340e-05 -0.000013 -1.440233e-05 -5.398043e-05 4.997692e-05 0.000152 ... 0.000234 0.000164 0.000145 0.000310 0.000310 0.000355 0.000218 -0.001445 -0.000792 0.000055
汽车0.000170 -0.000315 0.000091 -0.000006 1.631817e-05 -0.000052 1.313062e-04 -1.220259e-04 6.749136e-05 0.000096 ... 0.000630 0.000464 0.000372 0.000182 0.000592 0.000365 0.000525 -0.002128 -0.000983 0.000562
消费者服务0.000033 -0.000123 -0.000566 0.000149 2.958023e-05 0.000047 -8.186208e-05 4.652137e-05 2.568024e-05 -0.000114 ... 0.000511 0.000650 0.000981 0.000680 0.000591 0.000420 0.000273 -0.002048 -0.000781 0.000910
煤炭-0.000285 0.000848 -0.000266 0.000053 -6.842190e-05 0.000318 2.361473e-04 -2.647335e-04 -1.578106e-04 -0.000505 ... 0.000468 -0.000059 -0.000921 -0.002150 -0.000406 -0.001354 0.008761 -0.001411 -0.000050 0.000557
电力及公用事业0.000137 0.000090 0.000265 -0.000089 -4.733007e-05 0.000038 1.291258e-04 -1.126767e-04 3.723079e-06 0.000094 ... 0.000308 0.000460 0.000164 -0.000190 0.000417 0.000014 0.000924 -0.001747 -0.000978 0.000068
电力设备及新能源0.000344 -0.000208 0.000780 -0.000020 -1.284099e-05 -0.000068 7.599085e-05 -8.151875e-05 7.738806e-05 0.000322 ... 0.000242 0.000256 -0.000006 0.000545 0.000403 0.000628 0.000077 -0.001995 -0.001165 0.000147
电子0.000365 -0.000586 0.000656 -0.000003 7.686605e-05 -0.000337 -1.584191e-04 -8.880432e-05 1.345832e-04 0.000408 ... 0.000205 0.000454 0.000135 0.001516 0.000542 0.001436 -0.000536 -0.001820 -0.001685 -0.000385
石油石化-0.000297 0.000679 -0.000449 -0.000238 -3.903648e-05 0.000244 2.784371e-04 2.575526e-04 -3.535490e-05 0.000007 ... -0.000284 -0.000166 -0.000537 -0.001216 -0.000686 -0.000740 0.000705 -0.000166 -0.000565 -0.000205
纺织服装-0.000035 -0.000080 -0.000432 -0.000067 -4.638686e-05 0.000103 -9.714661e-05 -1.014918e-04 3.687117e-05 -0.000113 ... 0.002112 0.000753 0.000433 -0.000162 0.000548 0.000252 0.001265 -0.001965 -0.000730 0.000644
综合-0.000162 0.000009 -0.001010 -0.000126 -2.097626e-05 0.000075 -6.421795e-05 3.071627e-06 2.201877e-05 -0.000150 ... 0.000753 0.003068 0.000865 0.000225 0.000735 0.000472 0.001226 -0.002276 -0.001080 0.000329
综合金融-0.000015 0.000479 -0.000307 -0.000046 -3.131022e-05 0.000117 -1.907693e-04 -9.316430e-06 -5.916113e-05 0.000166 ... 0.000433 0.000865 0.012777 0.000259 0.000042 0.000386 0.000531 -0.001891 -0.000631 0.000231
计算机0.000496 -0.000683 0.000750 -0.000028 1.562095e-04 -0.000439 -2.839100e-04 -1.010311e-04 1.378490e-04 0.000526 ... -0.000162 0.000225 0.000259 0.004219 0.000371 0.002199 -0.001691 -0.000930 -0.001175 -0.000786
轻工制造0.000119 -0.000270 0.000249 0.000020 -6.506015e-05 -0.000009 -1.151912e-04 -8.041214e-05 5.191646e-05 0.000054 ... 0.000548 0.000735 0.000042 0.000371 0.002153 0.000392 0.000598 -0.001971 -0.000976 0.000565
通信0.000188 -0.000372 0.000031 -0.000026 1.113834e-04 -0.000245 -2.348988e-04 -6.803749e-05 4.457633e-05 0.000263 ... 0.000252 0.000472 0.000386 0.002199 0.000392 0.003877 -0.000836 -0.001544 -0.001467 -0.000430
钢铁-0.000318 0.001352 -0.001076 -0.000140 -2.963463e-04 0.000739 4.826072e-05 -2.230691e-04 6.597295e-05 -0.000454 ... 0.001265 0.001226 0.000531 -0.001691 0.000598 -0.000836 0.019536 -0.005261 -0.001310 0.001378
银行-0.000891 -0.000476 -0.002098 -0.000226 3.028849e-04 -0.000294 -2.352887e-05 5.137291e-04 -4.090413e-05 -0.000448 ... -0.001965 -0.002276 -0.001891 -0.000930 -0.001971 -0.001544 -0.005261 0.013030 0.001157 -0.003146
非银行金融0.000075 0.000317 0.000910 0.000198 -1.004018e-04 0.000129 -9.926253e-05 -8.829111e-06 -9.349944e-05 -0.000161 ... -0.000730 -0.001080 -0.000631 -0.001175 -0.000976 -0.001467 -0.001310 0.001157 0.006073 -0.000987
食品饮料0.000007 0.000462 0.000374 0.000475 -1.351070e-04 0.000184 1.522055e-04 -1.041277e-04 -7.117080e-05 -0.000217 ... 0.000644 0.000329 0.000231 -0.000786 0.000565 -0.000430 0.001378 -0.003146 -0.000987 0.007102
```

## get_specific_risk - 获取一组股票的特异波动率

```python
get_specific_risk(order_book_ids, start_date, end_date, horizon= 'daily', model = 'v1',industry_mapping='sws_2021')
```

### 参数

| 参数             | 类型        | 解释                                                         |
| :--------------- | :---------- | :----------------------------------------------------------- |
| order_book_ids   | str or list | 证券代码（例如：'600705.XSHG'）                             |
| start_date       | str         | 开始日期（例如：'2017-03-03'）                               |
| end_date         | str         | 结束日期（例如：'2017-03-20'）                               |
| horizon          | str         | 预测期限。默认为日度（'daily'），可选月度（'monthly'）或季度（'quarterly'） |
| model            | str         | 'v1' 米筐老风险模型 'v2' 米筐新风险模型，新风险模型        |
| industry_mapping | str         | 'sws_2021' - 申万2021 行业分类 'citics_2019' - 中信2019 行业分类默认：industry_mapping = 'sws_2021'。 |

### 返回

pandas.DataFrame

index 为日期， column 为个股的order_book_id

### 范例

**获取一只股票的特异波动率**

```python
In [18]: get_specific_risk('600705.XSHG','20170303','20170308',horizon='daily')
Out[18]: order_book_id
date
2017-03-03 0.191777
2017-03-06 0.187424
2017-03-07 0.189880
2017-03-08 0.186224
```

**获取一组股票的特异波动率**

```python
In [31]: get_specific_risk(['600705.XSHG','600100.XSHG'],'20170303','20170308',horizon='daily')
Out[31]: order_book_id
date
2017-03-03 0.146083 0.191777
2017-03-06 0.142691 0.187424
2017-03-07 0.142536 0.189880
2017-03-08 0.139857 0.186224
```

**获取一只股票中信2019 的特异波动率**

```python
In [18]: rqdatac.get_specific_risk('600705.XSHG','20170303','20170308',horizon='daily',industry_mapping='citics_2019')
Out[18]: order_book_id
date
2017-03-03 0.192311
2017-03-06 0.187687
2017-03-07 0.190031
2017-03-08 0.186779
```

## 字段说明

表1 和表2 给出了上述部分数据API 中风格因子、行业因子的字段说明。表3 给出了市场联动因子的解释。

### 表1. 风格因子字段说明

备注： 对于包含细分因子的风格因子，风格因子字段加粗标记，细分因子字段星号标记

| 字段                | 说明   | 解释                                                         |
| :------------------ | :----- | :----------------------------------------------------------- |
| **beta**            | 贝塔   | 个股/投资组合收益对基准收益的敏感度                          |
| **momentum**        | 动量   | 股票收益变化的总体趋势特征                                   |
| **size**            | 规模   | 上市公司的市值规模                                           |
| **book_to_price**   | 账面市值比 | 上市公司的股东权益-市值比，反映其估值水平                    |
| **non_linear_size** | 非线性市值 | 反映中等市值股票和大/小市值股票的表现差异                    |
| **earnings_yield**  | 盈利率 | 上市公司的营收能力                                           |
| * trailing_earnings_to_price_ratio |        |                                                              |
| * cash_earnings_to_price_ratio |        |                                                              |
| * predicted_earning_to_price |        |                                                              |
| **residual_volatility** | 残余波动率 | 个股残余收益的波动程度                                       |
| * daily_standard_deviation |        |                                                              |
| * cumulative_range |        |                                                              |
| * historical_sigma |        |                                                              |
| **growth**          | 成长性 | 上市公司的营收增长情况                                       |
| * sales_growth     |        |                                                              |
| * earnings_growth  |        |                                                              |
| * short_term_predicted_earnings_growth |        |                                                              |
| * long_term_predicted_earnings_growth |        |                                                              |
| **leverage**        | 杠杆率 | 上市企业负债占资产比例，反映企业的经营杠杆率                 |
| * market_leverage  |        |                                                              |
| * debt_to_assets   |        |                                                              |
| * book_leverage    |        |                                                              |
| **liquidity**       | 流动性 | 股票换手率，反映个股交易的活跃程度                           |
| * one_month_share_turnover |        |                                                              |
| * three_month_share_turnover |        |                                                              |
| * twelve_month_share_turnover |        |                                                              |

### 表2. 行业因子字段说明

*备注：下表为2021 年后的申万行业分类名称*

| 序号 | 行业     | 序号 | 行业     |
| :--- | :------- | :--- | :------- |
| 1    | 农林牧渔 | 17   | 商贸零售 |
| 2    | 煤炭     | 18   | 社会服务 |
| 3    | 基础化工 | 19   | 综合     |
| 4    | 钢铁     | 20   | 建筑材料 |
| 5    | 有色金属 | 21   | 建筑装饰 |
| 6    | 电子     | 22   | 电力设备 |
| 7    | 家用电器 | 23   | 国防军工 |
| 8    | 食品饮料 | 24   | 计算机   |
| 9    | 纺织服饰 | 25   | 传媒     |
| 10   | 轻工制造 | 26   | 通信     |
| 11   | 医药生物 | 27   | 银行     |
| 12   | 公用事业 | 28   | 非银金融 |
| 13   | 交通运输 | 29   | 汽车     |
| 14   | 房地产   | 30   | 机械设备 |
| 15   | 石油石化 | 31   | 环保     |
| 16   | 美容护理 |      |          |

### 表3. 市场联动因子解释

| 市场联动因子 | 解释                                                         |
| :----------- | :----------------------------------------------------------- |
| comovement   | 反映市场整体涨落对个股或投资组合的影响。个股对市场联动因子的暴露度均为1，因此对于任意两个满仓的股票组合，其获得的市场收益及承担的市场整体波动风险相同。而对于“股票+现金”的组合，随着组合中股票仓位比例的增加，市场联动因子对组合收益影响越大；例如：组合的股票平均仓位是70%，那么该组合对市场联动因子的暴露度为0.7。 |

*如需更详细的米筐多因子风险模型白皮书，请联系我们的销售或者致电公司官方电话*

## 新风险模型（长期风险模型）介绍

### 与上一代模型的区别

*   新增6 个风格因子，共计16 个风格因子，覆盖更广范围的风险。
*   PIT 处理的基本面数据，和更快的基本面更新频率
*   使用最新的行业分类

### 与同类型模型的区别

*   基于申万行业分类定义行业因子
*   提供细分因子暴露度、个股对各指数贝塔等衍生因子数据，灵活应用于不同投研场景；
*   提供以常用指数（沪深300、中证500 及中证800）成分股为股票池的因子收益率，便于挑选在特定指数有效的因子，构建指数增强策略
*   特异风险模型中，对新股、复牌股、停牌股进行了优化处理，有效提升了风险预测精度和权重优化效果

### 新定义的因子

*   **Earnings variability**：
    *   因子逻辑：利润，收入，现金流的波动性
    *   因子方向：正暴露
    *   因子构成：营收波动（Variability in Sales），盈利波动（Variability in Earnings），现金流波动（Variation in Cashflows），分析师预期EP 波动（Variation in FW EPS）
*   **Dividend yield**：
    *   因子逻辑：股息率
    *   因子方向：正暴露
    *   具有较高的股息率
    *   因子构成：股息率（Dividend-to-price ratio）
*   **Earnings quality**：
    *   因子逻辑：通过资产负债表应计和现金流量表应计去衡量公司的盈利质量
    *   因子方向：正暴露
    *   标明应计部分较少
    *   因子构成：资产负债表应计（Accruals Balance sheet version），现金流量表应计（Accruals Cashflow version）
*   **Long term reversal**
    *   因子逻辑：长期股价趋势和长期的alpha
    *   因子方向：正暴露
    *   标明较差的长期收益
    *   因子构成：长期相对强度（Long-term relative strength），长期历史alpha（Long-term historical alpha）
*   **Investment quality**
    *   因子逻辑：衡量管理层和资本扩张的程度
    *   因子方向：正暴露
    *   标明较低的扩张速度
    *   因子构成：总资产增长率（asset growth），流通股本增长率（issuance growth），资本支出增长率（capital expenditure growth）
*   **Profitability**：
    *   因子逻辑：公司的运营效率，盈利能力的衡量
    *   因子方向：正暴露
    *   标明较高的盈利能力和经营效率
    *   因子构成：运营效率（Asset turnover），总盈利能力（Gross profitability），毛利率（Gross Margin），资产回报ROA（Return on asset）

### 原已有因子的定义改动

*   **Earnings yield**：增加了一个EBIT/EV 的细分因子
*   **Liquidity**：增加日频换手的细分因子
*   **Momentum**：增加了历史alpha 细分因子
*   **Growth**：剔除了短期预测的因子，只是用长期预测因子
*   **Beta**：改变了计算时的指数加权的半衰期

### 新风格因子结构：

| 风格因子          | 细分因子                           | 说明               |
| :---------------- | :--------------------------------- | :----------------- |
| Liquidity         | STOM                               | Monthly share turnover 月换手率 |
|                   | STOQ                               | Quarterly share turnover 季换手率 |
|                   | STOA                               | Annual share turnover 年换手率 |
|                   | ATVR                               | Annualized traded value ratio 年化交易量比率 |
| Leverage          | MLEV                               | Market Leverage 市场杠杆 |
|                   | BLEV                               | Book Leverage 账面杠杆 |
|                   | DTOA                               | Debt to asset ratio 资产负债比 |
| Earning Variablity | VSAL                               | Variation in Sales 营业收入波动率 |
|                   | VERN                               | Variation in Earings 盈利波动率 |
|                   | VFLO                               | Variation in Cash Flows 现金流波动率 |
|                   | SPIBS                              | Variation in FW EPS 分析师预测盈市率标准差 |
| Earnings Quality  | ACBS                               | Accruals Balancesheet version 资产负债表应计项目 |
|                   | ACCF                               | Accruals Cashflow version 现金流量表应计项目 |
| Profitability     | ATO                                | Asset turnover 资产周转率 |
|                   | GP                                 | Gross profitability 资产毛利率 |
|                   | GM                                 | Gross Margin 销售毛利率 |
|                   | ROA                                | Return on asset 总资产收益率 |
| Investment Quality | AGRO                               | asset growth 资产增长率 |
|                   | IGRO                               | issuance growth 股票发行量增长率 |
|                   | CXGRO                              | capital expenditure growth 资本支出增长率 |
| BTOP              | BTOP                               | book to price 账面市值比 |
| Earnings Yield    | ETOP                               | Trailing Earnings-to-Price ratio EP比 |
|                   | EPIBS                              | Analyst predicted earnings to price 分析师预测EP比 |
|                   | CETOP                              | cash earnings to price 现金盈利价格比 |
|                   | ENMU                               | Enterprise multiple（Ebitda to Ev） 企业价值倍数的倒数 |
| Long Term reversal | LTRSTR                             | Longterm relative strength 长期相对强度 |
|                   | LTHALPHA                           | Longterm historical alpha 长期历史Alpha |
| Growth            | EGRLF                              | Predicted growth 3 year 分析师预测长期盈利增长率 |
|                   | EGRO                               | Historical earnings per share growth rate 每股收益增长率 |
|                   | SGRO                               | Historical sales per share growth rate 每股营业收入增长率 |
| Momentum          | RSTR                               | Relative strength 相对于市场的强度 |
|                   | HALPHA                             | Historical alpha 历史Alpha |
| Mid cap           | MIDCAP                             | mid cap 中市值     |
| Size              | LSIZE                              | size 规模          |
| Beta              | BETA                               | beta 贝塔          |
| Resival Volatility | HSIGMA                             | Hist sigma 历史sigma |
|                   | DASTD                              | daily std dec 日标准差 |
|                   | CMRA                               | Cumulative range 累计收益范围 |
| Dividend Yield    | DTOP                               | Dividend-to-price ratio 股息率 |
|                   | DPIBS                              | Analyst predicted dividend to price ratio 分析师预测分红价格比 |

# 现货市场数据

## 可获取的现货数据

可获取上海黄金现货交易所上市的现货，包含黄金、铂金、白银等的基础信息及日行情、分钟行情、tick 行情数据。

### 合约列表

使用 `all_instruments` 调取。参数及字段详情请参考API-all_instruments。

```python
all_instruments('Spot')
```

**输出示例:**

```
de_listed_date exchange listed_date market_tplus order_book_id symbol trading_hours type
0 0000-00-00 SGEX 2003-07-30 0 PT9995.SGEX 铂金现货(Pt99.95合约) 09:31-11:30,13:01-15:00 Spot
1 0000-00-00 SGEX 2014-09-19 0 IAU100G.SGEX 国际黄金现货(iAu100g合约) 09:31-11:30,13:01-15:00 Spot
2 0000-00-00 SGEX 2006-10-30 0 AU9995.SGEX 黄金现货(Au99.95合约) 09:31-11:30,13:01-15:00 Spot
3 0000-00-00 SGEX 2002-10-30 0 AU9999.SGEX 黄金现货(Au99.99合约) 09:31-11:30,13:01-15:00 Spot
...
```

### 基础合约信息

使用 `instruments` 调取。参数及字段详情请参考API-all_instruments。

```python
instruments('AUTD.SGEX')
```

**输出示例:**

```
Instrument(order_book_id='AUTD.SGEX', symbol='黄金现货(Au(T+D)合约)', exchange='SGEX', listed_date='2004-08-16', type='Spot', de_listed_date='0000-00-00', trading_hours='09:31-11:30,13:01-15:00', market_tplus=0)
```

### 日行情数据

使用 `get_price` 调取。参数及字段详情请参考API-get_price。

```python
get_price('AUTD.SGEX',20200301,20200317)
```

**输出示例:**

```
           open_interest    open   volume      low  close  total_turnover    high
2020-03-02     259220.0  365.30  189052.0  352.34  359.00    6.804021e+10  366.07
2020-03-03     256530.0  359.70   81836.0  356.17  358.23    2.927269e+10  359.98
...                 ...     ...       ...     ...     ...             ...     ...
2020-03-16     266458.0  354.89  320914.0  340.03  344.66    1.117367e+11  358.15
2020-03-17     277112.0  330.77  244946.0  330.77  330.78    8.159528e+10  339.50
```

### 分钟行情数据

可使用 `get_price` 调取历史和实时分钟数据。参数和字段详情请参考API-get_price。

```python
get_price('AUTD.SGEX',20200810,20200810,'1m')
```

**输出示例:**

```
                      close    high     low  open_interest    open  volume trading_date  total_turnover
datetime
2020-08-07 20:01:00  443.55  444.86  443.12       290956.0  444.60   998.0   2020-08-10    4.430369e+08
2020-08-07 20:02:00  444.60  444.60  443.30       290876.0  443.60  1108.0   2020-08-10    4.917667e+08
2020-08-07 20:03:00  445.18  445.18  444.50       290974.0  444.60   822.0   2020-08-10    3.656133e+08
...                                 ...            ...     ...     ...        ...             ...
2020-08-10 15:28:00  441.91  441.91  441.82       300704.0  441.88   316.0   2020-08-10    1.396362e+08
2020-08-10 15:29:00  441.92  441.92  441.82       300718.0  441.90   156.0   2020-08-10    6.893474e+07
2020-08-10 15:30:00  442.00  442.00  441.90       289656.0  441.92   278.0   2020-08-10    1.228589e+08
```

使用 `current_minute` 获取AUTD.SGEX 最近的分钟线数据。参数和字段请参考API-current_minute。

```python
current_minute('AUTD.SGEX')
```

**输出示例:**

```
                           close    high     low    open  open_interest  total_turnover  trading_date  volume
order_book_id  datetime
AUTD.SGEX      2020-08-26 15:30:00  412.38  412.39  412.3  412.39       251432.0      47830960.0      20200826     116
```

### tick 行情数据

可使用 `get_price` 调取历史和实时tick 数据。参数和字段详情请参考API-get_price。

```python
get_price('AUTD.SGEX',20200810,20200810,'tick')
```

**输出示例:**

```
                                  trading_date   open   last    high     low  prev_settlement  prev_close    volume  open_interest  total_turnover  ...  a2_v  a3_v  a4_v  a5_v  b1_v  b2_v  b3_v  b4_v  b5_v  change_rate
datetime
2020-08-07 19:33:16.446 2020-08-10    0.0  445.72   0.00   0.0           445.94       445.72       0.0       290930.0    0.000000e+00  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.000493
2020-08-07 19:45:05.514 2020-08-10    0.0  445.72   0.00   0.0           445.94       445.72       0.0       290930.0    0.000000e+00  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.000493
2020-08-07 19:45:16.267 2020-08-10    0.0  445.72   0.00   0.0           445.94       445.72       0.0       290930.0    0.000000e+00  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.000493
...                                     ...    ...     ...      ...        ...            ...        ...         ...            ...           ...  ...   ...   ...   ...   ...   ...   ...   ...   ...          ...
2020-08-10 15:44:23.717 2020-08-10  444.6  442.00  447.09  438.9           445.94       445.72   171746.0       289656.0    7.603756e+10  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.008835
2020-08-10 15:44:33.717 2020-08-10  444.6  442.00  447.09  438.9           445.94       445.72   171746.0       289656.0    7.603756e+10  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.008835
2020-08-10 15:44:43.717 2020-08-10  444.6  442.00  447.09  438.9           445.94       445.72   171746.0       289656.0    7.603756e+10  ...   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0   0.0    -0.008835
```

使用 `current_snapshot` 获取AUTD.SGEX 快照数据。参数和字段请参考API-current_snapshot。

```python
current_snapshot('AUTD.SGEX')
```

**输出示例:**

```
Tick(ask_vols: [3, 6, 3, 1, 1], asks: [412.38, 412.4, 412.45, 412.48, 412.49], bid_vols: [56, 2, 3, 1, 1], bids: [412.3, 412.25, 412.2, 412.17, 412.1], datetime: 2020-08-26 15:45:08.031000, high: 414.1, iopv: nan, last: 412.38, low: 411.16, open: 412.8, open_interest: 245154.0, order_book_id: AUTD.SGEX, prev_close: 413.8, prev_iopv: nan, prev_settlement: 414.68, total_turnover: 21982639060.0, trading_phase_code: None, volume: 53250)
```

## `get_spot_benchmark_price` - 获取现货早午盘价

获取现货一段时间的早午盘价格。

```python
get_price_change_rate(id_or_symbols, start_date='20130104', end_date='20140104', expect_df=True)
```

### 参数

| 参数             | 类型                                     | 说明                                        |
| :--------------- | :--------------------------------------- | :------------------------------------------ |
| `id_or_symbols`  | `str` or `str list`                      | 可输入'AU9999.SGEX'或者'AG9999.SGEX'         |
| `start_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为近3 个月                     |
| `end_date`       | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为近3 个月                     |

### 返回

`pandas DataFrame`

| 返回            | 类型           | 说明           |
| :-------------- | :------------- | :------------- |
| `order_book_ids` | `str`          | 合约代码       |
| `date`          | `datetime.date` | 日期           |
| `morning`       | `float`        | 早盘价度       |
| `noon`          | `float`        | 午盘价度       |

### 范例

获取黄金一段时间的早午盘价格。

```python
rqdatac.get_spot_benchmark_price('AU9999.SGEX',20230501,20230508)
```

**输出示例:**

```
         morning   noon order_book_id       date
AU9999.SGEX   2023-05-04  453.77  453.24
AU9999.SGEX   2023-05-05  455.84  455.57
```



# 货币市场数据

## 国债回购

可获取深交所、上交所上市的各个期限的国债回购基础数据及行情数据，具体调用方式请参考：

### 合约列表

请使用 `all_instruments` 调取，参数及字段详情请参考API-all_instruments。

```python
In: all_instruments('Repo')
Out:
  abbrev_symbol de_listed_date exchange listed_date ... status  \
0         R-273     2012-12-10     XSHE  2012-12-10 ... Delisted
1         GC001     0000-00-00     XSHG  2006-05-08 ...   Active
2         GC002     0000-00-00     XSHG  2006-05-08 ...   Active
3         GC003     0000-00-00     XSHG  2006-05-08 ...   Active
4         GC004     0000-00-00     XSHG  2006-05-08 ...   Active
...
                                      symbol             trading_hours  type
0             深市国债273天回购  09:31-11:30,13:01-15:30  Repo
1                1天国债回购  09:31-11:30,13:01-15:30  Repo
2                2天国债回购  09:31-11:30,13:01-15:30  Repo
3                3天国债回购  09:31-11:30,13:01-15:30  Repo
4                4天国债回购  09:31-11:30,13:01-15:30  Repo
```

### 基础合约

请使用 `instruments` 调取，参数及字段详情请参考API-instruments。

```python
In[]:instruments('131810.XSHE')
Out[]:Instrument(order_book_id='131810.XSHE', symbol='深市国债1天回购', abbrev_symbol='R-001', exchange='XSHE', status='Active', listed_date='2012-12-10', de_listed_date='0000-00-00', round_lot=1000, type='Repo', trading_hours='09:31-11:30,13:01-15:30')
```

### 历史日行情、分钟行情、tick 行情数据

请使用 `get_price` 调取，参数及字段详情请参考API-get_price。

```python
In: get_price('131810.XSHE')
Out:
              num_trades   open     volume   low  close     total_turnover   high
date
2019-12-18    794349.0  2.600 84141413.0 1.600  1.600       8.414141e+10  2.951
2019-12-19    836675.0  2.300 82446069.0 1.450  1.450       8.244607e+10  2.706
2019-12-20    811976.0  2.100 81157294.0 2.060  2.800       8.115729e+10  3.300
...
```

### 实时行情

请使用 `get_ticks` 调取，参数及字段详情请参考API-get_ticks。

```python
In[]:get_ticks('131810.XSHE')
Out[]:
                      open   last  high    low  iopv  prev_iopv  limit_up  \
datetime
2020-03-18 09:15:03  0.000  0.004   0.0  0.000   0.0        0.0  1.000000e+09
2020-03-18 09:15:12  0.000  0.004   0.0  0.000   0.0        0.0  1.000000e+09
2020-03-18 09:15:30  0.000  0.004   0.0  0.000   0.0        0.0  1.000000e+09
...
                     limit_down  prev_close  volume  total_turnover     a1  \
datetime
2020-03-18 09:15:03       0.001       0.004     0.0    0.000000e+00  0.008
2020-03-18 09:15:12       0.001       0.004     0.0    0.000000e+00  0.008
2020-03-18 09:15:30       0.001       0.004     0.0    0.000000e+00  0.008
...
                         a2    a3    a4    a5     b1      b2    b3    b4    b5  \
datetime
2020-03-18 09:15:03     0.0   0.0   0.0  0.00  0.008     0.0   0.0   0.0   0.0
2020-03-18 09:15:12     0.0   0.0   0.0  0.00  0.008     0.0   0.0   0.0   0.0
2020-03-18 09:15:30     0.0   0.0   0.0  0.00  0.008     0.0   0.0   0.0   0.0
...
                       a1_v     a2_v     a3_v     a4_v     a5_v     b1_v  \
datetime
2020-03-18 09:15:03  376020.0      0.0      0.0      0.0      0.0   376020.0
2020-03-18 09:15:12  463360.0      0.0      0.0      0.0      0.0   463360.0
2020-03-18 09:15:30  464720.0      0.0      0.0      0.0      0.0   464720.0
...
                         b2_v      b3_v      b4_v      b5_v
datetime
2020-03-18 09:15:03  3042780.0      0.0      0.0      0.0
2020-03-18 09:15:12  2965440.0      0.0      0.0      0.0
2020-03-18 09:15:30  2964080.0      0.0      0.0      0.0
```

## 银行间同业拆放利率

### get_interbank_offered_rate - 获取上海银行间同业拆放利率数据

```python
rqdatac.get_interbank_offered_rate(start_date=None, end_date=None, fields=None, source='Shibor')
```

获取上海银行间同业拆放利率（Shanghai Interbank Offered Rate，简称Shibor）数据

### 参数

| 参数         | 类型                                      | 说明                               |
| :----------- | :---------------------------------------- | :--------------------------------- |
| `start_date` | `str, datetime.date, datetime.datetime, pandasTimestamp` | 开始日期，默认为去年的查询当日     |
| `end_date`   | `str, datetime.date, datetime.datetime, pandasTimestamp` | 结束日期，默认为查询当日           |
| `fields`     | `str, str list`                           | 字段名称，默认获取全部字段         |
| `source`     | `str`                                     | 数据源，目前仅支持`Shibor`         |

### 返回

`pandas DataFrame`

| 返回     | 类型         | 说明                         |
| :------- | :----------- | :--------------------------- |
| `date`   | `datetime.date` | 日期                         |
| `fields` | `float`      | 'ON'：隔夜<br>'1W'：1 周<br>'2W'：2 周<br>'1M'：1 个月<br>'3M'：3 个月<br>'6M'：6 个月<br>'9M'：9 个月<br>'1Y'：1 年 |

### 范例

获取一段时间的Shibor 数据。

```python
[In] rqdatac.get_interbank_offered_rate(20230501,20230530)
[Out]
         ON     1W     2W     1M     3M     6M     9M     1Y
date
2023-05-04  1.658  1.963  1.979  2.294  2.414  2.501  2.582  2.636
2023-05-05  1.234  1.815  1.920  2.277  2.401  2.491  2.564  2.618
2023-05-06  1.095  1.745  1.764  2.259  2.393  2.481  2.548  2.603
2023-05-08  1.408  1.795  1.793  2.248  2.386  2.470  2.540  2.596
2023-05-09  1.239  1.825  1.813  2.235  2.377  2.462  2.529  2.583
2023-05-10  1.110  1.902  1.861  2.228  2.363  2.452  2.521  2.569
2023-05-11  1.102  1.803  1.818  2.212  2.349  2.439  2.502  2.548
2023-05-12  1.320  1.829  1.846  2.199  2.325  2.427  2.487  2.524
2023-05-15  1.523  1.874  1.909  2.189  2.311  2.419  2.474  2.508
2023-05-16  1.476  1.766  1.822  2.180  2.294  2.410  2.462  2.496
2023-05-17  1.540  1.844  1.870  2.160  2.288  2.403  2.454  2.490
2023-05-18  1.463  1.760  1.948  2.136  2.278  2.392  2.447  2.490
2023-05-19  1.408  1.910  2.062  2.131  2.269  2.390  2.445  2.489
2023-05-22  1.276  1.897  2.100  2.124  2.262  2.379  2.442  2.489
2023-05-23  1.224  1.824  2.065  2.117  2.255  2.372  2.438  2.484
2023-05-24  1.271  1.762  2.050  2.113  2.252  2.366  2.438  2.481
2023-05-25  1.578  1.908  2.050  2.111  2.243  2.364  2.436  2.478
2023-05-26  1.438  1.992  2.096  2.108  2.238  2.358  2.434  2.478
2023-05-29  1.370  1.937  2.051  2.104  2.233  2.355  2.425  2.474
```

# 宏观经济数据


## econ.get_reserve_ratio - 获取存款准备金率

`econ.get_reserve_ratio(reserve_type,start_date,end_date,date_type)`


**参数**

| 参数         | 类型                                     | 说明                                                         |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- |
| `reserve_type` | `str`                                    | 目前有大型金融机构（'major'） 和其他金融机构（'other'）两种分类。 默认为all，即所有类别都会返回。 |
| `start_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为去年的查询当日（基准为信息公布日）。       |
| `end_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为查询当日。                                   |

**返回**

`pandas dataframe`

| 字段           | 类型    | 说明             |
| :------------- | :------ | :--------------- |
| `reserve_type` | `str`   | 存款准备金类别   |
| `info_date`    | `str`   | 消息公布日期     |
| `effective_date` | `str`   | 存款准备金率生效日期 |
| `ratio_floor`  | `float` | 存款准备金下限   |
| `ratio_ceiling` | `float` | 存款准备金上限   |

**范例**

```python
[In] econ.get_reserve_ratio(reserve_type='major',start_date='20170101',end_date='20181017')
[Out] reserve_type effective_date ratio_ceiling ratio_floor info_date
2018-10-07 major_financial_institution 2018-10-15 15.0 15.0 2018-04-17
major_financial_institution 2018-04-25 16.0 16.0
```



## econ.get_money_supply - 获取货币供应量

`econ.get_money_supply(start_date,end_date)`


**参数**

| 参数         | 类型                                     | 说明                                                         |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- |
| `start_date`   | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 开始日期，默认为去年的查询当日（基准为信息公布日）。       |
| `end_date`     | `str`, `datetime.date`, `datetime.datetime`, `pandasTimestamp` | 结束日期，默认为查询当日。                                   |

**返回**

`pandas dataframe`

| 字段            | 类型    | 说明                 |
| :-------------- | :------ | :------------------- |
| `info_date`     | `str`   | 消息公布日期         |
| `effective_date` | `str`   | 货币供应量生效日期   |
| `m0`            | `float` | 市场现金流通量(百万元) |
| `m1`            | `float` | 狭义货币(百万元)     |
| `m2`            | `float` | 广义货币(百万元)     |
| `m0_growth_yoy` | `float` | 市场现金流通量同比   |
| `m1_growth_yoy` | `float` | 狭义货币同比         |
| `m2_growth_yoy` | `float` | 广义货币同比         |

**范例**

```python
[In] econ.get_money_supply(start_date='20180801',end_date='20181017')
[Out] effective_date m2 m1 m0 m2_growth_yoy m1_growth_yoy m0_growth_yoy info_date
2018-09-21 2018-08-31 178867043.0 53832464.0 6977539.0 0.082 0.039 0.033 2018-08-16 2018-07-31 177619611.0 53662429.0 6953059.0 0.085 0.051 0.036
```



## econ.get_factors- 获取宏观因子数据

`econ.get_factors(factors, start_date, end_date)`


获取宏观因子数据。

**参数**

| 参数         | 类型     | 说明                                                         |
| :----------- | :------- | :----------------------------------------------------------- |
| `factors`    | `str`    | 宏观因子名称，见 https://assets.ricequant.com/vendor/rqdata/econ_get_factors.xlsx |
| `start_date` | `datetime` | 起始日期                                                     |
| `end_date`   | `datetime` | 截止日期                                                     |

**返回**

| 字段        | 类型       | 说明       |
| :---------- | :--------- | :--------- |
| `info_date` | `str`      | 因子发布日期 |
| `start_date` | `datetime` | 起始日期   |
| `end_date`  | `datetime` | 截止日期   |
| `value`     | `float`    | 指标数据   |

**范例**

获取工业品出厂价格指数PPI当月同比(上年同月=100)在2017-08-01 到2018-08-01 数据。

```python
[In]econ.get_factors( factors='工业品出厂价格指数PPI_当月同比_(上年同月=100)', start_date='20170801', end_date='20180801')
[Out] [start_date end_date value factor info_date
工业品出厂价格指数PPI_当月同比_(上年同月=100)
2017-08-09 09:30:00 2017-06-30 2017-07-31 105.5000
2017-09-09 09:30:00 2017-07-31 2017-08-31 106.3000
2017-10-16 09:30:00 2017-08-31 2017-09-30 106.9000
2017-11-09 09:30:00 2017-09-30 2017-10-31 106.9000
2017-12-09 09:30:00 2017-10-31 2017-11-30 105.8000 ...]
```
