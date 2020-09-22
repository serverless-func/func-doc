# 信用卡消费邮件

## 招行-每日信用管家

```shell
curl -X POST "https://fun.dongfg.com/ebill/cmb" -d '{"username": "xxxxx", "password": "yyyyyy", "hour": 24}'
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "name": "尾号3885 消费 支付宝-xxxxx有限公司",
      "time": "2020-09-21 07:53:41",
      "amount": "8.00"
    },
    {
      "name": "尾号3885 消费 支付宝-xxxx",
      "time": "2020-09-21 11:35:57",
      "amount": "17.00"
    }
  ],
  "msg": "success",
  "timestamp": 1600757374
}
```

解析邮件获取最近的消费记录

### HTTP Request

`POST https://fun.dongfg.com/ebill/cmb`

### JSON Body Fields

| Field    | Default | Description                          |
| -------- | ------- | ------------------------------------ |
| username |         | 邮箱用户名                           |
| password |         | 邮箱密码                             |
| hour     | 24      | 解析的邮件范围，可为空，默认 24 小时 |

<aside class="warning">时间范围太大时响应时间较慢.</aside>
