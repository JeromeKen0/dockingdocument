# 签名说明

请求参数实例

```
{
    "charset": "UTF-8",
    "biz_req_body": "{\"amount\":\"168.00\",\"out_trade_no\":\"\",\"user_id\":\"13429\",\"order_desc\":\"\",\"notify_url\":\"\"}",
    "sign": "sign",
    "service_no": "netpay",
    "app_id": "app_id",
    "version": "v1.0.0",
    "sign_type": "RSA2"
}
```

除去 `sign`、`sign_type` 两个参数外，其他请求参数均为待签的参数

将剩下参数进行 **URLDecode**，然后进行字典排序，组成字符串，得到待签名字符串 。拼接格式为key1=value1\&key2=value2\&key3...

使用私钥进行RSA签名,SHA256 +PKCS1 方式。签名结果转为Base64字符串。得到提交的签名

## 示例

请求参数：

```
{
    "charset": "UTF-8",
    "biz_req_body": "{\"amount\":\"168.00\",\"out_trade_no\":\"\",\"user_id\":\"13429\",\"order_desc\":\"\",\"notify_url\":\"\"}",
    "sign": "sign",
    "service_no": "netpay",
    "app_id": "app_id",
    "version": "v1.0.0",
    "sign_type": "RSA2"
}
```

待签名参数为:

```
app_id=app_id&biz_req_body={"amount":"168.00","out_trade_no":"","user_id":"13429","order_desc":"","notify_url":""}&charset=UTF-8&service_no=netpay&version=v1.0.0
```

&#x20;公钥:

```
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAudKS49v1bLKolnfEm91d6+7r5xOneLTaER9ctMVOQb/EcmZmbYCubOHYRR6oCv4nNgntnKlVTo89y6YmV/Z0q+r+dX+pSkmBvBJp4sS3ek/Ciml/glMqRThYw4ZDoa/2aukt9pm8QauKu+iJLSqFNLV+jZxVKmFoEDi3HkUXjWpR42AhQCPkz8v2XyRa0BkT/YY3WAueARP11g9mzWvVy8DBAt60P8Jw//tw+AILb0Ry1TmjMRiFw+VXFaWEGiewjtlKYB3rwv92PcMmRGeLRlF9zfP/Y2LBm8iJRZLVdOc+s0UdOW3ZSu9XOpNvlDbbgHo5HmeiA0cvTVNryW0FLQIDAQAB
```

&#x20;私钥:

```
MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQC50pLj2/VssqiWd8Sb3V3r7uvnE6d4tNoRH1y0xU5Bv8RyZmZtgK5s4dhFHqgK/ic2Ce2cqVVOjz3LpiZX9nSr6v51f6lKSYG8EmnixLd6T8KKaX+CUypFOFjDhkOhr/Zq6S32mbxBq4q76IktKoU0tX6NnFUqYWgQOLceRReNalHjYCFAI+TPy/ZfJFrQGRP9hjdYC54BE/XWD2bNa9XLwMEC3rQ/wnD/+3D4AgtvRHLVOaMxGIXD5VcVpYQaJ7CO2UpgHevC/3Y9wyZEZ4tGUX3N8/9jYsGbyIlFktV05z6zRR05bdlK71c6k2+UNtuAejkeZ6IDRy9NU2vJbQUtAgMBAAECggEAO/FeqxxohkD3u1o1VSZKxvISrT8c1gZZFg7s4++F+BW5dEHuJsLNAZi1IE7sXGdyFK+NM+039JimkYwucE+zgUXUAelFng4qSJYUDC/zFASot+eiV1Mmnp+3mpM0O/M8ZW6FAjjDjteccFNp9OTzhXZKtbnJi1tSq3DwOVaGa3pkuQmXqgfvjXb9WqEnyqECEcX6M+0r5E2IQiL9uiJgwa+yqtCCgylWQE6OPeIjDptrZNm0JBgu+WzpJpfcfwzED70m7nuRThCu/l5bfngYIq4SOggHR3HX6KqlmQ8/JeFuXf2O4chelVlwptno5kPVmt3sAY+u1DNQGFGpi06HTQKBgQDO85Bm65lk/+T2wS1zTsC3EPS3N2jHU+V30h2TEy1fzDcrOfQVuZN1vEjbxQ7aYA7diaXzShOROd6//Ip9CVvVKgzAu9NFqvkAEQGuCRCF4K98CttxZ2Hfoti31L3bH8Trv1f0w/jzimHebzKfQH1MdFD6qSStDsSaaH4mZemQzwKBgQDl3QzuZGJ4rMqRgDfaUiuWQGZLlkKSp02z8QoI8iBlKmkXh0VAnxskRTnGauCtbHfntPxueqBQqIrQvmVHq0tHzRGHESKIn+Pp10wMdlc+a/zWwtzdYOVE/rr4cqym3MxYjeU6Y2xLp3yBw4loICGBZ3IeVrOm9htV0lhnJkOxQwKBgEKQ5Wm1bmmmRad5C32DX0mDErO8Bt/WhIC9/PVJvdaKgVROF8zFHEFKhsTp5ZUoQJ/RnqdatGCKFLP8Ly94yykNlXyI7bQDAoSa88de8wmc89UaSOt5LWoZn0vCCi9pUJXjvg7k2ja71C8P5WCEBcmJwGJf9YQUs/hWk/0V2sLRAoGABH/+V9BpSRmA4bZT4ZdIOSnLluE7LmnOEJ7AZopu7ewVoJtKVMiInH4qcmL3QQ3ljwixBGysJMgX55xCmVOWJrKyDCXeujP/Hz3SxE+wx40PpxirgD38XwxplqGQFbgu2/DzMuBtZ1HBEz1DvGEcps7iogtqevNId7alemd6XccCgYA5BHGoW9Qeb4QG7sn6K41JoI9xTC9DSYgZ734L7qdjX1M5BYBZT70av9ig6sJEBQ4Sx6IXy17qJTmBPbIJSOyF39Mq/9vYyaHzQqolQUUvnlDrNMDVN/bO7rrO6Cxhyr6EUbJ6UOZgEMOfou1CoC9ck+0QRDhA0oFDv+qvGD+6Tg== 
```

签名结果:

```
maxyR5G035DWz4FcD7htnUVuDeoh3LlsAesXVMVrnRAqZ4ddUbmBbqzOTf339Y4Y7SdzL7oGm5HOWeuMD3Vj2V2rgTmGzJIwLNECnm+PDE0SuTYeveDFJbmM0yz2pHQjZweK8R8XSuedl5O3jw75D2Of4xS/OvEThbxR3z9GLBJivAgK5DujeYhg8RPTZXelOh58DPAGjVmSXVlbpLZO/ThFiO9Hw18dBpJErySeBdpaeNMKkl0zeXI6Z7Ltfxw7tB2pfh+GX5NTOh6PQn/rM732ZARlLUrwGI8BABlDurJO15srfuVBy9+7PWnXnfbvfW/szjHtt9Y5+mD+TeiaJg==
```

