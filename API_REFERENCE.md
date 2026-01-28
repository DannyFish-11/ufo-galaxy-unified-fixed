# UFO³ Galaxy API 参考文档

**版本**: v5.0 Unified  
**更新日期**: 2026-01-24  
**总节点数**: 102 个

---

## 📚 目录

1. [网关 API](#网关-api)
2. [节点 API](#节点-api)
3. [LLM API](#llm-api)
4. [任务 API](#任务-api)
5. [监控 API](#监控-api)
6. [WebSocket API](#websocket-api)
7. [AIP 协议](#aip-协议)

---

## 网关 API

### 基础信息

| 属性 | 值 |
|:---|:---|
| **服务名称** | UFO³ Galaxy Gateway |
| **版本** | 1.0.0 |
| **基础 URL** | `http://localhost:8888` |
| **健康检查** | `GET /health` |

### 端点概览

```
GET  /                    # 服务信息
GET  /health              # 健康检查
GET  /api/stats           # 系统统计
```

### 服务信息

**请求:**
```bash
curl http://localhost:8888/
```

**响应:**
```json
{
  "service": "UFO³ Galaxy Gateway",
  "version": "1.0.0",
  "status": "online",
  "endpoints": {
    "llm": {
      "chat": "/api/llm/chat",
      "ask": "/api/llm/ask",
      "code": "/api/llm/code",
      "search": "/api/llm/search"
    },
    "node": {
      "list": "/api/node/list",
      "info": "/api/node/{node_id}",
      "call": "/api/node/call",
      "health": "/api/node/{node_id}/health"
    },
    "task": {
      "execute": "/api/task/execute",
      "batch": "/api/task/batch"
    }
  }
}
```

---

## 节点 API

### 列出所有节点

**请求:**
```bash
GET /api/node/list
GET /api/node/list?category=CORE
GET /api/node/list?status=online
```

**参数:**
| 参数 | 类型 | 必填 | 说明 |
|:---|:---|:---:|:---|
| category | string | 否 | 节点类别: CORE, INTEGRATION, HARDWARE, AI, SYSTEM |
| status | string | 否 | 节点状态: online, offline, unknown |

**响应:**
```json
{
  "count": 102,
  "nodes": [
    {
      "node_id": "node_01",
      "name": "OneAPI",
      "description": "多模型 LLM 网关",
      "category": "CORE",
      "url": "http://localhost:8001",
      "port": 8001,
      "methods": ["chat", "completion"],
      "status": "online",
      "priority": 1
    }
  ]
}
```

### 获取节点信息

**请求:**
```bash
GET /api/node/{node_id}
```

**示例:**
```bash
curl http://localhost:8888/api/node/node_01
```

### 检查节点健康

**请求:**
```bash
GET /api/node/{node_id}/health
```

**响应:**
```json
{
  "node_id": "node_01",
  "healthy": true,
  "status": "online"
}
```

### 调用节点方法

**请求:**
```bash
POST /api/node/call
Content-Type: application/json
```

**请求体:**
```json
{
  "node_id": "node_22",
  "method": "search",
  "params": {
    "query": "UFO Galaxy",
    "limit": 10
  }
}
```

**响应:**
```json
{
  "results": [...],
  "status": "success"
}
```

---

## LLM API

### 聊天接口

**请求:**
```bash
POST /api/llm/chat
Content-Type: application/json
```

**请求体:**
```json
{
  "messages": [
    {"role": "system", "content": "你是一个助手"},
    {"role": "user", "content": "你好"}
  ],
  "model": "auto",
  "temperature": 0.7,
  "max_tokens": 2000,
  "stream": false
}
```

**参数说明:**
| 参数 | 类型 | 必填 | 默认值 | 说明 |
|:---|:---|:---:|:---:|:---|
| messages | array | 是 | - | 消息列表 |
| model | string | 否 | "auto" | 模型名称或 "auto" |
| temperature | float | 否 | 0.7 | 温度参数 (0-1) |
| max_tokens | int | 否 | 2000 | 最大生成token数 |
| stream | bool | 否 | false | 是否流式输出 |

**支持的模型:**
| 模型 | 提供商 | 用途 |
|:---|:---|:---|
| auto | - | 自动选择最优模型 |
| groq/llama-3.3-70b | Groq | 快速响应 |
| zhipu/glm-4-flash | 智谱AI | 中文任务 |
| perplexity/sonar-pro | Perplexity | 实时搜索 |
| claude/claude-3.5 | Claude | 复杂推理 |
| local/qwen2.5-7b | Ollama | 本地推理 |
| local/deepseek-coder | Ollama | 代码生成 |

### 简单问答

**请求:**
```bash
POST /api/llm/ask
Content-Type: application/json
```

**请求体:**
```json
{
  "question": "什么是UFO Galaxy?",
  "model": "auto",
  "system_prompt": "你是一个技术专家"
}
```

### 代码生成

**请求:**
```bash
POST /api/llm/code
Content-Type: application/json
```

**请求体:**
```json
{
  "prompt": "写一个Python函数计算斐波那契数列",
  "language": "python"
}
```

### 实时搜索

**请求:**
```bash
POST /api/llm/search?question=今天的新闻
```

**响应:**
```json
{
  "result": "搜索结果..."
}
```

---

## 任务 API

### 执行智能任务

**请求:**
```bash
POST /api/task/execute
Content-Type: application/json
```

**请求体:**
```json
{
  "task": "搜索今天的科技新闻并总结",
  "auto_route": true,
  "context": {}
}
```

**响应:**
```json
{
  "task": "搜索今天的科技新闻并总结",
  "analysis": "任务分析结果...",
  "status": "analyzed",
  "message": "任务分析完成"
}
```

### 批量执行任务

**请求:**
```bash
POST /api/task/batch
Content-Type: application/json
```

**请求体:**
```json
{
  "tasks": [
    {
      "node": "node_22",
      "method": "search",
      "params": {"query": "AI news"}
    },
    {
      "node": "node_24",
      "method": "get_weather",
      "params": {"city": "Beijing"}
    }
  ]
}
```

**响应:**
```json
{
  "total": 2,
  "success": 2,
  "failed": 0,
  "results": [
    {"node": "node_22", "status": "success", "result": {...}},
    {"node": "node_24", "status": "success", "result": {...}}
  ]
}
```

---

## 监控 API

### 系统统计

**请求:**
```bash
GET /api/stats
```

**响应:**
```json
{
  "total_nodes": 102,
  "categories": {
    "CORE": 10,
    "INTEGRATION": 16,
    "HARDWARE": 17,
    "AI": 15,
    "SYSTEM": 6
  },
  "status": {
    "online": 98,
    "offline": 2,
    "unknown": 2
  },
  "llm_client": {
    "one_api_url": "http://localhost:8001",
    "local_llm_url": "http://localhost:11434"
  }
}
```

---

## WebSocket API

### Agent 连接端点

**URL:** `ws://localhost:8888/ws/agent`

用于 Android Agent 和其他设备建立实时连接。

**连接示例 (JavaScript):**
```javascript
const ws = new WebSocket('ws://localhost:8888/ws/agent');

ws.onopen = () => {
  console.log('Connected to Galaxy Gateway');
  ws.send(JSON.stringify({
    type: 'register',
    device_id: 'android_001',
    capabilities: ['adb', 'screen']
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Received:', data);
};
```

**消息类型:**
| 类型 | 说明 |
|:---|:---|
| register | 设备注册 |
| command | 发送命令 |
| response | 命令响应 |
| heartbeat | 心跳保持 |
| status | 状态更新 |

---

## AIP 协议

### 协议概述

AIP (Agent Interaction Protocol) 是 UFO Galaxy 系统中节点间通信的标准协议。

**版本:** AIP/1.0

### 消息格式

```json
{
  "version": "AIP/1.0",
  "message_id": "uuid",
  "timestamp": "2026-01-24T10:00:00Z",
  "sender": "node_01",
  "receiver": "node_02",
  "message_type": "REQUEST|RESPONSE|EVENT",
  "payload": {...}
}
```

### 消息类型

| 类型 | 说明 |
|:---|:---|
| REQUEST | 请求消息，需要响应 |
| RESPONSE | 响应消息 |
| EVENT | 事件消息，无需响应 |
| ERROR | 错误消息 |

### 标准响应格式

**成功响应:**
```json
{
  "status": "success",
  "data": {...},
  "message_id": "uuid"
}
```

**错误响应:**
```json
{
  "status": "error",
  "error_code": "NODE_NOT_FOUND",
  "error_message": "节点不存在",
  "message_id": "uuid"
}
```

---

## 错误代码

| 代码 | HTTP状态 | 说明 |
|:---|:---:|:---|
| NODE_NOT_FOUND | 404 | 节点不存在 |
| NODE_OFFLINE | 503 | 节点离线 |
| METHOD_NOT_FOUND | 404 | 方法不存在 |
| INVALID_PARAMS | 400 | 参数无效 |
| LLM_ERROR | 500 | LLM调用失败 |
| TIMEOUT | 504 | 请求超时 |
| UNAUTHORIZED | 401 | 未授权 |
| RATE_LIMITED | 429 | 请求频率限制 |

---

## 代码示例

### Python

```python
import httpx
import asyncio

async def galaxy_api_demo():
    base_url = "http://localhost:8888"
    
    async with httpx.AsyncClient() as client:
        # 1. 检查健康
        response = await client.get(f"{base_url}/health")
        print("Health:", response.json())
        
        # 2. 列出节点
        response = await client.get(f"{base_url}/api/node/list")
        nodes = response.json()
        print(f"Total nodes: {nodes['count']}")
        
        # 3. LLM 聊天
        response = await client.post(
            f"{base_url}/api/llm/chat",
            json={
                "messages": [{"role": "user", "content": "你好"}],
                "model": "auto"
            }
        )
        print("Chat:", response.json())
        
        # 4. 调用节点
        response = await client.post(
            f"{base_url}/api/node/call",
            json={
                "node_id": "node_24",
                "method": "get_weather",
                "params": {"city": "Beijing"}
            }
        )
        print("Weather:", response.json())

asyncio.run(galaxy_api_demo())
```

### cURL

```bash
# 健康检查
curl http://localhost:8888/health

# 列出节点
curl http://localhost:8888/api/node/list

# LLM 聊天
curl -X POST http://localhost:8888/api/llm/chat \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [{"role": "user", "content": "你好"}],
    "model": "auto"
  }'

# 调用天气节点
curl -X POST http://localhost:8888/api/node/call \
  -H "Content-Type: application/json" \
  -d '{
    "node_id": "node_24",
    "method": "get_weather",
    "params": {"city": "Beijing"}
  }'
```

---

## 相关文档

- [API 配置指南](API_CONFIGURATION_GUIDE.md)
- [节点功能清单](FINAL_NODE_STATUS.md)
- [部署指南](COMPLETE_SYSTEM_DEPLOYMENT_GUIDE.md)

---

**维护者**: Manus AI  
**最后更新**: 2026-01-24
