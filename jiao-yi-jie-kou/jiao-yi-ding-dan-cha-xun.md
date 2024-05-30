---
description: 调用该接口，查询交易订单交易结果及订单明细信息。 注意：此接口仅支持查询近45天内的交易订单，超过45天的订单请联系运营查询。
---

# 交易订单查询

### 支持请求地址 <a href="#bzwdg" id="bzwdg"></a>

| **生产环境** | 联系商务获取 |
| -------- | -------------------------------- |
| **请求方式** | 支持POST/GET 任意方式请求                |

### 请求参数说明 <a href="#eo5ea" id="eo5ea"></a>

**公共请求参数**

<table data-full-width="false"><thead><tr><th align="center">参数</th><th align="center">类型</th><th align="center">是否必填</th><th align="center">参数说明</th></tr></thead><tbody><tr><td align="center">app_id</td><td align="center">String</td><td align="center">是</td><td align="center">商户应用号</td></tr><tr><td align="center">version</td><td align="center">String</td><td align="center">是</td><td align="center">接口版本：v1.0.0</td></tr><tr><td align="center">charset</td><td align="center">String</td><td align="center">是</td><td align="center">UTF-8</td></tr><tr><td align="center">sign_type</td><td align="center">String</td><td align="center">是</td><td align="center">签名类型：RSA，支持RSA、SM2</td></tr><tr><td align="center">sign</td><td align="center">String</td><td align="center">是</td><td align="center">签名字符串，再用Base64编码</td></tr><tr><td align="center">service_no</td><td align="center">String</td><td align="center">是</td><td align="center">服务编码：queryUnifyOrder</td></tr><tr><td align="center">biz_req_body</td><td align="center">String</td><td align="center">是</td><td align="center">业务请求参数的集合，JSON格式</td></tr></tbody></table>

**业务请求参数（biz\_req\_body）**

|       参数       |   类型   | 是否必填 （必填一项） |             参数说明            |
| :------------: | :----: | :---------: | :-------------------------: |
|    trade\_no   | String |      否      | 商户系统生成的用户编号，**须保证在商户端不重复**， |
| out\_trade\_no | String |      否      |  商户系统生成的订单号，**须保证在商户端不重复**， |

**请求示例**

```
{
  "app_id": "app_id",
  "version": "1.0",
  "charset": "utf-8",
  "sign_type": "RSA",
  "sign": "",
  "service_no": "queryUnifyOrder",
  "biz_req_body": "{\"out_trade_no\":\"商户订单号\"}"
}
```

### 响应参数说明 <a href="#s4wck" id="s4wck"></a>

#### **公共响应参数** <a href="#s4wck" id="s4wck"></a>

| **参数** | **类型** | **是否必填** | **参数说明**         |
| ------ | ------ | -------- | ---------------- |
| code   | String | 是        | 响应码              |
| msg    | String | 否        | 响应码说明            |
| data   | JSON   | 否        | 业务响应参数的集合，JSON数据 |
| sign   | String | 是        | 签名字符串，再用Base64编码 |

**业务响应参数（data）**

| **参数**          | **类型** | **是否必填** | **参数说明**                                                                                                                 |
| --------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| trade\_no       | String | 是        | 交易流水号                                                                                                                    |
| out\_trade\_no  | String | 是        | 商户系统生成的订单号                                                                                                               |
| gmt\_create     | Data   | 是        | 交易创建时间                                                                                                                   |
| trade\_status   | String | 是        | 交易状态：WAIT\_BUYER\_PAY（交易创建，等待买家付款）、TRADE\_CLOSED（未付款交易超时关闭，或支付完成后全额退款）、TRADE\_SUCCESS（交易支付成功）、TRADE\_FINISHED（交易结束，不可退款） |
| total\_amount   | Number | 是        | <p>订单总金额。<br>单位为元，精确到小数点后两位，取值范围：[0.01,100000000] 。</p>                                                                  |
| receipt\_amount | Number | 是        | 实收金额                                                                                                                     |
| pay\_mode       | String | 否        | 支付方式                                                                                                                     |
| remark          | String | 否        | 备注                                                                                                                       |

**响应示例**&#x20;

```
{
    "time_stamp": 1698393791854,
    "data": {
        "trade_no": "C2023102705180002",
        "out_trade_no": "10808998921809",
        "trade_status": "WAIT_BUYER_PAY",
        "total_amount": 0.20,
        "receipt_amount": 0.20,
        "pay_mode": "06030003",
        "remark": null
    },
    "code": 200,
    "msg": "成功",
    "sign": "sign"
}
```

### 业务响应码说明 <a href="#grogv" id="grogv"></a>

| **响应码(code)** | **响应码说明(msg)** |
| ------------- | -------------- |
| 200           | 业务受理成功         |
| 3000          | 商户未开通此权限       |
| 3001          | 请求数据验签失败       |
| 3003          | 订单失败，具体看msg提示  |

###
