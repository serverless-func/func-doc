# Favicon 服务

## 获取网站 icon 信息

```shell
curl "https://favicon.func.dongfg.com/fetch?url=https://baidu.com"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "url": "https://psstatic.cdn.bcebos.com/video/wiseindex/aa6eef91f8b5b1a33b454c401_1660835115000.png",
      "mimetype": "image/png",
      "extension": "png",
      "width": 0,
      "height": 0,
      "hash": "1d98c55bf95f00b647bd5c73e140d56f9f51005741a30c6ad27f1afc608d9b50"
    },
    {
      "url": "https://www.baidu.com/favicon.ico",
      "mimetype": "image/x-icon",
      "extension": "ico",
      "width": 0,
      "height": 0,
      "hash": "fcd05645b38f5a3abbca1cf4ee774cf74d3457115253169ef456462bba4f4af7"
    }
  ],
  "msg": "success",
  "timestamp": 1700631102
}
```

获取网站 icon 信息

### HTTP Request

`GET https://favicon.func.dongfg.com/fetch`

### Query Parameters

| Field | Default | Description  |
| ----- | ------- | ------------ |
| url   |         | 要查询的网站 |

## 查看网站 icon

```shell
curl "https://favicon.func.dongfg.com/preview?url=https://baidu.com"
```

> 会使用 http 状态码 307 跳转到 icon 地址


查看网站 icon

### HTTP Request

`GET https://favicon.func.dongfg.com/fetch`

### Query Parameters

| Field | Default | Description  |
| ----- | ------- | ------------ |
| url   |         | 要查询的网站 |
