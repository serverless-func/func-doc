# 通知服务

## Health Check

```shell
curl https://notify.fun.dongfg.com/ping
```

## 企业微信通知

```shell
curl --request POST \
  --url https://notify.fun.dongfg.com/wechat \
  --header 'Authorization: Basic <credentials>' \
  --header 'Content-Type: application/json' \
  --data '{
    "title": "快递提醒",
    "desc": "您的中通快递已到一单元102室请凭038410来取，取件时间09:30-21:30",
    "url": "http://link"
    }'
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "errcode": 0,
    "errmsg": "ok",
    "invaliduser": "",
    "invalidparty": "",
    "invalidtag": ""
  },
  "msg": "success",
  "timestamp": 1600757374
}
```

发送企业微信通知

### HTTP Request

`POST https://notify.fun.dongfg.com/wechat`

### JSON Body Fields

| Field | Default | Description      |
| ----- | ------- | ---------------- |
| title |         | 标题             |
| desc  |         | 内容             |
| url   |         | 详情链接，可为空 |

## 钉钉群通知

```shell
curl --request POST \
  --url https://notify.fun.dongfg.com/dingtalk \
  --header 'Authorization: Basic <credentials>' \
  --header 'Content-Type: application/json' \
  --data '{
    "title": "快递提醒",
    "desc": "您的中通快递已到一单元102室请凭038410来取，取件时间09:30-21:30",
    "url": "http://link"
    }'
```

> The above command returns JSON structured like this:

```json
{
  "data": {},
  "msg": "success",
  "timestamp": 1600757374
}
```

发送钉钉群通知

### HTTP Request

`POST https://notify.fun.dongfg.com/dingtalk`

### JSON Body Fields

| Field | Default | Description      |
| ----- | ------- | ---------------- |
| title |         | 标题             |
| desc  |         | 内容             |
| url   |         | 详情链接，可为空 |
