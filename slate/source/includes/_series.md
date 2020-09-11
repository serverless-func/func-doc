# 剧集

## 剧集搜索

```shell
curl -X GET "https://fun.dongfg.com/series?keyword=权力的游戏&details=true"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "10733",
      "cnName": "权力的游戏",
      "poster": "http://tu.jstucdn.com/ftp/2019/0322/d2b4282fe50dffaad4c73b6f3d6176ff.jpg",
      "enName": "Game of Thrones",
      "link": "http://www.rrys2020.com/resource/10733",
      "rssLink": "http://rss.rrys.tv/rss/feed/10733",
      "area": "美国",
      "category": "战争/剧情/魔幻/历史/古装/史诗"
    },
    {
      "id": "35844",
      "cnName": "权力的游戏：征服与反抗",
      "poster": "http://tu.jstucdn.com/ftp/2017/1214/754eb87fb49adbadcbbe46348370ff73.jpg",
      "enName": "Game of Thrones: Conquest and Rebellion",
      "link": "http://www.rrys2020.com/resource/35844",
      "area": "美国",
      "category": "动画"
    }
  ],
  "msg": "success",
  "timestamp": 1595403185
}
```


通过关键字搜索剧集

### HTTP Request

`GET https://fun.dongfg.com/series`

### Query Parameters

Parameter |Default| Description
--------- | ----- | -----------
keyword | |搜索关键字
details| false |是否返回详情, false 时仅返回 id, cnName, poster

<aside class="warning">搜索使用`details=true`时接口响应时间较慢.</aside>

## 获取剧集详情

```shell
curl -X GET "https://fun.dongfg.com/series/10733"
```

> The above command returns JSON structured like this:

```json
{
  "id": "10733",
  "cnName": "权力的游戏",
  "poster": "http://tu.jstucdn.com/ftp/2019/0322/d2b4282fe50dffaad4c73b6f3d6176ff.jpg",
  "enName": "Game of Thrones",
  "link": "http://www.rrys2020.com/resource/10733",
  "rssLink": "http://rss.rrys.tv/rss/feed/10733",
  "area": "美国",
  "category": "战争/剧情/魔幻/历史/古装/史诗"
}
```

获取剧集详情详细信息, 等价于搜索时加 `details=true`.

### HTTP Request

`GET https://fun.dongfg.com/series/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | 搜索返回的id

## 获取剧集下载地址

```shell
curl -X GET "https://fun.dongfg.com/series/10733/episodes"
```

> The above command returns JSON structured like this:

```json
{
  "msg": "success",
  "timestamp": 1595403802,
  "data": [
    {
      "seriesId": "10733",
      "name": "权力的游戏.Game.of.Thrones.S08E03.中英字幕.WEBrip.720P-人人影视.mp4",
      "season": 8,
      "episode": 3,
      "ed2k": "ed2k://|file|%E6%9D%83%E5%8A%9B%E7%9A%84%E6%B8%B8%E6%88%8F.Game.of.Thrones.S08E03.%E4%B8%AD%E8%8B%B1%E5%AD%97%E5%B9%95.WEBrip.720P-%E4%BA%BA%E4%BA%BA%E5%BD%B1%E8%A7%86.V1.mp4|1240802301|946a2ef12f9f128403a208c44c596b99|h=a7a7j5whhhih57fsl2eckv5mlnzzokgr|/",
      "magnet": "magnet:?xt=urn:btih:702778ba56195ed3844bb92e059d320539c530ec"
    }
  ]
}
```

获取剧集下载地址, 包含 `ed2k/magnet` 地址及对应 `Season` `Episode` 信息.

### HTTP Request

`GET https://fun.dongfg.com/series/<ID>/episodes`

### URL Parameters

Parameter | Description
--------- | -----------
ID | 搜索返回的 id
