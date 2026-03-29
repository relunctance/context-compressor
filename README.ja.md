# 🦅 Context Compressor

> **Pure Context Compression Engine** — メモリマネージャーではなく、命を救うツール。

AI会話のコンテキストが爆発しそうなとき、Context Compressorが即座に圧縮します。180k → 35k。何もなかったかのようにチャットを続けられます。

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## context-hawk との違いは？

| ツール | 機能 | 使用タイミング |
|--------|------|---------------|
| **Context Compressor** | **現在の**会話を圧縮 | 今すぐ、コンテキストが満タンのとき |
| **context-hawk** | セッションをまたぐ**永続的**メモリを管理 | 日常、会話の間 |

**Context Compressor = 緊急救助。context-hawk = 日常メンテナンス。**

---

## 動作原理

### 圧縮前（180k tokens — 爆発寸前）

```
[完全な会話履歴 — 180k tokens — 88%]
System: You are a helpful assistant...
User: 最初の質問...
Assistant: 最初の回答...
User: 2番目の質問...
...（どんどん長くなり、コストも速度も悪化）
```

### 圧縮後（35k tokens — クリーン）

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[恒久保持のシステムプロンプト]", "status": "preserved"},
    {"role": "user", "content": "[最新の質問全文]", "status": "preserved"},
    {"role": "assistant", "content": "[最新の回答全文]", "status": "preserved"},
    {"role": "summary", "content": "[早期45件のメッセージ要約]", "status": "summarized"}
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

## ✨ コア機能

| 機能 | 説明 |
|---------|-------|
| **自動トリガー** | コンテキストが70%に達すると自動圧縮 |
| **4段階の圧縮レベル** | light / normal / heavy / emergency |
| **構造化JSON出力** | 完全統計：tokens、比、件数 |
| **システムプロンプト保持** | ロール定義は絶対に圧縮されない |
| **重要度フィルタリング** | ノイズを破棄し、決定/ルール/コードを保持 |
| **メッセージ重複排除** | 繰り返される確認をマージ |
| **コード折りたたみ** | 長いコードブロックはメタ情報に折りたたみ |
| **Pure Python** | データベース不要、依存関係なし |
| **メモリへの書き込み** | 圧縮履歴は `memory/today.md` に保存 |

---

## 🚀 クイックスタート

```bash
# インストール
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# 現在の会話を圧縮（自動レベル検出）
hawk-compress

# 特定のレベルで圧縮
hawk-compress --level heavy

# 書き込まずにプレビュー
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

## 圧縮レベル

| レベル | タイミング | 効果 |
|--------|-----------|------|
| `light` | 60-70% | 30日以上のメッセージを要約 |
| `normal` | 70-85% | 要約 + 最近の10件を保持 ← **デフォルト** |
| `heavy` | 85-95% | 最近の5件のみ保持 |
| `emergency` | > 95% | 最近の3件のみ保持 |

---

## 自動トリガー

コンテキストが70%に達すると、すべての回答に以下が含まれます：

```
[🦅 Context: 72%] 圧縮推奨: /hawk-compress
  148k → ~35k | 113k tokens節約
```

85%以上では、続行前に強制確認が発生します。

---

## ファイル構造

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLIツール
└── references/
    ├── compression-logic.md    # 圧縮アルゴリズム
    ├── auto-trigger.md        # 自動トリガーシステム
    ├── structured-output.md    # JSON出力フォーマット
    └── cli.md                # CLIリファレンス
```

---

## 必要要件

- Python 3.8以上
- 外部依存なし
- データベース不要

---

## ライセンス

MIT — 無償で自由に使用、改変、配布できます。
