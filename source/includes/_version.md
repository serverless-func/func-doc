# 最新版本检查服务

## 获取 GitHub 最新 Release

```shell
curl "https://version.func.dongfg.com/github?repo=dani-garcia/vaultwarden&prerelease=true"
```

> The above command returns JSON structured like this:

```json
{
  "data": "1.30.1",
  "msg": "success",
  "timestamp": 1701490585
}
```

获取 GitHub 最新 Release

### HTTP Request

`GET https://version.func.dongfg.com/github`

### Query Parameters

| Field      | Default | Description         |
| ---------- | ------- | ------------------- |
| repo       |         | 要查询的仓库名称    |
| prerelease | false   | 是否包含 prerelease |

## 获取 NPM 包最新版本

```shell
curl "https://version.func.dongfg.com/npm?pkg=qs"
```

> The above command returns JSON structured like this:

```json
{
  "data": "6.11.2",
  "msg": "success",
  "timestamp": 1701490585
}
```


获取 NPM 包最新版本

### HTTP Request

`GET https://version.func.dongfg.com/npm`

### Query Parameters

| Field | Default | Description  |
| ----- | ------- | ------------ |
| pkg   |         | 要查询的包名 |
