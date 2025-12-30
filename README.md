Image AIGC Gateway

A unified Image AIGC gateway compatible with OpenAI Images API.
Bring your own API keys. No model hosting. No resale. No lock-in.

🚀 项目简介

Image AIGC Gateway 是一个用于 统一接入主流“联网版”图像生成式 AI API 的开源网关服务。

它对外 完全兼容 OpenAI Images API，对内通过 Adapter 适配不同厂商/平台，使调用方可以：

不改代码切换模型 / 平台

同时混用多个图像生成 API

自带 API Key，不经过任何中转存储

用一个接口覆盖同步 / 异步 / 轮询类模型

🎯 解决的问题

当前市面上的图像生成 API 存在以下碎片化问题：

接口格式不统一（OpenAI / 非 OpenAI）

鉴权方式各异（Bearer / Header / 本地签名）

同步 / 异步 / 多阶段任务并存

返回结果结构不一致

本项目的目标：

提供一个“事实标准层”，
让调用方只面对 OpenAI Images API。

✨ 特性一览

✅ 100% OpenAI Images API 兼容

✅ 支持多 Provider（OpenAI / Midjourney / 聚合平台）

✅ 同步 + 异步任务自动处理

✅ 用户自带 Key，项目不提供任何 Key

✅ 可云端部署，也可本地部署

✅ 适合 Web / App / Agent / 自动化脚本

🧩 支持的 Provider（v0.1）
Provider	模型示例	说明
OpenAI	DALL·E / gpt-image-1	原生同步
Midjourney API	v5 / v6	异步 + 轮询
GrsAi	Gemini / SDXL	OpenAI-like

后续 Provider 通过 Adapter 扩展，不影响现有接口。

🔐 关于 API Key（非常重要）

本项目 不提供、不代充、不存储任何 API Key

所有 Key 均由用户自行提供

Key 仅在运行时使用，不写入数据库

👉 Bring Your Own Key (BYOK) 是本项目的核心原则。

📦 安装与运行（本地）
1️⃣ 克隆项目
git clone https://github.com/yourname/image-aigc-gateway.git
cd image-aigc-gateway

2️⃣ 配置环境变量
cp .env.example .env


示例：

OPENAI_API_KEY=sk-xxxx
MJ_API_KEY=xxxx
GRSAI_API_KEY=xxxx

3️⃣ 启动服务
npm install
npm run start


默认监听：

http://localhost:3000

🔌 API 使用方式
统一入口（与 OpenAI 完全一致）
POST /v1/images/generations

请求示例
{
  "model": "mj-v6",
  "prompt": "a futuristic city at night",
  "size": "1024x1024"
}

返回示例
{
  "created": 1735564800,
  "data": [
    {
      "url": "https://example.com/image.png"
    }
  ]
}

🧠 模型选择说明

model 字段用于 逻辑路由

并不等同于底层厂商的真实模型名

实际映射关系由 Provider Adapter 决定

示例：

请求 model	实际调用
gpt-image-1	OpenAI
mj-v6	Midjourney
sd-fast	GrsAi
🏗️ 项目结构（简化）
src/
├─ api/            # OpenAI-compatible API
├─ providers/      # 各平台 Adapter
├─ engine/         # 异步任务管理
└─ server.js

🧭 设计原则

不绑定 UI

不绑定云平台

不绑定单一模型

接口优先，抽象优先

⚠️ 免责声明

本项目不托管任何模型

本项目不提供 API Key

使用者需遵守各平台的使用条款

本项目仅做协议与调用标准化

🤝 贡献方式

欢迎以下类型的贡献：

新 Provider Adapter

模型映射优化

文档改进

Bug 修复

请通过 PR 或 Issue 参与。

📌 Roadmap

 Provider 配置热加载

 模型别名系统

 自动 fallback

 本地化部署模式（Local Gateway）
