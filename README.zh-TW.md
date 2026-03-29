# 🦅 Context Compressor

> **純上下文壓縮引擎** — 不是記憶管理員，是救命稻草。

當 AI 對話上下文即將爆炸時，Context Compressor 可以即時壓縮。180k → 35k。繼續聊天，就像什麼都沒發生過。

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## 和 context-hawk 有什麼區別？

| 工具 | 功能 | 使用時機 |
|------|------|---------|
| **Context Compressor** | 壓縮**當前**對話 | 立即，當上下文滿的時候 |
| **context-hawk** | 管理**持久化**記憶，跨 session | 日常，兩次對話之間 |

**Context Compressor = 緊急救援。context-hawk = 日常維護。**

---

## 工作原理

### 壓縮前（180k tokens — 即將爆炸）

```
[完整對話歷史 — 180k tokens — 處於 88%]
System: You are a helpful assistant...
User: 第一個問題...
Assistant: 第一個回答...
User: 第二個問題...
...（越來越長，越來越貴，越來越慢）
```

### 壓縮後（35k tokens — 乾淨）

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[永久保留的系統提示]", "status": "preserved"},
    {"role": "user", "content": "[最新問題完整原文]", "status": "preserved"},
    {"role": "assistant", "content": "[最新回答完整原文]", "status": "preserved"},
    {"role": "summary", "content": "[早期45條消息摘要]", "status": "summarized"}
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

| 功能 | 說明 |
|---------|-------|
| **自動觸發** | 在 70% 上下文閾值時自動壓縮 |
| **4 種壓縮級別** | light / normal / heavy / emergency |
| **結構化 JSON 輸出** | 完整統計：tokens、數量、比值 |
| **系統提示保留** | 角色定義永遠不壓縮 |
| **重要性過濾** | 丟棄噪音，保留決策/規則/代碼 |
| **消息去重** | 合併重複的確認消息 |
| **代碼折疊** | 長代碼塊折疊為元信息 |
| **純 Python** | 無數據庫，無依賴 |
| **寫入記憶** | 壓縮歷史保存到 `memory/today.md` |

---

## 🚀 快速開始

```bash
# 安裝
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# 壓縮當前對話（自動檢測級別）
hawk-compress

# 指定級別壓縮
hawk-compress --level heavy

# 預覽但不寫入
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

## 壓縮級別

| 級別 | 觸發時機 | 效果 |
|--------|-------|------|
| `light` | 60-70% | 摘要超過 30 天的消息 |
| `normal` | 70-85% | 摘要 + 保留最近 10 條 ← **預設** |
| `heavy` | 85-95% | 僅保留最近 5 條 |
| `emergency` | > 95% | 僅保留最近 3 條 |

---

## 自動觸發

當上下文達到 70% 時，每個回答都會包含：

```
[🦅 Context: 72%] 建議壓縮: /hawk-compress
  148k → ~35k | 節省 113k tokens
```

達到 85% 及以上時，強制確認後才能繼續。

---

## 檔案結構

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLI 工具
└── references/
    ├── compression-logic.md    # 壓縮演算法
    ├── auto-trigger.md        # 自動觸發系統
    ├── structured-output.md    # JSON 輸出格式
    └── cli.md                # CLI 參考
```

---

## 要求

- Python 3.8+
- 無外部依賴
- 無需資料庫

---

## 授權

MIT — 免費使用、修改與分發。
