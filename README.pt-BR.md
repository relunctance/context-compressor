# 🦅 Context Compressor

> **Motor de Compressão de Contexto Puro** — não é um gestor de memória, é um salva-vidas.

Quando o contexto da conversa de IA está prestes a explodir, Context Compressor o comprime instantaneamente. 180k → 35k. Continue conversando como se nada tivesse acontecido.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## Em que difere do context-hawk?

| Ferramenta | O que faz | Quando usar |
|-------------|-----------|-------------|
| **Context Compressor** | Comprime a conversa **atual** | Agora, quando o contexto está cheio |
| **context-hawk** | Gerencia memória **persistente** entre sessões | Diariamente, entre conversas |

**Context Compressor = Resgate de emergência. context-hawk = Manutenção diária.**

---

## Como funciona

### Antes da compressão (180k tokens — explodindo)

```
[Histórico completo da conversa — 180k tokens — em 88%]
System: You are a helpful assistant...
User: Primeira pergunta...
Assistant: Primeira resposta...
User: Segunda pergunta...
... (cada vez mais longo, mais caro, mais lento)
```

### Depois da compressão (35k tokens — limpo)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Prompt de sistema preservado permanentemente]", "status": "preserved"},
    {"role": "user", "content": "[Pergunta recente completa]", "status": "preserved"},
    {"role": "assistant", "content": "[Resposta recente completa]", "status": "preserved"},
    {"role": "summary", "content": "[Resumo das primeiras 45 mensagens]", "status": "summarized"}
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

## ✨ Funcionalidades principais

| Funcionalidade | Descrição |
|----------------|-----------|
| **Gatilho automático** | Comprime automaticamente em 70% do limite de contexto |
| **4 níveis de compressão** | light / normal / heavy / emergency |
| **Saída JSON estruturada** | Estatísticas completas: tokens, razão, contagens |
| **Prompt de sistema preservado** | Definições de papel nunca são comprimidas |
| **Filtragem por importância** | Descarta ruído, mantém decisões/regras/código |
| **Deduplicação de mensagens** | Mescla confirmações repetidas |
| **Colapso de código** | Blocos de código longos dobrados em metadados |
| **Python puro** | Sem banco de dados, sem dependências |
| **Escreve na memória** | Histórico de compressão salvo em `memory/today.md` |

---

## 🚀 Início rápido

```bash
# Instalar
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Comprimir conversa atual (detecção automática de nível)
hawk-compress

# Comprimir com nível específico
hawk-compress --level heavy

# Pré-visualizar sem escrever
hawk-compress --dry-run

# API Python
python3 -c "
from context_compressor import ContextCompressor
c = ContextCompressor(keep_recent=5)
result = c.compress(your_chat_history)
print(result['stats']['ratio'])
"
```

---

## Níveis de compressão

| Nível | Quando | Efeito |
|-------|--------|--------|
| `light` | 60-70% | Resumir mensagens com mais de 30 dias |
| `normal` | 70-85% | Resumir + manter os 10 recentes ← **padrão** |
| `heavy` | 85-95% | Manter apenas os 5 recentes |
| `emergency` | > 95% | Manter apenas os 3 recentes |

---

## Gatilho automático

Quando o contexto atinge 70%, cada resposta inclui:

```
[🦅 Context: 72%] Compressão recomendada: /hawk-compress
  148k → ~35k | Economize 113k tokens
```

Em 85% ou mais, é necessária confirmação forçada antes de continuar.

---

## Estrutura de arquivos

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Ferramenta CLI Python
└── references/
    ├── compression-logic.md    # Algoritmo de compressão
    ├── auto-trigger.md        # Sistema de gatilho automático
    ├── structured-output.md    # Formato de saída JSON
    └── cli.md                # Referência CLI
```

---

## Requisitos

- Python 3.8+
- Sem dependências externas
- Sem banco de dados necessário

---

## Licença

MIT — livre para usar, modificar e distribuir.
