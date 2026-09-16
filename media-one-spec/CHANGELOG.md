# MEDIA ONE Spec｜CHANGELOG

所有重要架构、规则和数据源策略的变化都记录在这里。

---

## V1.0 — 2026-09-17

### 建立长期规范项目

首次把此前分散在聊天中的核心规则整理并写入 GitHub。

### 已确认架构

- MEDIA ONE 一级模块：主页 / 影视 / 直播 / 音乐 / 图库。
- 当前优先定型影视模块。
- 作品采用 WORK → EDITION → MEDIA VERSION → MEDIA ASSET 四层模型。
- 外部网站仅作为 Source / Evidence，自有数据库为最终主库。
- 所有重要字段保留来源、原始值、主值、可信度、验证状态和人工锁定能力。

### 1905

- 将 1905 提升为中文影片与影人核心资料源之一。
- 重点利用星路历程 / 人物经历、完整演职员、角色、获奖、机构、图片、视频、合作关系。
- 星路历程需要结构化为 PERSON EVENT 时间轴。
- 1905 不视为绝对完整，人物作品表需要多源并集与补全。

### 多源体系

当前核心来源包括：

- 1905
- 豆瓣
- TMDb
- IMDb
- 猫眼
- 淘票票
- 灯塔
- Mtime
- IMP Awards
- 6huo
- Wikidata
- 官方来源

### 图片

- IMP Awards 作为高清、多版本海报的重要专用源。
- TMDb 提供国际 Poster / Backdrop / Logo。
- 1905 / 豆瓣 / 猫眼补中文宣发图、剧照和人物图片。

### 视频

- 6huo 作为预告片发现、分类和上游来源索引。
- Mtime / 猫眼 / 官方来源用于确认真正上游和官方性。
- 视频必须区分 Teaser / Trailer / Final Trailer / IMAX / Featurette / Clip / MV / BTS 等类型。

### 按影人自动整理影视资源

确定核心流程：

PERSON → FILMOGRAPHY → WORK MATCHING → LIBRARY CHECK → RESOURCE MATCHING → TRANSFER → SCRAPE → VERIFY

目标示例：

“把沈腾的电影找齐、转存到网盘并刮削好。”

系统先生成多源完整作品表，再做实体匹配、已有资源检查、合法可访问资源匹配、转存、命名、刮削和完整度校验。

---

## 后续待补

- 字段级详细优先级矩阵
- SOURCE AUTHORITY 权重规则
- WORK / PERSON / SOURCE 数据表结构
- OpenList / 网盘连接规范
- Jellyfin / Emby / Plex / Kodi 命名兼容规则
- 海报自动评分算法
- 预告片官方性判断规则
- 影人时间轴事件分类字典
- 多源冲突自动仲裁规则
