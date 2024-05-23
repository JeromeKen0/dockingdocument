---
description: 调用该接口，生成支付链接，跳转到银行网银页面进行支付。 支持：PC网银、手机网银
---

# 交易下单

### 用户请求地址 <a href="#rpuji" id="rpuji"></a>

| **生产环境** | http://pay.zrlive369.com/rhp/pay |
| -------- | -------------------------------- |
| **请求方式** | **支持POST/GET 任意方式请求**            |

### 请求参数说明 <a href="#eo5ea" id="eo5ea"></a>

**公共请求参数**

| **参数**         | **类型** | **是否必填** | **参数说明**                                        |
| -------------- | ------ | -------- | ----------------------------------------------- |
| app\_id        | String | 是        | 商户应用号                                           |
| version        | String | 是        | 接口版本：v1.0.0                                     |
| charset        | String | 是        | UTF-8                                           |
| sign\_type     | String | 是        | 签名类型：RSA，支持RSA、SM2                              |
| sign           | String | 是        | 签名字符串，再用Base64编码                                |
| service\_no    | String | 是        |  银联扫码：netpay 银联快捷：quickbindpay 支付宝扫码：alipayscan |
| biz\_req\_body | String | 是        | 业务请求参数的集合，JSON格式                                |

**业务请求参数（biz\_req\_body）**

| **参数**         | **类型**      | **是否必填** | **参数说明**                                     |
| -------------- | ----------- | -------- | -------------------------------------------- |
| user\_id       | String      | 是        | 商户系统生成的用户编号，**须保证在商户端不重复**，                  |
| out\_trade\_no | String      | 是        | 商户系统生成的订单号，**须保证在商户端不重复**，                   |
| amount         | String      | 是        | <p>交易金额，单位：元</p><p>取值范围：0.01-99999999.99</p> |
| order\_desc    | String      | 是        | 商品名称/订单标题                                    |
| remark         | String      | 是        | 用户实名                                         |
| extend         | String      | 否        | 扩展参数                                         |
| time\_out      | String      | 否        | <p>订单有效期，单位：分钟</p><p>默认值：20</p>              |
| notify\_url    | String      | 是        | 支付成功结果异步通知地址，为空则不通知                          |
| return\_url    | String      | 否        | 重定向链接，支付成功后重定向到此链接                           |

**请求示例**&#x20;

```
{
    "charset": "UTF-8",
    "biz_req_body": "{\"amount\":\"50\",\"notify_url\":\"回调地址\",\"order_desc\":\"订单备注\",\"out_trade_no\":\"OrderNo\",\"remark\":\"用户实名\",\"return_url\":\"回调地址\",\"time_out\":20,\"user_id\":\"1760615303131168768\"}",
    "version": "v1.0.0",
    "service_no": "netpay",
    "app_id": "appId",
    "sign_type": "RSA2",
    "sign": "QRUzaczJvwJpEsQ6h29EbsoIPV2Zoli13NACwlRlVsehSrS yUC7NqF4tFM0lBFLcGhBtst4Iw13q3RHbIvnWsIfd7qNCGxWk1y4cDVDvQu15YrvdDyYMeFZTXrUCPPh/EqkdzShakrJywvzJlZyWQZYPMFLdo v9GUwIlBVT3pGUoQeSU2o8pcTclEWp9AEwnCWQ5OllvMKKdCuPi0aKK6rxjmNcHPukqDImFhfVUnoSrRL tmKXg5PIWaykM2iKIAa8RQry2c7HO5Uel5g7Ub3vk0UaizMGq5sb NrHLhvlvAtEpTDwJPZd5iJTheEPmtkJ7vhJM9dX B0L6Q8BQ=="
}
```

### 响应参数说明 <a href="#s4wck" id="s4wck"></a>

**公共响应参数**

| **参数**      | **类型** | **是否必填** | **参数说明**         |
| ----------- | ------ | -------- | ---------------- |
| code        | String | 是        | 响应码              |
| msg         | String | 是        | 响应码说明            |
| data        | JSON   | 是        | 业务响应参数的集合，JSON数据 |
| sign        | String | 否        | 签名字符串，再用Base64编码 |
| time\_stamp | String | 是        | 响应时间戳            |

**业务响应参数（data）**

| **参数**         | **类型** | **是否必填** | **参数说明**                                    |
| -------------- | ------ | -------- | ------------------------------------------- |
| trade\_no      | String | 是        | 交易流水号                                       |
| out\_trade\_no | String | 是        | 商户系统生成的订单号                                  |
| amount         | String | 是        | 交易金额，单位：元                                   |
| pay\_url       | String | 否        | <p>支付链接，用于跳转银行网银支付页面</p><p>链接默认有效期为20分钟</p> |

### 响应成功示例&#x20;

```
{
    "code": 200,
    "msg": "下单成功",
    "data": {
        "trade_no": "20240523134146439912",
        "out_trade_no": "aac74e19-4f6c-41e7-987a-8eee1505686b",
        "total_amount": "500",
        "pay_url": "https://pay.zrlive369.com/rhp/cashier-unionpayScan?OrderNo=202405231341464399"
    },
    "time_stamp": 1716442906951,
    "sign": "C1NJ0Rf2nMGGQ5blZ8l+VBiEXeW1HhlxCrI7h34Ee7AkyX5MISKHHbfpZCOD2whP6TfsPufY5qzjXljZ79i2yNVi/bi/3Lnt1TDjuBM1hk6rg7/93Fj1FniENe5R3Q8rwvxGMMlh/EAlTJA2vuqxTn8bwDa8+K4oFFEJ8QQaPOrTxAjkaYHODWIBQb5r9TuJKx0N3+qJ2/rzj4HzEf/4Lh2krvEPAb/iRMi71tG1WprTqHTDGqO1Sf+vzC2UsyHGPc3GXUaAxu7ln00EVUL9RqbsfjgXzW8R4faT8QDiQn+X3yqk1cVB7nzhUjnefDNCgD+deNqb9aBs0OoIlcOPQg=="
}
```

### 业务响应码说明 <a href="#grogv" id="grogv"></a>

| **响应码(code)** | **响应码说明(msg)** |
| ------------- | -------------- |
| 200           | 业务受理成功         |
| 3000          | 商户未开通此权限       |
| 3001          | 请求数据验签失败       |
| 3003          | 订单失败，具体看msg提示  |



\
