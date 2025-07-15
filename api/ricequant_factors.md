
# ricequant财务指标数据说明



## 快报数据

须使用`current_performance`调取

| 财务快报字段                   | 解释                                          |
| :----------------------------- | :-------------------------------------------- |
| operating_revenue              | 营业收入or 主营业务收入(元)                   |
| gross_profit                   | 主营业务利润(元)                              |
| operating_profit               | 营业利润(元)                                  |
| total_profit                   | 利润总额(元)                                  |
| np_parent_owners               | 归属母公司净利润(元)                          |
| net_profit_cut                 | 扣除非经常性损益后净利润(元)                  |
| net_operate_cashflow           | 经营活动现金流量净额(元)                      |
| total_assets                   | 总资产(元)                                    |
| se_without_minority            | 归属母公司普通股东权益(元)                    |
| se_parent_owners               | 归属母公司股东权益(元)                        |
| total_shares                   | 总股本(股)                                    |
| basic_eps                      | 基本每股收益                                  |
| eps_weighted                   | 每股收益(加权)(元)                            |
| eps_cut_epscut                 | 每股收益(扣除)(元)                            |
| eps_cut_weighted               | 每股收益(扣除加权)(元)                        |
| roe                            | 净资产收益率(摊薄)(%)                         |
| roe_weighted                   | 净资产收益率(加权)(%)                         |
| roe_cut                        | 净资产收益率(扣除摊薄)(%)                     |
| roe_cut_weighted               | 净资产收益率(扣除加权)(%)                     |
| net_operate_cashflow_per_share | 每股经营活动现金流量净额(元)                  |
| equity_per_share               | 每股净资产(元)                                |
| operating_revenue_yoy          | 主营业务收入同比(%)                           |
| gross_profit_yoy               | 主营业务利润同比(%)                           |
| operating_profit_yoy           | 营业利润同比(%)                               |
| total_profit_yoy               | 利润总额同比(%)                               |
| np_parent_minority_pany_yoy    | 归属母公司净利润同比(%)                       |
| ne_t_minority_ty_yoy           | 扣除非经常性损益后净利润同比(%)               |
| net_operate_cash_flow_yoy      | 经营活动现金流量净额同比(%)                   |
| total_assets_to_opening        | 总资产较期初比(%)                             |
| se_without_minority_to_opening | 归属母公司股东权益较期初比(%)                 |
| basic_eps_yoy                  | 每股收益(摊薄) 同比(%)                        |
| eps_weighted_yoy               | 每股收益(加权) 同比(%)                        |
| eps_cut_yoy                    | 每股收益(扣除) 同比(%)                        |
| eps_cut_weighted_yoy           | 每股收益(扣除加权) 同比(%)                    |
| roe_yoy                        | 净资产收益率(摊薄) 同比(%)                    |
| roe_weighted_yoy               | 净资产收益率(加权) 同比(%)                    |
| roe_cut_yoy                    | 净资产收益率(扣除摊薄) 同比(%)                |
| roe_cut_weighted_yoy           | 净资产收益率(扣除加权) 同比(%)                |
| net_operate_cash_flow_per_share_yoy | 每股经营活动现金流量净额同比(%)          |
| net_asset_psto_opening         | 每股净资产较期初比(%)                         |

## 业绩预告数据

需使用`performance_forecast`调取

| 业绩预告字段           | 解释             |
| :--------------------- | :--------------- |
| forecast_type          | 整体业绩预期     |
| forecast_description   | 业绩预期时间段描述 |
| forecast_growth_rate_floor | 最小预期增长幅度 |
| forecast_growth_rate_ceiling | 最大预期增长幅度 |
| forecast_earning_floor | 最小预期收入     |
| forecast_earning_ceiling | 最大预期收入     |
| forecast_np_floor      | 最小预期净利润   |
| forecast_np_ceiling    | 最大预期净利润   |
| forecast_eps_floor     | 最小预期每股收益 |
| forecast_eps_ceiling   | 最大预期每股收益 |
| net_profit_yoy_const_forecast | 一致预期净利润增幅 |

## 基础财务数据

以下表格的字段全部基于新会计准则并直接采集于三大财报（资产负债表、利润表和现金流量表）。财报本身的来源常见有交易所的定期公告中的常规季报和年报、临时公告中的业绩快报和比较式财务报告以及招股说明书等。该部分数据一定来源于完整的财报，所以一般意义上的业绩快报，业绩预增报告中的数据并不会出现。

**单季度数据处理**除提供三大表基础财务数据外，我们还对其进行了单季度处理：

*   每日提供近12 期的单季度数据，并以`_mrq_n`后缀(most recent quarter) 方式在原有基础字段上进行拓展，可以使用`get_factor`函数调用，例如: `net_profit_mrq_0` 表示距离当前查询日期最近一期财报当中的单季净利润指标,`cash_equivalent_mrq_1` 表示距离当前查询日最近一期的上一期财报当中的单季货币资金指标
*   对每一期数据进行Point-in-Time（PIT）处理，考虑如下例子：

    在发布财务报告以后，上市公司可能会对数据进行修正。因此，在进行指标计算的时候，需要考虑当前时间点所能取到的最新数据，以避免未来数据的问题。该处理称为Point-in-Time（PIT）处理。例如，考虑以下例子：

    *   某一上市公司的2018 年4 月1 日发布2018 年一季度报告；
    *   5 月1 日修改了一季报净利润数据；
    *   6 月1 日再次修改净利润数据；
    *   7 月1 日发布2018 年二季报报告

    则在2018 年4 月2 日、2018 年5 月2 日和2018 年7 月2 日计算该公司最近八期的PIT 单季度净利润数据如下表所示：

    | 2018-04-02         | 2018-05-02         | 2018-07-02         |
    | :----------------- | :----------------- | :----------------- |
    | 2018 年一季度（4 月1 日发布） | 2018 年一季度（5 月1 日调整） | 2018 年二季度      |
    | 2017 年四季度      | 2017 年四季度      | 2018 年一季度（6 月1 日调整） |
    | 2017 年三季度      | 2017 年三季度      | 2017 年四季度      |
    | 2017 年二季度      | 2017 年二季度      | 2017 年三季度      |
    | 2017 年一季度      | 2017 年一季度      | 2017 年二季度      |
    | 2016 年四季度      | 2016 年四季度      | 2017 年一季度      |
    | 2016 年三季度      | 2016 年三季度      | 2016 年四季度      |
    | 2016 年二季度      | 2016 年二季度      | 2016 年三季度      |

**TTM 处理**

TTM 是Trailing Twelve Months 的简称，会使用过去4 个季度的滚动财务数据进行计算，可避免某一期财报数据的偶然性。（对于来自利润表和现金流量表的数据TTM 为滚动加和，来自资产负债表的数据TTM 为滚动求平均）。

提供最近9 期的TTM 数据，并以`_ttm_n`后缀的方式在原有基础字段上进行拓展，可以使用`get_factor`函数调用。例如：`revenue_ttm_0` 代表最近一期滚动净利润数据，数值上等于`revenue_mrq_0 + revenue_mrq_1 + revenue_mrq_2 + revenue_mrq_3`

**LYR 处理**

LYR 是Last Year Ratio 的简称，会使用最近一期年报的数据。

提供最近9 期的LYR 数据，并以`_lyr_n`后缀的方式在原有基础字段上进行拓展，可以使用`get_factor`函数调用。例如：`revenue_lyr_0` 代表最近一期年报的净利润数据

注意对于利润表中的基本每股收益，目前米筐的单季度处理以及TTM 处理都直接采用的每股收益指标相减的方式。

### 利润表

