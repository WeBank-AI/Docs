# 发票 OCR 请求接口文档

**版本记录**
 
| 版本 | 更新内容 |
| ---- | ---- |
| 20200115 | 初稿 |
| 20200225 | 增加支持的发票种类 |
| 20200303 | 增加发票验真接口 |
| 20200309 | 更新发票验真接口 |
| 20200413 | 更新机票行程单返回接口，新增新车发票、二手车发票 |

# I. 发票识别接口

## 1. 接口信息

 * 接口：https://fis.webank.com/saas/ocr/invoice
 * 类型: POST

## 2. 请求参数

请求包体为 **JSON** 格式。

### 2.1 参数列表

| 参数名称 | 参数类型 | 参数含义 |
| ---- | ---- | ---- |
| image | String | Base64 编码的图片 |
| request_id | String | 请求ID，在结果中会直接返回 |

### 2.2 请求样例


```
{
  "image" : "xxx",
  "request_id" : "xxx"
}
```

## 3. 返回结果

OCR 结果返回为 JSON 字符串，UTF-8 编码。

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| error | | 错误码，详见后文 | 0/1/2 |
| request_id | | 请求ID | |
| stats | | 服务端统计信息 | |
| | __version__ | OCR 服务版本信息 | 20200107 |
| invoices | | JSONArray 类型，返回识别结果中的发票列表，其中每张发票对应一个 JSONObject，具体内容见后文 | JSONArray |
| | name | 发票类型名称，见 3.1.1 | VAT_invoice |
| | result | 各类型发票识别结果 | JSONObject |

### 3.1 发票识别结果字段

#### 3.1.1 发票类型名称

| 发票类型名称 | 发票名称 | 
| ---- | ---- |
| `VAT_special_invoice` | 增值税专用发票 |
| `VAT_invoice` | 增值税普通发票 |
| `VAT_e-invoice` | 增值税电子普通发票 |
| `VAT_v-invoice` | 增值税普通发票（卷票） |
| `train_ticket` | 火车票 |
| `taxi_invoice` | 出租车发票 |
| `flight_itinerary` | 飞机票行程单 |
| `passenger_invoice` | 公路客运发票 |
| `tolls_invoice` | 高速公路过路费发票 |
| `quota_invoice` | 通用定额发票 |
| `general_invoice` | 通用机打发票 |
| `vehicle_invoice` | 新车增值税发票 |
| `second_hand_vehicle_invoice` | 二手车增值税发票 |

#### 3.1.2 增值税专用发票与增值税普通发票

该系列包含增值税专用发票（`VAT_special_invoice`）、增值税普通发票（`VAT_invoice`），具体包含字段信息如下：

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_name | | 发票名称 | 北京增值税电子普通发票 |
| invoice_code | | 发票代码 | 011001900611 |
| invoice_id | | 发票号码 | 33058151 |
| invoice_date | | 开票日期 | 2018年12月31日 |
| invoice_form | | 发票联 | |
| check_code | | 校验码 | 02512696691240914194 |
| machine_code | | 机器编号 | 499099835387 |
| password_area | | 密码区域 | |
| total_amount | | 合计金额 | 100.00 |
| total_taxes | | 合集税额 | 10.00 |
| price_plus_taxes_in_words | | 价税合计（大写） | 贰佰贰伍拾元 |
| price_plus_taxes | | 价税合计（小写） | 110.00 |
| buyer_name | | 购买方-名称 | 深圳前海微众银行股份有限公司 |
| buyer_id | | 购买方-纳税人识别号 | 9144030031977063XH |
| buyer_address | | 购买方-地址、电话 | 深圳市南山区沙河西路、0755-12345678 |
| buyer_bank | | 购买方开户行及账号 | 微众银行0000 |
| seller_name | | 销售方-名称 | 深圳前海微众银行股份有限公司 |
| seller_id | | 销售方-纳税人识别号 | 9144030031977063XH |
| seller_address | | 销售方-地址、电话 | 深圳市南山区沙河西路、0755-12345678 |
| seller_bank | | 销售方-开户行及账号 | 微众银行0000 |
| payee | | 收款人 | 张三 |
| reviewer | | 复核 | 李四 |
| drawer | | 开票人 | 王五 | 
| details | | 明细列表，JSONArray 类型，其中每一项明细为一个 JSONObject | |
| | services_of_goods_or_taxable | 货物或应税劳务、服务名称 ｜ 餐饮服务 ｜
| | specifications | 规格型号 | |
| | unit | 单位 | |
| | quality | 数量 | |
| | unit_price | 单价 | |
| | amount | 金额 | |
| | tax_rate | 税率 | |
| | tax_amount | 税额 | | 

