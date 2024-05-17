---
description: 收银台对商户的请求数据处理完成后，会将处理的结果数据通过服务器主动通知的方式，通知到商户服务器。通知参数包含公共参数和业务参数，具体业务结果参考。
---

# 异步通知说明

#### 通知地址 <a href="#v1czy" id="v1czy"></a>

业务请求接口传的异步通知地址，未传值则不通知。

#### 通知策略 <a href="#oysnm" id="oysnm"></a>

收到通知后需返回字符串"success"，告知接收通知成功。2秒内未回写或回写内容非"success"，则判定为通知失败，将会延迟重试最多5次，延迟时间为：1s 。5次后停止通知。正常回写的情况下，有可能因网络原因造成重发，请做好重复通知处理。

<mark style="color:red;">**注：通知失败达到2000次则通知地址拉黑，后续无法接收通知！！！**</mark>

#### 参数说明 <a href="#gi9wh" id="gi9wh"></a>

异步通知请求基于POST、GET方式，Content-Type类型为application/x-www-form-urlencoded，参数格式为map，具体拼接格式为key1=value1\&key2=value2\&key3...



|       参数名       |  参数类型  | 是否必填 |                        参数说明                        |
| :-------------: | :----: | :--: | :------------------------------------------------: |
|     app\_id     | String |   是  |                        应用ID                        |
|                 |        |      |                                                    |
|    trade\_no    | String |   是  |                         订单号                        |
|  out\_trade\_no | String |   是  |                        商户订单号                       |
|   notify\_time  | String |   是  |            通知时间 格式为 yyyy-MM-dd HH:mm:ss            |
|   notify\_type  | String |   是  |      <p>通知类型</p><p>枚举值：trade_status_sync。</p>      |
|    notify\_id   | String |   是  |                      通知校验 ID。                      |
|    sign\_type   | String |   是  | 签名类型。商家生成签名字符串所使用的签名算法类型，目前支持 RSA2 和 RSA，推荐使用 RSA2 |
|       sign      | String |   是  |                         签名。                        |
|  trade\_status  | String |   是  |                        交易状态                        |
|  total\_amount  | Number |   是  |        订单金额。本次交易支付的订单金额，单位为人民币（元）。支持小数点后两位。        |
| receipt\_amount | Number |   是  |       实收金额。商家在交易中实际收到的款项，单位为人民币（元）。支持小数点后两位。       |
|   gmt\_create   | String |   否  |     交易创建时间。该笔交易创建的时间。格式 为 yyyy-MM-dd HH:mm:ss。     |
|   gmt\_payment  | String |   否  |    交易 付款时间。该笔交易的买家付款时间。格式为 yyyy-MM-dd HH:mm:ss。    |
|   gmt\_refund   | String |   否  |     交易退款时间。该笔交易的退款时间。格式 为 yyyy-MM-dd HH:mm:ss。     |
|    gmt\_close   | String |   否  |     交易关闭时间。该笔交易的退款时间。格式 为 yyyy-MM-dd HH:mm:ss。     |
|   order\_desc   | String |   否  |    订单标题。商品的标题/交易标题/订单标题/订单关键字等，是请求时对应的参数，原样通知回来。   |
|      remark     | String |   否  |                        订单备注                        |
|       body      | String |   否  |      商品描述。该订单的备注、描述、明细等。对应请求时的 body 参数，原样通知回来。     |



#### 交易状态说明 <a href="#m89dm" id="m89dm"></a>

| TRADE\_FINISHED  | 交易结束，不可退款。            |
| ---------------- | --------------------- |
| TRADE\_SUCCESS   | 交易支付成功。               |
| TRADE\_CLOSED    | 未付款交易超时关闭，或支付完成后全额退款。 |
| WAIT\_BUYER\_PAY | 交易创建，等待买家付款           |

#### 签名与验签示例 <a href="#mdqhf" id="mdqhf"></a>

异步返回结果验签\
第一步：在通知返回参数列表中，除去 sign、sign\_type 两个参数外，通知返回的参数均为待验签的参数。\
第二步： 将剩下参数进行 URLDecode，然后进行字典排序，组成字符串，得到待签名字符串：\
第三步： 将签名参数（sign）使用 base64 解码为字节码串。\
第四步： 使用 RSA 的验签方法，通过签名字符串、签名参数（经过 base64 解码）及公钥验证签名。\
第五步：在步骤四验证签名正确后，必须再严格按照如下描述校验通知数据的正确性。\
1 商家需要验证该通知数据中的 out\_trade\_no 是否为商家系统中创建的订单号。\
2 判断 total\_amount 是否确实为该订单的实际金额（即商家订单创建时的金额）。验证receipt\_amount金额是否和total\_amount 一致。\
3 验证 app\_id 是否为该商家本身。\
4 上述 1、2、3 有任何一个验证不通过，则表明本次通知是异常通知，务必忽略。在上述验证通过后商家必须根据平台不同类型的业务通知，正确的进行不同的业务处理，并且过滤重复的通知结果数据。在收银平台的业务通知中，只有交易通知状态为 TRADE\_SUCCESS 或 TRADE\_FINISHED 时，收银平台才会认定为买家付款成功。

\
请求内容实例 （JSON格式,实际请求为FromData请求）

```json
{
    "app_id": "app_id",
    "trade_no": "trade_no",
    "out_trade_no": "out_trade_no",
    "notify_time": "2024-03-28 17:46:30",
    "gmt_create": "2024-03-28 17:44:28",
    "notify_type": "trade_status_async",
    "notify_id": "6819",
    "trade_status": "TRADE_SUCCESS",
    "total_amount": "1.00",
    "receipt_amount": "1.00",
    "remark": null,
    "body": "测试",
    "sign": "Q15SOKQirDG0lXvzBWZ/fyi07p3FF8jz0czAS8yKIreifWXvwitHp6t9yXzDaD+OzewgfvXDsKW9tftepKQ6i4GGK6SAzRnRBBqhJU8ZXqw+WEiEhK7sxh49OACJ5PPzTdm5cGe1NVigXBTFIpVbl7urv5B+DQFIj5QW32OjssnUqkevhHuUS+59jq7YyowUvkoHGVPWsx/R5/tm7KtCyOgue42IxuUBtEgYvok/pdsEyEKxc2nZfdMNCExQxF8/AbWiVNOIo6cQVXUYv5FukZ+0HKf8lCfw8BSAkU4ZK7XiLAYqRZHS28IGOg7TYgdyvPsCmNQfSkeLIlpgcn/Hvw==",
    "gmt_payment": "2024-03-28 17:46:30",
    "gmt_refund": null,
    "gmt_close": null
}
```