可点击下载[利润表字段说明](https://assets.ricequant.com/vendor/rqdata/%E5%88%A9%E6%B6%A6%E8%A1%A8.xlsx)

| 字段                          | 释义、备注                                    |
| :---------------------------- | :-------------------------------------------- |
| revenue                       | 营业总收入：公司经营所取得的收入总额金融类公司不公布营业总收入，因此revenue 指标只能使用类似的一个指标-operating_revenue 来参考: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E6%80%BB%E6%94%B6%E5%85%A5) |
| operating_revenue             | 营业收入：公司经营主要业务所取得的收入总额: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E6%94%B6%E5%85%A5) |
| net_interest_income           | 利息净收入                                    |
| net_commission_income         | 手续费及佣金净收入                            |
| commission_income             | 其中:手续费及佣金收入                         |
| commission_expense            | 其中:手续费及佣金支出                         |
| net_proxy_security_income     | 其中:代理买卖证券业务净收入                   |
| sub_issue_security_income     | 其中:证券承销业务净收入                       |
| net_trust_income              | 其中:受托客户资产管理业务净收入               |
| earned_premiums               | 已赚保费                                      |
| premiums_income               | 保险业务收入                                  |
| reinsurance_income            | 其中:分保费收入                               |
| reinsurance                   | 减:分出保费                                   |
| unearned_premium_reserve      | 提取未到期责任准备金                          |
| total_expense                 | 营业总成本                                    |
| operating_expense             | 营业支出(金融类企业披露)                      |
| refunded_premiums             | 退保金                                        |
| compensation_expense          | 赔付支出                                      |
| amortization_expense          | 减:摊回赔付支出                               |
| premium_reserve               | 提取保险责任准备金                            |
| amortization_premium_reserve  | 减:摊回保险责任准备金                         |
| policy_dividend_payout        | 保单红利支出                                  |
| reinsurance_cost              | 分保费用                                      |
| other_operating_revenue       | 其他经营收入                                  |
| other_operating_cost          | 其他经营成本                                  |
| r_n_d                         | 研发费用                                      |
| other_net_income              | 非经营性净收益                                |
| net_open_hedge_income         | 净敞口套期收益                                |
| other_revenue                 | 其他收益                                      |
| credit_asset_impairment       | 信用资产减值损失                              |
| o_n_a_expense                 | 业务及管理费用                                |
| amortization_reinsurance_cost | 减:摊回分保费用                               |
| insurance_commission_expense  | 保险手续费及佣金支出                          |
| disposal_income_on_asset      | 资产处置收益                                  |
| cost_of_goods_sold            | 营业成本(非金融类企业披露)：公司经营主要业务产生的实际成本: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E6%88%90%E6%9C%AC) |
| sales_tax                     | 营业税: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E7%A8%8E) |
| gross_profit                  | 主营业务利润: [investopedia](https://www.investopedia.com/terms/g/grossprofit.asp) |
| selling_expense               | 销售费用：指企业在销售产品、自制半成品和工业性劳务等过程中发生的各项费用: [mba](https://wiki.mbalib.com/wiki/%E9%94%80%E5%94%AE%E8%B4%B9%E7%94%A8) |
| ga_expense                    | 管理费用：指企业的行政管理部门为管理和组织经营而发生的各项费用: [mba](https://wiki.mbalib.com/wiki/%E7%AE%A1%E7%90%86%E8%B4%B9%E7%94%A8) |
| financing_expense             | 财务费用： 指企业为筹集生产经营所需资金等而发生的费用，包括利息支出（减利息收入）、汇兑损失（减汇兑收益）以及相关的手续费等: [mba](https://wiki.mbalib.com/wiki/%E8%B4%A2%E5%8A%A1%E8%B4%B9%E7%94%A8) |
| financing_interest_income     | 利息收入（财务费用），财务费用科目下进一步细分的子会计科目 |
| financing_interest_expense    | 利息支出（财务费用），财务费用科目下进一步细分的子会计科目 |
| exchange_gains_or_losses      | 兑汇损益：发生外币交易后期末账户因此调整时，由于采用不同货币，或同一货币不同比价的汇率核算时产生的、按记账本位币折算的差额: [mba](https://wiki.mbalib.com/wiki/%E6%B1%87%E5%85%8D%E6%8D%9F%E7%9B%8A) |
| profit_from_operation         | 营业利润： 企业在其全部销售业务中实现的利润，又称营业利润、经营利润，它包含主营业务利润: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E5%88%A9%E6%B6%A6) |
| invest_income_associates      | 对联营合营企业的投资收益                      |
| fair_value_change_income      | 公允价值变动净收益                            |
| investment_income             | 投资收益：指企业进行投资所获得的经济利益: [mba](https://wiki.mbalib.com/wiki/%E6%8A%95%E8%B5%84%E6%94%B6%E7%9B%8A) |
| asset_impairment              | 资产减值损失                                  |
| interest_income               | 利息收入                                      |
| interest_expense              | 利息支出                                      |
| non_operating_revenue         | 营业外收入：指企业发生的与其生产经营无直接关系的各项收入，包括固定资产盘盈、非货币性交易收益、出售无形资产收益等: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E5%A4%96%E6%94%B6%E5%85%A5) |
| non_operating_expense         | 营业外支出：企业发生的与其生产经营无直接关系的各项支出，如固定资产盘亏、债务重组损失、罚款支出、捐赠支出、非常损失等: [mba](https://wiki.mbalib.com/wiki/%E8%90%A5%E4%B8%9A%E5%A4%96%E6%94%AF%E5%87%BA) |
| disposal_loss_on_asset        | 非流动资产处置净损失：包括固定资产处置损失和无形资产出售损失: [mba](https://wiki.mbalib.com/wiki/%E9%9D%9E%E6%B5%81%E5%8A%A8%E8%B5%84%E4%BA%A7%E5%A4%84%E7%BD%AE%E6%8D%9F%E5%A4%B1) |
| other_effecting_total_profits_items | 影响利润总额的其他科目                        |
| profit_before_tax             | 利润总额： 指税前利润，也就是企业在所得税前一定时期内经营活动的总成果: [mba](https://wiki.mbalib.com/wiki/%E5%88%A9%E6%B6%A6%E6%80%BB%E9%A2%9D) |
| income_tax                    | 所得税：以纳税人的所得额为课税对象的各种税收的统称: [mba](https://wiki.mbalib.com/wiki/%E6%89%80%E5%BE%97%E7%A8%8E) |
| unrealised_investment_loss    | 未确认的投资损失： 因母公司和子公司确认子公司损益方式不同而在合并报表中使用的一个调节性科目 |
| other_effecting_net_profits_items | 影响净利润的其他科目                          |
| net_profit                    | 净利润（收益）是指在利润总额中按规定交纳了所得税以后公司的利润留存，一般也称为税后利润或净收入: [mba](https://wiki.mbalib.com/wiki/%E5%87%80%E5%88%A9%E6%B6%A6) |
| non_recurring_pnl             | 非经常性损益                                  |
| net_profit_deduct_non_recurring_pnl | 扣除非经常性损益后的净利润                    |
| classified_by_continuity_operation | (一)按经营持续性分类                          |
| continuous_operation_net_profit | 持续经营净利润                                |
| discontinued_operation_net_profit | 终止经营净利润                                |
| classified_by_ownership       | (二)按所有权归属分类                          |
| net_profit_parent_company     | 归属母公司净利润： 反映在企业合并净利润中，归属于母公司股东（所有者）所有的那部分净利润: [others](https://www.baidu.com/s?wd=%E5%BD%92%E5%B1%9E%E4%BA%8E%E6%AF%8D%E5%85%AC%E5%8F%B8%E8%82%A1%E4%B8%9C%E5%88%A9%E6%B6%A6&ie=utf-8) |
| minority_profit               | 少数股东损益                                  |
| other_income                  | 其他综合收益：指企业根据企业会计准则规定未在损益中确认的各项利得和损失扣除所得税影响后的净额: [mba](https://wiki.mbalib.com/wiki/%E5%85%B6%E4%BB%96%E7%BB%BC%E5%90%88%E6%94%B6%E7%9B%8A) |
| other_income_unclassified_income_statement | (一)以后不能重分类进损益表的其他综合收益      |
| remearsured_other_income      | 1.1 重新计量设定收益计划净负债或净资产的变动  |
| other_income_equity_unclassified_income_statement | 1.2 权益法下在被投资单位不能重分类进损益表的其他综合收益中享有的份额 |
| other_equity_instruments_change | 1.3 其他权益工具投资公允价值变动              |
| corporate_credit_risk_change  | 1.4 企业自身信用风险公允价值变动              |
| other_income_classified_income_statement | (二)以后能重分类进损益表的其他综合收益        |
| other_income_equity_classified_income_statement | 2.1 权益法下在被投资单位能重分类进损益表的其他综合收益中享有的份额 |
| financial_asset_available_for_sale_change | 2.2 可供出售金融资产公允价值变动损益          |
| financial_asset_hold_to_maturity_change | 2.3 持有至到期投资重分类为可供出售金融资产损益 |
| cash_flow_hedging_effective_portion | 2.4 现金流量套期损益的有效部分                  |
| foreign_currency_statement_converted_difference | 2.5 外币财务报表分析折算差额                  |
| others                        | 2.6 其他                                      |
| other_debt_investment_change  | 2.7 其他债权投资公允价值变动                  |
| assets_reclassified_other_income | 2.8 金融资产重分类计入其他综合收益的金额      |
| other_debt_investment_reserve | 2.9 其他债权投资信用减值准备                  |
| other_income_minority         | 归属于少数股东的其他综合收益总额              |
| total_income                  | 综合收益总额：反映企业净利润与其他综合收益的合计金额: [mba](https://wiki.mbalib.com/wiki/%E7%BB%BC%E5%90%88%E6%94%B6%E7%9B%8A%E6%80%BB%E9%A2%9D) |
| total_income_parent_company   | 归属于母公司所有者的综合收益总额              |
| total_income_minority         | 归属于少数股东的综合收益总额                  |
| basic_earnings_per_share      | 基本每股收益：本每股收益是指企业应当按照属于普通股股东的当期净利润，除以发行在外普通股的加权平均数从而计算出的每股收益: [mba](https://wiki.mbalib.com/wiki/%E5%9F%BA%E6%9C%AC%E6%AF%8F%E8%82%A1%E6%94%B6%E7%9B%8A) |
| fully_diluted_earnings_per_share | 稀释每股收益                                  |
| adjust_asset_impairment       | 资产减值损失：根据财政部发布的《关于修订印发2019 年度一般企业财务报表格式的通知》格式，“资产减值损失”不隶属于营业总成本部分。因企业披露不一致性，经研究，从2020.07.08 披露的2020 年半年报开始，字段数值按照原文披露展示，历史报告期维持原有规则。 |
| adjust_credit_asset_impairment | 信用减值损失：根据财政部发布的《关于修订印发2019 年度一般企业财务报表格式的通知》格式，“信用减值损失”不隶属于营业总成本部分。因企业披露不一致性，经研究，从2020.07.08 披露的2020 年半年报开始，字段数值按照原文披露展示，历史报告期维持原有规则。 |

### 资产负债表

可点击下载[资产负债表字段说](https://assets.ricequant.com/vendor/rqdata/%E8%B5%84%E4%BA%A7%E8%B4%9F%E5%80%BA%E8%A1%A8.xlsx)

| 字段                          | 释义、备注                                    |
| :---------------------------- | :-------------------------------------------- |
| financial_asset_held_for_trading | 企业为了近期内出售而持有的金融资产。通常情况下，以赚取差价为目的从二级市场购入的股票、债券和基金会分类为交易性金融资产: [mba](https://wiki.mbalib.com/wiki/%E4%BA%A4%E6%98%93%E6%80%A7%E9%87%91%E8%9E%8D%E8%B5%84%E4%BA%A7) : [wikipedia](https://zh.wikipedia.org/wiki/%E4%BA%A4%E6%98%93%E6%80%A7%E9%87%91%E8%9E%8D%E8%B5%84%E4%BA%A7) |
| cash_equivalent               | 货币资金                                      |
| client_deposits               | 其中:客户资金存款                             |
| bill_receivable               | 应收票据：指企业持有的还没有到期、尚未兑现的票据: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E6%94%B6%E7%A5%A8%E6%8D%AE) |
| dividend_receivable           | 应收股利： 指企业因股权投资而应收取的现金股利以及应收其他单位的利润，不包括应收的股票股利: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E6%94%B6%E8%82%A1%E5%88%A9) |
| bill_accts_receivable         | 应收票据及应收账款                            |
| interest_receivable           | 应收利息：短期债券投资实际支付的价款中包含的已到付息期但尚未领取的债券利息: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E6%94%B6%E5%88%A9%E6%81%AF) |
| bad_debt_reserve              | 坏账准备：指对应收账款预提的，对不能收回或回收可能性极低的应收账款用来抵销，是应收账款的备抵账户: [mba](https://wiki.mbalib.com/wiki/%E5%9D%8F%E8%B4%A6%E5%87%86%E5%A4%87) |
| net_accts_receivable          | 应收账款净额                                  |
| contract_assets               | 合同资产                                      |
| prepayment                    | 预付账款：企业因购货和接受劳务，按照合同规定预付给供应单位的款项: [mba](https://wiki.mbalib.com/wiki/%E9%A2%84%E4%BB%98%E8%B4%A6%E6%AC%BE) |
| financial_receivable          | 应收款项融资                                  |
| financial_lease_receivable    | 应收融资租赁款                                |
| other_equity_investment       | 其他权益工具投资                              |
| other_illiquidy_financial_assets | 其他非流动金融资产                            |
| non_current_asset_due_one_year | 一年内到期的非流动资产                        |
| other_receivables_interest_dividend | 其他应收款(含利息和股利)                      |
| inventory                     | 存货: 指企业在日常活动中持有的以备出售的产成品或商品、处在生产过程中的在产品、在生产过程或提供劳务过程中耗用的材料和物料等: [mba](https://wiki.mbalib.com/wiki/%E5%AD%98%E8%B4%A7) |
| consumable_biological_assets  | 消耗性生物资产                                |
| deferred_expense              | 待摊费用： 指支出先发生，费用归属后发生的事项，按照时间长短分为短期待摊费用和长期待摊费用: [mba](https://wiki.mbalib.com/wiki/%E5%BE%85%E6%91%8A%E8%B4%B9%E7%94%A8) |
| assets_hold_for_sale          | 划分为持有待售的资产                          |
| contract_work                 | 工程施工： 工程施工是指按照设计图纸和相关文件的要求，在建设场地上将设计意图付诸实现的测量、作业、检验，形成工程实体建成最终产品的活动: [mba](https://wiki.mbalib.com/wiki/%E5%B7%A5%E7%A8%8B%E6%96%BD%E5%B7%A5) |
| other_current_assets          | 其他流动资产： 指除货币资金、短期投资、应收票据、应收账款、其他应收款、存货等流动资产以外的流动资产: [mba](https://wiki.mbalib.com/wiki/%E5%85%B6%E4%BB%96%E6%B5%81%E5%8A%A8%E8%B5%84%E4%BA%A7) |
| current_assets                | 流动资产合计： 指企业可以在一年内或者超过一年的一个营业周期内变现或者耗用的资产: [mba](https://wiki.mbalib.com/wiki/%E6%B5%81%E5%8A%A8%E8%B5%84%E4%BA%A7%E5%90%88%E8%AE%A1) |
| financial_asset_available_for_sale | 可供出售金融资产： 指初始确认时即被指定为可供出售的非衍生金融资产， 以及贷款和应收款项、持有至到期投资、交易性金融资产之外的非衍生金融资产: [mba](https://wiki.mbalib.com/wiki/%E5%8F%AF%E4%BE%9B%E5%87%BA%E5%94%AE%E9%87%91%E8%9E%8D%E8%B5%84%E4%BA%A7) |
| non_current_liability_due_one_year | 一年内到期的非流动负债                        |
| debt_investment               | 债权投资                                      |
| other_debt_investment         | 其他债权投资                                  |
| financial_asset_hold_to_maturity | 持有至到期投资： 指企业有明确意图并有能力持有至到期，到期日固定、回收金额固定或可确定的非衍生金融资产: [mba](https://wiki.mbalib.com/wiki/%E6%8C%81%E6%9C%89%E8%87%B3%E5%88%B0%E6%9C%9F%E6%8A%95%E8%B5%84) |
| real_estate_investment        | 投资性房地产： 指为赚取租金或资本增值，或两者兼有而持有的房地产: [mba](https://wiki.mbalib.com/wiki/%E6%8A%95%E8%B5%84%E6%80%A7%E6%88%BF%E5%9C%B0%E4%BA%A7) |
| long_term_receivables         | 长期应收款： 长期应收款是根据长期应收款的账户余额减去未确认融资收益还有一年内到期的长期应收款: [mba](https://wiki.mbalib.com/wiki/%E9%95%BF%E6%9C%9F%E5%BA%94%E6%94%B6%E6%AC%BE) |
| net_long_term_equity_investment | 长期股权投资净额                              |
| accumulated_depreciation      | 累计折旧： "累计折旧"账户属于资产类的备抵调整账户，其结构与一般资产账户的结构刚好相反，贷方登记增加，借方登记减少，余额在贷方: [mba](https://wiki.mbalib.com/wiki/%E7%B4%AF%E8%AE%A1%E6%8A%98%E6%97%A7) |
| depreciation_reserve          | 固定资产减值准备： 指由于固定资产市价持续下跌，或技术陈旧、损坏、长期闲置等原因导致其可收回金额低于账面价值的， 应当将可收回金额低于其账面价值的差额作为固定资产减值准备: [mba](https://wiki.mbalib.com/wiki/%E5%9B%BA%E5%AE%9A%E8%B5%84%E4%BA%A7%E5%87%8F%E5%80%BC%E5%87%86%E5%A4%87) |
| net_fixed_assets              | 固定资产净额： 固定资产原值减累计折旧再减减值准备后的差额: [mba](https://wiki.mbalib.com/wiki/%E5%9B%BA%E5%AE%9A%E8%B5%84%E4%BA%A7%E5%87%80%E5%80%BC) |
| total_fixed_assets            | 固定资产合计                                  |
| engineer_material             | 工程物资： 指用于固定资产建造的建筑材料，如钢材、水泥、玻璃等。在资产负债表中并入在建工程项目: [mba](https://wiki.mbalib.com/wiki/%E5%B7%A5%E7%A8%8B%E7%89%A9%E8%B5%84) |
| construction_in_progress      | 在建工程： 指企业固定资产的新建、改建、扩建，或技术改造、设备更新和大修理工程等尚未完工的工程支出: [mba](https://wiki.mbalib.com/wiki/%E5%9C%A8%E5%BB%BA%E5%B7%A5%E7%A8%8B) |
| total_construction_in_progress | 在建工程合计                                  |
| fixed_asset_to_be_disposed    | 固定资产清理： 指企业因出售、报废和毁损等原因转入清理的固定资产价值及其在清理过程中所发生的清理费用和清理收入等: [mba](https://wiki.mbalib.com/wiki/%E5%9B%BA%E5%AE%9A%E8%B5%84%E4%BA%A7%E6%B8%85%E7%90%86) |
| capitalized_biological_assets | 生产性生物资产： 指为产出农产品、提供劳务或出租等目的而持有的生物资产，包括经济林、薪炭林、产畜和役畜等: [mba](https://wiki.mbalib.com/wiki/%E7%94%9F%E4%BA%A7%E6%80%A7%E7%94%9F%E7%89%A9%E8%B5%84%E4%BA%A7) |
| oil_and_gas_assets            | 油气资产： 指油气开采企业所拥有或控制的井及相关设施和矿区权益。油气资产属于递耗资产: [mba](https://wiki.mbalib.com/wiki/%E6%B2%B9%E6%B0%94%E8%B5%84%E4%BA%A7) |
| intangible_assets             | 无形资产： 指企业拥有或者控制的没有实物形态的可辨认非货币性资产: [mba](https://wiki.mbalib.com/wiki/%E6%97%A0%E5%BD%A2%E8%B5%84%E4%BA%A7) |
| seat_costs                    | 交易席位费                                    |
| impairment_intangible_assets  | 开发支出： 反映企业开发无形资产过程中能够资本化形成无形资产成本的支出部分: [mba](https://wiki.mbalib.com/wiki/%E5%BC%80%E5%8F%91%E6%94%AF%E5%87%BA) |
| use_right_assets              | 使用权资产                                    |
| goodwill                      | 商誉： 指能在未来期间为企业经营带来超额利润的潜在经济价值，或一家企业预期的获利能力超过可辨认资产正常获利能力（如社会平均投资回报率）的资本化价值: [mba](https://wiki.mbalib.com/wiki/%E5%95%86%E8%B6%A3) |
| long_term_deferred_expenses   | 长期待摊费用： 指企业已经支出，但摊销期限在1 年以上(不含1 年)的各项费用: [mba](https://wiki.mbalib.com/wiki/%E9%95%BF%E6%9C%9F%E5%BE%85%E6%91%8A%E8%B4%B9%E7%94%A8) |
| deferred_income_tax_assets    | 递延所得税资产： 指对于可抵扣暂时性差异，以未来期间很可能取得用来抵扣可抵扣暂时性差异的应纳税所得额为限确认的一项资产: [mba](https://wiki.mbalib.com/wiki/%E9%80%92%E5%BB%B6%E6%89%80%E5%BE%97%E7%A8%8E%E8%B5%84%E4%BA%A7) |
| other_non_current_assets      | 其他非流动资产： 指除资产负债表上所列非流动资产项目以外的其他周转期超过1 年的长期资产: [mba](https://wiki.mbalib.com/wiki/%E5%85%B6%E4%BB%96%E9%9D%9E%E6%B5%81%E5%8A%A8%E8%B5%84%E4%BA%A7) |
| non_current_assets            | 非流动资产合计                                |
| loan_account_receivables      | 投资-贷款及应收款项(应收款项类投资)           |
| fund_providing                | 融出资金                                      |
| reinsurance_reserve_receivable | 应收分保合同准备金                            |
| settlement_provision          | 结算备付金                                    |
| client_provision              | 客户备付金                                    |
| interbank_deposits            | 存放同业款项                                  |
| precious_metals               | 贵金属                                        |
| lend_capital                  | 拆出资金                                      |
| derivative_financial_assets   | 衍生金融资产                                  |
| resale_financial_assets       | 买入返售金融资产                              |
| loans_advances_to_customers   | 发放贷款和垫款                                |
| insurance_receivable          | 应收保费                                      |
| subrogation_fee_receivable    | 应收代位追偿款                                |
| reinsurance_receivable        | 应收分保账款                                  |
| unearned_reserve_receivable   | 应收分保未到期责任准备金                      |
| unclaimed_reserve_receivable  | 应收分保未决赔款准备金                        |
| life_reserve_receivable       | 应收分保寿险责任准备金                        |
| health_reserve_receivable     | 应收分保长期健康险责任准备金                  |
| insurer_mortgage_loan         | 保户质押贷款                                  |
| fixed_deposits                | 定期存款                                      |
| refundable_deposits           | 存出保证金                                    |
| refundable_capital_deposits   | 存出资本保证金                                |
| independent_account_assets    | 独立账户资产                                  |
| other_assets                  | 其他资产                                      |
| other_accts_receivable        | 其他应收款(原值)：是企业除应收票据、应收账款和预付账款以外的各种应收暂付款项 |
| total_assets                  | 总资产： 指企业拥有或可控制的能以货币计量的经济资源，包括各种财产、债权和其他权利: [mba](https://wiki.mbalib.com/wiki/%E6%80%BB%E8%B5%84%E4%BA%A7) |
| mortgaged_loan                | 质押借款                                      |
| short_term_loans              | 短期借款： 还款期一年以下，企业用来维持正常的生产经营所需的资金或为抵偿某项债务而向银行或其他金融机构等外单位借入的资金: [mba](https://wiki.mbalib.com/wiki/%E7%9F%AD%E6%9C%9F%E5%80%9F%E6%AC%BE) |
| financial_liabilities         | 交易性金融负债： 交易性金融负债，指企业采用短期获利模式进行融资所形成的负债，比如应付短期债券: [others](https://www.baidu.com/s?wd=%E4%BA%A4%E6%98%93%E6%80%A7%E9%87%91%E8%9E%8D%E8%B4%9F%E5%80%BA&ie=utf-8) |
| notes_payable                 | 应付票据： 应付票据是指企业购买材料、商品和接受劳务供应等而开出、承兑的商业汇票，包括商业承兑汇票和银行承兑汇票。 在我国应收票据、应付票据仅指"商业汇票"，包括"银行承兑汇票"和"商业承兑汇票"两种，属于远期票据，付款期一般在1 个月以上，6 个月以内: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BB%98%E7%A5%A8%E6%8D%AE) |
| accts_payable                 | 应付账款： 应付帐款是指企业因购买材料、物资和接受劳务供应等而付给供货单位的帐款: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BB%98%E8%B4%A6%E6%AC%BE) |
| bill_accts_payable            | 应付票据及应付账款                            |
| contract_liabilities          | 合同负债                                      |
| advance_from_customers        | 预收账款： 预收账款指买卖双方协议商定，由购货方预先支付一部分货款给供应方而发生的一项负债: [mba](https://wiki.mbalib.com/wiki/%E9%A2%84%E6%94%B6%E8%B4%A6%E6%AC%BE) |
| payroll_payable               | 应付职工薪酬： 应付职工薪酬是指企业为获得职工提供的服务而给予各种形式的报酬以及其他相关支出: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BB%98%E8%81%8C%E5%B7%A5%E8%96%AA%E9%85%AC) |
| dividend_payable              | 应付股利： 应付股利是指企业根据年度利润分配方案，确定分配的股利: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BB%98%E8%82%A1%E5%88%A9) |
| tax_payable                   | 应交税费： 应交税费是指企业根据在一定时期内取得的营业收入、实现的利润等，按照现行税法规定，采用一定的计税方法计提的应交纳的各种税费: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BA%A4%E7%A8%8E%E8%B4%B9) |
| interest_payable              | 应付利息： 应付利息，是指金融企业根据存款或债券金额及其存续期限和规定的利率，按期计提应支付给单位和个人的利息: [investopedia](https://www.investopedia.com/terms/a/accruedinterest.asp) |
| other_fees_payable            | 其他应交款： 指企业需要向国家缴纳的各项款项中除了税金以外的各种应交款项，主要包括教育附加费、车辆购置附加费等。: [others](https://www.baidu.com/s?wd=%E5%85%B6%E4%BB%96%E5%BA%94%E4%BA%A4%E6%AC%BE&ie=utf-8) |
| other_payable                 | 其他应付款： 该科目只核算企业应付其他单位或个人的零星款项，如应付经营租入固定资产和包装物的租金、存入保证金等: [mba](https://wiki.mbalib.com/wiki/%E5%85%B6%E4%BB%96%E5%BA%94%E4%BB%98%E6%AC%BE) |
| other_payable_interest_dividend | 其他应付款（含利息和股利）                    |
| short_term_debt               | 应付短期债券： 应付短期债券是企业筹资发行一年以下期限的债券，属于流动负债: [others](https://www.baidu.com/s?wd=%E5%BA%94%E4%BB%98%E7%9F%AD%E6%9C%9F%E5%80%BA%E5%88%B8&ie=utf-8) |
| accrued_expense               | 预提费用： 预提费用是指企业按规定预先提取但尚未实际支付的各项费用。 就是企业还没支付，但应该要支付的，要记入负债: [others](https://www.baidu.com/s?wd=%E9%A2%84%E6%8F%90%E8%B4%B9%E7%94%A8&ie=utf-8) |
| liabilities_hold_for_sale     | 划分为持有待售的负债                          |
| estimated_liabilities         | 预计负债： 预计负债是因或有事项可能产生的负债: [mba](https://wiki.mbalib.com/wiki/%E9%A2%84%E8%AE%A1%E8%B4%9F%E5%80%BA) |
| deferred_income               | 递延收益： 递延收益是指尚待确认的收入或收益，也可以说是暂时未确认的收益，它是权责发生制在收益确认上的运用: [mba](https://wiki.mbalib.com/wiki/%E9%80%92%E5%BB%B6%E6%94%B6%E7%9B%8A) |
| long_term_liabilities_due_one_year | 一年内到期的长期负债： 一年内到期的长期负债是指反映企业长期负债中自编表日起一年内到期的长期负债: [others](https://www.baidu.com/s?wd=%E4%B8%80%E5%B9%B4%E5%86%85%E5%88%B0%E6%9C%9F%E7%9A%84%E9%95%BF%E6%9C%9F%E8%B4%9F%E5%80%BA&ie=utf-8)【该数据来自旧会计准则】 |
| other_current_liabilities     | 其他流动负债： 指不能归属于短期借款，应付短期债券券，应付票据，应付帐款，应付所得税，其他应付款，预收账款这七款项目的流动负债。 但以上各款流动负债，其金额未超过流动负债合计金额百分之五者，得并入其他流动负债内: [others](https://www.baidu.com/s?wd=%E5%85%B6%E4%BB%96%E6%B5%81%E5%8A%A8%E8%B4%9F%E5%80%BA&ie=utf-8) |
| current_liabilities           | 流动负债合计： 流动负债合计是指企业在一年内或超过一年的一个营业周期内需要偿还的债务: [mba](https://wiki.mbalib.com/wiki/%E6%B5%81%E5%8A%A8%E8%B4%9F%E5%80%BA%E5%90%88%E8%AE%A1) |
| long_term_loans               | 长期借款： 长期借款是指企业从银行或其他金融机构借入的期限在一年以上(不含一年)的借款: [mba](https://wiki.mbalib.com/wiki/%E9%95%BF%E6%9C%9F%E5%80%9F%E6%AC%BE) |
| bond_payable                  | 应付债券： 公司为筹集长期资金而实际发行的债券及应付的利息: [mba](https://wiki.mbalib.com/wiki/%E5%BA%94%E4%BB%98%E5%80%BA%E5%88%B8) |
| preference_shares             | 优先股                                        |
| perpetual_bond                | 永续债（应付债券）                            |
| long_term_payable             | 长期应付款： 指企业除了长期借款和应付债券以外的长期负债，包括应付引进设备款、应付融资租入固定资产的租赁费等: [mba](https://wiki.mbalib.com/wiki/%E9%95%BF%E6%9C%9F%E5%BA%94%E4%BB%98%E6%AC%BE) |
| accrued_staff_costs           | 长期应付职工薪酬                              |
| grants_received               | 专项应付款： 企业接受国家作为企业所有者拨入的具有专门用途的款项所形成的不需要以资产或增加其他负债偿还的负债: [others](https://www.baidu.com/s?wd=%E4%B8%93%E9%A1%B9%E5%BA%94%E4%BB%98%E6%AC%BE&ie=utf-8) |
| housing_revolving_funds       | 住房周转金： 房周转金是指企业从各种规定来源取得的、用于职工住房各方面开支的，除公益金、住房折旧和住房公积金以外的住房基金: [mba](https://wiki.mbalib.com/wiki/%E4%BD%8F%E6%88%BF%E5%91%A8%E8%BD%AC%E9%87%91) |
| deferred_income_tax_liabilities | 递延所得税负债： 指根据应纳税暂时性差异计算的未来期间应付所得税的金额: [mba](https://wiki.mbalib.com/wiki/%E9%80%92%E5%BB%B6%E6%89%80%E5%BE%97%E7%A8%8E%E8%B4%9F%E5%80%BA) |
| lease_liabilities             | 租赁负债                                      |
| financial_lease_payable       | 应付融资租赁款                                |
| other_non_current_liabilities | 其他非流动负债： 反映企业除长期借款、应付债券等项目以外的其他非流动负债: [mba](https://wiki.mbalib.com/wiki/%E5%85%B6%E4%BB%96%E9%9D%9E%E6%B5%81%E5%8A%A8%E8%B4%9F%E5%80%BA) |
| non_current_liabilities       | 非流动负债合计： 指偿还期在一年或者超过一年的一个营业周期以上的债务。非流动负债的主要项目有长期借款和应付债券: [mba](https://wiki.mbalib.com/wiki/%E9%9D%9E%E6%B5%81%E5%8A%A8%E8%B4%9F%E5%80%BA%E5%90%88%E8%AE%A1) |
| borrowings_from_central_banks | 向中央银行借款                                |
| deposits_of_interbank         | 同业及其他金融机构存放款项                    |
| borrowings_capital            | 拆入资金                                      |
| derivative_financial_liabilities | 衍生金融负债                                  |
| buy_back_security_proceeds    | 卖出回购金融资产款                            |
| deposits                      | 吸收存款                                      |
| proxy_security_proceeds       | 代理买卖证券款                                |
| sub_issue_security_proceeds   | 代理承销证券款                                |
| security_deposits_received    | 存入保证金                                    |
| advance_insurance             | 预收保费                                      |
| comission_payable             | 应付手续费及佣金                              |
| reinsurance_payable           | 应付分保账款                                  |
| compensation_payable          | 应付赔付款                                    |
| policy_dividend_payable       | 应付保单红利                                  |
| deposits_from_interbank       | 吸收存款及同业存款                            |
| insurance_contract_reserve    | 保险合同准备金                                |
| insurer_deposit_investment    | 保户储金及投资款                              |
| uncertained_premium_reserve   | 未到期责任准备金                              |
| unclaimed_indemnity_reserve   | 未决赔款准备金                                |
| life_insurance_reserve        | 寿险责任准备金                                |
| health_insurance_reserve      | 长期健康险责任准备金                          |
| independent_account_liabilities | 独立账户负债                                  |
| other_liabilities             | 其他负债                                      |
| deferred_revenue              | 递延收益(长期负债)： 递延收益是指尚待确认的收入或收益，也可以说是暂时未确认的收益，它是权责发生制在收益确认上的运用: [mba](https://wiki.mbalib.com/wiki/%E9%80%92%E5%BB%B6%E6%94%B6%E7%9B%8A) |
| total_liabilities             | 负债合计： 指企业所承担的能以货币计量，将以资产或劳务偿还的债务: [mba](https://wiki.mbalib.com/wiki/%E8%B4%9F%E5%80%BA%E5%90%88%E8%AE%A1) |
| paid_in_capital               | 实收资本(或股本)： 指企业的投资者按照企业章程或合同、协议的约定，实际投入企业的资本: [mba](https://wiki.mbalib.com/wiki/%E5%AE%9E%E6%94%B6%E8%B5%84%E6%9C%AC) |
| other_equity_instruments      | 其他权益工具                                  |
| equity_preferred_stock        | 权益部分的优先股                              |
| perpetual_equity_debt         | 永续债（其他权益工具）                        |
| capital_reserve               | 资本公积金： 企业收到的投资者的超出其在企业注册资本所占份额，以及直接计入所有者权益的利得和损失等: [mba](https://wiki.mbalib.com/wiki/%E8%B5%84%E6%9C%AC%E5%85%AC%E7%A7%AF%E9%87%91)【该数据来自旧会计准则】 |
| surplus_reserve               | 盈余公积： 指企业从税后利润中提取形成的、存留于企业内部、具有特定用途的收益积累: [others](https://www.baidu.com/s?wd=%E7%9B%88%E4%BD%99%E5%85%AC%E7%A7%AF&ie=utf-8) |
| undistributed_profit          | 未分配利润： 未分配利润是企业未作分配的利润。它在以后年度可继续进行分配，在未进行分配之前，属于所有者权益的组成部分: [others](https://www.baidu.com/s?wd=%E6%9C%AA%E5%88%86%E9%85%8D%E5%88%A9%E6%B6%A6&ie=utf-8) |
| treasury_stock                | 减:库存股                                     |
| equity_parent_company         | 归属于母公司所有者权益合计： 母公司股东权益反映的是母公司所持股份部分的所有者权益数: [others](https://www.baidu.com/s?wd=%E5%BD%92%E5%B1%9E%E6%AF%8D%E5%85%AC%E5%8F%B8%E8%82%A1%E4%B8%9C%E6%9D%83%E7%9B%8A&ie=utf-8) |
| total_equity                  | 股东权益合计： 所有者权益合计是指企业投资人对企业净资产的所有权: [others](https://www.baidu.com/s?wd=%E8%82%A1%E4%B8%9C%E6%9D%83%E7%9B%8A%E5%90%88%E8%AE%A1&ie=utf-8) |
| general_reserve               | 一般风险准备                                  |
| trade_risk_allowances         | 交易风险准备                                  |
| foreign_currency_converted_difference | 外币报表折算差额                              |
| uncertained_impairment_losses | 未确认投资损失                                |
| other_reserves                | 其他储备(公允价值变动储备)                    |
| specific_reserve              | 专项储备                                      |
| minority_interest             | 少数股东权益： 少数股东损益是一个流量概念，是指公司合并报表的子公司其它非控股股东享有的损益: [mba](https://wiki.mbalib.com/wiki/%E5%B0%91%E6%95%B0%E8%82%A1%E4%B8%9C%E6%8D%9F%E7%9B%8A) |
| total_equity_and_liabilities  | 负债和股东权益总计                            |
### 现金流量表

可点击下载[现金流量表](hhttps://assets.ricequant.com/vendor/rqdata/%E7%8E%B0%E9%87%91%E6%B5%81%E9%87%8F%E8%A1%A8.xlsx)

| 字段                              | 释义、备注                                    |
| :-------------------------------- | :-------------------------------------------- |
| cash_received_from_sales_of_goods | 销售商品、提供劳务收到的现金： 公司销售商品、提供劳务实际收到的现金: [investopedia](https://www.investopedia.com/terms/c/cashfromsales.asp) |
| refunds_of_taxes                  | 收到的税费返还： 公司按规定收到的增值税、所得税等税费返还额: [mba](https://wiki.mbalib.com/wiki/%E6%94%B6%E5%88%B0%E7%9A%84%E7%A8%8E%E8%B4%B9%E8%BF%94%E8%BF%98) |
| net_deposit_increase              | 客户存款和同业存放款项净增加额                |
| net_increase_from_central_bank    | 向中央银行借款净增加额                        |
| net_increase_from_other_financial_institutions | 向其他金融机构拆入资金净增加额                |
| draw_back_canceled_loans          | 收回已核销贷款                                |
| cash_received_from_interests_and_commissions | 收取利息、手续费及佣金的现金                  |
| net_increase_from_disposing_financial_assets | 处置交易性金融资产净增加额                    |
| net_increase_from_repurchasing_business | 回购业务资金净增加额                          |
| cash_received_from_original_insurance | 收到原保险合同保费取得的现金                  |
| cash_received_from_reinsurance    | 收到再保业务现金净额                          |
| net_increase_from_insurer_deposit_investment | 保户储金及投资款净增加额                      |
| net_increase_from_financial_institutions | 拆入资金净增加额                              |
| cash_received_from_proxy_security | 代理买卖证券收到的现金净额                    |
| cash_received_from_sub_issue_security | 代理承销证券收到的现金净额                    |
| cash_from_other_operating_activities | 收到其它与经营活动有关的现金：公司除了上述各项目外，收到的其他与经营活动有关的现金，如捐赠现金收入、罚款收入、流动资产损失中由个人赔偿的现金收入等: [mba](https://wiki.mbalib.com/wiki/%E6%94%B6%E5%88%B0%E5%85%B6%E4%BB%96%E4%B8%8E%E7%BB%8F%E8%90%A5%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_from_operating_activities    | 经营活动现金流入小计                          |
| cash_paid_for_goods_and_services  | 购买商品、接受劳务支付的现金： 公司购买商品、接受劳务实际支付的现金: [investopedia](https://www.investopedia.com/terms/c/cashpaidforgoods.asp) |
| assets_depreciation_reserves      | 资产减值准备                                  |
| exchange_rate_change_effect       | 汇率变动对现金及现金等价物的影响              |
| other_effecting_cash_equivalent_items | 影响现金及现金等价物的其他科目                |
| cash_equivalent_increase          | 现金及现金等价物净增加额（来源现金流量表主表） |
| begin_period_cash_equivalent      | 加:期初现金及现金等价物余额                   |
| end_period_cash_equivalent        | 期末现金及现金等价物余额                      |
| cash_paid_for_employee            | 支付给职工以及为职工支付的现金： 公司实际支付给职工，以及为职工支付的现金，包括本期实际支付给职工的工资、奖金、各种津贴和补贴等: [mba](https://wiki.mbalib.com/wiki/%E6%94%AF%E4%BB%98%E7%BB%99%E8%81%8C%E5%B7%A5%E5%8F%8A%E4%B8%BA%E8%81%8C%E5%B7%A5%E6%94%AF%E4%BB%98%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_for_taxes               | 支付的各项税费： 反映企业按规定支付的各种税费，包括本期发生并支付的税费，以及本期支付以前各期发生的税费和预交的税金等: [mba](https://wiki.mbalib.com/wiki/%E6%94%AF%E4%BB%98%E7%9A%84%E5%90%84%E9%A1%B9%E7%A8%8E%E8%B4%B9) |
| net_increase_from_loans_and_advances | 客户贷款及垫款净增加额                        |
| net_increase_from_central_bank_and_banks | 存放中央银行和同业款项净增加额                |
| net_increase_from_lending_capital | 拆出资金净增加额                              |
| cash_paid_for_comissions          | 支付手续费及佣金的现金                        |
| cash_paid_for_orignal_insurance   | 支付原保险合同赔付款项的现金                  |
| cash_paid_for_reinsurance         | 支付再保业务现金净额                          |
| cash_paid_for_policy_dividends    | 支付保单红利的现金                            |
| net_increase_from_trading_financial_assets | 为交易目的而持有的金融资产净增加额            |
| net_increase_from_operating_buy_back | 返售业务资金净增加额(经营)                    |
| cash_paid_for_other_operation_activities | 支付其他与经营活动有关的现金： 反映企业支付的其他与经营活动有关的现金支出，如罚款支出、支付的差旅费、业务招待费的现金支出、支付的保险费等: [mba](https://wiki.mbalib.com/wiki/%E6%94%AF%E4%BB%98%E5%85%B6%E4%BB%96%E4%B8%8E%E7%BB%8F%E8%90%A5%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_for_operation_activities | 经营活动现金流出小计                          |
| cash_flow_from_operating_activities | 经营活动产生的现金流量净额： 指企业投资活动和筹资活动以外的所有交易活动和事项的现金流入和流出量: [mba](https://wiki.mbalib.com/wiki/%E7%BB%8F%E8%90%A5%E6%B4%BB%E5%8A%A8%E4%BA%A7%E7%94%9F%E7%9A%84%E7%8E%B0%E9%87%91%E6%B5%81%E9%87%8F%E5%87%80%E9%A2%9D) |
| cash_received_from_disposal_of_investment | 收回投资收到的现金                            |
| cash_received_from_investment     | 取得投资收益收到的现金                        |
| cash_received_from_disposal_of_asset | 处置固定资产、无形资产和其他长期资产收回的现金净额： 公司处置固定资产、无形资产和其他长期资产收回的现金: [investopedia](https://www.investopedia.com/terms/c/cashfromassetdisposal.asp) |
| cash_received_from_other_investment_activities | 收到其他与投资活动有关的现金： 公司除了上述各项以外，收到的其他与投资活动有关的现金: [mba](https://wiki.mbalib.com/wiki/%E6%94%B6%E5%88%B0%E5%85%B6%E4%BB%96%E4%B8%8E%E6%8A%95%E8%B5%84%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_received_from_investment_activities | 投资活动现金流入小计                          |
| cash_paid_for_asset               | 购建固定资产、无形资产和其他长期资产所支付的现金: [wikipedia](https://en.wikipedia.org/wiki/Cash_paid_for_assets) |
| cash_paid_to_acquire_investment   | 投资支付的现金： 反映企业进行权益性投资和债权性投资支付的现金，包括企业取得的除现金等价物以外的股票投资和债券投资等支付的现金等: [mba](https://wiki.mbalib.com/wiki/%E6%8A%95%E8%B5%84%E6%94%AF%E4%BB%98%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_for_other_investment_activities | 支付其他与投资活动有关的现金： 反映企业除了上述各项以外，支付的其他与投资活动有关的现金流出: [mba](https://wiki.mbalib.com/wiki/%E6%94%AF%E4%BB%98%E5%85%B6%E4%BB%96%E4%B8%8E%E6%8A%95%E8%B5%84%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_for_investment_activities | 投资活动产生的现金流出小计                    |
| cash_flow_from_investing_activities | 投资活动产生的现金流量净额：指企业长期资产的购建和对外投资活动（不包括现金等价物范围的投资）的现金流入和流出量: [mba](https://wiki.mbalib.com/wiki/%E6%8A%95%E8%B5%84%E6%B4%BB%E5%8A%A8%E4%BA%A7%E7%94%9F%E7%9A%84%E7%8E%B0%E9%87%91%E6%B5%81%E9%87%8F%E5%87%80%E9%A2%9D) |
| cash_received_from_investors      | 吸收投资收到的现金：反映企业收到的投资者投入现金，包括以发行股票、债券等方式筹集的资金实际收到的净额: [mba](https://wiki.mbalib.com/wiki/%E5%90%B8%E6%94%B6%E6%8A%95%E8%B5%84%E6%94%B6%E5%88%B0%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_received_from_minority_invest_subsidiaries | 其中:子公司吸收少数股东投资收到的现金         |
| cash_received_from_issuing_security | 发行债券收到的现金                            |
| cash_received_from_financial_institution_borrows | 取得借款收到的现金： 公司向银行或其他金融机构等借入的资金: [mba](https://wiki.mbalib.com/wiki/%E5%8F%96%E5%BE%97%E5%80%9F%E6%AC%BE%E6%94%B6%E5%88%B0%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_received_from_issuing_equity_instruments | 发行其他权益工具收到的现金                    |
| net_increase_from__financing_buy_back | 回购业务资金净增加额(筹资)                    |
| cash_received_from_other_financing_activities | 收到其他与筹资活动有关的现金：反映企业收到的其他与筹资活动有关的现金流入，如接受现金捐赠等: [mba](https://wiki.mbalib.com/wiki/%E6%94%B6%E5%88%B0%E5%85%B6%E4%BB%96%E4%B8%8E%E7%AD%B9%E8%B5%84%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_received_from_financing_activities | 筹资活动现金流入小计                          |
| cash_paid_for_debt                | 偿还债务支付的现金：公司以现金偿还债务的本金，包括偿还银行或其他金融机构等的借款本金、偿还债券本金等: [mba](https://wiki.mbalib.com/wiki/%E5%81%BF%E8%BF%98%E5%80%BA%E5%8A%A1%E6%94%AF%E4%BB%98%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_for_dividend_and_interest | 分配股利、利润或偿付利息支付的现金：反映企业实际支付给投资人的利润以及支付的借款利息、债券利息等: [mba](https://wiki.mbalib.com/wiki/%E5%88%86%E9%85%8D%E8%82%A1%E5%88%A9%E3%80%81%E5%88%A9%E6%B6%A6%E6%88%96%E5%81%BF%E4%BB%98%E5%88%A9%E6%81%AF%E6%94%AF%E4%BB%98%E7%9A%84%E7%8E%B0%E9%87%91) |
| dividends_paid_to_minority_by_subsidiaries | 其中:子公司支付给少数股东的股利、利润或偿付的利息 |
| cash_paid_for_other_financing_activities | 支付其他与筹资活动有关的现金：反映企业支付的其他与筹资活动有关的现金流出: [mba](https://wiki.mbalib.com/wiki/%E6%94%AF%E4%BB%98%E5%85%B6%E4%BB%96%E4%B8%8E%E7%AD%B9%E8%B5%84%E6%B4%BB%E5%8A%A8%E6%9C%89%E5%85%B3%E7%9A%84%E7%8E%B0%E9%87%91) |
| cash_paid_to_financing_activities | 筹资活动现金流出小计                          |
| cash_flow_from_financing_activities | 筹资活动产生的现金流量净额：指企业接受投资和借入资金导致的现金流入和流出量: [mba](https://wiki.mbalib.com/wiki/%E7%AD%B9%E8%B5%84%E6%B4%BB%E5%8A%A8%E4%BA%A7%E7%94%9F%E7%9A%84%E7%8E%B0%E9%87%91%E6%B5%81%E9%87%8F%E5%87%80%E9%A2%9D) |
| net_cash_deal_from_sub            | 处置子公司及其他营业单位收到的现金净额        |
| net_cash_payment_from_sub         | 取得子公司及其他营业单位支付的现金净额        |
| net_increase_in_pledge_loans      | 质押贷款净增加额                              |
| net_increase_from_investing_buy_back | 返售业务资金净增加额(投资)                    |
| net_inc_cash_and_equivalents      | 现金及现金等价物净增加额（来源为财务附注）    |
| fixed_asset_depreciation          | 固定资产折旧                                  |
| deferred_expense_amortization     | 长期待摊费用摊销                              |
| intangible_asset_amortization     | 无形资产摊销                                  |

## 衍生财务指标

可以使用`get_factor`函数调用,`get_fundamentals`,`get_pit_financials`,`get_financials`不支持基于LF、LYR、TTM 三种财务数据处理逻辑的指标

| 衍生财务指标 | 说明                                  |
| :----------- | :------------------------------------ |
| 估值衍生指标 | 每天随行情变化而变化，反映上市公司估值情况（例如市盈率） |
| 经营衍生指标 | 利润表衍生的指标，反映上市公司经营情况（例如每股收益） |
| 现金流衍生指标 | 现金流量表衍生的指标，反映上市公司现金流情况（例如每股现金流） |
| 财务衍生指标 | 资产负债表衍生的指标，反映上市公司权益/负债情况（例如带息债务） |
| 成长性衍生指标 | 反映上市公司经营/财务情况同比变化（例如每股净资产同比增长率） |

**衍生指标计算逻辑**

若衍生指标定义中只涉及资产负债表字段，则提供基于最近一期财报（LF）、基于最近一期年报（LYR）、以及滚动12 个月（TTM）三种财务数据处理逻辑 若衍生指标定义中涉及现金流量表或利润表字段，则提供LYR 和TTM 两种处理逻辑

| 处理逻辑            | 优点                                     | 缺点                                     |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| LF, Last File       | 时效性最好                               | 某一期报财报数据存在较大的偶然性，且上市公司季报/中报一般没有审计要求，数据可靠性相对较差 |
| LYR, Last Year Ratio | 上市公司年报有审计要求，数据可靠性最高   | 时效性最差，例如在2017 年11 月，上市公司实际财务和经营情况可能已和2016 年年报数据有较大差异 |
| TTM, Trailing Twelve Months | 时效性较好，滚动4 个报告期计算，可避免某一期财报数据的偶然性 | 时效性不如LF 处理；可靠性不如LYR 处理    |

衍生指标命名规则估值、经营、现金流、财务和成长衍生指标，经LF、LYR、TTM 处理后的命名规则和三大基础会计科目经相同方式处理后的命名规则存在区别

| 类别                     | 命名规则                                     | 范例                                     |
| :----------------------- | :------------------------------------------- | :--------------------------------------- |
| 三大报表基础会计科目     | 以`_mrq_n` ,`_lyr_n`,`_ttm_n` 等后缀方式在原有基础字段上进行拓展，带数字尾缀n | 例如：`revenue_lyr_0` 代表最近一期年报的净利润数据 |
| 估值、经营、现金流、财务和成长衍生指标 | 以`_lf` ,`_lyr`,`_ttm` 等后缀方式在原有字段上进行拓展，不带数字尾缀n | 例如：`pe_ratio_lyr` 代表经LYR 处理后的市盈率 |


### 估值有关指标

为方便阅读，可点这里下载[Excel 版本的指标列表](https://assets.ricequant.com/vendor/rqdata/%E8%A1%8D%E7%94%9F%E8%B4%A2%E5%8A%A1%E6%8C%87%E6%A0%87.xlsx)

| 字段                          | 中文名                  | 说明                                         | 公式                                         |
| :---------------------------- | :---------------------- | :------------------------------------------- | :------------------------------------------- |
| pe_ratio_lyr                  | 市盈率lyr               | 总市值/ 归属母公司净利润lyr                  | `market_cap_3 / net_profit_parent_company_lyr_0` |
| pe_ratio_ttm                  | 市盈率ttm               | 总市值/ 归属母公司净利润ttm                  | `market_cap_3 / net_profit_parent_company_ttm_0` |
| ep_ratio_lyr                  | 盈市率lyr               | 归属母公司净利润lyr / 总市值                 | `net_profit_parent_company_lyr_0 / market_cap_3` |
| ep_ratio_ttm                  | 盈市率ttm               | 连续四季度报表披露归属母公司净利润之和/ 当前股票总市值 | `net_profit_parent_company_ttm_0 / market_cap_3` |
| pcf_ratio_total_lyr           | 市现率_总现金流lyr      | 总市值/（经营活动产生的现金流量净额lyr + 投资活动产生的现金流量净额lyr + 筹资活动产生的现金流量净额lyr） | `market_cap_3 / (cash_flow_from_operating_activities_lyr_0 + cash_flow_from_investing_activities_lyr_0 + cash_flow_from_financing_activities_lyr_0)` |
| pcf_ratio_total_ttm           | 市现率_总现金流ttm      | 总市值/（经营活动产生的现金流量净额ttm + 投资活动产生的现金流量净额ttm + 筹资活动产生的现金流量净额ttm) | `market_cap_3 / (cash_flow_from_operating_activities_ttm_0 + cash_flow_from_investing_activities_ttm_0 + cash_flow_from_financing_activities_ttm_0)` |
| pcf_ratio_lyr                 | 市现率_经营lyr          | 总市值/ 经营活动产生的现金流量净额lyr        | `market_cap_3 / cash_flow_from_operating_activities_lyr_0` |
| pcf_ratio_ttm                 | 市现率_经营ttm          | 总市值/ 经营活动产生的现金流量净额ttm        | `market_cap_3 / cash_flow_from_operating_activities_ttm_0` |
| cfp_ratio_lyr                 | 现金收益率lyr           | 现金收益率=（经营活动产生的现金流量净额lyr + 投资活动产生的现金流量净额lyr + 筹资活动产生的现金流量净额lyr）/ 总市值 | `(cash_flow_from_operating_activities_lyr_0 + cash_flow_from_investing_activities_lyr_0 + cash_flow_from_financing_activities_lyr_0) / market_cap_3` |
| cfp_ratio_ttm                 | 现金收益率ttm           | 现金收益率= (经营活动产生的现金流量净额ttm + 投资活动产生的现金流量净额ttm + 筹资活动产生的现金流量净额ttm）/ 总市值 | `(cash_flow_from_operating_activities_ttm_0 + cash_flow_from_investing_activities_ttm_0 + cash_flow_from_financing_activities_ttm_0) / market_cap_3` |
| pb_ratio_lyr                  | 市净率lyr               | 当前股票总市值/ 归属母公司股东权益合计lyr    | `market_cap_3 / equity_parent_company_lyr_0` |
| pb_ratio_ttm                  | 市净率ttm               | 当前股票总市值/ 归属母公司股东权益合计ttm    | `market_cap_3 / equity_parent_company_ttm_0` |
| pb_ratio_lf                   | 市净率lf                | 当前股票总市值/ 归属母公司股东权益合计mrq    | `market_cap_3 / equity_parent_company_mrq_0` |
| pb_ratio_1_lyr                | 市净率（股东权益剔除其他权益工具) lyr | 当前股票总市值 / (归属母公司股东权益合计-其他权益工具) lyr | `market_cap_3 /( equity_parent_company_lyr_0-other_equity_instruments_lyr_0)` |
| pb_ratio_1_ttm                | 市净率( 股东权益剔除其他权益工具）ttm | 当前股票总市值 / （归属母公司股东权益合计-其他权益工具）ttm | `market_cap_3 / (equity_parent_company_ttm_0-other_equity_instruments_ttm_0)` |
| pb_ratio_1_lf                 | 市净率（股东权益剔除其他权益工具） lf | 当前股票总市值 / （归属母公司股东权益合计-其他权益工具） mrq | `market_cap_3 /(equity_parent_company_mrq_0-other_equity_instruments_mrq_0)` |
| book_to_market_ratio_lyr      | 账面市值比lyr           | 归属母公司股东权益合计lyr / 总市值           | `equity_parent_company_lyr_0 / market_cap_3` |
| book_to_market_ratio_ttm      | 账面市值比ttm           | 归属母公司股东权益合计ttm / 总市值           | `equity_parent_company_ttm_0 / market_cap_3` |
| book_to_market_ratio_lf       | 账面市值比lf            | 归属母公司股东权益合计mrq / 总市值           | `equity_parent_company_mrq_0 / market_cap_3` |
| dividend_yield_ttm            | 股息率ttm               | 连续四季度报表公布股利之和/ 公司当前股票总市值 | `dividend_per_share / close_price`           |
| peg_ratio_lyr                 | PEG 值lyr               | 市盈率lyr / 公司过去一年归属母公司净利润增长率平均值*100 lyr | `pe_ratio_lyr / (100*(net_profit_parent_company_lyr_0 - net_profit_parent_company_lyr_1) / net_profit_parent_company_lyr_1)` |
| peg_ratio_ttm                 | PEG 值ttm               | 市盈率ttm / 公司过去一年归属母公司净利润增长率平均值*100 ttm | `pe_ratio_ttm / (100*(net_profit_parent_company_ttm_0 - net_profit_parent_company_ttm_4) / net_profit_parent_company_ttm_4)` |
| ps_ratio_lyr                  | 市销率lyr               | 总市值/ 营利收入lyr                          | `market_cap_3 / operating_revenue_lyr_0`     |
| ps_ratio_ttm                  | 市销率ttm               | 总市值/ 营利收入ttm                          | `market_cap_3 / operating_revenue_ttm_0`     |
| sp_ratio_lyr                  | 销售收益率lyr           | 营利收入lyr / 总市值                         | `operating_revenue_lyr_0 / market_cap_3`     |
| sp_ratio_ttm                  | 销售收益率ttm           | 营利收入ttm / 总市值                         | `operating_revenue_ttm_0 / market_cap_3`     |
| market_cap                    | 总市值1                 | 总市值= 总股本* A 股未复权收盘价             | `total * close_price`                        |
| market_cap_2                  | 流通股总市值            | 流通股总市值= 流通股本* A 股未复权收盘价     | `circulation_a * close_price`                |
| market_cap_3                  | 总市值                  | 总市值= 总股本* A 股未复权收盘价此处采用了PIT 处理方式，即公告公布之后才对股本数据进行调节，而不对截止日期（例如，半年报截止日期为06-30）至公布日期之间的数据进行覆盖更新 | `total * close_price`                        |
| a_share_market_val_3          | A 股市值                | A 股市值= A 股股本x A 股未复权收盘价此处股本采用PIT 处理方式，股本处理方式同market_cap_3 说明部分 | `total_a * close_price`                      |
| a_share_market_val_in_circulation | 流通A 股市值            | 流通A 股市值= 流通A 股* A 股未复权收盘价此处股本采用PIT 处理方式，股本处理方式同market_cap_3 说明部分 | `circulation_a * close_price`                |
| ev_lyr                        | 企业价值lyr             | 总市值+ 负债合计lyr                          | `market_cap_3 + total_liabilities_lyr_0`     |
| ev_ttm                        | 企业价值ttm             | 总市值+ 负债合计ttm                          | `market_cap_3 + total_liabilities_ttm_0`     |
| ev_lf                         | 企业价值lf              | 总市值+ 负债合计mrq                          | `market_cap_3 + total_liabilities_mrq_0`     |
| ev_no_cash_lyr                | 企业价值(不含货币资金)lyr | 总市值+ 负债合计lyr - 货币资金lyr            | `market_cap_3 + total_liabilities_lyr_0 - cash_equivalent_lyr_0` |
| ev_no_cash_ttm                | 企业价值(不含货币资金)ttm | 总市值+ 负债合计ttm - 货币资金ttm            | `market_cap_3 + total_liabilities_ttm_0 - cash_equivalent_ttm_0` |
| ev_no_cash_lf                 | 企业价值(不含货币资金)lf  | 总市值+ 负债合计mrq - 货币资金mrq            | `market_cap_3 + total_liabilities_mrq_0 - cash_equivalent_mrq_0` |
| ev_to_ebitda_lyr              | 企业倍数lyr             | 企业价值lyr / 息税折旧摊销前利润lyr          | `ev_lyr / ebitda_lyr`                        |
| ev_to_ebitda_ttm              | 企业倍数ttm             | 企业价值ttm / 息税折旧摊销前利润ttm          | `ev_ttm / ebitda_ttm`                        |
| ev_no_cash_to_ebit_lyr        | 企业倍数(不含货币资金)lyr | 企业价值lyr / 息税折旧摊销前利润lyr          | `ev_no_cash_lyr / ebitda_lyr`                |
| ev_no_cash_to_ebit_ttm        | 企业倍数(不含货币资金)ttm | 企业价值ttm / 息税折旧摊销前利润ttm          | `ev_no_cash_ttm / ebitda_ttm`                |

### 经营衍生指标表

为方便阅读，可点这里下载[Excel 版本的指标列表](https://assets.ricequant.com/vendor/rqdata/%E8%A1%8D%E7%94%9F%E8%B4%A2%E5%8A%A1%E6%8C%87%E6%A0%87.xlsx)

| 字段                               | 中文名                           | 说明                                         | 公式                                         |
| :--------------------------------- | :------------------------------- | :------------------------------------------- | :------------------------------------------- |
| diluted_earnings_per_share_lyr     | 摊薄每股收益lyr                  | 归属母公司净利润lyr / 该报告期末普通股总股本 | `net_profit_parent_company_lyr_0/ total_shares` |
| diluted_earnings_per_share_ttm     | 摊薄每股收益ttm                  | 归属母公司净利润ttm / 该报告期末普通股总股本 | `net_profit_parent_company_ttm_0 / total_shares` |
| adjusted_earnings_per_share_lyr    | 基本每股收益_扣除lyr             | 扣除非经常性损益的净利润lyr / 普通股加权股本lyr | `adjusted_net_profit_lyr_0 / weighted_common_stock_lyr` |
| adjusted_earnings_per_share_ttm    | 基本每股收益_扣除ttm             | 扣除非经常性损益的净利润ttm / 普通股加权股本ttm | `adjusted_net_profit_ttm_0 / weighted_common_stock_ttm` |
| adjusted_fully_diluted_earnings_per_share_lyr | 稀释每股收益_扣除lyr             | 扣除非经常损益的净利润lyr / 稀释普通股lyr    | `adjusted_net_profit_lyr_0 / diluted_common_stock_lyr` |
| adjusted_fully_diluted_earnings_per_share_ttm | 稀释每股收益_扣除ttm             | 扣除非经常损益的净利润ttm / 稀释普通股ttm    | `adjusted_net_profit_ttm_0 / diluted_common_stock_ttm` |
| inc_adjusted_net_profit_lyr        | 扣除非经常损益归属母公司股东的净利润同比增长率lyr | 扣除非经营性损益归属母公司股东净利润lyr / 去年扣除非经营性损益归属母公司股东净利润lyr - 1 | `(net_profit_deduct_non_recurring_pnl_lyr_0 / net_profit_deduct_non_recurring_pnl_lyr_1) -1` |
| inc_adjusted_net_profit_ttm        | 扣除非经常损益归属母公司股东的净利润同比增长率ttm | 扣除非经营性损益归属母公司股东净利润ttm / 去年扣除非经营性损益归属母公司股东净利润ttm - 1 | `(net_profit_deduct_non_recurring_pnl_ttm_0 / net_profit_deduct_non_recurring_pnl_ttm_4) -1` |
| weighted_common_stock_lyr          | 普通股加权股本lyr                | 归属于母公司所有者的净利润lyr / 基本每股收益lyr | `net_profit_parent_company_lyr_0 / basic_earnings_per_share_lyr_0` |
| weighted_common_stock_ttm          | 普通股加权股本ttm                | 归属于母公司所有者的净利润ttm / 基本每股收益ttm | `net_profit_parent_company_ttm_0 / basic_earnings_per_share_ttm_0` |
| diluted_common_stock_lyr           | 稀释普通股lyr                    | 归属于母公司所有者的净利润lyr / 稀释每股收益lyr | `net_profit_parent_company_lyr_0/fully_diluted_earnings_per_share_lyr_0` |
| diluted_common_stock_ttm           | 稀释普通股ttm                    | 归属于母公司所有者的净利润ttm / 稀释每股收益ttm | `net_profit_parent_company_ttm_0/fully_diluted_earnings_per_share_ttm_0` |
| operating_total_revenue_per_share_lyr | 每股营业总收入lyr                | 营业总收入lyr / 总股本                 | `revenue_lyr_0 / total_shares`               |
| operating_total_revenue_per_share_ttm | 每股营业总收入ttm                | 营业总收入ttm / 总股本                 | `revenue_ttm_0 / total_shares`               |
| operating_revenue_per_share_lyr    | 每股营业收入lyr                  | 营业收入lyr / 总股本                   | `operating_revenue_lyr_0 /total_shares`      |
| operating_revenue_per_share_ttm    | 每股营业收入ttm                  | 营业收入ttm / 总股本                   | `operating_revenue_ttm_0 /total_shares`      |
| ebit_lyr                           | 息税前利润lyr                    | 利润总额lyr + 利息支出(财务费用) lyr - 利息收入(财务费用) lyr | `profit_before_tax_lyr_0 + financing_interest_expense_lyr_0 - financing_interest_income_lyr_0` |
| ebit_ttm                           | 息税前利润ttm                    | 利润总额ttm + 利息支出(财务费用) ttm - 利息收入(财务费用) ttm | `profit_before_tax_ttm_0 + financing_interest_expense_ttm_0 - financing_interest_income_ttm_0` |
| ebitda_lyr                         | 息税折旧摊销前利润lyr            | 息税前利润lyr ＋ 固定资产折旧lyr ＋ 无形资产摊销lyr ＋ 长期待摊费用摊销lyr | `(ebit_lyr + fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_lyr_0 + deferred_expense_amort_lyr_0)` |
| ebitda_ttm                         | 息税折旧摊销前利润ttm            | 息税前利润ttm + 固定资产折旧ttm ＋ 无形资产摊销ttm ＋ 长期待摊费用摊销ttm | `(ebit_ttm + fixed_asset_depreciation_ttm_0 + intangible_asset_amortization_ttm_0 + deferred_expense_amort_ttm_0)` |
| ebit_per_share_lyr                 | 每股息税前利润lyr                | 息税前利润lyr / 总股本                 | `ebit_lyr / total_shares`                    |
| ebit_per_share_ttm                 | 每股息税前利润ttm                | 息税前利润ttm / 总股本                 | `ebit_ttm / total_shares`                    |
| return_on_equity_lyr               | 净资产收益率lyr                  | 归属母公司净利润lyr * 2 /（归属母公司股东权益合计lyr + 上期报表披露归属母公司股东权益合计lyr） | `net_profit_parent_company_lyr_0 * 2 / (equity_parent_company_lyr_0 + equity_parent_company_lyr_1)` |
| return_on_equity_ttm               | 净资产收益率ttm                  | 归属母公司净利润ttm * 2 / (归属母公司股东权益合计ttm + 上期报表披露归属母公司股东权益合计ttm) | `net_profit_parent_company_ttm_0 * 2 / (equity_parent_company_ttm_0 + equity_parent_company_ttm_1)` |
| return_on_equity_diluted_lyr       | 摊薄净资产收益率lyr              | 归属母公司净利润lyr / 归属母公司股东权益合计lyr | `net_profit_parent_company_lyr_0 / equity_parent_company_lyr_0` |
| return_on_equity_diluted_ttm       | 摊薄净资产收益率ttm              | 归属母公司净利润ttm / 归属母公司股东权益合计ttm | `net_profit_parent_company_ttm_0 / equity_parent_company_ttm_0` |
| adjusted_return_on_equity_lyr      | 净资产收益率_扣除lyr             | 扣除非经常性损益后的归属母公司净利润lyr * 2 /（归属母公司股东权益合计lyr + 上期报表披露的归属母公司股东权益合计lyr） | `(net_profit_parent_company_lyr_0 - non_recurring_pnl_lyr_0)* 2 / (equity_parent_company_lyr_0 + equity_parent_company_lyr_1)` |
| adjusted_return_on_equity_ttm      | 净资产收益率_扣除ttm             | 扣除非经常性损益后的归属母公司净利润ttm * 2/ （归属母公司股东权益合计ttm + 上期报表披露的归属母公司股东权益合计ttm） | `(net_profit_parent_company_ttm_0 - non_recurring_pnl_ttm_0)* 2 / (equity_parent_company_ttm_0+equity_parent_company_ttm_1))` |
| adjusted_return_on_equity_diluted_lyr | 摊薄净资产收益率_扣除lyr         | 扣除非经常性损益后归属母公司的净利润lyr / 归属母公司的股东权益合计lyr | `(net_profit_parent_company_lyr_0 - non_recurring_pnl_lyr_0) / equity_parent_company_lyr_0` |
| adjusted_return_on_equity_diluted_ttm | 摊薄净资产收益率_扣除ttm         | 扣除非经常性损益后归属母公司的净利润ttm / 归属母公司的股东权益合计ttm | `(net_profit_parent_company_ttm_0 - non_recurring_pnl_ttm_0) / equity_parent_company_ttm_0` |
| return_on_asset_lyr                | 总资产报酬率lyr                  | 息税前利润lyr / 总资产lyr              | `ebit_lyr / total_assets_lyr_0`              |
| return_on_asset_ttm                | 总资产报酬率ttm                  | 息税前利润ttm / 总资产ttm              | `ebit_ttm / total_assets_ttm_0`              |
| return_on_asset_net_profit_lyr     | 总资产净利率lyr                  | 净利润lyr / 总资产lyr                  | `net_profit_lyr_0 / total_assets_lyr_0`      |
| return_on_asset_net_profit_ttm     | 总资产净利率ttm                  | 净利润ttm / 总资产ttm                  | `net_profit_ttm_0 / total_assets_ttm_0`      |
| return_on_invested_capital_lyr     | 投入资本回报率lyr                | (净利润lyr + 财务费用lyr）/（资产总计lyr - 流动负债lyr + 应付票据lyr + 短期借款lyr + 一年内到期的非流动负债lyr） | `(net_profit_lyr_0 + financing_expense_lyr_0) / (total_assets_lyr_0 - current_liabilities_lyr_0 + notes_payable_lyr_0 + short_term_loans_lyr_0 + non_current_liability_due_one_year_lyr_0)` |
| return_on_invested_capital_ttm     | 投入资本回报率ttm                | （净利润ttm + 财务费用ttm）/（资产总计ttm - 流动负债ttm + 应付票据ttm + 短期借款ttm + 一年内到期的非流动负债ttm） | `(net_profit_ttm_0 + financing_expense_ttm_0) / (total_assets_ttm_0 - current_liabilities_ttm_0 + notes_payable_ttm_0 + short_term_loans_ttm_0 + non_current_liability_due_one_year_ttm_0)` |
| net_profit_margin_lyr              | 销售净利率lyr                    | 净利润lyr / 营业收入lyr                | `net_profit_lyr_0 / operating_revenue_lyr_0` |
| net_profit_margin_ttm              | 销售净利率ttm                    | 净利润ttm / 营业收入ttm                | `net_profit_ttm_0 / operating_revenue_ttm_0` |
| gross_profit_margin_lyr            | 销售毛利率lyr                    | (营业收入lyr - 营业成本lyr）/ 营业收入lyr | `(operating_revenue_lyr_0 - cost_of_goods_sold_lyr_0) / operating_revenue_lyr_0` |
| gross_profit_margin_ttm            | 销售毛利率ttm                    | (营业收入ttm - 营业成本ttm）/ 营业收入ttm | `(operating_revenue_ttm_0 - cost_of_goods_sold_ttm_0) / operating_revenue_ttm_0` |
| cost_to_sales_lyr                  | 销售成本率lyr                    | 营业成本lyr / 营业收入lyr              | `cost_of_goods_sold_lyr_0 / operating_revenue_lyr_0` |
| cost_to_sales_ttm                  | 销售成本率ttm                    | 营业成本ttm / 营业收入ttm              | `cost_of_goods_sold_ttm_0 / operating_revenue_ttm_0` |
| net_profit_to_revenue_lyr          | 经营净利率lyr                    | 净利润lyr / 营业总收入lyr              | `net_profit_lyr_0 / revenue_lyr_0`           |
| net_profit_to_revenue_ttm          | 经营净利率ttm                    | 净利润ttm / 营业总收入ttm              | `net_profit_ttm_0 / revenue_ttm_0`           |
| profit_from_operation_to_revenue_lyr | 营业利润率lyr                    | 营业利润lyr / 营业总收入lyr            | `profit_from_operation_lyr_0 / revenue_lyr_0` |
| profit_from_operation_to_revenue_ttm | 营业利润率ttm                    | 营业利润ttm / 营业总收入ttm            | `profit_from_operation_ttm_0 / revenue_ttm_0` |
| ebit_to_revenue_lyr                | 税前收益率lyr                    | 息税前利润lyr / 营业总收入lyr          | `ebit_lyr / revenue_lyr_0`                   |
| ebit_to_revenue_ttm                | 税前收益率ttm                    | 息税前利润ttm / 营业总收入ttm          | `ebit_ttm / revenue_ttm_0`                   |
| expense_to_revenue_lyr             | 经营成本率lyr                    | 营业总成本lyr / 营业总收入lyr          | `total_expense_lyr_0 / revenue_lyr_0`        |
| expense_to_revenue_ttm             | 经营成本率ttm                    | 营业总成本ttm / 营业总收入ttm          | `total_expense_ttm_0 / revenue_ttm_0`        |
| operating_profit_to_profit_before_tax_lyr | 经营活动净收益与利润总额之比lyr  | (营业总收入lyr－营业总成本lyr) / 利润总额lyr | `(revenue_lyr_0 - total_expense_lyr_0) / profit_before_tax_lyr_0` |
| operating_profit_to_profit_before_tax_ttm | 经营活动净收益与利润总额之比ttm  | (营业总收入ttm－营业总成本ttm) / 利润总额ttm | `(revenue_ttm_0 - total_expense_ttm_0) / profit_before_tax_ttm_0` |
| investment_profit_to_profit_before_tax_lyr | 价值变动净收益与利润总额之比lyr  | (投资收益lyr + 公允价值变动净收益lyr + 兑汇损益lyr - 对联营合营公司的投资收益lyr) / 利润总额lyr | `（investment_income_lyr_0 + fair_value_change_income_lyr_0 + exchange_gains_or_losses_lyr_0 - invest_income_associates_lyr_0） / profit_before_tax_lyr_0` |
| investment_profit_to_profit_before_tax_ttm | 价值变动净收益与利润总额之比ttm  | (投资收益ttm + 公允价值变动净收益ttm + 兑汇损益ttm - 对联营合营公司的投资收益ttm) / 利润总额ttm | `（investment_income_ttm_0 + fair_value_change_income_ttm_0 + exchange_gains_or_losses_ttm_0 - invest_income_associates_ttm_0） / profit_before_tax_ttm_0` |
| non_operating_profit_to_profit_before_tax_lyr | 营业外收支净额与利润总额之比lyr  | 营业外收支净额lyr / 利润总额lyr      | `(non_operating_revenue_lyr_0 - non_operating_expense_lyr_0) / profit_before_tax_lyr_0` |
| non_operating_profit_to_profit_before_tax_ttm | 营业外收支净额与利润总额之比ttm  | 营业外收支净额ttm / 利润总额ttm      | `(non_operating_revenue_ttm_0 - non_operating_expense_ttm_0) / profit_before_tax_ttm_0` |
| income_tax_to_profit_before_tax_lyr | 所得税与利润总额之比lyr          | 所得税lyr / 利润总额lyr              | `income_tax_lyr_0 / profit_before_tax_lyr_0` |
| income_tax_to_profit_before_tax_ttm | 所得税与利润总额之比ttm          | 所得税ttm / 利润总额ttm              | `income_tax_ttm_0 / profit_before_tax_ttm_0` |
| adjusted_profit_to_total_profit_lyr | 扣除非经常损益后的净利润与净利润之比lyr | 扣除非经常性损益后的净利润lyr / 净利润lyr | `adjusted_net_profit_lyr_0 / net_profit_lyr_0` |
| adjusted_profit_to_total_profit_ttm | 扣除非经常损益后的净利润与净利润之比ttm | 扣除非经常性损益后的净利润ttm / 净利润ttm | `adjusted_net_profit_ttm_0 / net_profit_ttm_0` |
| ebitda_to_debt_lyr                 | 息税折旧摊销前利润/负债总计lyr   | 息税折旧摊销前利润lyr / 负债总计lyr  | `ebitda_lyr / total_liabilities_lyr_0`       |
| ebitda_to_debt_ttm                 | 息税折旧摊销前利润/负债总计ttm   | 息税折旧摊销前利润ttm / 负债总计ttm  | `ebitda_ttm / total_liabilities_ttm_0`       |
| account_payable_turnover_rate_lyr  | 应付账款周转率lyr                | 营业成本lyr / 最新年报披露应付账款lyr | `cost_of_goods_sold_lyr_0 / accts_payable_lyr_0` |
| account_payable_turnover_rate_ttm  | 应付账款周转率ttm                | 营业成本ttm /当期报表披露应付账款ttm | `cost_of_goods_sold_ttm_0 / accts_payable_ttm_0` |
| account_payable_turnover_days_lyr  | 应付账款周转天数lyr              | 360 / 应付账款周转率lyr              | `360 / account_payable_turnover_rate_lyr`    |
| account_payable_turnover_days_ttm  | 应付账款周转天数ttm              | 360 / 应付账款周转率ttm              | `360 / account_payable_turnover_rate_ttm`    |
| account_receivable_turnover_rate_lyr | 应收账款周转率lyr                | 营业收入lyr / 最新年报披露应收账款净额lyr | `operating_revenue_lyr_0 / net_accts_receivable_lyr_0` |
| account_receivable_turnover_rate_ttm | 应收账款周转率ttm                | 营业收入ttm / 当期报表披露应收账款净额ttm | `operating_revenue_ttm_0 / net_accts_receivable_ttm_0` |
| account_receivable_turnover_days_lyr | 应收账款周转天数lyr              | 360 / 应收账款周转率lyr              | `360 / account_receivable_turnover_rate_lyr` |
| account_receivable_turnover_days_ttm | 应收账款周转天数ttm              | 360 / 应收账款周转率ttm              | `360 / account_receivable_turnover_rate_ttm` |
| inventory_turnover_lyr             | 存货周转率lyr                    | 营业成本lyr / 本期年报披露存货lyr    | `cost_of_goods_sold_lyr_0 / inventory_lyr_0` |
| inventory_turnover_ttm             | 存货周转率ttm                    | 营业成本ttm / 当期报表披露存货ttm    | `cost_of_goods_sold_ttm_0 / inventory_ttm_0` |
| current_asset_turnover_lyr         | 流动资产周转率lyr                | 营业收入lyr /最新年报披露流动资产总计lyr | `operating_revenue_lyr_0 / current_assets_lyr_0` |
| current_asset_turnover_ttm         | 流动资产周转率ttm                | 营业收入ttm / 当期报表披露流动资产总计ttm | `operating_revenue_ttm_0 / current_assets_ttm_0` |
| fixed_asset_turnover_lyr           | 固定资产周转率lyr                | 营业收入lyr /最新年报披露固定资产总计lyr | `operating_revenue_lyr_0 / total_fixed_assets_lyr_0` |
| fixed_asset_turnover_ttm           | 固定资产周转率ttm                | 营业收入ttm / 当期报表披露固定资产总计ttm | `operating_revenue_ttm_0 / total_fixed_assets_ttm_0` |
| total_asset_turnover_lyr           | 总资产周转率lyr                  | 营业收入lyr /（最新年报披露总资产lyr  | `operating_revenue_lyr_0 / total_assets_lyr_0` |
| total_asset_turnover_ttm           | 总资产周转率ttm                  | 营业收入ttm / 当期报表披露总资产ttm  | `operating_revenue_ttm_0 / total_assets_ttm_0` |
| du_profit_margin_lyr               | 净利率(杜邦分析）lyr             | 净利润lyr / 利润总额lyr              | `net_profit_lyr_0 / profit_before_tax_lyr_0` |
| du_profit_margin_ttm               | 净利率(杜邦分析）ttm             | 净利润ttm / 利润总额ttm              | `net_profit_ttm_0 / profit_before_tax_ttm_0` |
| du_return_on_equity_lyr            | 净资产收益率ROE(杜邦分析)lyr     | 净利率(杜邦分析)lyr *总资产周转率lyr *权益乘数(杜邦分析)lyr | `du_profit_margin_lyr* total_asset_turnover_lyr * du_equity_multiplier_lyr` |
| du_return_on_equity_ttm            | 净资产收益率ROE(杜邦分析)ttm     | 净利率(杜邦分析)ttm *总资产周转率ttm *权益乘数(杜邦分析)ttm | `du_profit_margin_ttm* total_asset_turnover_ttm * du_equity_multiplier_ttm` |
| du_return_on_sales_lyr             | 息税前利润/营业总收入lyr         | 息税前利润lyr / 营业总收入lyr          | `ebit_lyr / revenue_lyr_0`                   |
| du_return_on_sales_ttm             | 息税前利润/营业总收入ttm         | 息税前利润ttm / 营业总收入ttm          | `ebit_ttm / revenue_ttm_0`                   |
| income_from_main_operations_lyr    | 主营业务利润lyr                  | 营业收入lyr - 营业成本lyr - 营业税金及附加lyr | `operating_revenue_lyr_0 - cost_of_goods_sold_lyr_0 - sales_tax_lyr_0` |
| income_from_main_operations_ttm    | 主营业务利润ttm                  | 营业收入ttm - 营业成本ttm - 营业税金及附加ttm | `operating_revenue_ttm_0 - cost_of_goods_sold_ttm_0 - sales_tax_ttm_0` |
| time_interest_earned_ratio_lyr     | 利息保障倍数lyr                  | 息税前利润lyr / (利息支出(财务费用) lyr - 利息收入(财务费用) lyr) | `ebit_lyr / (financing_interest_expense_lyr_0 - financing_interest_income_lyr_0)` |
| time_interest_earned_ratio_ttm     | 利息保障倍数ttm                  | 息税前利润ttm / (利息支出(财务费用) ttm - 利息收入(财务费用) ttm) | `ebit_ttm / (financing_interest_expense_ttm_0 - financing_interest_income_ttm_0)` |
| equity_turnover_ratio_lyr          | 股东权益周转率lyr                | 营业收入lyr / 归属母公司股东权益合计lyr | `operating_revenue_lyr_0 / equity_parent_company_lyr_0` |
| equity_turnover_ratio_ttm          | 股东权益周转率ttm                | 营业收入ttm / 归属母公司股东权益合计ttm | `operating_revenue_ttm_0 / equity_parent_company_ttm_0` |
| operating_cycle_lyr                | 营业周期lyr                      | 应收账款周转天数lyr + 存货周转天数lyr | `account_receivable_turnover_days_lyr + 360/inventory_turnover_lyr` |
| operating_cycle_ttm                | 营业周期ttm                      | 应收账款周转天数ttm + 存货周转天数ttm | `account_receivable_turnover_days_ttm + 360/inventory_turnover_ttm` |
| average_payment_period_lyr         | 应付账款付款期lyr                | 平均应付账款lyr / 营业成本lyr / 360 | `accts_payable_lyr_0 / cost_of_goods_sold_lyr_0 / 360` |
| average_payment_period_ttm         | 应付账款付款期ttm                | 平均应付账款ttm / 营业成本ttm / 360 | `accts_payable_ttm_0 / cost_of_goods_sold_ttm_0 / 360` |
| cash_conversion_cycle_lyr          | 现金转换周期lyr                  | 营业周期lyr - 应付账款付款期lyr      | `operating_cycle_lyr_0 - average_payment_period_lyr_0` |
| cash_conversion_cycle_ttm          | 现金转换周期ttm                  | 营业周期ttm - 应付账款付款期ttm      | `operating_cycle_ttm_0 - average_payment_period_ttm_0` |

### 现金流衍生指标

为方便阅读，可点这里下载[Excel 版本的指标列表](https://assets.ricequant.com/vendor/rqdata/%E8%A1%8D%E7%94%9F%E8%B4%A2%E5%8A%A1%E6%8C%87%E6%A0%87.xlsx)

| 字段                              | 中文名                  | 说明                                         | 公式                                         |
| :-------------------------------- | :---------------------- | :------------------------------------------- | :------------------------------------------- |
| cash_flow_per_share_lyr           | 每股现金流lyr           | （经营活动产生的现金流量净额lyr + 投资活动产生的现金流量净额lyr + 筹资活动产生的现金流量净额lyr）/ 总股本 | `(cash_flow_from_operating_activities_lyr_0 + cash_flow_from_investing_activities_lyr_0 + cash_flow_from_financing_activities_lyr_0) / total_shares` |
| cash_flow_per_share_ttm           | 每股现金流ttm           | （经营活动产生的现金流量净额ttm + 投资活动产生的现金流量净额ttm + 筹资活动产生的现金流量净额ttm）/ 总股本 | `(cash_flow_from_operating_activities_ttm_0 + cash_flow_from_investing_activities_ttm_0 + cash_flow_from_financing_activities_ttm_0) / total_shares` |
| operating_cash_flow_per_share_lyr | 每股经营现金流lyr       | 经营活动产生的现金流量净额lyr / 总股本     | `cash_flow_from_operating_activities_lyr_0 / total_shares` |
| operating_cash_flow_per_share_ttm | 每股经营现金流ttm       | 经营活动产生的现金流量净额ttm / 总股本     | `cash_flow_from_operating_activities_ttm_0 / total_shares` |
| fcff_lyr                          | 企业自由现金流量lyr     | 归属于母公司所有者的净利润lyr ＋ 资产减值准备lyr ＋ 固定资产折旧lyr ＋ 无形资产摊销lyr ＋ 长期待摊费用摊销lyr + 利息费用lyr *（1 － 所得税lyr / 利润总额lyr）－（本期固定资产合计lyr－上期固定资产合计lyr ＋ 固定资产折旧lyr）－营运资本变动额lyr 其中：利息费用=利息支出(财务费用)-利息收入(财务费用)，如果企业未披露利息收入(财务费用)和利息支出(财务费用)，则“利息费用=财务费用”；营运资本变动额＝期末[（流动资产合计－货币资金）－(流动负债合计－应付票据－一年内到期的非流动负债）]－期初[（流动资产合计－货币资金）－(流动负债合计－应付票据－一年内到期的非流动负债） | `net_profit_parent_company_lyr_0 + assets_depreciation_reserves_lyr_0 + fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_lyr_0 + deferred_expense_amortization_lyr_0 + (financing_interest_expense_lyr_0 - financing_interest_income_lyr_0) * (1 - income_tax_lyr_0 / profit_before_tax_lyr_0) - (total_fixed_assets_lyr_0 - total_fixed_assets_lyr_1 + fixed_asset_depreciation_lyr_0) - (current_assets_lyr_0 - cash_equivalent_lyr_0 - current_liabilities_lyr_0 + notes_payable_lyr_0 + non_current_liability_due_one_year_lyr_0 - (current_assets_lyr_1 - cash_equivalent_lyr_1 - current_liabilities_lyr_1 + notes_payable_lyr_1 + non_current_liability_due_one_year_lyr_1))` |
| fcff_ttm                          | 企业自由现金流量ttm     | 归属于母公司所有者的净利润ttm ＋ 资产减值准备ttm ＋ 固定资产折旧ttm ＋ 无形资产摊销ttm ＋ 长期待摊费用摊销ttm + 利息费用ttm *（1 － 所得税ttm / 利润总额ttm）－（本期固定资产合计ttm－上期固定资产合计ttm ＋ 固定资产折旧ttm）－营运资本变动额ttm 其中：利息费用=利息支出(财务费用)-利息收入(财务费用)，如果企业未披露利息收入(财务费用）和利息支出（财务费用)，则“利息费用=财务费用”；营运资本变动额＝期末[（流动资产合计－货币资金）－(流动负债合计－应付票据－一年内到期的非流动负债）]－期初[（流动资产合计－货币资金）－(流动负债合计－应付票据－一年内到期的非流动负债） | `net_profit_parent_company_ttm_0 + assets_depreciation_reserves_ttm_0 + fixed_asset_depreciation_ttm_0 + intangible_asset_amortization_ttm_0 + deferred_expense_amortization_ttm_0 + (financing_interest_expense_ttm_0 - financing_interest_income_ttm_0) * (1 - income_tax_ttm_0 / profit_before_tax_ttm_0) - (total_fixed_assets_ttm_0 - total_fixed_assets_ttm_1 + fixed_asset_depreciation_ttm_0) - (current_assets_ttm_0 - cash_equivalent_ttm_0 - current_liabilities_ttm_0 + notes_payable_ttm_0 + non_current_liability_due_one_year_ttm_0 - (current_assets_ttm_1 - cash_equivalent_ttm_1 - current_liabilities_ttm_1 + notes_payable_ttm_1 + non_current_liability_due_one_year_ttm_1))` |
| fcfe_lyr                          | 股权自由现金流量lyr     | 归属于母公司所有者的净利润lyr ＋ 资产减值准备lyr ＋ 固定资产折旧lyr ＋ 无形资产摊销lyr ＋ 长期待摊费用摊销lyr -（本期固定资产合计lyr - 上期固定资产合计lyr + 固定资产折旧lyr）- 营运资本变动额lyr + 净债务增加lyr 其中：营运资本变动额＝期末[（流动资产合计－货币资金）－(流动负债合计-应付票据－一年内到期的非流动负债）]－期初[（流动资产合计－货币资金）－(流动负债合计-应付票据－一年内到期的非流动负债）]；净债务增加＝期末（短期借款+长期借款+应付债券）－期初（短期借款+长期借款+应付债券） | `net_profit_parent_company_lyr_0 + assets_depreciation_reserves_lyr_0 + fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_lyr_0 + deferred_expense_amortization_lyr_0 - (total_fixed_assets_lyr_0 - total_fixed_assets_lyr_1 + fixed_asset_depreciation_lyr_0) - (current_assets_lyr_0 - cash_equivalent_lyr_0 - current_liabilities_lyr_0 + notes_payable_lyr_0 + non_current_liability_due_one_year_lyr_0 - (current_assets_lyr_1 - cash_equivalent_lyr_1 - current_liabilities_lyr_1 + notes_payable_lyr_1 + non_current_liability_due_one_year_lyr_1)) + (short_term_loans_lyr_0 + long_term_loans_lyr_0 + bond_payable_lyr_0 - (short_term_loans_lyr_1 + long_term_loans_lyr_1 + bond_payable_lyr_1))` |
| fcfe_ttm                          | 股权自由现金流量ttm     | 归属于母公司所有者的净利润ttm ＋ 资产减值准备ttm ＋ 固定资产折旧ttm ＋ 无形资产摊销ttm ＋ 长期待摊费用摊销ttm -（本期固定资产合计ttm - 上期固定资产合计ttm + 固定资产折旧ttm）- 营运资本变动额ttm + 净债务增加ttm 其中：营运资本变动额＝期末[（流动资产合计－货币资金）－(流动负债合计-应付票据－一年内到期的非流动负债）]－期初[（流动资产合计－货币资金）－(流动负债合计-应付票据－一年内到期的非流动负债）]；净债务增加＝期末（短期借款+长期借款+应付债券）－期初（短期借款+长期借款+应付债券） | `net_profit_parent_company_ttm_0 + assets_depreciation_reserves_ttm_0 + fixed_asset_depreciation_ttm_0 + intangible_asset_amortization_ttm_0 + deferred_expense_amortization_ttm_0 - (total_fixed_assets_ttm_0 - total_fixed_assets_ttm_1 + fixed_asset_depreciation_ttm_0) - (current_assets_ttm_0 - cash_equivalent_ttm_0 - current_liabilities_ttm_0 + notes_payable_ttm_0 + non_current_liability_due_one_year_ttm_0 - (current_assets_ttm_1 - cash_equivalent_ttm_1 - current_liabilities_ttm_1 + notes_payable_ttm_1 + non_current_liability_due_one_year_ttm_1)) + (short_term_loans_ttm_0 + long_term_loans_ttm_0 + bond_payable_ttm_0 - (short_term_loans_ttm_1 + long_term_loans_ttm_1 + bond_payable_ttm_1))` |
| free_cash_flow_company_per_share_lyr | 每股企业自由现金流lyr   | 企业自由现金流量lyr / 总股本                 | `fcff_lyr_0 / total_shares`                  |
| free_cash_flow_company_per_share_ttm | 每股企业自由现金流ttm   | 企业自由现金流量ttm / 总股本                 | `fcff_ttm_0 / total_shares`                  |
| free_cash_flow_equity_per_share_lyr | 每股股东自由现金流lyr   | 股东自由现金流量lyr / 总股本                 | `fcfe_lyr_0 / total_shares`                  |
| free_cash_flow_equity_per_share_ttm | 每股股东自由现金流ttm   | 股东自由现金流量ttm / 总股本                 | `fcfe_ttm_0 / total_shares`                  |
| ocf_to_debt_lyr                   | 经营活动产生的现金流量净额/负债合计lyr | 经营活动产生的现金流量净额lyr / 负债合计lyr | `cash_flow_from_operating_activities_lyr_0 / total_liabilities_lyr_0` |
| ocf_to_debt_ttm                   | 经营活动产生的现金流量净额/负债合计ttm | 经营活动产生的现金流量净额ttm / 负债合计ttm | `cash_flow_from_operating_activities_ttm_0 / total_liabilities_ttm_0` |
| surplus_cash_protection_multiples_lyr | 盈余现金保障倍数lyr     | 经营活动产生的现金流量净额lyr/净利润lyr   | `cash_flow_from_operating_activities_lyr_0/net_profit_lyr_0` |
| surplus_cash_protection_multiples_ttm | 盈余现金保障倍数ttm     | 经营活动产生的现金流量净额ttm/净利润ttm   | `cash_flow_from_operating_activities_ttm_0/net_profit_ttm_0` |
| ocf_to_interest_bearing_debt_lyr  | 经营活动产生的现金流量净额/带息债务lyr | 经营活动产生的现金流量净额lyr / 带息债务lyr | `cash_flow_from_operating_activities_lyr_0 / interest_bearing_debt_lyr` |
| ocf_to_interest_bearing_debt_ttm  | 经营活动产生的现金流量净额/带息债务ttm | 经营活动产生的现金流量净额ttm / 带息债务ttm | `cash_flow_from_operating_activities_ttm_0 / interest_bearing_debt_ttm` |
| ocf_to_current_ratio_lyr          | 经营活动产生的现金流量净额/流动负债lyr | 连续四季度经营活动产生的现金流量净额lyr / 流动负债lyr | `cash_flow_from_operating_activities_lyr_0 / current_liabilities_lyr_0` |
| ocf_to_current_ratio_ttm          | 经营活动产生的现金流量净额/流动负债ttm | 连续四季度经营活动产生的现金流量净额ttm / 流动负债ttm | `cash_flow_from_operating_activities_ttm_0 / current_liabilities_ttm_0` |
| ocf_to_net_debt_lyr               | 经营活动产生的现金流量净额/净债务lyr | 经营活动产生的现金流量净额lyr / 净债务lyr | `cash_flow_from_operating_activities_lyr_0 / net_debt_lyr` |
| ocf_to_net_debt_ttm               | 经营活动产生的现金流量净额/净债务ttm | 经营活动产生的现金流量净额ttm / 净债务ttm | `cash_flow_from_operating_activities_ttm_0 / net_debt_ttm` |
| depreciation_and_amortization_lyr | 当期计提折旧与摊销lyr   | 固定资产折旧lyr ＋ 无形资产摊销lyr ＋ 长期待摊费用摊销lyr | `fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_lyr_0 + deferred_expense_amortization_lyr_0` |
| depreciation_and_amortization_ttm | 当期计提折旧与摊销ttm   | 固定资产折旧ttm ＋ 无形资产摊销ttm ＋ 长期待摊费用摊销ttm | `fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_ttm_0 + deferred_expense_amortization_ttm_0` |
| cash_flow_ratio_lyr               | 现金流量比率lyr         | 未扣除利息所得税折旧和摊销前的盈余lyr / 年利息支出lyr | `ebitda_lyr / financing_interest_expense_lyr_0` |
| cash_flow_ratio_ttm               | 现金流量比率ttm         | 未扣除利息所得税折旧和摊销前的盈余ttm / 年利息支出ttm | `ebitda_ttm / financing_interest_expense_ttm_0` |

### 财务衍生指标

为方便阅读，可点这里下载[Excel 版本的指标列表](https://assets.ricequant.com/vendor/rqdata/%E8%A1%8D%E7%94%9F%E8%B4%A2%E5%8A%A1%E6%8C%87%E6%A0%87.xlsx)

| 字段                               | 中文名                     | 说明                                         | 公式                                         |
| :--------------------------------- | :------------------------- | :------------------------------------------- | :------------------------------------------- |
| non_interest_bearing_current_debt_lyr | 无息流动负债lyr            | 应付帐款lyr ＋ 预收帐款lyr ＋ 应付职工薪酬lyr ＋ 应交税费lyr ＋ 其他应付款lyr ＋ 预提费用lyr ＋ 递延收益lyr ＋ 其他流动负债lyr | `accts_payable_lyr_0 + advance_from_customers_lyr_0 + payroll_payable_lyr_0 + tax_payable_lyr_0 + other_payable_lyr_0 + accrued_expense_lyr_0 + deferred_income_lyr_0 + other_current_liabilities_lyr_0` |
| non_interest_bearing_current_debt_ttm | 无息流动负债ttm            | 应付帐款ttm ＋ 预收帐款ttm ＋ 应付职工薪酬ttm ＋ 应交税费ttm ＋ 其他应付款ttm ＋ 预提费用ttm ＋ 递延收益ttm ＋ 其他流动负债ttm | `accts_payable_ttm_0 + advance_from_customers_ttm_0 + payroll_payable_ttm_0 + tax_payable_ttm_0 + other_payable_ttm_0 + accrued_expense_ttm_0 + deferred_income_ttm_0 + other_current_liabilities_ttm_0` |
| non_interest_bearing_current_debt_lf | 无息流动负债lf             | 应付帐款mrq ＋ 预收帐款mrq ＋ 应付职工薪酬mrq ＋ 应交税费mrq ＋ 其他应付款mrq ＋ 预提费用mrq ＋ 递延收益mrq ＋ 其他流动负债mrq | `accts_payable_mrq_0 + advance_from_customers_mrq_0 + payroll_payable_mrq_0 + tax_payable_mrq_0 + other_payable_mrq_0 + accrued_expense_mrq_0 + deferred_income_mrq_0 + other_current_liabilities_mrq_0` |
| non_interest_bearing_non_current_debt_lyr | 无息非流动负债lyr          | 非流动负债合计lyr － 长期借款lyr － 应付债券lyr | `non_current_liabilities_lyr_0 - long_term_loans_lyr_0 - bond_payable_lyr_0` |
| non_interest_bearing_non_current_debt_ttm | 无息非流动负债ttm          | 非流动负债合计ttm － 长期借款ttm － 应付债券ttm | `non_current_liabilities_ttm_0 - long_term_loans_ttm_0 - bond_payable_ttm_0` |
| non_interest_bearing_non_current_debt_lf | 无息非流动负债lf           | 非流动负债合计mrq － 长期借款mrq － 应付债券mrq | `non_current_liabilities_mrq_0 - long_term_loans_mrq_0 - bond_payable_mrq_0` |
| interest_bearing_debt_lyr          | 带息债务lyr                | 负债合计lyr - 无息流动负债lyr - 无息非流动负债lyr | `total_liabilities_lyr_0 - non_interest_bearing_current_debt_lyr - non_interest_bearing_non_current_debt_lyr` |
| interest_bearing_debt_ttm          | 带息债务ttm                | 负债合计ttm - 无息流动负债ttm - 无息非流动负债ttm | `total_liabilities_ttm_0 - non_interest_bearing_current_debt_ttm - non_interest_bearing_non_current_debt_ttm` |
| interest_bearing_debt_lf           | 带息债务lf                 | 负债合计mrq - 无息流动负债lf - 无息非流动负债lf | `total_liabilities_mrq_0 - non_interest_bearing_current_debt_lf - non_interest_bearing_non_current_debt_lf` |
| capital_reserve_per_share_lyr      | 每股资本公积金lyr          | 资本公积金lyr / 总股本             | `capital_reserve_lyr_0 / total_shares`       |
| capital_reserve_per_share_ttm      | 每股资本公积金ttm          | 资本公积金ttm / 总股本             | `capital_reserve_ttm_0 / total_shares`       |
| capital_reserve_per_share_lf       | 每股资本公积金lf           | 资本公积金mrq / 总股本             | `capital_reserve_mrq_0 / total_shares`       |
| earned_reserve_per_share_lyr       | 每股盈余公积金lyr          | 盈余公积金lyr / 总股本             | `surplus_reserve_lyr_0 / total_shares`       |
| earned_reserve_per_share_ttm       | 每股盈余公积金ttm          | 盈余公积金ttm / 总股本             | `surplus_reserve_ttm_0 / total_shares`       |
| earned_reserve_per_share_lf        | 每股盈余公积金lf           | 盈余公积金mrq / 总股本             | `surplus_reserve_mrq_0 / total_shares`       |
| undistributed_profit_per_share_lyr | 每股未分配利润lyr          | 企业当期未分配利润总额lyr / 总股本 | `undistributed_profit_lyr_0 / total_shares`  |
| undistributed_profit_per_share_ttm | 每股未分配利润ttm          | 企业当期未分配利润总额ttm / 总股本 | `undistributed_profit_ttm_0 / total_shares`  |
| undistributed_profit_per_share_lf  | 每股未分配利润lf           | 企业当期未分配利润总额mrq / 总股本 | `undistributed_profit_mrq_0 / total_shares`  |
| retained_earnings_lyr              | 留存收益lyr                | 盈余公积金lyr + 未分配利润lyr      | `surplus_reserve_lyr_0 + undistributed_profit_lyr_0` |
| retained_earnings_ttm              | 留存收益ttm                | 盈余公积金ttm + 未分配利润ttm      | `surplus_reserve_ttm_0 + undistributed_profit_ttm_0` |
| retained_earnings_lf               | 留存收益lf                 | 盈余公积金mrq + 未分配利润mrq      | `surplus_reserve_mrq_0 + undistributed_profit_mrq_0` |
| retained_earnings_per_share_lyr    | 每股留存收益lyr            | 留存收益lyr / 总股本               | `retained_earnings_lyr / total_shares`       |
| retained_earnings_per_share_ttm    | 每股留存收益ttm            | 留存收益ttm / 总股本               | `retained_earnings_ttm / total_shares`       |
| retained_earnings_per_share_lf     | 每股留存收益lf             | 留存收益lf / 总股本                | `retained_earnings_lf / total_shares`        |
| debt_to_asset_ratio_lyr            | 资产负债率lyr              | 负债合计lyr / 总资产               | `total_liabilities_lyr_0 / total_assets_lyr_0` |
| debt_to_asset_ratio_ttm            | 资产负债率ttm              | 负债合计ttm / 总资产               | `total_liabilities_ttm_0 / total_assets_ttm_0` |
| debt_to_asset_ratio_lf             | 资产负债率lf               | 负债合计mrq / 总资产               | `total_liabilities_mrq_0 / total_assets_mrq_0` |
| equity_multiplier_lyr              | 权益乘数lyr                | 总资产lyr / 股东权益合计lyr        | `total_assets_lyr_0 / total_equity_lyr_0`    |
| equity_multiplier_ttm              | 权益乘数ttm                | 总资产ttm / 股东权益合计ttm        | `total_assets_ttm_0 / total_equity_ttm_0`    |
| equity_multiplier_lf               | 权益乘数lf                 | 总资产mrq / 股东权益合计mrq        | `total_assets_mrq_0 / total_equity_mrq_0`    |
| capital_to_equity_ratio_lyr        | 长期资本固定比率lyr        | (资产总计lyr-流动资产lyr)/所有者权益平均余额lyr | `2*(total_assets_lyr_0-current_assets_lyr_0)/(total_equity_lyr_0+total_equity_lyr_1)` |
| capital_to_equity_ratio_ttm        | 长期资本固定比率ttm        | (资产总计ttm-流动资产ttm)/所有者权益平均余额ttm | `2*(total_assets_ttm_0-current_assets_ttm_0)/(total_equity_ttm_0+total_equity_ttm_1)` |
| capital_to_equity_ratio_lf         | 长期资本固定比率lf         | (资产总计lf-流动资产lf)/所有者权益平均余额lf | `2*(total_assets_mrq_0-current_assets_mrq_0)/(total_equity_mrq_0+total_equity_mrq_1)` |
| current_asset_to_total_asset_lyr   | 流动资产比率lyr            | 流动资产合计lyr / 总资产lyr        | `current_assets_lyr_0 / total_assets_lyr_0`  |
| current_asset_to_total_asset_ttm   | 流动资产比率ttm            | 流动资产合计ttm / 总资产ttm        | `current_assets_ttm_0 / total_assets_ttm_0`  |
| current_asset_to_total_asset_lf    | 流动资产比率lf             | 流动资产合计mrq / 总资产mrq        | `current_assets_mrq_0 / total_assets_mrq_0`  |
| non_current_asset_to_total_asset_lyr | 非流动资产比率lyr          | 非流动资产合计lyr / 总资产lyr      | `non_current_assets_lyr_0 / total_assets_lyr_0` |
| non_current_asset_to_total_asset_ttm | 非流动资产比率ttm          | 非流动资产合计ttm / 总资产ttm      | `non_current_assets_ttm_0 / total_assets_ttm_0` |
| non_current_asset_to_total_asset_lf | 非流动资产比率lf           | 非流动资产合计mrq / 总资产mrq      | `non_current_assets_mrq_0 / total_assets_mrq_0` |
| invested_capital_lyr               | 全部投入资本lyr            | 归属于母公司所有者权益合计lyr + 带息债务lyr | `equity_parent_company_lyr_0 + interest_bearing_debt_lyr` |
| invested_capital_ttm               | 全部投入资本ttm            | 归属于母公司所有者权益合计ttm + 带息债务ttm | `equity_parent_company_ttm_0 + interest_bearing_debt_ttm` |
| invested_capital_lf                | 全部投入资本lf             | 归属于母公司所有者权益合计mrq + 带息债务lf | `equity_parent_company_mrq_0 + interest_bearing_debt_lf` |
| interest_bearing_debt_to_capital_lyr | 带息债务占企业全部投入成本的比重lyr | 带息债务lyr / 全部投入资本lyr      | `interest_bearing_debt_lyr / invested_capital_lyr` |
| interest_bearing_debt_to_capital_ttm | 带息债务占企业全部投入成本的比重ttm | 带息债务ttm / 全部投入资本ttm      | `interest_bearing_debt_ttm / invested_capital_ttm` |
| interest_bearing_debt_to_capital_lf | 带息债务占企业全部投入成本的比重lf | 带息债务lf / 全部投入资本lf        | `interest_bearing_debt_lf / invested_capital_lf` |
| current_debt_to_total_debt_lyr     | 流动负债率lyr              | 流动负债合计lyr / 负债合计lyr      | `current_liabilities_lyr_0 / total_liabilities_lyr_0` |
| current_debt_to_total_debt_ttm     | 流动负债率ttm              | 流动负债合计ttm / 负债合计ttm      | `current_liabilities_ttm_0 / total_liabilities_ttm_0` |
| current_debt_to_total_debt_lf      | 流动负债率lf               | 流动负债合计mrq / 负债合计mrq      | `current_liabilities_mrq_0 / total_liabilities_mrq_0` |
| non_current_debt_to_total_debt_lyr | 非流动负债率lyr            | 非流动负债合计lyr / 负债合计lyr    | `non_current_liabilities_lyr_0 / total_liabilities_lyr_0` |
| non_current_debt_to_total_debt_ttm | 非流动负债率ttm            | 非流动负债合计ttm / 负债合计ttm    | `non_current_liabilities_ttm_0 / total_liabilities_ttm_0` |
| non_current_debt_to_total_debt_lf  | 非流动负债率lf             | 非流动负债合计mrq / 负债合计mrq    | `non_current_liabilities_mrq_0 / total_liabilities_mrq_0` |
| current_ratio_lyr                  | 流动比率lyr                | 流动资产合计lyr / 流动负债合计lyr  | `current_assets_lyr_0 / current_liabilities_lyr_0` |
| current_ratio_ttm                  | 流动比率ttm                | 流动资产合计ttm / 流动负债合计ttm  | `current_assets_ttm_0 / current_liabilities_ttm_0` |
| current_ratio_lf                   | 流动比率lf                 | 流动资产合计mrq / 流动负债合计mrq  | `current_assets_mrq_0 / current_liabilities_mrq_0` |
| quick_ratio_lyr                    | 速动比率lyr                | （流动资产合计lyr - 存货lyr - 预付账款lyr - 待摊费用lyr）/ 流动负债合计lyr | `(current_assets_lyr_0 - inventory_lyr_0 - prepayment_lyr_0 - deferred_expense_lyr_0) / current_liabilities_lyr_0` |
| quick_ratio_ttm                    | 速动比率ttm                | （流动资产合计ttm - 存货ttm - 预付账款ttm - 待摊费用ttm）/ 流动负债合计ttm | `(current_assets_ttm_0 - inventory_ttm_0 - prepayment_ttm_0 - deferred_expense_ttm_0) / current_liabilities_ttm_0` |
| quick_ratio_lf                     | 速动比率lf                 | （流动资产合计mrq - 存货mrq - 预付账款mrq - 待摊费用mrq）/ 流动负债合计mrq | `(current_assets_mrq_0 - inventory_mrq_0 - prepayment_mrq_0 - deferred_expense_mrq_0) / current_liabilities_mrq_0` |
| super_quick_ratio_lyr              | 超速动比率lyr              | （货币资金lyr + 交易性金融资产lyr + 应收票据lyr + 应收账款lyr + 其他应收款lyr）/ 流动负债合计lyr | `(cash_equivalent_lyr_0 + financial_asset_held_for_trading_lyr_0 + bill_receivable_lyr_0 + net_accts_receivable_lyr_0 + other_accts_receivable_lyr_0) / current_liabilities_lyr_0` |
| super_quick_ratio_ttm              | 超速动比率ttm              | （货币资金ttm + 交易性金融资产ttm + 应收票据ttm + 应收账款ttm + 其他应收款ttm）/ 流动负债合计ttm | `(cash_equivalent_ttm_0 + financial_asset_held_for_trading_ttm_0 + bill_receivable_ttm_0 + net_accts_receivable_ttm_0 + other_accts_receivable_ttm_0) / current_liabilities_ttm_0` |
| super_quick_ratio_lf               | 超速动比率lf               | （货币资金mrq + 交易性金融资产mrq + 应收票据mrq + 应收账款mrq + 其他应收款mrq）/ 流动负债合计mrq | `(cash_equivalent_mrq_0 + financial_asset_held_for_trading_mrq_0 + bill_receivable_mrq_0 + net_accts_receivable_mrq_0 + other_accts_receivable_mrq_0) / current_liabilities_mrq_0` |
| debt_to_equity_ratio_lyr           | 产权比率lyr                | 负债合计lyr / 股东权益合计lyr      | `total_liabilities_lyr_0 / equity_parent_company_lyr_0` |
| debt_to_equity_ratio_ttm           | 产权比率ttm                | 负债合计ttm / 股东权益合计ttm      | `total_liabilities_ttm_0 / equity_parent_company_ttm_0` |
| debt_to_equity_ratio_lf            | 产权比率lf                 | 负债合计mrq / 股东权益合计mrq      | `total_liabilities_mrq_0 / equity_parent_company_mrq_0` |
| equity_to_debt_ratio_lyr           | 权益负债比率lyr            | 归属于母公司所有者权益合计lyr / 负债合计lyr | `equity_parent_company_lyr_0 / total_liabilities_lyr_0` |
| equity_to_debt_ratio_ttm           | 权益负债比率ttm            | 归属于母公司所有者权益合计ttm / 负债合计ttm | `equity_parent_company_ttm_0 / total_liabilities_ttm_0` |
| equity_to_debt_ratio_lf            | 权益负债比率lf             | 归属于母公司所有者权益合计mrq / 负债合计mrq | `equity_parent_company_mrq_0 / total_liabilities_mrq_0` |
| equity_to_interest_bearing_debt_lyr | 权益带息负债比率lyr        | 归属于母公司所有者权益合计lyr / 带息债务lyr | `equity_parent_company_lyr_0 / interest_bearing_debt_lyr` |
| equity_to_interest_bearing_debt_ttm | 权益带息负债比率ttm        | 归属于母公司所有者权益合计ttm / 带息债务ttm | `equity_parent_company_ttm_0 / interest_bearing_debt_ttm` |
| equity_to_interest_bearing_debt_lf | 权益带息负债比率lf         | 归属于母公司所有者权益合计mrq / 带息债务lf | `equity_parent_company_mrq_0 / interest_bearing_debt_lf` |
| net_debt_lyr                       | 净债务lyr                  | 带息债务lyr - 货币资金lyr          | `interest_bearing_debt_lyr - cash_equivalent_lyr_0` |
| net_debt_ttm                       | 净债务ttm                  | 带息债务ttm - 货币资金ttm          | `interest_bearing_debt_ttm - cash_equivalent_ttm_0` |
| net_debt_lf                        | 净债务lf                   | 带息债务lf - 货币资金mrq           | `interest_bearing_debt_lf - cash_equivalent_mrq_0` |
| working_capital_lyr                | 营运资本lyr                | 流动资产合计lyr - 流动负债合计lyr  | `current_assets_lyr_0 - current_liabilities_lyr_0` |
| working_capital_ttm                | 营运资本ttm                | 流动资产合计ttm - 流动负债合计ttm  | `current_assets_ttm_0 - current_liabilities_ttm_0` |
| working_capital_lf                 | 营运资本lf                 | 流动资产合计mrq - 流动负债合计mrq  | `current_assets_mrq_0 - current_liabilities_mrq_0` |
| net_working_capital_lyr            | 净营运资本lyr              | 流动资产合计lyr - 货币资金lyr - 无息流动负债lyr | `current_assets_lyr_0 - cash_equivalent_lyr_0 - non_interest_bearing_current_debt_lyr` |
| net_working_capital_ttm            | 净营运资本ttm              | 流动资产合计ttm - 货币资金ttm - 无息流动负债ttm | `current_assets_ttm_0 - cash_equivalent_ttm_0 - non_interest_bearing_current_debt_ttm` |
| net_working_capital_lf             | 净营运资本lf               | 流动资产合计mrq - 货币资金mrq - 无息流动负债lf | `current_assets_mrq_0 - cash_equivalent_mrq_0 - non_interest_bearing_current_debt_lf` |
| long_term_debt_to_working_capital_lyr | 长期债务与营运资金比率lyr  | 长期负债lyr/营运资本lyr            | `non_current_liabilities_lyr_0 / working_capital_lyr` |
| long_term_debt_to_working_capital_ttm | 长期债务与营运资金比率ttm  | 长期负债ttm/营运资本ttm            | `non_current_liabilities_ttm_0 / working_capital_ttm` |
| long_term_debt_to_working_capital_lf | 长期债务与营运资金比率lf   | 长期负债lf/营运资本lf              | `non_current_liabilities_mrq_0 / working_capital_lf` |
| book_value_per_share_lyr           | 每股净资产lyr              | 归属母公司股东权益合计lyr / 总股本 | `equity_parent_company_lyr_0 / total_shares` |
| book_value_per_share_ttm           | 每股净资产ttm              | 归属母公司股东权益合计ttm / 总股本 | `equity_parent_company_ttm_0 / total_shares` |
| book_value_per_share_lf            | 每股净资产lf               | 股东权益合计mrq / 总股本           | `equity_parent_company_mrq_0 / total_shares` |
| du_equity_multiplier_lyr           | 权益乘数(杜邦分析)lyr      | （本期总资产lyr + 上年总资产lyr）/ (本期股东权益合计lyr + 上年股东权益合计lyr) | `(total_assets_lyr_0 + total_assets_lyr_1) / (total_equity_lyr_0 + total_equity_lyr_1)` |
| du_equity_multiplier_ttm           | 权益乘数(杜邦分析)ttm      | （本期总资产ttm + 上期总资产ttm）/ (本期股东权益合计ttm + 上期股东权益合计ttm) | `(total_assets_ttm_0 + total_assets_ttm_1) / (total_equity_ttm_0 + total_equity_ttm_1)` |
| du_equity_multiplier_lf            | 权益乘数(杜邦分析)lf       | （本期总资产mrq + 上期总资产mrq）/ (本期股东权益合计mrq + 上期股东权益合计mrq) | `(total_assets_mrq_0 + total_assets_mrq_1) / (total_equity_mrq_0 + total_equity_mrq_1)` |
| book_leverage_lyr                  | 账面杠杆lyr                | 非流动复债合计lyr / 归母公司股东权益合计lyr | `non_current_liabilities_lyr_0 / equity_parent_company_lyr_0` |
| book_leverage_ttm                  | 账面杠杆ttm                | 非流动复债合计ttm / 归母公司股东权益合计ttm | `non_current_liabilities_ttm_0 / equity_parent_company_ttm_0` |
| book_leverage_lf                   | 账面杠杆lf                 | 非流动复债合计mrq / 归母公司股东权益合计mrq | `non_current_liabilities_mrq_0 / equity_parent_company_mrq_0` |
| market_leverage_lyr                | 市场杠杆lyr                | 非流动负债合计lyr / (非流动负债合计lyr + 总市值) | `non_current_liabilities_lyr_0 / (non_current_liabilities_lyr_0 + market_cap_3)` |
| market_leverage_ttm                | 市场杠杆ttm                | 非流动负债合计ttm / (非流动负债合计ttm + 总市值) | `non_current_liabilities_ttm_0 / (non_current_liabilities_ttm_0 + market_cap_3)` |
| market_leverage_lf                 | 市场杠杆lf                 | 非流动负债合计mrq / (非流动负债合计mrq + 总市值) | `non_current_liabilities_mrq_0 /(non_current_liabilities_mrq_0 + market_cap_3)` |
| equity_ratio_lyr                   | 股东权益比率lyr            | 归属母公司股东权益总计lyr / 资产总计lyr | `equity_parent_company_lyr_0 / total_assets_lyr_0` |
| equity_ratio_ttm                   | 股东权益比率ttm            | 归属母公司股东权益总计ttm / 资产总计ttm | `equity_parent_company_ttm_0 / total_assets_ttm_0` |
| equity_ratio_lf                    | 股东权益比率lf             | 归属母公司股东权益总计mrq / 资产总计mrq | `equity_parent_company_mrq_0 / total_assets_mrq_0` |
| fixed_asset_ratio_lyr              | 固定资产比率lyr            | (固定资产lyr + 工程物资lyr + 在建工程合计lyr) / 资产总计lyr | `(total_fixed_assets_lyr_0 + engineer_material_lyr_0 + total_construction_in_progress_lyr_0) / total_assets_lyr_0` |
| fixed_asset_ratio_ttm              | 固定资产比率ttm            | (固定资产ttm + 工程物资ttm + 在建工程合计ttm) / 资产总计ttm | `(total_fixed_assets_ttm_0 + engineer_material_ttm_0 + total_construction_in_progress_ttm_0) / total_assets_ttm_0` |
| fixed_asset_ratio_lf               | 固定资产比率lf             | (固定资产mrq + 工程物资mrq + 在建工程合计mrq) / 资产总计mrq | `(total_fixed_assets_mrq_0 + engineer_material_mrq_0 + total_construction_in_progress_mrq_0) / total_assets_mrq_0` |
| intangible_asset_ratio_lyr         | 无形资产比率lyr            | (无形资产lyr + 开发支出lyr + 商誉lyr) / 资产总计lyr | `(intangible_assets_lyr_0 + impairment_intangible_assets_lyr_0 + goodwill_lyr_0) / total_assets_lyr_0` |
| intangible_asset_ratio_ttm         | 无形资产比率ttm            | (无形资产ttm + 开发支出ttm + 商誉ttm) / 资产总计ttm | `(intangible_assets_ttm_0 + impairment_intangible_assets_ttm_0 + goodwill_ttm_0) / total_assets_ttm_0` |
| intangible_asset_ratio_lf          | 无形资产比率lf             | (无形资产mrq + 开发支出mrq + 商誉mrq) / 资产总计mrq | `(intangible_assets_mrq_0 + impairment_intangible_assets_mrq_0 + goodwill_mrq_0) / total_assets_mrq_0` |
| equity_fixed_asset_ratio_lyr       | 股东权益与固定资产比率lyr  | 归属母公司股东权益合计lyr / (固定资产合计lyr + 工程物资lyr + 在建工程合计lyr) | `equity_parent_company_lyr_0 / (total_fixed_assets_lyr_0 + engineer_material_lyr_0 + total_construction_in_progress_lyr_0)` |
| equity_fixed_asset_ratio_ttm       | 股东权益与固定资产比率ttm  | 归属母公司股东权益合计ttm / (固定资产合计ttm + 工程物资ttm + 在建工程合计ttm) | `equity_parent_company_ttm_0 / (total_fixed_assets_ttm_0 + engineer_material_ttm_0 + total_construction_in_progress_ttm_0)` |
| equity_fixed_asset_ratio_lf        | 股东权益与固定资产比率lf   | 归属母公司股东权益合计mrq / (固定资产合计mrq + 工程物资mrq + 在建工程合计mrq) | `equity_parent_company_mrq_0 / (total_fixed_assets_mrq_0 + engineer_material_mrq_0 + total_construction_in_progress_mrq_0)` |
| tangible_asset_per_share_lyr       | 每股有形资产lyr            | （资产总计lyr - 无形资产lyr - 商誉lyr）/ 总股本 | `(total_assets_lyr_0 - intangible_assets_lyr_0 - goodwill_lyr_0) / total_shares` |
| tangible_asset_per_share_ttm       | 每股有形资产ttm            | （资产总计ttm - 无形资产ttm - 商誉ttm）/ 总股本 | `(total_assets_ttm_0 - intangible_assets_ttm_0 - goodwill_ttm_0) / total_shares` |
| tangible_asset_per_share_lf        | 每股有形资产lf             | （资产总计mrq - 无形资产mrq - 商誉mrq）/ 总股本 | `(total_assets_mrq_0 - intangible_assets_mrq_0 - goodwill_mrq_0) / total_shares` |
| liabilities_per_share_lyr          | 每股负债lyr                | 负债合计lyr / 总股本               | `total_liabilities_lyr_0 / total_shares`     |
| liabilities_per_share_ttm          | 每股负债ttm                | 负债合计ttm / 总股本               | `total_liabilities_ttm_0 / total_shares`     |
| liabilities_per_share_lf           | 每股负债lf                 | 负债合计mrq / 总股本               | `total_liabilities_mrq_0 / total_shares`     |
| depreciation_per_share_lyr         | 每股折旧和摊销lyr          | （固定资产折旧lyr + 无形资产摊销lyr + 长期待摊费用摊销lyr）/ 总股本 | `(fixed_asset_depreciation_lyr_0 + intangible_asset_amortization_lyr_0 + deferred_expense_amortization_lyr_0) / total_shares` |
| depreciation_per_share_ttm         | 每股折旧和摊销ttm          | （固定资产折旧ttm + 无形资产摊销ttm + 长期待摊费用摊销ttm）/ 总股本 | `(fixed_asset_depreciation_ttm_0 + intangible_asset_amortization_ttm_0 + deferred_expense_amortization_ttm_0) / total_shares` |
| depreciation_per_share_lf          | 每股折旧和摊销lf           | （固定资产折旧lf + 无形资产摊销lf + 长期待摊费用摊销lf）/ 总股本 | `(fixed_asset_depreciation_mrq_0 + intangible_asset_amortization_mrq_0 + deferred_expense_amortization_mrq_0) / total_shares` |
| cash_ratio_lyr                     | 现金比率lyr                | 货币资金余额lyr / 流动负债合计lyr  | `cash_equivalent_lyr_0 / current_liabilities_lyr_0` |
| cash_ratio_ttm                     | 现金比率ttm                | 货币资金余额ttm / 流动负债合计ttm  | `cash_equivalent_ttm_0 / current_liabilities_ttm_0` |
| cash_ratio_lf                      | 现金比率lf                 | 货币资金余额mrq / 流动负债合计mrq  | `cash_equivalent_mrq_0 / current_liabilities_mrq_0` |
| cash_equivalent_per_share_lyr      | 每股货币资金余额lyr        | 货币资金余额lyr / 总股本           | `cash_equivalent_lyr_0 / total_shares`       |
| cash_equivalent_per_share_ttm      | 每股货币资金余额ttm        | 货币资金余额ttm / 总股本           | `cash_equivalent_ttm_0 / total_shares`       |
| cash_equivalent_per_share_lf       | 每股货币资金余额lf         | 货币资金余额mrq / 总股本           | `cash_equivalent_mrq_0 / total_shares`       |
| dividend_amount_ly0                | 最近年度分红总额           | 事件进度仅包含方案实施             |                                              |
| dividend_amount_ly1                | 最近年度分红总额           | 事件进度包含决案、方案实施         |                                              |
| dividend_amount_ly2                | 最近年度分红总额           | 事件进度包含预案、决案和方案实施   |                                              |
| dividend_amount_ttm0               | 最近四个季度分红总额       | 事件进度仅包含方案实施             |                                              |
| dividend_amount_ttm1               | 最近四个季度分红总额       | 事件进度包含决案、方案实施         |                                              |
| dividend_amount_ttm2               | 最近四个季度分红总额       | 事件进度包含预案、决案和方案实施   |                                              |

### 增长衍生指标

为方便阅读，可点这里下载[Excel 版本的指标列表](https://assets.ricequant.com/vendor/rqdata/%E8%A1%8D%E7%94%9F%E8%B4%A2%E5%8A%A1%E6%8C%87%E6%A0%87.xlsx)

| 字段                               | 中文名                     | 说明                                         | 公式                                         |
| :--------------------------------- | :------------------------- | :------------------------------------------- | :------------------------------------------- |
| inc_revenue_lyr                    | 营业总收入同比增长率lyr    | 营业总收入lyr / 上年营业总收入lyr - 1      | `revenue_lyr_0 / revenue_lyr_1 - 1`          |
| inc_revenue_ttm                    | 营业总收入同比增长率ttm    | 营业总收入ttm / 上年营业总收入ttm - 1      | `revenue_ttm_0 / revenue_ttm_4 - 1`          |
| inc_return_on_equity_lyr           | 净资产收益率(摊薄）同比增长率lyr | 摊薄净资产收益率lyr / 去年摊薄净资产收益率lyr - 1 | `(net_profit_lyr_0 / total_equity_lyr_0) / (net_profit_lyr_1 / total_equity_lyr_1) - 1` |
| inc_return_on_equity_ttm           | 净资产收益率(摊薄）同比增长率ttm | 摊薄净资产收益率ttm / 去年摊薄净资产收益率ttm - 1 | `(net_profit_ttm_0 / total_equity_ttm_0) / (net_profit_ttm_4 / total_equity_ttm_4) - 1` |
| inc_book_per_share_lyr             | 每股净资产同比增长率lyr    | 每股净资产lyr / 去年每股净资产lyr - 1      | `(equity_parent_company_lyr_0 / total_shares) / (equity_parent_company_lyr_1 / prev_year_total_shares)-1` |
| inc_book_per_share_ttm             | 每股净资产同比增长率ttm    | 每股净资产ttm / 去年每股净资产ttm - 1      | `(equity_parent_company_ttm_0 / total_shares) / (equity_parent_company_ttm_4 / prev_year_total_shares)-1` |
| inc_book_per_share_lf              | 每股净资产同比增长率lf     | 每股净资产lf / 去年每股净资产lf - 1        | `(equity_parent_company_mrq_0 / total_shares) / (equity_parent_company_mrq_4 / prev_year_total_shares)-1` |
| operating_profit_growth_ratio_lyr  | 营业利润同比增长率lyr      | (营业利润lyr - 去年营业利润lyr) / 去年营业利润lyr | `(profit_from_operation_lyr_0 - profit_from_operation_lyr_1) / profit_from_operation_lyr_1` |
| operating_profit_growth_ratio_ttm  | 营业利润同比增长率ttm      | (营业利润ttm - 去年营业利润ttm) / 去年营业利润ttm | `(profit_from_operation_ttm_0 - profit_from_operation_ttm_4) / profit_from_operation_ttm_4` |
| net_profit_growth_ratio_lyr        | 净利润同比增长率lyr        | (净利润lyr - 去年净利润lyr) / 去年净利润lyr | `(net_profit_lyr_0 - net_profit_lyr_1) / net_profit_lyr_1` |
| net_profit_growth_ratio_ttm        | 净利润同比增长率ttm        | (净利润ttm - 去年净利润ttm) / 去年净利润ttm | `(net_profit_ttm_0 - net_profit_ttm_4) / net_profit_ttm_4` |
| profit_growth_ratio_lyr            | 利润总额同比增长率lyr      | (利润总额lyr - 去年利润总额lyr) / 去年利润总额lyr | `(profit_before_tax_lyr_0 - profit_before_tax_lyr_1) / profit_before_tax_lyr_1` |
| profit_growth_ratio_ttm            | 利润总额同比增长率ttm      | (利润总额ttm - 去年利润总额ttm) / 去年利润总额ttm | `(profit_before_tax_ttm_0 - profit_before_tax_ttm_4) / profit_before_tax_ttm_4` |
| gross_profit_growth_ratio_lyr      | 毛利润同比增长率lyr        | (营业收入lyr - 营业总成本lyr) / (去年营业收入lyr - 去年营业总成本lyr) - 1 | `(operating_revenue_lyr_0-total_expense_lyr_0)/(operating_revenue_lyr_1-total_expense_lyr_1)-1` |
| gross_profit_growth_ratio_ttm      | 毛利润同比增长率ttm        | (营业收入ttm - 营业总成本ttm) / (去年营业收入ttm - 去年营业总成本ttm) - 1 | `(operating_revenue_ttm_0-total_expense_ttm_0)/(operating_revenue_ttm_4-total_expense_ttm_4)-1` |
| operating_revenue_growth_ratio_lyr | 营业收入同比增长率lyr      | (营业收入lyr - 去年营业收入lyr) / 去年营业收入lyr | `(operating_revenue_lyr_0 - operating_revenue_lyr_1) / operating_revenue_lyr_1` |
| operating_revenue_growth_ratio_ttm | 营业收入同比增长率ttm      | (营业收入ttm - 去年营业收入ttm) / 去年营业收入ttm | `(operating_revenue_ttm_0 - operating_revenue_ttm_4) / operating_revenue_ttm_4` |
| net_asset_growth_ratio_lyr         | 净资产同比增长率lyr        | 归属于母公司所有者权益合计lyr / 去年归属于母公司所有者权益合计lyr - 1 | `equity_parent_company_lyr_0 / equity_parent_company_lyr_1 - 1` |
| net_asset_growth_ratio_ttm         | 净资产同比增长率ttm        | 归属于母公司所有者权益合计ttm / 去年归属于母公司所有者权益合计ttm - 1 | `equity_parent_company_ttm_0 / equity_parent_company_ttm_4 - 1` |
| net_asset_growth_ratio_lf          | 净资产同比增长率lf         | 归属于母公司所有者权益合计mrq / 去年归属于母公司所有者权益合计mrq - 1 | `equity_parent_company_mrq_0 / equity_parent_company_mrq_4 - 1` |
| total_asset_growth_ratio_lyr       | 总资产同比增长率lyr        | (总资产lyr - 去年总资产lyr) / 去年总资产lyr | `(total_assets_lyr_0 - total_assets_lyr_1) / total_assets_lyr_1` |
| total_asset_growth_ratio_ttm       | 总资产同比增长率ttm        | (总资产ttm - 去年总资产ttm) / 去年总资产ttm | `(total_assets_ttm_0 - total_assets_ttm_4) / total_assets_ttm_4` |
| total_asset_growth_ratio_lf        | 总资产同比增长率lf         | (总资产mrq - 去年总资产mrq) / 去年总资产mrq | `(total_assets_mrq_0 - total_assets_mrq_4) / total_assets_mrq_4` |
| net_profit_parent_company_growth_ratio_lyr | 归属母公司所有者的净利润同比增长率lyr | 归属于母公司所有者的净利润lyr / 去年归属于母公司所有者的净利润lyr - 1 | `net_profit_parent_company_lyr_0 / net_profit_parent_company_lyr_1 - 1` |
| net_profit_parent_company_growth_ratio_ttm | 归属母公司所有者的净利润同比增长率ttm | 归属于母公司所有者的净利润ttm / 去年归属于母公司所有者的净利润ttm - 1 | `net_profit_parent_company_ttm_0 / net_profit_parent_company_ttm_4 - 1` |
| net_cash_flow_growth_ratio_lyr     | 净现金流增长率lyr          | 最近年报的现金及现金等价物净增加额lyr / 上年年报的现金及现金等价物净增加额lyr - 1 | `cash_equivalent_increase_lyr_0 / cash_equivalent_increase_lyr_1 - 1` |
| net_cash_flow_growth_ratio_ttm     | 净现金流增长率ttm          | 连续四季度的现金及现金等价物净增加额ttm / 上年连续四季度的现金及现金等价物净增加额ttm - 1 | `cash_equivalent_increase_ttm_0 / cash_equivalent_increase_ttm_4 - 1` |
| net_operate_cash_flow_growth_ratio_lyr | 经营现金流量净额同比增长率lyr | 经营活动产生的现金流量净额lyr / 去年经营活动产生的现金流量净额lyr - 1 | `cash_flow_from_operating_activities_lyr_0 / cash_flow_from_operating_activities_lyr_1 - 1` |
| net_operate_cash_flow_growth_ratio_ttm | 经营现金流量净额同比增长率ttm | 经营活动产生的现金流量净额ttm / 去年经营活动产生的现金流量净额ttm - 1 | `cash_flow_from_operating_activities_ttm_0 / cash_flow_from_operating_activities_ttm_4 - 1` |
| net_investing_cash_flow_growth_ratio_lyr | 投资现金流量净额同比增长率lyr | 投资活动产生的现金流量净额lyr / 去年投资活动产生的现金流量净额lyr - 1 | `cash_flow_from_investing_activities_lyr_0 / cash_flow_from_investing_activities_lyr_1 - 1` |
| net_investing_cash_flow_growth_ratio_ttm | 投资现金流量净额同比增长率ttm | 投资活动产生的现金流量净额ttm / 去年投资活动产生的现金流量净额ttm - 1 | `cash_flow_from_investing_activities_ttm_0 / cash_flow_from_investing_activities_ttm_4 - 1` |
| net_financing_cash_flow_growth_ratio_lyr | 筹资现金流量净额同比增长率lyr | 筹资活动产生的现金流量净额lyr / 去年筹资活动产生的现金流量净额lyr - 1 | `cash_flow_from_financing_activities_lyr_0 / cash_flow_from_financing_activities_lyr_1 - 1` |
| net_financing_cash_flow_growth_ratio_ttm | 筹资现金流量净额同比增长率ttm | 筹资活动产生的现金流量净额ttm / 去年筹资活动产生的现金流量净额ttm - 1 | `cash_flow_from_financing_activities_ttm_0 / cash_flow_from_financing_activities_ttm_4 - 1` |


# ricequant技术指标数据说明

## alpha101 因子

对应的函数计算逻辑请参考工具函数部分说明。可以直接使用 `get_factor` 函数调用，例如：

```python
# In
get_factor(['000001.XSHE', '600000.XSHG'],'WorldQuant_alpha010', '20190601', '20190610')
# Out
600000.XSHG 000001.XSHE
2019-06-03 0.162771 0.093489
2019-06-04 0.255633 0.281502
2019-06-05 0.789430 0.222253
2019-06-06 0.437743 0.415231
2019-06-10 0.935448 0.134391
```


```
CLOSE = Factor('close')
RETURNS = (CLOSE - REF(CLOSE, 1)) / REF(CLOSE, 1)
CAP = Factor('market_cap')
VWAP = Factor('total_turnover') / Factor('volume)
alpha001 = (RANK(TS_ARGMAX(SIGNEDPOWER(IF((RETURNS < 0), STDDEV(RETURNS, 20), CLOSE), 2.), 5)) - 0.5)
alpha002 = (-1 * CORRELATION(RANK(DELTA(LOG(VOLUME), 2)), RANK(((CLOSE - OPEN) / OPEN)), 6))
alpha003 = (-1 * CORRELATION(RANK(OPEN), RANK(VOLUME), 10))
alpha004 = (-1 * TS_RANK(RANK(LOW), 9))
alpha005 = (RANK((OPEN - (SUM(VWAP, 10) / 10))) * (-1 * ABS(RANK((CLOSE - VWAP)))))
alpha006 = (-1 * CORRELATION(OPEN, VOLUME, 10))
alpha007 = IF((ADV(20) < VOLUME), ((-1 * TS_RANK(ABS(DELTA(CLOSE, 7)), 60)) * SIGN(DELTA(CLOSE, 7))), (-1 * 1))
alpha008 = (-1 * RANK(((SUM(OPEN, 5) * SUM(RETURNS, 5)) - DELAY((SUM(OPEN, 5) * SUM(RETURNS, 5)), 10))))
alpha009 = IF((0 < TS_MIN(DELTA(CLOSE, 1), 5)), DELTA(CLOSE, 1), (IF((TS_MAX(DELTA(CLOSE, 1), 5) < 0), DELTA(CLOSE, 1), (-1 * DELTA(CLOSE, 1)))))
alpha010 = RANK(IF((0 < TS_MIN(DELTA(CLOSE, 1), 4)), DELTA(CLOSE, 1), IF((TS_MAX(DELTA(CLOSE, 1), 4) < 0), DELTA(CLOSE, 1), (-1 * DELTA(CLOSE, 1)))))
alpha011 = ((RANK(TS_MAX((VWAP - CLOSE), 3)) + RANK(TS_MIN((VWAP - CLOSE), 3))) * RANK(DELTA(VOLUME, 3)))
alpha012 = (SIGN(DELTA(VOLUME, 1)) * (-1 * DELTA(CLOSE, 1)))
alpha013 = (-1 * RANK(COVARIANCE(RANK(CLOSE), RANK(VOLUME), 5)))
alpha014 = ((-1 * RANK(DELTA(RETURNS, 3))) * CORRELATION(OPEN, VOLUME, 10))
alpha015 = (-1 * SUM(RANK(CORRELATION(RANK(HIGH), RANK(VOLUME), 3)), 3))
alpha016 = (-1 * RANK(COVARIANCE(RANK(HIGH), RANK(VOLUME), 5)))
alpha017 = (((-1 * RANK(TS_RANK(CLOSE, 10))) * RANK(DELTA(DELTA(CLOSE, 1), 1))) * RANK(TS_RANK((VOLUME / ADV(20)), 5)))
alpha018 = (-1 * RANK(((STDDEV(ABS((CLOSE - OPEN)), 5) + (CLOSE - OPEN)) + CORRELATION(CLOSE, OPEN, 10))))
alpha019 = ((-1 * SIGN(((CLOSE - DELAY(CLOSE, 7)) + DELTA(CLOSE, 7)))) * (1 + RANK((1 + SUM(RETURNS, 250)))))
alpha020 = (((-1 * RANK((OPEN - DELAY(HIGH, 1)))) * RANK((OPEN - DELAY(CLOSE, 1)))) * RANK((OPEN - DELAY(LOW, 1))))
alpha021 = IF((((SUM(CLOSE, 8) / 8) + STDDEV(CLOSE, 8)) < (SUM(CLOSE, 2) / 2)), (-1 * 1), IF(((SUM(CLOSE, 2) / 2) < ((SUM(CLOSE, 8) / 8) - STDDEV(CLOSE, 8))), 1, IF(((1 < (VOLUME / ADV(20))) | ((VOLUME / ADV(20)) == 1)), 1, (-1 * 1))))
alpha022 = (-1 * (DELTA(CORRELATION(HIGH, VOLUME, 5), 5) * RANK(STDDEV(CLOSE, 20))))
alpha023 = IF(((SUM(HIGH, 20) / 20) < HIGH), (-1 * DELTA(HIGH, 2)), 0)
alpha024 = IF((((DELTA((SUM(CLOSE, 100) / 100), 100) / DELAY(CLOSE, 100)) < 0.05) | ((DELTA((SUM(CLOSE, 100) / 100), 100) / DELAY(CLOSE, 100)) == 0.05)), (-1 * (CLOSE - TS_MIN(CLOSE, 100))), (-1 * DELTA(CLOSE, 3)))
alpha025 = RANK(((((-1 * RETURNS) * ADV(20)) * VWAP) * (HIGH - CLOSE)))
alpha026 = (-1 * TS_MAX(CORRELATION(TS_RANK(VOLUME, 5), TS_RANK(HIGH, 5), 5), 3))
alpha027 = IF((0.5 < RANK((SUM(CORRELATION(RANK(VOLUME), RANK(VWAP), 6), 2) / 2.0))), (-1 * 1), 1)
alpha028 = SCALE(((CORRELATION(ADV(20), LOW, 5) + ((HIGH + LOW) / 2)) - CLOSE))
alpha029 = (MIN(PRODUCT(RANK(RANK(SCALE(LOG(SUM(TS_MIN(RANK(RANK((-1 * RANK(DELTA((CLOSE - 1), 5))))), 2), 1))))), 1), 5) + TS_RANK(DELAY((-1 * RETURNS), 6), 5))
alpha030 = (((1.0 - RANK(((SIGN((CLOSE - DELAY(CLOSE, 1))) + SIGN((DELAY(CLOSE, 1) - DELAY(CLOSE, 2)))) + SIGN((DELAY(CLOSE, 2) - DELAY(CLOSE, 3)))))) * SUM(VOLUME, 5)) / SUM(VOLUME, 20))
alpha031 = ((RANK(RANK(RANK(DECAY_LINEAR((-1 * RANK(RANK(DELTA(CLOSE, 10)))), 10)))) + RANK((-1 * DELTA(CLOSE, 3)))) + SIGN(SCALE(CORRELATION(ADV(20), LOW, 12))))
alpha032 = (SCALE(((SUM(CLOSE, 7) / 7) - CLOSE)) + (20 * SCALE(CORRELATION(VWAP, DELAY(CLOSE, 5), 230))))
alpha033 = RANK((-1 * ((1 - (OPEN / CLOSE))**1)))
alpha034 = RANK(((1 - RANK((STDDEV(RETURNS, 2) / STDDEV(RETURNS, 5)))) + (1 - RANK(DELTA(CLOSE, 1)))))
alpha035 = ((TS_RANK(VOLUME, 32) * (1 - TS_RANK(((CLOSE + HIGH) - LOW), 16))) * (1 - TS_RANK(RETURNS, 32)))
alpha036 = (((((2.21 * RANK(CORRELATION((CLOSE - OPEN), DELAY(VOLUME, 1), 15))) + (0.7 * RANK((OPEN - CLOSE)))) + (0.73 * RANK(TS_RANK(DELAY((-1 * RETURNS), 6), 5)))) + RANK(ABS(CORRELATION(VWAP, ADV(20), 6)))) + (0.6 * RANK((((SUM(CLOSE, 200) / 200) - OPEN) * (CLOSE - OPEN)))))
alpha037 = (RANK(CORRELATION(DELAY((OPEN - CLOSE), 1), CLOSE, 200)) + RANK((OPEN - CLOSE)))
alpha038 = ((-1 * RANK(TS_RANK(CLOSE, 10))) * RANK((CLOSE / OPEN)))
alpha039 = ((-1 * RANK((DELTA(CLOSE, 7) * (1 - RANK(DECAY_LINEAR((VOLUME / ADV(20)), 9)))))) * (1 + RANK(SUM(RETURNS, 250))))
alpha040 = ((-1 * RANK(STDDEV(HIGH, 10))) * CORRELATION(HIGH, VOLUME, 10))
alpha041 = (((HIGH * LOW)**0.5) - VWAP)
alpha042 = (RANK((VWAP - CLOSE)) / RANK((VWAP + CLOSE)))
alpha043 = (TS_RANK((VOLUME / ADV(20)), 20) * TS_RANK((-1 * DELTA(CLOSE, 7)), 8))
alpha044 = (-1 * CORRELATION(HIGH, RANK(VOLUME), 5))
alpha045 = (-1 * ((RANK((SUM(DELAY(CLOSE, 5), 20) / 20)) * CORRELATION(CLOSE, VOLUME, 2)) * RANK(CORRELATION(SUM(CLOSE, 5), SUM(CLOSE, 20), 2))))
alpha046 = IF((0.25 < (((DELAY(CLOSE, 20) - DELAY(CLOSE, 10)) / 10) - ((DELAY(CLOSE, 10) - CLOSE) / 10))), (-1 * 1), IF(((((DELAY(CLOSE, 20) - DELAY(CLOSE, 10)) / 10) - ((DELAY(CLOSE, 10) - CLOSE) / 10)) < 0), 1, ((-1 * 1) * (CLOSE - DELAY(CLOSE, 1)))))
alpha047 = ((((RANK((1 / CLOSE)) * VOLUME) / ADV(20)) * ((HIGH * RANK((HIGH - CLOSE))) / (SUM(HIGH, 5) / 5))) - RANK((VWAP - DELAY(VWAP, 5))))
alpha048 = (INDUSTRY_NEUTRALIZE(((CORRELATION(DELTA(CLOSE, 1), DELTA(DELAY(CLOSE, 1), 1), 250) * DELTA(CLOSE, 1)) / CLOSE)) / SUM(((DELTA(CLOSE, 1) / DELAY(CLOSE, 1))**2), 250))
alpha049 = IF(((((DELAY(CLOSE, 20) - DELAY(CLOSE, 10)) / 10) - ((DELAY(CLOSE, 10) - CLOSE) / 10)) < (-1 * 0.1)), 1, ((-1 * 1) * (CLOSE - DELAY(CLOSE, 1))))
alpha050 = (-1 * TS_MAX(RANK(CORRELATION(RANK(VOLUME), RANK(VWAP), 5)), 5))
alpha051 = IF(((((DELAY(CLOSE, 20) - DELAY(CLOSE, 10)) / 10) - ((DELAY(CLOSE, 10) - CLOSE) / 10)) < (-1 * 0.05)), 1, ((-1 * 1) * (CLOSE - DELAY(CLOSE, 1))))
alpha052 = ((((-1 * TS_MIN(LOW, 5)) + DELAY(TS_MIN(LOW, 5), 5)) * RANK(((SUM(RETURNS, 240) - SUM(RETURNS, 20)) / 220))) * TS_RANK(VOLUME, 5))
alpha053 = (-1 * DELTA((((CLOSE - LOW) - (HIGH - CLOSE)) / (CLOSE - LOW)), 9))
alpha054 = ((-1 * ((LOW - CLOSE) * (OPEN**5))) / ((LOW - HIGH) * (CLOSE**5)))
alpha055 = (-1 * CORRELATION(RANK(((CLOSE - TS_MIN(LOW, 12)) / (TS_MAX(HIGH, 12) - TS_MIN(LOW, 12)))), RANK(VOLUME), 6))
alpha056 = (0 - (1 * (RANK((SUM(RETURNS, 10) / SUM(SUM(RETURNS, 2), 3))) * RANK((RETURNS * CAP)))))
alpha057 = (0 - (1 * ((CLOSE - VWAP) / DECAY_LINEAR(RANK(TS_ARGMAX(CLOSE, 30)), 2))))
alpha058 = (-1 * TS_RANK(DECAY_LINEAR(CORRELATION(INDUSTRY_NEUTRALIZE(VWAP), VOLUME, 4), 8), 6))
alpha059 = (-1 * TS_RANK(DECAY_LINEAR(CORRELATION(INDUSTRY_NEUTRALIZE(((VWAP * 0.728317) + (VWAP * (1 - 0.728317)))), VOLUME, 4), 16), 8))
alpha060 = (0 - (1 * ((2 * SCALE(RANK(((((CLOSE - LOW) - (HIGH - CLOSE)) / (HIGH - LOW)) * VOLUME)))) - SCALE(RANK(TS_ARGMAX(CLOSE, 10))))))
alpha061 = (RANK((VWAP - TS_MIN(VWAP, 16))) < RANK(CORRELATION(VWAP, ADV(180), 18)))
alpha062 = ((RANK(CORRELATION(VWAP, SUM(ADV(20), 22), 10)) < RANK(((RANK(OPEN) + RANK(OPEN)) < (RANK(((HIGH + LOW) / 2)) + RANK(HIGH))))) * -1)
alpha063 = ((RANK(DECAY_LINEAR(DELTA(INDUSTRY_NEUTRALIZE(CLOSE), 2), 8)) - RANK(DECAY_LINEAR(CORRELATION(((VWAP * 0.318108) + (OPEN * (1 - 0.318108))), SUM(ADV(180), 37), 14), 12))) * -1)
alpha064 = ((RANK(CORRELATION(SUM(((OPEN * 0.178404) + (LOW * (1 - 0.178404))), 13), SUM(ADV(120), 13), 17)) < RANK(DELTA(((((HIGH + LOW) / 2) * 0.178404) + (VWAP * (1 - 0.178404))), 4))) * -1)
alpha065 = ((RANK(CORRELATION(((OPEN * 0.00817205) + (VWAP * (1 - 0.00817205))), SUM(ADV(60), 9), 6)) < RANK((OPEN - TS_MIN(OPEN, 14)))) * -1)
alpha066 = ((RANK(DECAY_LINEAR(DELTA(VWAP, 4), 7)) + TS_RANK(DECAY_LINEAR(((((LOW * 0.96633) + (LOW * (1 - 0.96633))) - VWAP) / (OPEN - ((HIGH + LOW) / 2))), 11), 7)) * -1)
alpha067 = ((RANK((HIGH - TS_MIN(HIGH, 2)))**RANK(CORRELATION(INDUSTRY_NEUTRALIZE(VWAP), INDUSTRY_NEUTRALIZE(ADV(20)), 6))) * -1)
alpha068 = ((TS_RANK(CORRELATION(RANK(HIGH), RANK(ADV(15)), 9), 14) < RANK(DELTA(((CLOSE * 0.518371) + (LOW * (1 - 0.518371))), 1))) * -1)
alpha069 = ((RANK(TS_MAX(DELTA(INDUSTRY_NEUTRALIZE(VWAP), 3), 5))**TS_RANK(CORRELATION(((CLOSE * 0.490655) + (VWAP * (1 - 0.490655))), ADV(20), 5), 9)) * -1)
alpha070 = ((RANK(DELTA(VWAP, 1))**TS_RANK(CORRELATION(INDUSTRY_NEUTRALIZE(CLOSE), ADV(50), 18), 18)) * -1)
alpha071 = MAX(TS_RANK(DECAY_LINEAR(CORRELATION(TS_RANK(CLOSE, 3), TS_RANK(ADV(180), 12), 18), 4), 16), TS_RANK(DECAY_LINEAR((RANK(((LOW + OPEN) - (VWAP + VWAP)))**2), 16), 4))
alpha072 = (RANK(DECAY_LINEAR(CORRELATION(((HIGH + LOW) / 2), ADV(40), 9), 10)) / RANK(DECAY_LINEAR(CORRELATION(TS_RANK(VWAP, 4), TS_RANK(VOLUME, 19), 7), 3)))
# Modify DELTA(VWAP, 4.72775) to DELTA(VWAP, 5)
alpha073 = (MAX(RANK(DECAY_LINEAR(DELTA(VWAP, 5), 3)), TS_RANK(DECAY_LINEAR(((DELTA(((OPEN * 0.147155) + (LOW * (1 - 0.147155))), 2) / ((OPEN * 0.147155) + (LOW * (1 - 0.147155)))) * -1), 3), 17)) * -1)
alpha074 = ((RANK(CORRELATION(CLOSE, SUM(ADV(30), 37), 15)) < RANK(CORRELATION(RANK(((HIGH * 0.0261661) + (VWAP * (1 - 0.0261661)))), RANK(VOLUME), 11))) * -1)
alpha075 = (RANK(CORRELATION(VWAP, VOLUME, 4)) < RANK(CORRELATION(RANK(LOW), RANK(ADV(50)), 12)))
# Modify TS_RANK(CORRELATION(INDUSTRY_NEUTRALIZE(LOW), ADV(81), 8), 19.569) to TS_RANK(CORRELATION(INDUSTRY_NEUTRALIZE(LOW), ADV(81), 8), 20)
alpha076 = (MAX(RANK(DECAY_LINEAR(DELTA(VWAP, 1), 12)), TS_RANK(DECAY_LINEAR(TS_RANK(CORRELATION(INDUSTRY_NEUTRALIZE(LOW), ADV(81), 8), 20), 17), 19)) * -1)
alpha077 = MIN(RANK(DECAY_LINEAR(((((HIGH + LOW) / 2) + HIGH) - (VWAP + HIGH)), 20)), RANK(DECAY_LINEAR(CORRELATION(((HIGH + LOW) / 2), ADV(40), 3), 6)))
alpha078 = (RANK(CORRELATION(SUM(((LOW * 0.352233) + (VWAP * (1 - 0.352233))), 20), SUM(ADV(40), 20), 7))**RANK(CORRELATION(RANK(VWAP), RANK(VOLUME), 6)))
alpha079 = (RANK(DELTA(INDUSTRY_NEUTRALIZE(((CLOSE * 0.60733) + (OPEN * (1 - 0.60733)))), 1)) < RANK(CORRELATION(TS_RANK(VWAP, 4), TS_RANK(ADV(150), 9), 15)))
alpha080 = ((RANK(SIGN(DELTA(INDUSTRY_NEUTRALIZE(((OPEN * 0.868128) + (HIGH * (1 - 0.868128)))), 4)))**TS_RANK(CORRELATION(HIGH, ADV(10), 5), 6)) * -1)
alpha081 = ((RANK(LOG(PRODUCT(RANK((RANK(CORRELATION(VWAP, SUM(ADV(10), 50), 8))**4)), 15))) < RANK(CORRELATION(RANK(VWAP), RANK(VOLUME), 5))) * -1)
alpha082 = (MIN(RANK(DECAY_LINEAR(DELTA(OPEN, 1), 15)), TS_RANK(DECAY_LINEAR(CORRELATION(INDUSTRY_NEUTRALIZE(VOLUME), ((OPEN * 0.634196) + (OPEN * (1 - 0.634196))), 17), 7), 13)) * -1)
alpha083 = ((RANK(DELAY(((HIGH - LOW) / (SUM(CLOSE, 5) / 5)), 2)) * RANK(RANK(VOLUME))) / (((HIGH - LOW) / (SUM(CLOSE, 5) / 5)) / (VWAP - CLOSE)))
alpha084 = SIGNEDPOWER(TS_RANK((VWAP - TS_MAX(VWAP, 15)), 21), DELTA(CLOSE, 5))
alpha085 = (RANK(CORRELATION(((HIGH * 0.876703) + (CLOSE * (1 - 0.876703))), ADV(30), 10))**RANK(CORRELATION(TS_RANK(((HIGH + LOW) / 2), 4), TS_RANK(VOLUME, 10), 7)))
alpha086 = ((TS_RANK(CORRELATION(CLOSE, SUM(ADV(20), 15), 6), 20) < RANK(((OPEN + CLOSE) - (VWAP + OPEN)))) * -1)
alpha087 = (MAX(RANK(DECAY_LINEAR(DELTA(((CLOSE * 0.369701) + (VWAP * (1 - 0.369701))), 2), 3)), TS_RANK(DECAY_LINEAR(ABS(CORRELATION(INDUSTRY_NEUTRALIZE(ADV(81)), CLOSE, 13)), 5), 14)) * -1)
# Modify TS_RANK(ADV(60), 20.6966) to TS_RANK(ADV(60), 21),
alpha088 = MIN(RANK(DECAY_LINEAR(((RANK(OPEN) + RANK(LOW)) - (RANK(HIGH) + RANK(CLOSE))), 8)), TS_RANK(DECAY_LINEAR(CORRELATION(TS_RANK(CLOSE, 8), TS_RANK(ADV(60), 21), 8), 7), 3))
alpha089 = (TS_RANK(DECAY_LINEAR(CORRELATION(((LOW * 0.967285) + (LOW * (1 - 0.967285))), ADV(10), 7), 6), 4) - TS_RANK(DECAY_LINEAR(DELTA(INDUSTRY_NEUTRALIZE(VWAP), 3), 10), 15))
alpha090 = ((RANK((CLOSE - TS_MAX(CLOSE, 5)))**TS_RANK(CORRELATION(INDUSTRY_NEUTRALIZE(ADV(40)), LOW, 5), 3)) * -1)
alpha091 = ((TS_RANK(DECAY_LINEAR(DECAY_LINEAR(CORRELATION(INDUSTRY_NEUTRALIZE(CLOSE), VOLUME, 10), 16), 4), 5) - RANK(DECAY_LINEAR(CORRELATION(VWAP, ADV(30), 4), 3))) * -1)
alpha092 = MIN(TS_RANK(DECAY_LINEAR(AS_FLOAT((((HIGH + LOW) / 2) + CLOSE) < (LOW + OPEN)), 15), 19), TS_RANK(DECAY_LINEAR(CORRELATION(RANK(LOW), RANK(ADV(30)), 8), 7), 7))
alpha093 = (TS_RANK(DECAY_LINEAR(CORRELATION(INDUSTRY_NEUTRALIZE(VWAP), ADV(81), 17), 20), 8) / RANK(DECAY_LINEAR(DELTA(((CLOSE * 0.524434) + (VWAP * (1 - 0.524434))), 3), 16)))
alpha094 = ((RANK((VWAP - TS_MIN(VWAP, 12)))**TS_RANK(CORRELATION(TS_RANK(VWAP, 20), TS_RANK(ADV(60), 4), 18), 3)) * -1)
alpha095 = (RANK((OPEN - TS_MIN(OPEN, 12))) < TS_RANK((RANK(CORRELATION(SUM(((HIGH + LOW) / 2), 19), SUM(ADV(40), 19), 13))**5), 12))
# Modify TS_RANK(ADV(60), 4.13242) to TS_RANK(ADV(60), 4)
alpha096 = MAX(TS_RANK(DECAY_LINEAR(CORRELATION(RANK(VWAP), RANK(VOLUME), 4), 4), 8), TS_RANK(DECAY_LINEAR(TS_ARGMAX(CORRELATION(TS_RANK(CLOSE, 7), TS_RANK(ADV(60), 4), 4), 13), 14), 13)) * -1
# Modify TS_RANK(LOW, 7.87871) to TS_RANK(LOW, 8)
alpha097 = ((RANK(DECAY_LINEAR(DELTA(INDUSTRY_NEUTRALIZE(((LOW * 0.721001) + (VWAP * (1 - 0.721001)))), 3), 20)) - TS_RANK(DECAY_LINEAR(TS_RANK(CORRELATION(TS_RANK(LOW, 8), TS_RANK(ADV(60), 17), 5), 19), 16), 7)) * -1)
alpha098 = (RANK(DECAY_LINEAR(CORRELATION(VWAP, SUM(ADV(5), 26), 6), 7)) - RANK(DECAY_LINEAR(TS_RANK(TS_ARGMIN(CORRELATION(RANK(OPEN), RANK(ADV(15)), 21), 9), 7), 8)))
alpha099 = ((RANK(CORRELATION(SUM(((HIGH + LOW) / 2), 20), SUM(ADV(60), 20), 9)) < RANK(CORRELATION(LOW, VOLUME, 6))) * -1)
alpha100 = (0 - (1 * (((1.5 * SCALE(INDUSTRY_NEUTRALIZE(INDUSTRY_NEUTRALIZE(RANK(((((CLOSE - LOW) - (HIGH - CLOSE)) / (HIGH - LOW)) * VOLUME)))))) - SCALE(INDUSTRY_NEUTRALIZE((CORRELATION(CLOSE, RANK(ADV(20)), 5) - RANK(TS_ARGMIN(CLOSE, 30)))))) * (VOLUME / ADV(20)))))
alpha101 = ((CLOSE - OPEN) / ((HIGH - LOW) + .001))
```

## 技术指标

### 均线类指标

均线类指标计算逻辑请参考表格中的因子计算逻辑部分。可以直接使用 `get_factor` 函数调用，例如：

```python
# In
get_factor('000001.XSHE','MACD_DIFF','20200101','20200110')
# Out
2020-01-02 0.197243
2020-01-03 0.242007
2020-01-06 0.265545
2020-01-07 0.287342
2020-01-08 0.262057
2020-01-09 0.249631
2020-01-10 0.229073
Name: 000001.XSHE, dtype: float64
```

| 因子字段                   | 函数名与默认参数                                    | 因子计算逻辑                                                                                                                                                                                                                                           |
| :------------------------- | :-------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MACD_DIFF, MACD_DEA, MACD_HIST | 指数平滑移动平均线MACD SHORT = 12, LONG = 26, M = 9 | DIFF = EMA(CLOSE, SHORT) - EMA(CLOSE, LONG) <br> DEA = EMA(DIFF, M) <br> HIST = (DIFF - DEA) * 2                                                                                                                                                       |
| TRIX, MATRIX               | 三重指数平均移动平均TRIX M1 = 12, M2 = 20           | TRIX = (TR - REF(TR, 1)) / REF(TR, 1) * 100; <br> TR = EMA(EMA(EMA(CLOSE, M1), M1), M1) <br> MATRIX= MA(TRIX, M2)                                                                                                                                   |
| BOLL, BOLL_UP, BOLL_DOWN   | 布林带BOLL N = 20, P = 2                          | BOLL = MA(CLOSE, N) <br> BOLLUP = BOLL + STD(CLOSE, N) * P <br> BOLLDOWN = BOLL - STD(CLOSE, N) * P                                                                                                                                                   |
| ASI, ASIT                  | 震动升降指标ASI M1= 26, M2 = 10                     | LC = REF(CLOSE, 1) <br> AA = ABS(HIGH - LC) <br> BB = ABS(LOW - LC) <br> CC = ABS(HIGH - REF(LOW, 1)) <br> DD = ABS(LC - REF(OPEN, 1)) <br> R = IF((AA BB) & (AA CC), AA + BB / 2 + DD / 4, IF((BB CC) & (BB AA), BB + AA / 2 + DD / 4, CC + DD / 4)) <br> X = (CLOSE - LC + (CLOSE - OPEN) / 2 + LC - REF(OPEN, 1)) <br> SI = X _ 16 / R _ MAX(AA, BB) <br> ASI = SUM(SI, M1) <br> ASIT = MA(ASI, M2) |
| MA3, 5, 10, 20, 30, 55, 60, 120, 250 | 移动均线MA N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | MA3, 5, 10… = MA(CLOSE, N)                                                                                                                                                                                                                             |
| EMA3, 5, 10, 20, 30, 55, 60, 120, 250 | 指数移动均线EMA N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | EMA3, 5, 10... = EMA(CLOSE, N)                                                                                                                                                                                                                         |
| HMA3, 5, 10, 20, 30, 55, 60, 120, 250 | 高价平均线HMA N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | HMA3, 5, 10... = MA(HIGH, N)                                                                                                                                                                                                                           |
| LMA3, 5, 10, 20, 30, 55, 60, 120, 250 | 低价平均线LMA N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | LMA3, 5, 10… = MA(LOW, N)…                                                                                                                                                                                                                             |
| VMA3, 5, 10, 20, 30, 55, 60, 120, 250 | 变异平均线VMA N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | VV = (HIGH+OPEN+LOW+CLOSE)/4 <br> VMA3, 5, 10... = MA(VV, N)…                                                                                                                                                                                               |
| AMV3, 5, 10, 20, 30, 55, 60, 120, 250 | 成本均线AMV N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | AMOV = VOLUME * (OPEN + CLOSE) / 2 <br> AMV3, 5, 10… = SUM(AMOV, N) / SUM(VOLUME, N)…                                                                                                                                                                       |
| VOL3, 5, 10, 20, 30, 55, 60, 120, 250 | 平均换手率(%) VOL N: 3, 5, 10, 20, 30, 55, 60, 120, 250 | HSL =100* VOLUME / CAPITAL <br> VOL3, 5, 10... = MA(HSL, N) <br> HSL 代表换手率 <br> CAPITAL 代表流通股本                                                                                                                                                 |
| DAVOL5, 10, 20             | 平均换手率与120 日平均换手率比值DAVOL N: 5, 10, 20 | DAVOL3, 5... = VOLN / VOL120                                                                                                                                                                                                                           |
| BBI, BBIBOLL_UP, BBIBOLL_DOWN | 多空指标BBIBOLL M1 = 3, M2 = 6, M3 = 12, M4 = 24, M = 6, N = 11 | BBI = (MA(CLOSE, M1) + MA(CLOSE, M2) + MA(CLOSE, M3) + MA(CLOSE, M4)) / 4 <br> BBIBOLLUP = BBI + M * STD(BBI, N) <br> BBIBOLLDOWN = BBI - M * STD(BBI, N)"                                                                                             |
| DPO, MADPO                 | 区间震荡线DPO M1 = 20, M2 = 10, M = 6             | DPO = CLOSE - REF(MA(CLOSE, M1), M2) <br> MADPO = MA(DPO, M3)                                                                                                                                                                                          |
| MCST                       | 市场成本MCST                                        | MCST = DMA(AMOUNT / VOLUME, 100 * VOLUME / CAPITAL) <br> AMOUNT 代表成交额 <br> CAPITAL 代表流通股本                                                                                                                                                      |

### 超买超卖指标

超买超卖指标计算逻辑请参考下面表格中的因子计算逻辑部分。可以直接使用 `get_factor` 函数调用，例如：

```python
# In
get_factor('000001.XSHE','OBOS','20200401','20200408')
# Out
2020-04-01 2.0
2020-04-02 4.0
2020-04-03 2.0
2020-04-07 4.0
2020-04-08 2.0
Name: 000001.XSHE, dtype: float64
```

| 因子字段               | 函数名与默认参数                            | 因子计算逻辑                                                                                                                                                                |
| :--------------------- | :------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| OBOS                   | 超买超卖指标OBOS N = 10                     | 过去N 日股票上涨家数之和– 过去N 日股票下跌家数之和。 <br> 如果当日股票收盘价大于上一交易日股票收盘价，则该股票今日为上涨。                                                    |
| KDJ_K, KDJ_D, KDJ_J    | 随机波动指标KDJ N = 9, M1 = 3, M2 = 3       | RSV = (CLOSE - LLV(LOW, N)) / (HHV(HIGH, N) - LLV(LOW, N)) * 100 <br> K = EMA(RSV, (M1 * 2 - 1)) <br> D = EMA(K, (M2 * 2 - 1)) <br> J = K * 3 - D * 2             |
| RSI6, RSI10            | 相对强弱指标RSI N1 = 6, 10                  | LC = REF(CLOSE, 1) <br> RSI = MA(MAX(CLOSE - LC, 0), N) / MA(ABS(CLOSE - LC), N) * 100                                                                                 |
| WR                     | 威廉指标WR N = 10, L1 = 6                   | WR = (HHV(HIGH, N) - CLOSE) / (HHV(HIGH, N) - LLV(LOW, N)) * 100                                                                                                     |
| LWR1, LWR2             | LWR 威廉指标LWR N = 9, M1 = 3, M2 = 3       | RSV = (HHV(HIGH,N)-CLOSE)/(HHV(HIGH,N)-LLV(LOW,N))*100 <br> LWR1 = SMA_CN(RSV,M1,1) <br> LWR2 = SMA_CN(LWR1,M2,1)                                                 |
| BIAS5, BIAS10, BIAS20  | 乖离率BIAS L1 = 5, 10, 20                   | (CLOSE - MA(CLOSE, L1)) / MA(CLOSE, L1) * 100                                                                                                                         |
| BIAS36, BIAS612, MABIAS | 36 乖离BIAS36                               | BIAS36 = MA(CLOSE, 3) – MA(CLOSE, 6) <br> BIAS612 = MA(CLOSE, 6) – MA(CLOSE, 12) <br> MABIAS = MA(BIAS36, M)                                                          |
| ACCER                  | 幅度涨速ACCER N = 8                         | ACCER = SLOPE (CLOSE, N) / CLOSE                                                                                                                                      |
| CYF                    | 市场能量CYF N = 21                          | CYF = 100 – 100 / (1 + EMA(HSL, N ))                                                                                                                                  |
| SWL, SWS               | 分水岭FSL                                   | SWL = (EMA(CLOSE,5)7+EMA(CLOSE,10)3)/10 <br> SWS = DMA(EMA(CLOSE,12),MAX(1,100(SUM(VOLUME,5)/(3CAPITAL)))) <br> CAPITAL 代表流通股本                              |
| ADTM, MAADTM           | 动态买卖气指标ADTM N = 23, M = 8            | DTM = IF(OPEN<=REF(OPEN,1),0,MAX((HIGH-OPEN),(OPEN-REF(OPEN,1)))) <br> DBM = IF(OPEN>=REF(OPEN,1),0,MAX((OPEN-LOW),(OPEN-REF(OPEN,1)))) <br> STM = SUM(DTM,N) <br> SBM = SUM(DBM,N) <br> ADTM = IF(STM>SBM,(STM-SBM)/STM,IF(STM=SBM,0,(STM-SBM)/SBM)) <br> MAADTM = MA(ADTM, M) |
| TR, ATR                | 真实波幅ATR N = 14，M1 = 9                  | TR = SUM(MAX(MAX(HIGH - LOW, ABS(HIGH - REF(CLOSE, 1))), ABS(LOW - REF(CLOSE, 1))), M1) <br> ATR = MA(TR, N)                                                        |
| DKX, MADKX             | 多空线DKX M = 10                            | MID = (3CLOSE+LOW+OPEN+HIGH)/6 <br> DKX = (20MID+19REF(MID,1)+18REF(MID,2)+17REF(MID,3)+16REF(MID,4)+15REF(MID,5)+14REF(MID,6)+<br> 13REF(MID,7)+12REF(MID,8)+11REF(MID,9)+10REF(MID,10)+9REF(MID,11)+8REF(MID,12)+7REF(MID,13)+<br> 6REF(MID,14)+5REF(MID,15)+4REF(MID,16)+3REF(MID,17)+2REF(MID,18)+REF(MID,20))/210 <br> MADKX = MA(DKX, M) |
| TAPI, MATAPI           | 加权指数成交值TAPI M = 6                    | TAPI = AMOUNT / CLOSE <br> MATAPI = MA(TAPI, M) <br> AMOUNT 代表成交额                                                                                                 |
| OSC                    | 变动速率线OSC N = 10                        | 100 * (CLOSE – MA(CLOSE, N))                                                                                                                                          |
| CCI                    | 商品路径指标CCI N = 14                      | CCI = (TYP – MA(TYP, N)) / (0.015 * AVEDEV (TYP, N)) <br> TYP = (HIGH + LOW + CLOSE) / 3                                                                               |
| ROC                    | 变形率指标ROC N = 12                        | ROC = 100 * (CLOSE – REF(CLOSE, N) / REF(CLOSE, N)                                                                                                                    |
| MFI                    | 资金流量指标MFI N = 14                      | TYP = (HIGH + LOW + CLOSE) / 3 <br> V1 = SUM(IF(TYP > REF(TYP, 1), TYP * VOLUME, 0), N) / SUM(IF(TYP < REF(TYP, 1), TYP * VOLUME, 0), N) <br> MFI = 100 - ( 100 / ( 1 + V1 ) ) |
| MTM, MAMTM             | 动量线MTM N = 14                            | MTM = CLOSE – REF(CLOSE, N) <br> MAMTM = MA(MTM, M)                                                                                                                   |
| MARSI6, MARSI10        | 相对强弱平均线MARSI N = 6, 10               | LC = REF(CLOSE, 1) <br> RSI = SMA(MAX(CLOSE - LC, 0), N) / SMA(ABS(CLOSE - LC), N) * 100 <br> MARSI = MA(RSI, N)                                                    |
| SKD_K, SKD_D           | 慢速随机指标SKD N = 9, M = 3                | LOWV = LLV(LOW, N) <br> HIGHV = HHV(HIGH, N) <br> RSV = EMA((CLOSE – LOWV) / (HIGHV – LOWV) * 100, M) <br> SKD_K = EMA(RSV , M) <br> SKD_D = MA(SKD_K, M) |
| UDL, MAUDL             | 引力线UDL N1 = 3, N2 = 5, N3 = 10, N4 = 20, M =6 | UDL = (MA(CLOSE,N1)+MA(CLOSE,N2)+MA(CLOSE,N3)+MA(CLOSE,N4))/4 <br> MAUDL = MA(UDL,M)                                                                                 |
| DI1, DI2, ADX, ADXR    | 趋向指标DMI M1 = 14, M2 = 6                 | TR = SUM(MAX(MAX(HIGH - LOW, ABS(HIGH - REF(CLOSE, 1))), ABS(LOW - REF(CLOSE, 1))), M1) <br> HD = HIGH - REF(HIGH, 1) <br> LD = REF(LOW, 1) - LOW <br> DMP = SUM(IF((HD 0) & (HD LD), HD, 0), M1) <br> DMM = SUM(IF((LD 0) & (LD HD), LD, 0), M1) <br> DI1 = DMP * 100 / TR <br> DI2 = DMM * 100 / TR <br> ADX = MA(ABS(DI2 - DI1) / (DI1 + DI2) * 100, M2) <br> ADXR = (ADX + REF(ADX, M2)) / 2" |

### 能量指标

能量指标计算逻辑请参考下面表格中的因子计算逻辑部分。可以直接使用 `get_factor` 函数调用，例如：

```python
# In
get_factor('000006.XSHE',['AR', 'BR'],'20200501','20200510')
# Out
AR BR
order_book_id date
000006.XSHE 2020-05-06 110.555556 116.022099
2020-05-07 110.497238 118.435754
2020-05-08 107.777778 112.154696
```

| 因子字段                   | 函数名与默认参数                    | 因子计算逻辑                                                                                                                                                                |
| :------------------------- | :---------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| AR, BR                     | 人气意愿指标ARBR M1 = 26            | AR = SUM(HIGH - OPEN, M1) / SUM(OPEN - LOW, M1) * 100 <br> BR = SUM(MAX(0, HIGH - REF(CLOSE, 1)), M1) / SUM(MAX(0, REF(CLOSE, 1) - LOW), M1) * 100                           |
| VR, MAVR                   | 容量比例VR M1 = 26, M = 6           | LC = REF(CLOSE, 1) <br> VR = SUM(IF(CLOSE LC, VOL, 0), M1) / SUM(IF(CLOSE <= LC, VOL, 0), M1) * 100 <br> MAVR = MA(VR, M)                                                  |
| CR,MACR1, MACR2, MACR3, MACR4 | CR 指标CR N = 26, M1 = 10, M2 = 20, M3 = 40, M4 = 62 | MID = REF(HIGH+LOW, 1) / 2 <br> CR = SUM(MAX(0,HIGH-MID),N)/SUM(MAX(0,MID-LOW),N)*100 <br> MACR1 = REF(MA(CR,M1),1+M1/2.5) <br> MACR2 = REF(MA(CR,M2),1+M2/2.5) <br> MACR3 = REF(MA(CR,M3),1+M3/2.5) <br> MACR4= REF(MA(CR,M4),1+M4/2.5) |
| MASS, MAMASS               | 梅斯线MASS N1 = 9, N2 = 25, M = 6   | MASS = SUM(MA(HIGH-LOW,N1)/MA(MA(HIGH-LOW,N1),N1),N2) <br> MAMASS = MA(MASS, M)                                                                                           |
| SY                         | 心理线SY N = 9                      | SY = COUNT(CLOSE>REF(CLOSE,1),N)/N*100                                                                                                                                |
| PCNT                       | 幅度比PCNT                          | PCNT = (CLOSE-REF(CLOSE,1))/CLOSE*100;                                                                                                                                |
| CYR, MACYR                 | 市场强弱CYR N = 13, M = 5           | DIVE = 0.01*EMA(AMOUNT,N)/EMA(VOLUME,N) <br> CYR = (DIVE/REF(DIVE,1)-1)*100 <br> MACYR = MA(CYR, M) <br> AMOUNT 代表成交额                                             |
| AMP1,AMP3,AMP5, AMP10,AMP20,AMP60 | 振幅AMP N:1，3，5，10，20，60     | AMP1,3,5… = (HHV(HIGH,N)-LLV(LOW,N))/REF(CLOSE,N)                                                                                                                     |
| WMA3,WMA5,WMA10,WMA20, WMA60,WMA120,WMA250 | 加权移动平均线WMA N:3，5，10，20，60，120，250 | WMA1,3,5… = (CLOSE*N+REF(CLOSE, 1)*(N-1)+…+REF(CLOSE, N-1)/(1+2+…+N))                                                                                                 |
| VOLT20, VOLT60             | 近20 日/60 日波动率N:20,60          | 20 日/60 日收盘价的标准差                                                                                                                                             |
| MDD20，MDD60               | 近20 日/60 日最大回撤N:20,60        | 20 日/60 日收盘价的最大回撤                                                                                                                                           |
| AROON_UP，AROON_DOWN       | 阿隆指标N=14                        | AROON_UP = [(计算期天数-最高价后的天数)/计算期天数]*100 <br> AROON_DOWN = [(计算期天数-最低价后的天数)/计算期天数]*100                                                   |
| QTYR_5_20                  | 5 日20 日量比N=5, M=20              | MA(VOLUME, N) / MA(VOLUME, M)                                                                                                                                         |
| OBV                        | 能量潮OBV                           | OBV=REF(OBV, 1) + sgn × VOLUME <br> 其中，sgn 是符号函数，其数值由下式决定： <br> sgn=1 , CLOSE>REF(CLOSE, 1) <br> sgn=0, CLOSE = REF(CLOSE, 1) <br> sgn=-1 , CLOSE< REF(CLOSE, 1) |