#### 3.1.3 增值税电子普通发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_name | | 发票名称 | 北京增值税电子普通发票 |
| invoice_code | | 发票代码 | 011001900611 |
| invoice_id | | 发票号码 | 33058151 |
| invoice_date | | 开票日期 | 2018年12月31日 |
| check_code | | 校验码 | 02512696691240914194 |
| machine_code | | 机器编号 | 499099835387 |
| password_area | | 密码区域 | |
| total_amount | | 合计金额 | 100.00 |
| total_taxes | | 合集税额 | 10.00 |
| price_plus_taxes_in_words | | 价税合计（大写） | 贰佰贰伍拾元 |
| price_plus_taxes | | 价税合计（小写） | 110.00 |
| buyer_name | | 购买方-名称 | 深圳前海微众银行股份有限公司 |
| buyer_id | | 购买方-纳税人识别号 | 9144030031977063XH |
| buyer_address | | 购买方-地址、电话 | 深圳市南山区沙河西路、0755-12345678 |
| buyer_bank | | 购买方-开户行及账号 | 微众银行0000 |
| seller_name | | 销售方-名称 | 深圳前海微众银行股份有限公司 |
| seller_id | | 销售方-纳税人识别号 | 9144030031977063XH |
| seller_address | | 销售方-地址、电话 | 深圳市南山区沙河西路、0755-12345678 |
| seller_bank | | 销售方-开户行及账号 | 微众银行0000 |
| payee | | 收款人 | 张三 |
| reviewer | | 复核 | 李四 |
| drawer | | 开票人 | 王五 | 
| details | | 明细列表，JSONArray 类型，其中每一项明细为一个 JSONObject | |
| | services_of_goods_or_taxable | 货物或应税劳务、服务名称 ｜ 餐饮服务 ｜
| | specifications | 规格型号 | |
| | unit | 单位 | |
| | quality | 数量 | |
| | unit_price | 单价 | |
| | amount | 金额 | |
| | tax_rate | 税率 | |
| | tax_amount | 税额 | | 

#### 3.1.4 增值税普通发票（卷票）

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 | 011001900611 |
| invoice_id | | 发票号码 | 33058151 |
| invoice_date | | 开票日期 | 2018年12月31日 |
| check_code | | 校验码 | 02512696691240914194 |
| password_area | | 密码区域 | |
| total_amount | | 合计金额 | 100.00 |
| total_taxes | | 合集税额 | 10.00 |
| price_plus_taxes_in_words | | 价税合计（大写） | 贰佰贰伍拾元 |
| price_plus_taxes | | 价税合计（小写） | 110.00 |
| buyer_name | | 购买方-名称 | 深圳前海微众银行股份有限公司 |
| buyer_id | | 购买方-纳税人识别号 | 9144030031977063XH |
| seller_name | | 销售方-名称 | 深圳前海微众银行股份有限公司 |
| seller_id | | 销售方-纳税人识别号 | 9144030031977063XH |

#### 3.1.5 出租车发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 | 011001900611 |
| invoice_id | | 发票号码 | 33058151 |
| invoice_date | | 开票日期 | 2018-12-31 |
| invoice_area | | 开票区域 | 深圳市 |
| amount | | 金额 | 50.00 |
| pick_up_time | | 上车时间 | 09:00 |
| drop_off_time | | 下长时间 | 10:00 |
| waiting_time | | 等候时间 | 00:10:00 |
| mileage | | 里程 | 10.2 |

