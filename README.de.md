# 🦅 Context Compressor

> **Reine Kontext-Komprimierungsmaschine** — kein Speichermanager, ein Lebensretter.

Wenn der KI-Konversationskontext kurz vor der Explosion steht, komprimiert Context Compressor ihn sofort. 180k → 35k. Chatte weiter, als wäre nichts gewesen.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## Was ist der Unterschied zu context-hawk?

| Tool | Was es tut | Wann verwenden |
|------|-----------|----------------|
| **Context Compressor** | Komprimiert die **aktuelle** Konversation | Jetzt, wenn der Kontext voll ist |
| **context-hawk** | Verwaltet **persistenten** Speicher über Sitzungen hinweg | Täglich, zwischen Konversationen |

**Context Compressor = Notfallrettung. context-hawk = Tägliche Wartung.**

---

## Wie es funktioniert

### Vor der Komprimierung (180k tokens — explodiert)

```
[Vollständiger Konversationsverlauf — 180k tokens — bei 88%]
System: You are a helpful assistant...
User: Erste Frage...
Assistant: Erste Antwort...
User: Zweite Frage...
... (immer länger, teurer, langsamer)
```

### Nach der Komprimierung (35k tokens — sauber)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Dauerhaft erhaltene System-Prompt]", "status": "preserved"},
    {"role": "user", "content": "[Aktuelle Frage vollständig]", "status": "preserved"},
    {"role": "assistant", "content": "[Aktuelle Antwort vollständig]", "status": "preserved"},
    {"role": "summary", "content": "[Zusammenfassung der ersten 45 Nachrichten]", "status": "summarized"}
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

## ✨ Hauptfunktionen

| Funktion | Beschreibung |
|----------|-------------|
| **Automatischer Trigger** | Komprimiert automatisch bei 70% Kontextschwelle |
| **4 Komprimierungsstufen** | light / normal / heavy / emergency |
| **Strukturierte JSON-Ausgabe** | Vollständige Statistiken: Tokens, Ratio, Anzahl |
| **System-Prompt erhalten** | Rollendefinitionen werden nie komprimiert |
| **Wichtigkeit filtern** | Lärm verwerfen, Entscheidungen/Regeln/Code behalten |
| **Nachrichten-Deduplizierung** | Wiederholte Bestätigungen zusammenführen |
| **Code-Einklappen** | Lange Codeblöcke zu Metadaten gefaltet |
| **Reines Python** | Keine Datenbank, keine Abhängigkeiten |
| **Schreibt in Speicher** | Komprimierungsverlauf in `memory/today.md` gespeichert |

---

## 🚀 Schnellstart

```bash
# Installieren
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Aktuelle Konversation komprimieren (automatische Stufenerkennung)
hawk-compress

# Mit bestimmter Stufe komprimieren
hawk-compress --level heavy

# Vorschau ohne zu schreiben
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

## Komprimierungsstufen

| Stufe | Wann | Effekt |
|-------|------|--------|
| `light` | 60-70% | Nachrichten älter als 30 Tage zusammenfassen |
| `normal` | 70-85% | Zusammenfassen + die letzten 10 behalten ← **Standard** |
| `heavy` | 85-95% | Nur die letzten 5 behalten |
| `emergency` | > 95% | Nur die letzten 3 behalten |

---

## Automatischer Trigger

Wenn der Kontext 70% erreicht, enthält jede Antwort:

```
[🦅 Context: 72%] Komprimierung empfohlen: /hawk-compress
  148k → ~35k | 113k Tokens sparen
```

Bei 85% oder mehr wird eine erzwungene Bestätigung angefordert, bevor fortgefahren wird.

---

## Dateistruktur

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLI-Tool
└── references/
    ├── compression-logic.md    # Komprimierungsalgorithmus
    ├── auto-trigger.md        # Automatisches Trigger-System
    ├── structured-output.md    # JSON-Ausgabeformat
    └── cli.md                # CLI-Referenz
```

---

## Anforderungen

- Python 3.8+
- Keine externen Abhängigkeiten
- Keine Datenbank erforderlich

---

## Lizenz

MIT — kostenlos zu verwenden, zu ändern und zu verbreiten.
