
# Jimeng AI Free 服务

支持即梦超强图像生成能力，包含最新 **jimeng-4.5**、**jimeng-4.1** 文生图模型、文生图、图生图功能，视频生成模型（目前官方每日赠送 66 积分，可生成 66 次），**Seedance 2.0 多图智能视频生成**，零配置部署，多路 token 支持。

与 OpenAI 接口完全兼容。

## 更新日志

### 2026-02-07 v0.7 更新 - Seedance 2.0 多图智能视频生成
- **新增 Seedance 2.0 模型**：支持多张图片混合生成视频
  - `seedance-2.0`：多图智能视频生成模型
  - `seedance-2.0-pro`：多图智能视频生成模型（专业版）
- **多图混合提示词**：支持使用 `@1`、`@2` 等占位符引用上传的图片
- **修复 multipart 文件上传**：优化 koa-body 配置，支持多文件上传
- **安全漏洞修复**：移除未使用的 `build` 依赖包，升级其他依赖修复 19 个安全漏洞
- **优化参数验证**：`prompt` 参数改为可选（Seedance 模型主要依赖图片）

### 2024-12-20 v0.6 更新 - 新增视频模型
- **新增 jimeng-video-3.5-pro 模型**：最新视频生成模型，使用 `dreamina_ic_generate_video_model_vgfm_3.5_pro` 内部模型
- **升级 Draft 版本**：jimeng-video-3.5-pro 使用 `3.3.4` 版本
- **动态版本管理**：根据不同视频模型自动选择对应的 draft 版本

### 2024-12-12 v0.5 更新 - 参数格式优化
- **统一参数格式**：使用 `ratio`（比例）和 `resolution`（分辨率）替代 `width`/`height` 参数
- **图片接口参数变更**：
  - 移除 `width`、`height`、`size` 参数支持
  - 新增 `ratio` 参数：支持 `1:1`、`4:3`、`3:4`、`16:9`、`9:16`、`3:2`、`2:3`、`21:9`
  - 新增 `resolution` 参数：支持 `1k`、`2k`（默认）、`4k`
- **视频接口参数变更**：
  - 移除 `width`、`height`、`size` 参数支持
  - 新增 `ratio` 参数：支持 `1:1`（默认）、`4:3`、`3:4`、`16:9`、`9:16`
  - `resolution` 参数调整：支持 `480p`、`720p`（默认）、`1080p`
  - 新增 `duration` 参数：视频时长，支持 5 或 10 秒
- **支持 multipart/form-data**：图生图和视频生成支持直接上传文件
- **优化错误提示**：使用不支持的参数时提供清晰的错误信息

### 2024-12-11 v0.4 更新 - 免积分优化
- **修复积分扣费问题**：优化请求参数，实现文生图、图生图功能不再扣除积分
- **核心修复内容**：
  - 添加关键请求参数：`da_version`、`web_component_open_flag`、`web_version`、`aigc_features`
  - 修正参数名：`web_id` → `webId`
  - 修复 `sceneOptions` 双重 JSON 序列化问题
  - 移除图生图中触发计费的 `babi_param` 参数
  - 统一 `DEFAULT_ASSISTANT_ID` 为数字类型
- **更新浏览器指纹**：Chrome 版本从 131 更新到 142
- **移除冗余 Header**：移除 `Last-event-id` 字段

### 2024-12-08 v0.3 更新
- **修复 jimeng-4.5 模型**：修复模型映射名称错误（`high_aes_general_v45` → `high_aes_general_v40l`）
- **更新 API 协议**：同步最新即梦 API 协议，更新 `draft_content` 和 `metrics_extra` 结构
- **升级版本号**：`DRAFT_VERSION` 升级到 `3.3.4`
- **扩展分辨率支持**：支持 1k/2k/4k 多种分辨率和比例（1:1, 4:3, 3:4, 16:9, 9:16, 3:2, 2:3, 21:9）
- **默认分辨率提升**：默认分辨率从 1024x1024 提升到 2048x2048
- **新增字段支持**：添加 `intelligent_ratio`、`resolution_type`、`metadata` 等字段


## 免责声明