#### 3.1.6 火车票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_id | | 车票号码 | D23402 |
| departure_datetime | | 开车日期和时间 | 2018年12月31日20:00开 |
| amount | | 票价 | 70.00 |
| train_no | | 车次 | G6001 |
| departure_station | | 出发车站 | 长沙南 |
| terminal_station | | 到达车站 | 深圳北 |
| seat_no | | 座位号 | 01车01A号 |
| seat_class | | 座席类型 | 二等座 |
| entrance | | 检票口 | 检票:01 |
| purchasing | | 支付方式 | 网 |
| traveller_id_name | | 旅客身份证号和姓名 | 1100002000***\*1234张三 |
| ticketing_id | | 售票流水号 | |
| ticketing_station | | 售票站点 | |

#### 3.1.7 飞机票行程单

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| traveller_name | | 旅客姓名 | |
| traveller_id | | 有效身份证件号码 | | 
| total_amount | | 合计金额 | |
| insurance | | 保险金额 | |
| fare | | 机票金额 ｜ ｜
| fuel_surcharge | | 燃油附加费金额 | |
| other_taxes | | 其他税费 | |
| e_ticket_no | | 电子客票号码 | |
| flights | | 行程明细 | |
| | departure_date | 出发日期 | |
| | departure_time | | 出发时间 | |
| | flight_no | | 航班号 | |
| | seat_class | | 座位等级 | |
| | departure | | 出发地机场 | |
| | terminal | | 目的地机场 | |
| | fare_basis | | 客票级别/客票类型 | |

#### 3.1.8 公路客运发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 |  |
| invoice_id | | 发票号码 |  |
| traveller_name | | 旅客姓名 | |
| departure_date | | 出发日期 | |
| departure_time | | 出发时间 | |
| departure_station | | 出发车站 | |
| terminal_station | | 到达车站 | |
| total_amount | | 票价 | |

#### 3.1.9 高速公路过路费发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 |  |
| invoice_id | | 发票号码 |  |
| invoice_date | | 出发日期 | |
| invoice_time | | 出发时间 | |
| entrance | | 高速入口站点 | |
| exit | | 高速出口站点 | |
| total_amount | | 票价 | |

#### 3.1.10 通用定额发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 |  |
| invoice_id | | 发票号码 |  |
| total_amount | | 票价 | |

#### 3.1.11 通用机打发票

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_code | | 发票代码 |  |
| invoice_id | | 发票号码 | |
| invoice_date | | 开票日期 | |
| total_amount | | 票价 | |

#### 3.1.12 新车、二手车增值税发票

该系列包含新车增值税发票（`vehicle_invoice`）、二手车增值税普通发票（`second_hand_vehicle_invoice`），具体包含字段信息如下：

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| invoice_name | | 发票名称 |  |
| invoice_code | | 发票代码 |  |
| invoice_id | | 发票号码 |  |
| invoice_date | | 开票日期 | |
| invoice_form | | 发票联 | |
| check_code | | 校验码 |  |
| machine_code | | 机器编号 |  |
| machine_print_code | | 机打代码 | |
| machine_print_id | | 机打号码 | |
| total_taxes | | 增值税税额 | |
| tax_rate | | 增值税税率 | |
| total_amount | | 不含税价格 | |
| price_plus_taxes_in_words | | 价税合计大写 | |
| price_plus_taxes | | 价税合计小写 | | 
| buyer_name | | 购买方名称 | |
| buyer_id | | 购买方身份证号码或组织机构代码 | |
| buyer_address | | 购买方住址 | |
| buyer_phone | | 购买方电话 | |
| seller_name | | 销售方名称 | |
| seller_id | | 销售方纳税人识别号 | |
| seller_address | | 销售方地址 | | 
| seller_phone | | 销售方电话 | |
| seller_bank_account | | 销售方帐号 | |
| seller_bank | | 销售方开户行 | 
| vehicle_type | | 车辆类型 | |
| vehicle_model | | 厂牌型号 | |
| vehicle_engine_id | | 发动机号码 | |
| vin | | 车辆识别代码 | |
| vehicle_license_no | | 车牌号码（二手车发票）| |
| production_place | | 产地 | |
| certificate_id | | 合格证号 | |
| import_certificate_id | | 进口证明书号 | |
| inspection_certificate_id | | 商检单号 | |
| password_area | | 税控码 | |
| taxer_name | | 主管税务机关 | |
| taxer_id | | 主管税务机关代码 | |
| registry_certificate_id | | 车辆登记证号（二手车发票）| |
| vehicle_management_name | | 转入地车辆管理所名称（二手车发票） | |

