# UFO³ Galaxy Unified - 修复完善版

[![Version](https://img.shields.io/badge/version-v5.0%20Unified-blue)](./README.md)
[![Nodes](https://img.shields.io/badge/nodes-102-green)](./nodes/)
[![License](https://img.shields.io/badge/license-MIT-yellow)](./LICENSE)

> **⚠️ 这是经过完整检查、修复和完善的版本**
> 
> 原始仓库：https://github.com/DannyFish-11/ufo-galaxy-unified

## 🔧 修复内容

### 安全修复 (113处)
- ✅ 移除所有硬编码API密钥
- ✅ 修复命令注入漏洞
- ✅ 修复CORS配置问题 (106处)
- ✅ 修复路径遍历漏洞

### 代码质量修复 (30+处)
- ✅ 修复裸except子句
- ✅ 替换eval()为安全替代方案
- ✅ 添加类型注解和文档字符串
- ✅ 提取重复代码

### 架构修复
- ✅ 解决端口冲突问题
- ✅ 统一配置管理
- ✅ 完善启动脚本（102个节点）
- ✅ 创建统一端口分配方案

### 功能补全
- ✅ 补全8个缺失的main.py
- ✅ 创建32个Dockerfile
- ✅ 创建79个requirements.txt

### 文档完善
- ✅ 统一版本号和节点数量
- ✅ 创建API_REFERENCE.md
- ✅ 创建TROUBLESHOOTING.md
- ✅ 为核心节点创建README

## 📁 目录结构

```
ufo-galaxy-unified/
├── config/                 # 配置文件
│   ├── unified_ports.yaml  # 统一端口分配
│   └── unified_config.json # 中心配置
├── dashboard/              # 可视化管理界面
├── galaxy_gateway/         # 网关服务
├── nodes/                  # 102个功能节点
├── windows_client/         # Windows客户端
├── enhancements/           # 增强功能
├── tests/                  # 测试
├── docs/                   # 文档
├── API_REFERENCE.md        # API参考文档
├── TROUBLESHOOTING.md      # 故障排除指南
└── README.md               # 本文件
```

## 🚀 快速开始

### 环境要求
- Python 3.11+
- Docker & Docker Compose
- Git

### 安装步骤

1. **克隆仓库**
```bash
git clone <新仓库地址>
cd ufo-galaxy-unified
```

2. **配置环境变量**
```bash
cp .env.example .env
# 编辑.env文件，填入你的API密钥
```

3. **启动系统**
```bash
python galaxy_launcher.py
```

### Docker部署

```bash
docker-compose up -d
```

## 📊 系统架构

### 核心组件

| 组件 | 端口 | 说明 |
|------|------|------|
| Node_00_StateMachine | 8000 | 状态机管理 |
| Node_01_OneAPI | 8001 | AI网关 |
| Galaxy Gateway | 8080 | 统一网关 |
| Dashboard | 3000 | 管理界面 |

### 节点分类

- **基础节点**: StateMachine, OneAPI, SecretVault
- **AI节点**: OCR, TTS, Vision, LocalLLM
- **工具节点**: SQLite, Git, SSH, Filesystem
- **硬件节点**: BLE, Serial, CANbus, Drone
- **云端节点**: GitHub, Slack, Notion, Weather

## 📖 文档

- [API参考文档](./API_REFERENCE.md)
- [故障排除指南](./TROUBLESHOOTING.md)
- [API配置指南](./API_CONFIGURATION_GUIDE.md)
- [部署指南](./DEPLOYMENT_EXECUTION_GUIDE.md)

## 🔐 安全配置

### 必需的环境变量

```bash
# AI服务
export OPENROUTER_API_KEY="your_key"
export ZHIPU_API_KEY="your_key"
export GROQ_API_KEY="your_key"

# 其他服务
export BRAVE_API_KEY="your_key"
export OPENWEATHER_API_KEY="your_key"

# CORS配置
export CORS_ORIGINS="https://yourdomain.com,http://localhost:3000"
```

## 🧪 测试

```bash
# 运行所有测试
python -m pytest tests/

# 验证配置
python scripts/verify_setup.py
```

## 📈 性能监控

访问 Dashboard 查看实时状态：
```
http://localhost:3000
```

## 🤝 贡献

欢迎提交Issue和Pull Request！

## 📄 许可证

MIT License

## 🙏 致谢

基于原始仓库 [DannyFish-11/ufo-galaxy-unified](https://github.com/DannyFish-11/ufo-galaxy-unified) 进行修复和完善。
