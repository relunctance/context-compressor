# 🦅 Context Compressor

> **Motore di Compressione del Contesto Puro** — non è un gestore di memoria, è un salvavita.

Quando il contesto della conversazione AI sta per esplodere, Context Compressor lo comprime all'istante. 180k → 35k. Continua a chattare come se niente fosse.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## In cosa differisce da context-hawk?

| Strumento | Cosa fa | Quando usarlo |
|----------|---------|--------------|
| **Context Compressor** | Comprime la conversazione **corrente** | Adesso, quando il contesto è pieno |
| **context-hawk** | Gestisce memoria **persistente** tra le sessioni | Quotidianamente, tra le conversazioni |

**Context Compressor = Soccorso d'emergenza. context-hawk = Manutenzione quotidiana.**

---

## Come funziona

### Prima della compressione (180k token — in esplosione)

```
[Storico completo conversazione — 180k token — all'88%]
System: You are a helpful assistant...
User: Prima domanda...
Assistant: Prima risposta...
User: Seconda domanda...
... (sempre più lungo, più costoso, più lento)
```

### Dopo la compressione (35k token — pulito)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Prompt di sistema preservato permanentemente]", "status": "preserved"},
    {"role": "user", "content": "[Domanda recente completa]", "status": "preserved"},
    {"role": "assistant", "content": "[Risposta recente completa]", "status": "preserved"},
    {"role": "summary", "content": "[Riassunto dei primi 45 messaggi]", "status": "summarized"}
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

## ✨ Funzionalità principali

| Funzionalità | Descrizione |
|-------------|-------------|
| **Trigger automatico** | Comprime automaticamente al 70% della soglia contesto |
| **4 livelli di compressione** | light / normal / heavy / emergency |
| **Output JSON strutturato** | Statistiche complete: token, ratio, conteggi |
| **Prompt di sistema preservato** | Le definizioni di ruolo non vengono mai compresse |
| **Filtraggio per importanza** | Scarta il rumore, tiene decisioni/regole/codice |
| **Deduplicazione messaggi** | Unisce conferme ripetute |
| **Collassamento codice** | Blocchi di codice lunghi piegati a metadati |
| **Python puro** | Nessun database, nessuna dipendenza |
| **Scrive in memoria** | Cronologia compressione salvata in `memory/today.md` |

---

## 🚀 Avvio rapido

```bash
# Installa
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Comprimi conversazione corrente (rilevamento automatico livello)
hawk-compress

# Comprimi con livello specifico
hawk-compress --level heavy

# Anteprima senza scrivere
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

## Livelli di compressione

| Livello | Quando | Effetto |
|---------|--------|---------|
| `light` | 60-70% | Riassumi messaggi più vecchi di 30 giorni |
| `normal` | 70-85% | Riassumi + mantieni gli ultimi 10 ← **predefinito** |
| `heavy` | 85-95% | Mantieni solo gli ultimi 5 |
| `emergency` | > 95% | Mantieni solo gli ultimi 3 |

---

## Trigger automatico

Quando il contesto raggiunge il 70%, ogni risposta include:

```
[🦅 Context: 72%] Compressione raccomandata: /hawk-compress
  148k → ~35k | Risparmia 113k token
```

All'85% e oltre, viene richiesta conferma forzata prima di continuare.

---

## Struttura dei file

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Strumento CLI Python
└── references/
    ├── compression-logic.md    # Algoritmo di compressione
    ├── auto-trigger.md        # Sistema di trigger automatico
    ├── structured-output.md    # Formato output JSON
    └── cli.md                # Riferimento CLI
```

---

## Requisiti

- Python 3.8+
- Nessuna dipendenza esterna
- Nessun database richiesto

---

## Licenza

MIT — libero di usare, modificare e distribuire.
