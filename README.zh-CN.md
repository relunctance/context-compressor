# 🦅 Context Compressor

> **纯上下文压缩引擎** — 不是记忆管理器，是救命稻草。

当 AI 对话上下文即将爆炸时，Context Compressor 可以即时压缩。180k → 35k。继续聊天，就像什么都没发生过。

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## 和 context-hawk 有什么区别？

| 工具 | 功能 | 使用时机 |
|------|------|---------|
| **Context Compressor** | 压缩**当前**对话 | 立即，当上下文满的时候 |
| **context-hawk** | 管理**持久化**记忆，跨 session | 日常，两次对话之间 |

**Context Compressor = 紧急救援。context-hawk = 日常维护。**

---

## 工作原理

### 压缩前（180k tokens — 即将爆炸）

```
[完整对话历史 — 180k tokens — 处于 88%]
System: You are a helpful assistant...
User: 第一个问题...
Assistant: 第一个回答...
User: 第二个问题...
...（越来越长，越来越贵，越来越慢）
```

### 压缩后（35k tokens — 干净）

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[永久保留的系统提示]", "status": "preserved"},
    {"role": "user", "content": "[最新问题完整原文]", "status": "preserved"},
    {"role": "assistant", "content": "[最新回答完整原文]", "status": "preserved"},
    {"role": "summary", "content": "[早期45条消息摘要]", "status": "summarized"}
  ],
  "stats": {
    "original_tokens": 180000,
    "compressed_tokens": 35000,
    "ratio": "5.1x",
    "kept_messages": 5,
    "summarized_count": 87,
    "level": "normal"
  }
}
```

---

## ✨ 核心功能

| 功能 | 说明 |
|---------|-------|
| **自动触发** | 在 70% 上下文阈值时自动压缩 |
| **4 种压缩级别** | light / normal / heavy / emergency |
| **结构化 JSON 输出** | 完整统计：tokens、数量、比值 |
| **系统提示保留** | 角色定义永远不压缩 |
| **重要性过滤** | 丢弃噪音，保留决策/规则/代码 |
| **消息去重** | 合并重复的确认消息 |
| **代码折叠** | 长代码块折叠为元信息 |
| **纯 Python** | 无数据库，无依赖 |
| **写入记忆** | 压缩历史保存到 `memory/today.md` |

---

## 🚀 快速开始

```bash
# 安装
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# 压缩当前对话（自动检测级别）
hawk-compress

# 指定级别压缩
hawk-compress --level heavy

# 预览但不写入
hawk-compress --dry-run

# Python API
python3 -c "
from context_compressor import ContextCompressor
c = ContextCompressor(keep_recent=5)
result = c.compress(your_chat_history)
print(result['stats']['ratio'])
"
```

---

## 压缩级别

| 级别 | 触发时机 | 效果 |
|--------|-------|------|
| `light` | 60-70% | 摘要超过 30 天的消息 |
| `normal` | 70-85% | 摘要 + 保留最近 10 条 ← **默认** |
| `heavy` | 85-95% | 仅保留最近 5 条 |
| `emergency` | > 95% | 仅保留最近 3 条 |

---

## 自动触发

当上下文达到 70% 时，每个回答都会包含：

```
[🦅 Context: 72%] 建议压缩: /hawk-compress
  148k → ~35k | 节省 113k tokens
```

达到 85% 及以上时，强制确认后才能继续。

---

## 文件结构

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLI 工具
└── references/
    ├── compression-logic.md    # 压缩算法
    ├── auto-trigger.md        # 自动触发系统
    ├── structured-output.md    # JSON 输出格式
    └── cli.md                # CLI 参考
```

---

## 要求

- Python 3.8+
- 无外部依赖
- 无需数据库

---

## 授权

MIT — 免费使用、修改与分发。
