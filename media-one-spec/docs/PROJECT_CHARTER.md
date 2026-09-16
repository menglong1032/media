# MEDIA ONE｜影视系统长期项目总纲

版本：V1.0  
建立日期：2026-09-17

## 1. 项目定位

MEDIA ONE 的目标不是做一个普通影视网页，也不是简单复刻 Plex、Emby、Jellyfin 或 Infuse，而是建设一个统一的私人影视知识与媒体系统。

一级模块长期规划：

- 首页
- 影视
- 直播
- 音乐
- 图库

当前优先完成影视模块，但从一开始预留与直播、音乐、图库之间的关联接口。

## 2. 影视系统核心原则

### 2.1 多源融合

不依赖任何单一网站。不同来源各有所长，系统按字段选最佳来源并保留溯源。

### 2.2 自有数据库是最终主库

外部网站属于 Evidence / Source。MEDIA ONE 自己的数据库负责：

- 统一身份
- 去重
- 合并
- 补全
- 冲突处理
- 来源记录
- 人工确认
- 资源管理

### 2.3 核心信息默认简洁，深度资料进入“更多”

普通用户第一屏看到海报、标题、评分、年份、类型、时长、简介、主创、播放；完整演职员、制作发行、奖项、不同版本、来源、新闻、图片、视频等进入深层页面。

## 3. 作品四层模型

### WORK｜作品

抽象意义上的统一作品身份。每部作品只有一个 WORK。

维护：

- Internal Work ID
- 中文名 / 原名 / 英文名
- 年份 / 类型 / 国家地区
- 1905 ID
- Douban ID
- TMDb ID
- IMDb ID
- Mtime ID
- 猫眼 ID
- 其他外部 ID

### EDITION｜剪辑/发行版本

例如：

- 院线版
- 导演剪辑版
- 加长版
- 国际版
- 修复版
- 特别版

### MEDIA VERSION｜媒体版本

例如：

- 2160P UHD Blu-ray
- 2160P WEB-DL
- 1080P Blu-ray
- HDR10 / Dolby Vision / SDR
- H.264 / HEVC / AV1 / REMUX

### MEDIA ASSET｜实际资源

表示真正存在的文件或存储位置，例如：

- 本地硬盘
- NAS
- OpenList
- 百度网盘
- 阿里云盘
- 其他合法可访问存储

## 4. 人物统一身份

每个影人建立独立 PERSON ID，并绑定：

- 1905 Person ID
- Douban Celebrity ID
- TMDb Person ID
- IMDb Name ID
- 猫眼 Celebrity ID
- Wikidata ID
- Mtime ID

用于解决同名人物、不同语言名和不同网站条目之间的映射。

## 5. 1905 的核心定位

1905 不是普通备用源，而是中文影片与影人体系的核心数据源之一。

重点利用：

- 中文影片资料
- 完整演职员
- 角色
- 制作与发行机构
- 上映信息
- 奖项
- 海报与剧照
- 视频
- 影人作品履历
- 人物评价
- 合作关系
- 星路历程 / 人物经历

### 星路历程结构化

不能把人物经历只保存成一段文本，而要拆成 PERSON EVENT 时间轴。

字段建议：

- event_id
- person_id
- event_date
- event_year
- event_type
- title
- description
- related_work_id
- related_person_id
- award_id
- source
- source_url
- confidence
- verification_status

人物页最终形成：

人物 → 经历 → 作品 → 角色 → 奖项 → 新闻 → 图片 → 视频

## 6. 1905 不完整问题

任何单一来源都可能遗漏或出错。1905 作为主干，但不能作为唯一真相。

人物候选作品全集采用多源并集：

1905 ∪ 豆瓣 ∪ TMDb ∪ IMDb ∪ 猫眼 ∪ Mtime ∪ 其他可靠来源

然后统一做：

- 类型分类
- 作品实体匹配
- 身份确认
- 年份确认
- 角色确认
- 多源冲突处理

