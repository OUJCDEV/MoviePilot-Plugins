# 呀哈哈封面工坊 · MoviePilot V3 适配版

本目录是 [呀哈哈封面工坊](https://github.com/justzerock/MoviePilot-Plugins) 面向 **MoviePilot V3** 的实现副本，功能与原作者 V2 版（2.2.11）保持一致，仅做 V3 合同迁移。

## 支持的媒体库类型

自 v3.1.0 起，除电影、剧集、合集（BoxSet）、播放列表、音乐外，以下媒体库类型也可自动生成封面：

| 媒体库 | Emby/Jellyfin `CollectionType` | 筛选条目 | 素材来源 |
| --- | --- | --- | --- |
| 音乐视频 | `musicvideos` | `MusicVideo` | 条目自身主图 |
| 家庭视频 | `homevideos` | `Video,Photo` | 条目主图 / 服务器生成缩略图 |
| 照片 | `photos` | `Photo` | 照片本身（跳过相册，避免与相册封面重复） |

说明：

- 预览页、数量徽标（`{count}`）与媒体库筛选列表对上述类型一并生效。
- 照片与视频素材按服务器原始大图下载时携带 `maxWidth=1920&quality=90` 限制，避免超大文件拖慢生成。
- 入库监控：照片、家庭视频等无法被 MoviePilot 识别为媒体的内容，请使用 **Webhook 入库监控**（Emby/Jellyfin 的 `library.new` / `ItemAdded` 事件），MoviePilot 整理监控（TransferComplete）无法感知此类入库。

## 与原 V2 版的差异

| 项目 | V2 原版 | 本 V3 版 |
| --- | --- | --- |
| 导入路径 | `app.core.*` / `app.helper.*` / `app.utils.*` / `app.log` | `app.sdk.config` / `app.sdk.events` / `app.sdk.media` / `app.sdk.services` / `app.sdk.network` / `app.sdk.utilities` / `app.sdk.logging` |
| 基类 | `app.plugins._PluginBase` | `app.sdk.plugin._PluginBase` |
| 版本 | 2.2.11 | **3.0.0** |
| 依赖声明 | `requirements.txt` | `pyproject.toml`（静态 `project.dependencies`） |
| 索引 | `package.v2.json` | 新增 `package.v3.json`，原 V2 条目标记 `"v3": false` |

## 保留的旧路径（V3 暂无 SDK 出口，实测仍可导入）

- `from app.chain.mediaserver import MediaServerChain`
- `from app.schemas import TransferInfo`
- `from app.schemas import ServiceInfo`
- `from app.schemas.types import EventType`

## 未做改动的部分

插件不访问宿主数据库、未使用已删除的 `MusicChain`、`get_api()` 路由不被宿主包装，因此 V3 迁移指南中"数据库与事务""链职责""REST 响应合同"三节无需改动。

## 许可

沿用上游仓库许可证（见仓库根目录 LICENSE）。适配改动仅为导入路径与元数据。
