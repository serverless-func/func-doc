# 统计

## 调用次数统计

```shell
curl -X GET "https://fun.dongfg.com/stats/service"
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "totalInvocations": 671,
    "services": [
      {
        "name": "crawler",
        "invocations": 487
      },
      {
        "name": "docs",
        "invocations": 184
      }
    ]
  },
  "msg": "success",
  "timestamp": 1600063094
}
```

统计本月计费请求调用次数，总调用次数及每个 service 的调用次数

### HTTP Request

`GET https://fun.dongfg.com/stats/service`

## 处理时间统计

```shell
curl -X GET "https://fun.dongfg.com/stats/function"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "name": "statping/index",
      "duration": 3.885768253968254
    },
    {
      "name": "crawler/nanjing-edu",
      "duration": 7753.4582020202015
    },
    {
      "name": "docs/index",
      "duration": 2.4151809523809527
    },
    {
      "name": "fc-domain-challenge/fc-15ff2db0-f42c-11ea-892d-024215000304-29828077",
      "duration": 12.11
    },
    {
      "name": "fun-doc/fun-doc",
      "duration": 100
    }
  ],
  "msg": "success",
  "timestamp": 1600063100
}
```

统计本月各个函数的平均响应时间

### HTTP Request

`GET https://fun.dongfg.com/stats/function`
