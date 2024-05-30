

# 商户余额查询

### number上虞请求地址 <a href="#rpuji" id="rpuji"></a>

| **生产环境** | 联系商务获取 |
| -------- | -------------------------------- |
| **请求方式** | **支持POST/GET 任意方式请求**            |

### 请求参数说明 <a href="#eo5ea" id="eo5ea"></a>

**公共请求参数**

| **参数**         | **类型** | **是否必填** | **参数说明**           |
| -------------- | ------ | -------- | ------------------ |
| app\_id        | String | 是        | 商户应用号              |
| version        | String | 是        | 接口版本：v1.0.0        |
| charset        | String | 是        | UTF-8              |
| sign\_type     | String | 是        | 签名类型：RSA，支持RSA、SM2 |
| sign           | String | 是        | 签名字符串，再用Base64编码   |
| service\_no    | String | 是        | 服务编码：queryBalance  |
| biz\_req\_body | String | 否        |                    |

请求示例

```
{
"charset": "UTF-8",
"biz_req_body": "",
"verion": "v1.0.0",
"service_no": "queryBalance",
"app_id": "app_id",
"sign_type": "RSA2",
"sign": "sign"
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

| **参数**  | **类型**  | **是否必填** | **参数说明** |
| ------- | ------- | -------- | -------- |
| balance | decimal | 是        | 商户余额     |

**响应成功示例**

```
{
    "code": 200,
    "msg": "查询成功",
    "data": {
        "balance": 0.0000
    },
    "sign": null,
    "time_stamp": 1711621117690
}
```

### 业务响应码说明 <a href="#grogv" id="grogv"></a>

| **响应码(code)** | **响应码说明(msg)** |
| ------------- | -------------- |
| 200           | 业务受理成功         |
| 3000          | 商户未开通此权限       |
| 3001          | 请求数据验签失败       |
| 3003          | 订单失败，具体看msg提示  |