## 7. 作品类型重新归一

不同网站口径不同，MEDIA ONE 自己重新分类：

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
- 舞台
- MV
- 广告
- 配音
- 自己 / 出镜
- 客串
- 特别出演

人物身份另行记录：

- 演员
- 导演
- 编剧
- 制片
- 监制
- 配音
- 主持
- 嘉宾
- 自己
- 其他

## 8. 图片资产系统

不能只有 poster_url，而应建立 IMAGE ASSET。

建议字段：

- Image ID
- WORK ID / PERSON ID
- 类型
- URL
- 本地文件
- 宽度 / 高度 / 比例
- 语言
- 国家地区
- 来源
- 是否官方
- 是否主图
- 质量评分

图片类型至少包括：

- Poster
- Alternate Poster
- Character Poster
- Teaser Poster
- International Poster
- Chinese Poster
- Backdrop
- Still
- Behind the Scenes
- Logo
- Clear Logo
- Profile
- Event Photo
- Wallpaper

## 9. 视频资产系统

不能只保存 video_url，要建立 VIDEO ASSET。

字段建议：

- Video ID
- WORK ID
- 标题
- 类型
- 时长
- 发布日期
- 来源
- 原始平台
- URL
- 封面
- 清晰度
- 官方/非官方
- 语言
- 地区

视频类型至少包括：

- Teaser
- Trailer
- Official Trailer
- Final Trailer
- IMAX Trailer
- TV Spot
- Clip
- Featurette
- Behind the Scenes
- Interview
- MV
- Promo
- Making Of

## 10. 数据溯源与冲突处理

每个重要字段保存 FIELD VALUE + SOURCE，而不是简单覆盖。

原则：

1. 保留所有原始来源值。
2. 单独选择 preferred_value。
3. 人工确认后允许 manual_lock。
4. 保留修改历史。
5. 保存 last_checked。
6. 保存 confidence / verification_status。

可信度可采用：

- HIGH
- MEDIUM
- LOW
- UNVERIFIED

同时建立 SOURCE AUTHORITY，不能仅按“来源数量”判断真伪。

## 11. 自动缺失检测

每个人物与作品都应有 Completeness Checker。

自动识别：

- 疑似漏片
- 角色缺失
- 年份冲突
- 类型冲突
- 外部 ID 缺失
- 高清海报缺失
- 预告片缺失
- 简介缺失
- 演职员缺失

形成待补全队列。

## 12. 跨模块关系

### 影视 → 音乐

关联 OST、主题曲、片尾曲、插曲、歌手、作曲。

### 影视 → 图库

影视页显示必要图片，完整海报、剧照、人物写真、幕后图、壁纸由图库统一管理。

### 影视 → 直播

未来关联电视台、电影频道、首映、发布会、颁奖礼、直播节目。

## 13. 推荐系统方向

不只做“猜你喜欢”，要逐步支持可解释推荐：

- 因为你看过……
- 同导演
- 同演员
- 同系列
- 同世界观
- 同编剧
- 同摄影
- 同奖项
- 同类型
- 相似主题

最终建立人物—作品—角色—系列—公司—奖项知识图谱。

## 14. 长期目标

用户只需要说：

“把沈腾的电影给我整理齐。”

系统应完成：

识别人物 → 生成完整作品表 → 检查已有媒体 → 找出缺失作品 → 匹配合法可访问资源 → 转存 → 规范命名 → 自动刮削 → 补高清海报 → 匹配官方预告片 → 补完整演职员 → 补人物经历 → 数据冲突检查 → 输出完整度报告。

最终得到的是一套完整、结构化、可搜索、可关联、可播放、可持续补全的私人影视知识库。

## 15. 长期维护原则

本总纲不是一次性说明文。

以后每次确定重要规则，应更新本项目文档并记录 CHANGELOG。

发现旧规则需要调整时，明确修改规范，不在实现中偷偷改变规则。
