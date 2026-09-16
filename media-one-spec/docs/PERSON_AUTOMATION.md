# MEDIA ONE｜按影人自动整理影视资源

版本：V1.0

## 目标

用户只需要给出一个影人和整理要求，例如：

> 帮我找一下沈腾的电影，转存到我的网盘，并刮削好。

系统应自动把自然语言请求转换为完整工作流，而不是要求用户逐部输入电影名。

---

# 总流程

PERSON → FILMOGRAPHY → WORK MATCHING → LIBRARY CHECK → RESOURCE MATCHING → TRANSFER → SCRAPE → VERIFY

---

## 1. PERSON｜识别人

输入：沈腾

先建立统一 Person ID，并绑定外部身份：

- 1905 Person ID
- Douban Celebrity ID
- TMDb Person ID
- IMDb Name ID
- 猫眼 Celebrity ID
- Wikidata ID
- Mtime ID

目的：解决同名人物、别名和不同网站人物条目的对应问题。

---

## 2. FILMOGRAPHY｜生成完整作品表

不能只使用 1905，也不能只使用任何一个网站。

候选作品全集：

1905 ∪ 豆瓣 ∪ TMDb ∪ IMDb ∪ 猫眼 ∪ Mtime ∪ 其他可靠来源

然后重新分类：

- 电影
- 网络电影
- 电视剧
- 网络剧
- 动画
- 短片
- 纪录片
- 综艺
- 真人秀
- 晚会
- MV
- 舞台
- 自己 / 出镜
- 其他

如果用户要求“沈腾的电影”，默认只保留符合规则的电影类作品。

人物身份也单独归一：

- 主演
- 演员
- 客串
- 特别出演
- 配音
- 导演
- 编剧
- 制片 / 监制
- 其他

---

## 3. WORK MATCHING｜逐部建立作品身份

作品不能只靠片名判断。

每部作品尽量绑定：

- Internal Work ID
- 中文片名
- 原名
- 年份
- 导演
- 主要演员
- 时长
- 1905 ID
- Douban ID
- TMDb ID
- IMDb ID
- 猫眼 ID

匹配规则结合：

片名 + 年份 + 导演 + 主演 + 时长 + 外部 ID

用于降低：

- 同名作品误匹配
- 第一部 / 第二部混淆
- 重拍版混淆
- 电影 / 电视剧混淆
- 花絮 / 正片混淆

---

## 4. LIBRARY CHECK｜检查已有媒体库

在搜索或转存资源之前，先检查用户已有：

- 本地硬盘
- NAS
- OpenList
- 百度网盘
- 其他已连接存储

如果已有，不重复转存。

同时分析现有媒体版本：

- 分辨率
- 视频编码
- HDR / Dolby Vision / SDR
- 音频格式
- 音轨
- 字幕
- 文件大小
- 来源
- EDITION

---

## 5. RESOURCE MATCHING｜匹配资源

只处理用户有权访问或合法拥有的媒体资源。

匹配资源时不能只搜索片名，应综合：

片名 + 年份 + 导演 + 主演 + 时长 + 版本

匹配完成后输出 confidence，低可信结果进入人工复核。

---

## 6. TRANSFER｜转存

符合条件后转存到目标媒体库 / 网盘。

记录：

- source
- target
- transfer_time
- destination_path
- file_size
- hash（条件允许）
- media_version
- status

---

## 7. 文件和目录规范

推荐作品目录：

`飞驰人生2 (2024) [tmdb-xxxxxx]/`

媒体文件示例：

`飞驰人生2 (2024) - 2160p WEB-DL DV HEVC.mkv`

配套资料：

- poster.jpg
- fanart.jpg
- logo.png
- clearart.png
- thumb.jpg
- trailer.mp4
- movie.nfo

最终规则继续根据 Jellyfin / Emby / Plex / Kodi 兼容性优化。

---

## 8. SCRAPE｜自动刮削

作品身份确认后，再进入多源刮削。

### 基础中文资料

1905 / 豆瓣 / 猫眼 / Mtime

### 国际身份与结构

TMDb / IMDb

### 高清海报

IMP Awards / TMDb / 1905 / 豆瓣 / 猫眼

### 预告片

6huo 发现和分类 → Mtime / 猫眼 / 官方来源确认

### 影人经历

1905 星路历程为主干 → 官方 / 权威媒体 / 豆瓣 / 猫眼 / Mtime 等补充

---

## 9. VERIFY｜完整性校验

最后生成每部作品状态。

建议状态字段：

- work_verified
- media_exists
- resource_found
- transferred
- metadata_complete
- poster_complete
- trailer_complete
- credits_complete
- missing_fields

用户最终应能看到类似状态表：

| 影片 | 已有资源 | 已找到 | 已转存 | 元数据 | 缺失 |
|---|---|---|---|---|---|
| 夏洛特烦恼 | ✓ | ✓ | ✓ | ✓ | — |
| 西虹市首富 | ✓ | ✓ | ✓ | ✓ | — |
| 某作品 | ✗ | ✓ | 待转存 | ✓ | 媒体文件 |
| 某作品 | ✓ | ✓ | ✓ | 部分 | 高清海报 |

---

# 自动漏片检测

对于人物 P：

A = 1905 作品集合  
B = 豆瓣作品集合  
C = TMDb 作品集合  
D = IMDb 作品集合  
E = 猫眼作品集合

候选全集：

U = A ∪ B ∪ C ∪ D ∪ E

逐项判断：

- 1905 缺失但其他多个来源一致 → 疑似 1905 漏片
- 只有单一来源存在 → 待验证
- 多源年份冲突 → 冲突队列
- 多源人物身份冲突 → 身份复核
- 同名作品无法确定 → 实体消歧

不能用“哪个网站数量最多”简单决定完整性。

---

# 后续自然语言能力

未来应支持：

- 找沈腾所有电影
- 找沈腾主演的电影
- 找沈腾 2018 年以后的电影
- 找沈腾和马丽合作的电影
- 找某演员评分达到指定条件的电影
- 把缺的作品整理出来
- 只转存没有的
- 已有 1080P 时寻找 4K 升级版
- 补齐海报 / 预告片 / NFO，而不替换视频

系统的重点不是“搜索片名”，而是理解人物、作品和用户媒体库之间的关系。
