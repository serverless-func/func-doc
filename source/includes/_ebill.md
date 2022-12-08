# 信用卡账单

## 招行-每日信用管家邮件

```shell
curl -X POST "https://ebill.func.dongfg.com/cmb" -d '{"username": "xxxxx", "password": "yyyyyy", "hour": 24}'
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

`POST https://ebill.func.dongfg.com/cmb`

### JSON Body Fields

| Field    | Default | Description                          |
| -------- | ------- | ------------------------------------ |
| username |         | 邮箱用户名                           |
| password |         | 邮箱密码                             |
| hour     | 24      | 解析的邮件范围，可为空，默认 24 小时 |

<aside class="warning">时间范围太大时响应时间较慢.</aside>

## 招行-月账单 pdf 解析

```shell
curl -F 'file=@CreditCardReckoning202010.pdf' "https://ebill.func.dongfg.com/file/cmb"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "tradingDate": "2020-09-04",
      "creditDate": "2020-09-05",
      "name": "支付宝-华润万家有限公司",
      "amount": "180.83",
      "tailNumber": "3885",
      "tradingAmount": "180.83(CN)"
    },
    {
      "tradingDate": "2020-09-09",
      "creditDate": "2020-09-10",
      "name": "支付宝-阿斯兰航空服务（上海）有限公",
      "amount": "939.50",
      "tailNumber": "3885",
      "tradingAmount": "939.50(CN)"
    }
    ...
  ],
  "msg": "success",
  "timestamp": 1605681442
}
```

解析招行月账单 PDF 文件

### HTTP Request

`POST https://ebill.func.dongfg.com/file/cmb`

### Multipart Form

| Field | Default | Description |
| ----- | ------- | ----------- |
| file  |         | pdf 文件    |

### Respnse Body Fields

| Field         | Description |
| ------------- | ----------- |
| tradingDate   | 交易日      |
| creditDate    | 记账日      |
| name          | 交易摘要    |
| amount        | 人民币金额  |
| tailNumber    | 卡号末四位  |
| tradingAmount | 交易地金额  |
