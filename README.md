# AI 漫剧自动制作系统

> 🤖 基于最新 AI 技术的端到端漫剧自动化制作平台

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![ComfyUI](https://img.shields.io/badge/ComfyUI-Workflow-green.svg)](https://github.com/comfyanonymous/ComfyUI)

## 📖 项目简介

本项目旨在打造一个**端到端的 AI 漫剧自动制作系统**，实现从剧本创作到成片输出的全流程自动化。通过整合最先进的 AI 技术（大语言模型、Stable Diffusion、视频生成、语音合成等），让用户只需输入故事大纲，即可自动生成包含角色设计、分镜绘制、动态视频、配音配乐在内的完整漫剧作品。

### ✨ 核心特性

- 📝 **智能剧本生成** - AI 辅助故事创作与分镜脚本
- 🎨 **角色一致性保持** - IPAdapter + LoRA 技术确保角色统一
- 🎬 **自动分镜绘制** - ComfyUI 工作流批量生成分镜
- 🎥 **视频动态化** - 可灵/即梦 AI 图生视频
- 🎵 **智能配音配乐** - ElevenLabs + Suno AI 音频生成
- 🔧 **全流程自动化** - 从剧本到成片一键生成

## 📂 文档目录

| 文档 | 说明 |
|------|------|
| [项目规划书](./AI漫剧自动制作系统_项目规划.md) | 完整的技术架构、开发路线图、成本估算 |
| [技术资源汇总](./技术资源汇总.md) | 所有工具链接、API文档、学习资源 |

## 🏗️ 系统架构

```
┌─────────────────────────────────────────────────────────────────┐
│                        用户交互层                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐         │
│  │ Web界面   │  │ API接口   │  │ 移动端    │  │ 插件扩展   │         │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        核心控制层                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              工作流编排引擎 (Workflow Engine)              │   │
│  │     剧本解析 → 任务调度 → 并行处理 → 结果合成 → 质量检查      │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        AI 处理层                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐ │
│  │ 剧本生成   │ │ 角色设计   │ │ 分镜绘制   │ │ 视频生成   │ │ 配音配乐 │ │
│  │  (LLM)    │ │  (SD)     │ │  (SD+CN)  │ │  (VD)     │ │ (TTS+AI)│ │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## 🛠️ 技术栈

### 核心 AI 技术

| 模块 | 技术方案 |
|------|---------|
| **剧本生成** | Claude 3.5 / GPT-4 / 通义千问 |
| **图像生成** | Stable Diffusion XL + ComfyUI |
| **角色一致性** | IPAdapter + InstantID + LoRA |
| **分镜控制** | ControlNet (OpenPose/Canny/Depth) |
| **视频生成** | 可灵AI / 即梦AI / AnimateDiff |
| **语音合成** | ElevenLabs / 讯飞配音 |
| **音乐生成** | Suno AI / Udio / Stable Audio |
| **后期合成** | FFmpeg / MoviePy |

### 开发框架

- **后端**: FastAPI + Celery + Redis
- **数据库**: PostgreSQL + 对象存储
- **部署**: Docker + Kubernetes
- **GPU**: NVIDIA CUDA 12.1+

## 🚀 快速开始

### 环境要求

- Python 3.10+
- NVIDIA GPU (12GB+ VRAM 推荐)
- CUDA 12.1+
- 50GB+ 存储空间

### 安装步骤

```bash
# 1. 克隆仓库
git clone https://github.com/yourusername/ai-manju-system.git
cd ai-manju-system

# 2. 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. 安装依赖
pip install -r requirements.txt

# 4. 配置环境变量
cp .env.example .env
# 编辑 .env 文件，填入 API 密钥

# 5. 启动服务
docker-compose up -d
```

### API 配置

在项目根目录创建 `.env` 文件：

```env
# LLM API
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key

# 视频生成 API
KLING_API_KEY=your_kling_key
DREAMINA_API_KEY=your_dreamina_key

# 语音合成 API
ELEVENLABS_API_KEY=your_elevenlabs_key

# 音乐生成 API
SUNO_COOKIE=your_suno_cookie

# 数据库
DATABASE_URL=postgresql://user:pass@localhost/manju
REDIS_URL=redis://localhost:6379
```

## 📊 项目路线图

### Phase 1: MVP (2-3个月)
- [x] 技术调研与架构设计
- [ ] 剧本生成模块
- [ ] 基础角色生成
- [ ] 分镜批量生成
- [ ] 图生视频集成
- [ ] 基础配音合成
- [ ] Web界面基础版

### Phase 2: 功能完善 (2-3个月)
- [ ] 角色一致性优化
- [ ] 分镜精细控制
- [ ] 本地视频生成
- [ ] 音乐自动生成
- [ ] 批量生产功能

### Phase 3: 平台化 (3-4个月)
- [ ] 用户管理系统
- [ ] 项目管理协作
- [ ] API开放平台
- [ ] 移动端适配

## 💰 成本估算

### 单部作品成本

| 类型 | 时长 | 估算成本 |
|------|------|---------|
| 简单漫剧 | 1-2分钟 | ¥5-15元 |
| 标准漫剧 | 3-5分钟 | ¥20-50元 |
| 精品漫剧 | 10分钟+ | ¥100-300元 |

### 运营成本 (月度)

| 项目 | 估算 |
|------|------|
| GPU云服务器 | ¥2-5万 |
| API调用 | ¥1-3万 |
| 存储/CDN | ¥0.5-1万 |
| **总计** | **¥4-10万/月** |

## 📚 文档导航

- [📋 完整项目规划书](./AI漫剧自动制作系统_项目规划.md) - 包含详细的技术架构、模块设计、数据库Schema、部署配置
- [🔗 技术资源汇总](./技术资源汇总.md) - 所有工具的官网链接、API文档、学习资源

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

感谢以下开源项目和平台的支持：

- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) - 节点式工作流平台
- [Stable Diffusion](https://stability.ai/) - 图像生成模型
- [StoryDiffusion](https://github.com/HVision-NKU/StoryDiffusion) - 角色一致性
- [ElevenLabs](https://elevenlabs.io/) - 语音合成
- [Suno](https://suno.com/) - AI音乐生成

## 📞 联系我们

- 项目主页: https://github.com/yourusername/ai-manju-system
- 问题反馈: https://github.com/yourusername/ai-manju-system/issues
- 邮箱: your.email@example.com

---

> 🎬 让每个人都能创作属于自己的漫剧作品！
