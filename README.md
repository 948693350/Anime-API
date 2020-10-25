<p align="center"><img src="https://ae01.alicdn.com/kf/U150c6f229b47468781c941fdd80545eak.png" width="200"></p>
<h3 align="center">- Anime API -</h3>
<p align="center">
<img src="https://img.shields.io/github/v/release/zaxtyson/Anime-API.svg?logo=bilibili">
</p>

## 简介

这是 AnimeSearcher 的后端 API 部分, 从原项目 [AnimeSearcherOld](https://github.com/zaxtyson/AnimeSearcherOld)
中独立出来, 负责解析动漫和弹幕, 并提供一系列 Web 接口给前端调用。

API 提供了一个解析框架, 使得用户编写资源搜索引擎变得容易, 可用很方便的进行扩展。

视频资源的解析是分步进行的, 搜索番剧、解析详情页、解析视频直链, 只有在用户请求相应的接口时才会进行,
即便加载了许多引擎, 接口的响应速度也很快。解析完成的结果会被缓存, 下一次访问会更快。

当视频直链无法播放时, API 可以代理访问原始视频, 并返回视频数据流给前端播放器, 以绕过视频的防盗链。

## 安装依赖

```
pip install -r requirements.txt
```

## 示例

> 很简单就可以启用一个路由

```
from api.router import Router

if __name__ == '__main__':
    rt = Router()
    rt.listen("127.0.0.1", 6001)
    rt.run()
```

浏览器打开 `127.0.0.1:6001` 就可以开始测试了

## Web 接口

```
API Interface:

GET /search/<name>                      Return anime summary information
GET /detail/<hash_key>                  Return anime details information
GET /video/<hash_key>/url               Return the direct URL of video
GET /video/<hash_key>/proxy             Return the binary stream of video by API proxy
GET /danmaku/search/<name>              Return danmaku summary information
GET /danmaku/detail/<hash_key>          Return danmaku details information
GET /danmaku/data/<hash_key>/v3/        Return the danmaku data with Dplayer supported format
GET /bangumi/timeline                   Return anime update info recently

Settings Interface:
GET  /settings                          Return the current settings information
POST /settings/engine                   Enable or disable the specific Engine
POST /settings/danmaku                  Enable or disable the specific Danmaku

Form /settings/engine                   {"name": "api.engines.xxx", "enable": false}
Form /settings/danmaku                  {"name": "api.danmaku.xxx", "enable": true}

Test Interface:
GET /video/<hash_key>/player            Play video online by its direct URL
GET /video/<hash_key>/proxy_player      Play video online by API proxy
```

## 扩展引擎

如果你发现一个好康的网站, 你可以为它编写一个资源引擎, 将其添加到本项目。放心, 利用 API 框架提供的工具, 你可以很方便的实现它, 你只需要按步骤提取网页数据即可。

将你编写的资源引擎放到 `api/engines` 目录下, 在配置文件 `api/config.json` 中将其启用即可

详见 [API 文档](https://anime-api.readthedocs.io/zh/latest/#)

## 更新日志

### `v0.7.0`

- 修复 bimibimi 部分视频解析失败的问题和弹幕 undefined 的问题
- 补充 bilibili 影视区弹幕
- 新增引擎 meijuxia
- 新增弹幕源 youku
- 新增弹幕源 tencent
- 增加新番更新表接口