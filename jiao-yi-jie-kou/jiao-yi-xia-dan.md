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
    "biz_req_body": "{\"amount\":\"168.00\",\"out_trade_no\":\"\",\"user_id\":\"13429\",\"order_desc\":\"\",\"notify_url\":\"\"}",
    "sign": "",
    "service_no": "netpay",
    "app_id": "app_id",
    "version": "v1.0.0",
    "sign_type": "RSA2"
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
    "time_stamp": 1698392800443,
    "data": {
        "trade_no": "C2023102715460001",
        "out_trade_no": "10808998921809",
        "total_amount": 0.2,
        "pay_url": "https://sandcash.mixienet.com.cn/pay/h5/fastpayment?version=10&mer_no=6888802123486&mer_order_no=C2023102715460001&create_time=20231027154640&expire_time=20231027164640&order_amt=0.2&notify_url=http%3A%2F%2Fpay.zrlive369.com%2Fcash%2Fcallback%2FsandToCallback&return_url=http%3A%2F%2Fwww.baidu.com&create_ip=127_0_0_1&goods_name=%E5%95%86%E5%93%81%E5%90%8D%E7%A7%B0&store_id=000000&product_code=05030001&clear_cycle=3&pay_extra=%7B%22userId%22%3A%222022%22%7D&meta_option=%5B%7B%22s%22%3A%22Android%22,%22n%22%3A%22wxDemo%22,%22id%22%3A%22com.pay.paytypetest%22,%22sc%22%3A%22com.pay.paytypetest%22%7D%5D&accsplit_flag=NO&jump_scheme=&sign_type=RSA&sign=XgR%2BI3vpo%2BDwRHPeUxoTFmiKSf49RW%2B%2BuDXfH6VWYBTFyEG1qw1W8nI0IHZqxvmO9MaTaPw2Ac3gOoe6%2B9d7%2FF2tqbMSAhXLw2BoDHmhqVn1LIkvQjl0v9a6uDLu%2FEO%2BpEJbgM6pR9hw1TeyphrUaPD0bufcqytsvVZ%2FTnYp9h7fkvTGZQLhlUGzpxhxpPX3TYc9fG51aYBR0SR5NRu%2Ft8MaiecVkUKIiWXZvbpt5LvxLdDimQRZPKMPny%2FmVVlf1cHZYzNrhyyKcj40ZwG7hVoFDGnB2jzew19gO7AGmID2x8INk%2FdPr35du%2BS7j8TRPBmpwus1%2FVvSlNPpOgfxHg%3D%3D"
    },
    "code": 200,
    "msg": "成功",
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
