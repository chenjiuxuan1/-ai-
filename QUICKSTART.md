# AI漫剧自动制作系统 - 快速开始指南

## 🚀 5分钟快速体验

### 方式一：在线体验（推荐）

访问我们的在线演示平台：[演示地址]

### 方式二：本地部署

#### 步骤1：环境准备

```bash
# 检查 NVIDIA GPU
nvidia-smi

# 检查 CUDA 版本
nvcc --version

# 安装 Python 3.10+
python --version
```

#### 步骤2：安装依赖

```bash
# 克隆仓库
git clone https://github.com/yourusername/ai-manju-system.git
cd ai-manju-system

# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 安装 Python 依赖
pip install -r requirements.txt

# 安装 ComfyUI（图像生成引擎）
bash scripts/install_comfyui.sh
```

#### 步骤3：配置 API 密钥

```bash
cp .env.example .env
nano .env  # 或使用你喜欢的编辑器
```

填入以下 API 密钥：

```env
# 必需：至少配置一个 LLM API
OPENAI_API_KEY=sk-...
# 或
ANTHROPIC_API_KEY=sk-ant-...

# 可选：视频生成（不配置则使用本地 AnimateDiff）
KLING_API_KEY=...

# 可选：语音合成（不配置则使用本地 GPT-SoVITS）
ELEVENLABS_API_KEY=...
```

#### 步骤4：启动服务

```bash
# 启动所有服务
docker-compose up -d

# 或分别启动
python -m api.server  # API 服务
celery -A worker.tasks worker  # 任务队列
```

#### 步骤5：创建你的第一个漫剧

```bash
# 使用 CLI 工具
python cli/create_manju.py \
  --story "一个关于勇者与恶龙的奇幻故事" \
  --style anime \
  --duration 3min \
  --output ./output/my_first_manju

# 或使用 Web 界面
open http://localhost:8000
```

## 📁 项目结构

```
ai-manju-system/
├── api/                    # REST API 服务
│   ├── server.py
│   ├── routes/
│   └── models/
├── worker/                 # Celery 任务队列
│   ├── tasks/
│   │   ├── script.py      # 剧本生成
│   │   ├── character.py   # 角色设计
│   │   ├── panel.py       # 分镜生成
│   │   ├── video.py       # 视频生成
│   │   ├── audio.py       # 配音配乐
│   │   └── composite.py   # 后期合成
│   └── celeryconfig.py
├── core/                   # 核心模块
│   ├── script_engine.py   # 剧本引擎
│   ├── visual_engine.py   # 视觉引擎
│   ├── video_engine.py    # 视频引擎
│   ├── audio_engine.py    # 音频引擎
│   └── postprocess.py     # 后期处理
├── comfyui/               # ComfyUI 工作流
│   ├── workflows/         # 工作流定义
│   └── custom_nodes/      # 自定义节点
├── web/                   # Web 前端
│   ├── src/
│   └── public/
├── docs/                  # 文档
├── scripts/               # 脚本工具
├── tests/                 # 测试
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## 🎨 自定义配置

### 修改默认风格

编辑 `config/styles.yaml`：

```yaml
styles:
  anime:
    model: "animagineXLV3_v30.safetensors"
    prompt_prefix: "masterpiece, best quality, anime style"
    negative_prompt: "lowres, bad anatomy, bad hands"
    
  chinese_comic:
    model: "guofeng3XL_v34.safetensors"
    prompt_prefix: "masterpiece, best quality, chinese painting style"
    negative_prompt: "lowres, bad anatomy"
```

### 添加自定义角色

```bash
python cli/add_character.py \
  --name "小明" \
  --description "16岁高中生，黑发，阳光开朗" \
  --images ./characters/xiaoming/*.png
```

### 配置工作流

编辑 `config/workflow.yaml`：

```yaml
workflow:
  script:
    llm: "claude-3.5-sonnet"
    max_scenes: 10
    
  character:
    consistency_method: "ipadapter"  # 或 "lora"
    generate_sheet: true
    
  panel:
    width: 768
    height: 512
    batch_size: 4
    
  video:
    provider: "kling"  # 或 "local"
    duration: 5  # 秒
    
  audio:
    tts_provider: "elevenlabs"
    music_provider: "suno"
```

## 🐛 常见问题

### Q: 没有 GPU 可以运行吗？

A: 可以，但速度会很慢。建议：
- 使用云端 API（可灵、ElevenLabs 等）代替本地模型
- 或使用 CPU 版本的 PyTorch（`pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu`）

### Q: 生成的人物角色不一致怎么办？

A: 尝试以下方法：
1. 使用 IPAdapter 时提高权重（0.8-1.0）
2. 训练专用的 LoRA 模型
3. 固定 Seed 值
4. 使用 Reference Only ControlNet

### Q: API 调用费用太高？

A: 成本优化方案：
- 使用本地模型替代云端 API
- 批量生成提高效率
- 使用开源替代方案（GPT-SoVITS 替代 ElevenLabs）

### Q: 视频生成失败？

A: 检查：
1. API 密钥是否有效
2. 网络连接是否正常
3. 输入图片格式是否正确（建议 768x512）
4. 查看日志 `logs/worker.log`

## 📚 进阶教程

- [自定义 ComfyUI 工作流](./docs/advanced_comfyui.md)
- [训练角色 LoRA](./docs/train_lora.md)
- [API 集成指南](./docs/api_integration.md)
- [部署到生产环境](./docs/production_deploy.md)

## 🤝 获取帮助

- 📖 查看完整文档：[项目规划书](./AI漫剧自动制作系统_项目规划.md)
- 🐛 提交 Issue：https://github.com/yourusername/ai-manju-system/issues
- 💬 加入讨论：https://github.com/yourusername/ai-manju-system/discussions

## ⭐ 下一步

完成快速开始后，建议阅读：
1. [项目规划书](./AI漫剧自动制作系统_项目规划.md) - 了解完整架构
2. [技术资源汇总](./技术资源汇总.md) - 获取工具链接和学习资源
3. 尝试修改配置文件，探索更多可能性！

---

祝你创作愉快！🎬
