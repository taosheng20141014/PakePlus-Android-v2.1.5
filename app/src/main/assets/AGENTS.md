# 小人国奇遇记 - 项目文档

## 项目概述

"小人国奇遇记"是一个儿童绘画网页应用，支持绘画、物品拖拽、背景切换和 AI 视频生成功能。

## 技术栈

-   **前端**: HTML5 + CSS3 + JavaScript (单文件)
-   **后端**: Python FastAPI
-   **AI 服务**: coze-coding-dev-sdk (豆包 AI)
    -   视频生成 (豆包 Seedance 模型)

## 目录结构

```
/workspace/projects/
├── index.html      # 主应用文件
├── server.py       # Python后端服务
├── .coze          # 项目配置文件
├── public/images/  # 静态资源目录（背景图+物品图）
│   ├── bg-*.jpg    # 背景图片
│   └── food_variants/*.png  # 物品图片
├── images/         # 原始图片备份
└── temp/          # 临时文件目录
```

## 重要提示

-   所有静态资源（背景图、物品图）路径统一使用 `/public/images/`
-   视频生成会自动将图片上传到对象存储获取公网 URL

## 视频生成功能

### 使用流程

1. 点击"AI 视频"按钮打开视频生成模态框
2. 点击"开始录音"使用麦克风录制讲解
3. 录音完成后可重新录制或试听
4. 填写视频描述（可选）
5. 选择时长（3 秒/5 秒/7 秒）和尺寸（1:1/16:9/9:16）
6. 点击"生成视频"等待 AI 处理

### API 接口

#### 健康检查

-   `GET /api/health` - 返回服务状态

#### 视频生成

-   `POST /api/generate-video` - 图生视频

    -   参数: `{ image_url, prompt, duration, ratio }`
    -   返回: `{ success, videoUrl, taskId }`

-   `POST /api/generate-video-with-audio` - 录音+视频生成
    -   参数: FormData (audio 文件, image_url, prompt, duration, ratio)
    -   返回: `{ success, videoUrl, taskId }`

#### 图片上传

-   `POST /api/upload-image-base64` - 上传 Base64 图片

## 视频生成参数

-   **时长**: 3 秒、5 秒、7 秒
-   **尺寸**: 1:1 方形、16:9 宽屏、9:16 竖屏
-   **模型**: doubao-seedance-1-5-pro-251215

## 启动服务

```bash
python server.py
```

服务将运行在 http://localhost:5000

## 注意事项

1. AI 视频生成可能需要较长时间（30 秒-2 分钟）
2. 图片文件大小限制: 10MB
3. 需要麦克风权限才能录制语音讲解