### 3.2 错误码

| 错误码 | 错误信息 | 
| ---- | ---- |
| 0 | 识别正常 |
| 1 | 服务内部错误 |
| 2 | HTTP 请求参数错误 |
| 3 | 图片解码错误 |

# II. 发票验真接口

发票验真依赖于国家税务提供的验真系统，该系统对发票有以下要求：

1. 支持增值税专用发票、增值税普通发票、增值税电子普通发票 及 增值税卷票；
2. 发票开票日期需在一年内；
3. 单张发票单日验真次数上限为5次。

## 1. 接口信息

 * 地址: https://wes.webank.com/ocr_invoice/verify
 * 类型: POST

## 2. 请求参数

请求包体为 **JSON** 格式。

### 2.1 参数列表

| 一级参数 | 二级参数 | 参数类型 | 参数含义 | 备注 |
| ---- | ---- | ---- | ---- | ---- |
| invoice | | JSONObject | 待验真的发票信息 | |
| | name | String | 发票名称，如 `VAT_special_invoice`, `VAT_invoice`, `VAT_e-invoice` 及 `VAT_v-invoice` | |
| | invoice_code | String | 发票代码，如 `044031900211` | |
| | invoice_id | String | 发票号码，如 `04296665` | |
| | invoice_date | String | 开票日期，如 `2020年1月10日` | |
| | total_amount | String | 合计金额（**不含税**）， 如 `100.50` | 普票可不传该参数 |
| | check_code | String | 校验码，如 `02512696691240914194` | 可只传后六位，专票可不传该参数 |
| request_id | | String | 请求ID，在结果中会直接返回 | |

### 2.2 请求样例


```
{
  "invoice" : {
      "name": "VAT_e-invoice", 
      "invoice_code": "044031900211",       
      "invoice_id": "04296665",
      "invoice_date": "2019年12月24日",
      "check_code": "56085752562565181941",
      "total_amount": "720.58"
  },
  "request_id" : "xxx"
}
```

## 3. 返回结果

发票验真结果返回为 JSON 字符串，UTF-8 编码。

| 一级 | 二级参数 | 参数含义 | 参数样本 |
| ---- | ---- | ---- | ---- |
| error | | 错误码，详见后文 #3.1 | 0/1/2/3 |
| request_id | | 请求ID | |
| stats | | 服务端统计信息 | JSONObject |
| | __version__ | OCR 服务版本信息 | 20200107 |
| result | | JSONObject 类型，返回验真结果及发票信息 | JSONObject |
| | verfication_code | 验真结果码，详见后文 #3.2 | 0/10/20 |
| | verfication_count | 该票据当日验真次数 | 0 |
| | invoice | JSONObject 类型，返回验真系统中票面真实信息，字段名称请参考发票 OCR 接口中各类票据的返回结果 |

### 3.1 错误码

| 错误码 | 错误信息 | 
| ---- | ---- |
| 0 | 识别正常 |
| 1 | 服务内部错误 |
| 2 | HTTP 请求参数错误 |

### 3.2 验真结果码

| 验真结果码 | 验真结果 | 错误样例 |
| ---- | ---- | ---- |
| 0 | 识别成功 |
| 10 | 不支持的发票类型 | 如传入机票行程单 `flight_itinerary` |
| 11 | 不支持开票日期为1年以前的发票 | 如开票日期为 `2018年11月11日` |
| 12 | 该发票超过当日验真次数 | |
| 20 | 发票明细比对错误 | 如传入的开票日期与实际发票日期不相符 | 
| 21 | 发票不存在 | 如传入的发票代码对应的发票不存在 ｜
| 30 | 传入的发票信息不规范 | 如传入的发票代码不符合校验规则 |