**逆向 API 是不稳定的，建议前往即梦 AI 官方 https://jimeng.jianying.com/ 体验功能，避免封禁的风险。**

**本组织和个人不接受任何资金捐助和交易，此项目是纯粹研究交流学习性质！**

**仅限自用，禁止对外提供服务或商用，避免对官方造成服务压力，否则风险自担！**

**仅限自用，禁止对外提供服务或商用，避免对官方造成服务压力，否则风险自担！**

**仅限自用，禁止对外提供服务或商用，避免对官方造成服务压力，否则风险自担！**

## 接入准备

从 [即梦](https://jimeng.jianying.com/) 获取 sessionid

进入即梦登录账号，然后 F12 打开开发者工具，从 Application > Cookies 中找到`sessionid`的值，这将作为 Authorization 的 Bearer Token 值：`Authorization: Bearer sessionid`

![example0](./doc/example-0.png)

### 多账号接入

你可以通过提供多个账号的 sessionid 并使用`,`拼接提供：

`Authorization: Bearer sessionid1,sessionid2,sessionid3`

每次请求服务会从中挑选一个。

## 效果展示

![image-20250910110743543](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250910110743543.png)

![image-20250912225128406](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20250912225128406.png)

![img](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/QQ_1757688779317.png)

多图合成

![img](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/QQ_1757688787070.png)

文生视频3.0

![img](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/QQ_1757688755495.png)

文生视频3.5 （支持视频+声音）

![image-20251220192531051](https://mypicture-1258720957.cos.ap-nanjing.myqcloud.com/Obsidian/image-20251220192531051.png)

## Docker 部署

1.  **拉取代码库**
    ```bash
    git clone https://github.com/zhizinan1997/jimeng-free-api-all.git
    ```
    *   **提示**: 如果您已将此项目 Fork 到自己的 GitHub 账号，请将上述命令中的 `https://github.com/zhizinan1997/jimeng-free-api-all.git` 替换为**您自己 Fork 后的仓库地址**。

2.  **进入项目文件夹**
    ```bash
    cd jimeng-free-api-all
    ```

3.  **构建 Docker 镜像**
    ```bash
    docker build -t jimeng-free-api-all:latest .
    ```
    *   此命令将根据项目中的 `Dockerfile` 构建一个名为 `jimeng-free-api-all` 的本地镜像。

4.  **启动 Docker 容器**
    ```bash
    docker run -it -d --init --name jimeng-free-api-all -p 8000:8000 -e TZ=Asia/Shanghai jimeng-free-api-all:latest
    ```
    *   `-p 8001:8000`: 将宿主机的 `8001` 端口映射到容器内部的 `8000` 端口。您可以根据需要修改 `8001`。
    *   `-e TZ=Asia/Shanghai`: 设置容器内的时区为上海，确保日志和时间戳正确。

## dockerhub 镜像仓库
下载镜像
```bash
docker pull wwwzhouhui569/jimeng-free-api-all:latest
```
启动 Docker 容器
```bash
docker run -it -d --init --name jimeng-free-api-all -p 8001:8000 -e TZ=Asia/Shanghai wwwzhouhui569/jimeng-free-api-all:latest
```



## 接口列表

### 对话补全（绘制图像&生成视频）

目前支持与 openai 兼容的 `/v1/chat/completions` 接口，可自行使用与 openai 或其他兼容的客户端接入接口，模型名称包括：
- **文生图模型**：`jimeng-4.5`（推荐）、`jimeng-4.1`、`jimeng-4.0`、`jimeng-3.1`、`jimeng-3.0`、`jimeng-2.1`、`jimeng-2.0-pro`、`jimeng-2.0`、`jimeng-1.4`、`jimeng-xl-pro`
- **视频生成模型**：`jimeng-video-3.5-pro`（最新）、`jimeng-video-3.0`、`jimeng-video-3.0-pro`、`jimeng-video-2.0`、`jimeng-video-2.0-pro`
- **多图智能视频模型**：`seedance-2.0`、`seedance-2.0-pro`（支持多张图片混合生成视频）

### 模型映射表

| 用户模型名 | 内部模型名 | 说明 |
|-----------|-----------|------|
| `jimeng-4.5` | `high_aes_general_v40l` | 最新模型，推荐使用 |
| `jimeng-video-3.5-pro` | `dreamina_ic_generate_video_model_vgfm_3.5_pro` | 最新视频模型 |
| `seedance-2.0` | `dreamina_seedance_40_pro` | 多图智能视频生成 |
| `seedance-2.0-pro` | `dreamina_seedance_40_pro` | 多图智能视频生成（专业版） |
| `jimeng-4.1` | `high_aes_general_v41` | 高质量模型 |
| `jimeng-4.0` | `high_aes_general_v40` | 稳定版本 |
| `jimeng-3.1` | `high_aes_general_v30l_art_fangzhou` | 艺术风格 |
| `jimeng-3.0` | `high_aes_general_v30l` | 通用模型 |

### 支持的分辨率

**1k 分辨率：**
- 1:1 (1024x1024), 4:3 (768x1024), 3:4 (1024x768), 16:9 (1024x576), 9:16 (576x1024), 3:2 (1024x682), 2:3 (682x1024), 21:9 (1195x512)

**2k 分辨率（默认）：**
- 1:1 (2048x2048), 4:3 (2304x1728), 3:4 (1728x2304), 16:9 (2560x1440), 9:16 (1440x2560), 3:2 (2496x1664), 2:3 (1664x2496), 21:9 (3024x1296)

**4k 分辨率：**
- 1:1 (4096x4096), 4:3 (4608x3456), 3:4 (3456x4608), 16:9 (5120x2880), 9:16 (2880x5120), 3:2 (4992x3328), 2:3 (3328x4992), 21:9 (6048x2592)

使用文生图模型时支持智能多图生成（jimeng-4.5、jimeng-4.1、jimeng-4.0 支持连续场景、绘本故事等多张图片生成），使用视频模型时为视频生成。

### 视频生成

视频生成接口，支持通过直接调用video接口或通过chat接口使用视频模型生成视频。

**POST /v1/videos/generations**

header 需要设置 Authorization 头部：

```
Authorization: Bearer [sessionid]
```

请求数据：

```json
{
  "model": "jimeng-video-3.5-pro",
  "prompt": "人物慢向前移动,还唱着歌",
  "ratio": "4:3",
  "resolution": "720p",
  "file_paths": ["首帧图片URL", "尾帧图片URL"]
}
```

**参数说明：**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| model | string | 否 | jimeng-video-3.0 | 模型名称 |
| prompt | string | 是 | - | 视频描述文本 |
| ratio | string | 否 | 1:1 | 宽高比：1:1, 4:3, 3:4, 16:9, 9:16 |
| resolution | string | 否 | 720p | 分辨率：480p, 720p, 1080p |
| duration | number | 否 | 5 | 视频时长：5 或 10 秒 |
| file_paths | array | 否 | [] | 首帧/尾帧图片URL数组 |

**视频分辨率对照表：**

| 分辨率 | 1:1 | 4:3 | 3:4 | 16:9 | 9:16 |
|--------|-----|-----|-----|------|------|
| 480p | 480x480 | 640x480 | 480x640 | 854x480 | 480x854 |
| 720p | 720x720 | 960x720 | 720x960 | 1280x720 | 720x1280 |
| 1080p | 1080x1080 | 1440x1080 | 1080x1440 | 1920x1080 | 1080x1920 |

响应数据：

```json
{
  "created": 1733593745,
  "data": [{
    "url": "https://v9-artist.vlabvod.com/...",
    "revised_prompt": "视频描述文本"
  }]
}
```
可用视频模型：`jimeng-video-3.5-pro`（推荐）、`jimeng-video-3.0`、`jimeng-video-3.0-pro`、`jimeng-video-2.0`、`jimeng-video-2.0-pro`

### Seedance 2.0 多图智能视频生成

Seedance 2.0 是即梦推出的多图智能视频生成模型，支持上传多张图片并通过提示词描述生成视频。

**POST /v1/videos/generations**

**multipart/form-data 格式请求：**

```bash
curl -X POST "http://localhost:8000/v1/videos/generations" \
  -H "Authorization: Bearer your_sessionid" \
  -F "model=seedance-2.0" \
  -F "prompt=@1 和 @2 两人开始跳舞" \
  -F "ratio=4:3" \
  -F "duration=4" \
  -F "files=@/path/to/image1.jpg" \
  -F "files=@/path/to/image2.jpg"
```

**JSON 格式请求（使用图片URL）：**

```json
{
  "model": "seedance-2.0",
  "prompt": "使用 @1 图片和 @2 图片，两人开始跳舞",
  "ratio": "4:3",
  "duration": 4,
  "file_paths": [
    "https://example.com/image1.jpg",
    "https://example.com/image2.jpg"
  ]
}
```

**Seedance 参数说明：**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| model | string | 是 | - | `seedance-2.0` 或 `seedance-2.0-pro` |
| prompt | string | 否 | - | 提示词，使用 `@1`、`@2` 等引用图片 |
| ratio | string | 否 | 4:3 | 宽高比：1:1, 4:3, 3:4, 16:9, 9:16 |
| duration | number | 否 | 4 | 视频时长（秒），默认 4 秒 |
| files | file[] | 是* | - | 上传的图片文件（multipart/form-data） |
| file_paths | array | 是* | - | 图片URL数组（JSON格式） |

> **注意**：`files` 和 `file_paths` 二选一，Seedance 模型至少需要一张图片。

**提示词占位符说明：**

- `@1` - 引用第一张上传的图片
- `@2` - 引用第二张上传的图片
- `@图1`、`@image1` - 同 `@1`

**示例提示词：**
- `"使用 @1 图片，人物开始跳舞"`
- `"@1 和 @2 两个角色互相对话"`
- `"让 @1 图片中的场景动起来，添加下雨效果"`


### 图像生成

图像生成接口，与 openai 的 [images-create-api](https://platform.openai.com/docs/api-reference/images/create) 兼容。

#### 文生图接口

**POST /v1/images/generations**

header 需要设置 Authorization 头部：

```
Authorization: Bearer [sessionid]
```

请求数据：

```json
{
  "model": "jimeng-4.5",
  "prompt": "美丽的风景画，夕阳下的湖泊",
  "negative_prompt": "",
  "ratio": "16:9",
  "resolution": "2k",
  "sample_strength": 0.5,
  "response_format": "url"
}
```

**参数说明：**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| model | string | 否 | jimeng-4.5 | 模型名称 |
| prompt | string | 是 | - | 提示词，jimeng-4.x 支持多图生成（如"生成4张连续场景的图片"） |
| negative_prompt | string | 否 | "" | 反向提示词 |
| ratio | string | 否 | 1:1 | 宽高比：1:1, 4:3, 3:4, 16:9, 9:16, 3:2, 2:3, 21:9 |
| resolution | string | 否 | 2k | 分辨率：1k, 2k, 4k |
| sample_strength | number | 否 | 0.5 | 精细度，取值范围 0-1 |
| intelligent_ratio | boolean | 否 | false | 智能比例 |
| response_format | string | 否 | url | 响应格式：url 或 b64_json |

**图片分辨率对照表：**

| 分辨率 | 1:1 | 4:3 | 3:4 | 16:9 | 9:16 | 3:2 | 2:3 | 21:9 |
|--------|-----|-----|-----|------|------|-----|-----|------|
| 1k | 1024x1024 | 768x1024 | 1024x768 | 1024x576 | 576x1024 | 1024x682 | 682x1024 | 1195x512 |
| 2k | 2048x2048 | 2304x1728 | 1728x2304 | 2560x1440 | 1440x2560 | 2496x1664 | 1664x2496 | 3024x1296 |
| 4k | 4096x4096 | 4608x3456 | 3456x4608 | 5120x2880 | 2880x5120 | 4992x3328 | 3328x4992 | 6048x2592 |

#### 图生图接口

**POST /v1/images/compositions**

支持多张输入图片的图像合成功能，支持 JSON 和 multipart/form-data 两种格式：

**JSON 格式请求：**

```json
{
  "model": "jimeng-4.5",
  "prompt": "将这些图片合成为一幅美丽的风景画",
  "images": [
    "https://example.com/image1.jpg",
    {"url": "https://example.com/image2.jpg"}
  ],
  "negative_prompt": "",
  "ratio": "16:9",
  "resolution": "2k",
  "sample_strength": 0.5,
  "response_format": "url"
}
```

**multipart/form-data 格式：** 支持直接上传图片文件，使用 `images` 字段。

**参数说明：**

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| model | string | 否 | jimeng-4.5 | 模型名称 |
| prompt | string | 是 | - | 提示词 |
| images | array | 是 | - | 输入图片数组（URL字符串或对象格式），1-10张 |
| negative_prompt | string | 否 | "" | 反向提示词 |
| ratio | string | 否 | 1:1 | 宽高比 |
| resolution | string | 否 | 2k | 分辨率：1k, 2k, 4k |
| sample_strength | number | 否 | 0.5 | 精细度，取值范围 0-1 |
| response_format | string | 否 | url | 响应格式：url 或 b64_json |

响应数据：

```json
{
  "created": 1733593745,
  "data": [
    {
      "url": "https://p9-heycan-hgt-sign.byteimg.com/..."
    }
  ],
  "input_images": 2,
  "composition_type": "multi_image_synthesis"
}
```

#### 客户端 curl 调用示例

**文生图接口调用：**

```bash
curl -X POST http://localhost:8000/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "jimeng-4.5",
    "prompt": "美丽的日落风景，湖边的小屋",
    "ratio": "16:9",
    "resolution": "2k",
    "sample_strength": 0.7
  }'
```

**jimeng-4.5 多图生成：**

```bash
curl -X POST http://localhost:8000/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "jimeng-4.5",
    "prompt": "生成4张连续场景的图片：春夏秋冬四季风景",
    "ratio": "4:3",
    "resolution": "2k",
    "sample_strength": 0.5
  }'
```

**图生图接口调用：**

```bash
curl -X POST http://localhost:8000/v1/images/compositions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "jimeng-4.5",
    "prompt": "将这些图片合成为一幅充满创意的艺术作品",
    "images": [
      "https://example.com/image1.jpg",
      "https://example.com/image2.jpg"
    ],
    "ratio": "16:9",
    "resolution": "2k",
    "sample_strength": 0.6
  }'
```

**视频生成接口调用：**

```bash
curl -X POST http://localhost:8000/v1/videos/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "jimeng-video-3.0",
    "prompt": "一只可爱的小猫在草地上玩耍",
    "ratio": "16:9",
    "resolution": "720p",
    "duration": 5
  }'
```

**带首帧图片的视频生成：**

```bash
curl -X POST http://localhost:8000/v1/videos/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "jimeng-video-3.0",
    "prompt": "让图片中的场景动起来",
    "ratio": "16:9",
    "resolution": "720p",
    "duration": 5,
    "file_paths": ["https://example.com/first_frame.jpg"]
  }'
```

**Seedance 2.0 多图视频生成（文件上传）：**

```bash
curl -X POST "http://localhost:8000/v1/videos/generations" \
  -H "Authorization: Bearer your_sessionid_here" \
  -F "model=seedance-2.0" \
  -F "prompt=@1 和 @2 两人开始跳舞" \
  -F "ratio=4:3" \
  -F "duration=4" \
  -F "files=@/path/to/image1.jpg" \
  -F "files=@/path/to/image2.jpg"
```

**Seedance 2.0 多图视频生成（URL方式）：**

```bash
curl -X POST http://localhost:8000/v1/videos/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid_here" \
  -d '{
    "model": "seedance-2.0",
    "prompt": "使用 @1 图片和 @2 图片，两人开始跳舞",
    "ratio": "4:3",
    "duration": 4,
    "file_paths": [
      "https://example.com/image1.jpg",
      "https://example.com/image2.jpg"
    ]
  }'
```


## 感谢

感谢 [jimeng-free-api-all](https://github.com/zhizinan1997/jimeng-free-api-all) 项目的贡献

感谢 [jimeng-free-api](https://github.com/LLM-Red-Team/jimeng-free-api) 项目的贡献
