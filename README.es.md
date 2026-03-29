# 🦅 Context Compressor

> **Motor de Compresión de Contexto Puro** — no es un gestor de memoria, es un salvavidas.

Cuando el contexto de conversación de IA está a punto de explotar, Context Compressor lo comprime al instante. 180k → 35k. Sigue chateando como si nada hubiera pasado.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## ¿En qué se diferencia de context-hawk?

| Herramienta | Qué hace | Cuándo usarla |
|-------------|----------|---------------|
| **Context Compressor** | Comprime la conversación **actual** | Ahora mismo, cuando el contexto está lleno |
| **context-hawk** | Gestiona memoria **persistente** entre sesiones | Diariamente, entre conversaciones |

**Context Compressor = Rescate de emergencia. context-hawk = Mantenimiento diario.**

---

## Cómo funciona

### Antes de comprimir (180k tokens — explotando)

```
[Historial completo de conversación — 180k tokens — al 88%]
System: You are a helpful assistant...
User: Primera pregunta...
Assistant: Primera respuesta...
User: Segunda pregunta...
... (cada vez más largo, más caro, más lento)
```

### Después de comprimir (35k tokens — limpio)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Prompt del sistema preservado permanentemente]", "status": "preserved"},
    {"role": "user", "content": "[Pregunta reciente completa]", "status": "preserved"},
    {"role": "assistant", "content": "[Respuesta reciente completa]", "status": "preserved"},
    {"role": "summary", "content": "[Resumen de los primeros 45 mensajes]", "status": "summarized"}
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

## ✨ Funciones principales

| Función | Descripción |
|---------|-------------|
| **Disparador automático** | Comprime automáticamente al 70% del umbral de contexto |
| **4 niveles de compresión** | light / normal / heavy / emergency |
| **Salida JSON estructurada** | Stats completas: tokens, ratio, conteos |
| **Prompt del sistema preservado** | Las definiciones de rol nunca se comprimen |
| **Filtrado por importancia** | Descarta ruido, guarda decisiones/reglas/código |
| **Deduplicación de mensajes** | Fusiona confirmaciones repetidas |
| **Colapso de código** | Bloques de código largos se pliegan a metadatos |
| **Python puro** | Sin base de datos, sin dependencias |
| **Escribe en memoria** | Historial de compresión guardado en `memory/today.md` |

---

## 🚀 Inicio rápido

```bash
# Instalar
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Comprimir conversación actual (detección automática de nivel)
hawk-compress

# Comprimir con nivel específico
hawk-compress --level heavy

# Vista previa sin escribir
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

## Niveles de compresión

| Nivel | Cuándo | Efecto |
|-------|--------|--------|
| `light` | 60-70% | Resumir mensajes de más de 30 días |
| `normal` | 70-85% | Resumir + mantener los 10 recientes ← **predeterminado** |
| `heavy` | 85-95% | Mantener solo los 5 recientes |
| `emergency` | > 95% | Mantener solo los 3 recientes |

---

## Disparador automático

Cuando el contexto alcanza 70%, cada respuesta incluye:

```
[🦅 Context: 72%] Compresión recomendada: /hawk-compress
  148k → ~35k | Ahorra 113k tokens
```

A 85% o más, se requiere confirmación forzada antes de continuar.

---

## Estructura de archivos

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Herramienta CLI Python
└── references/
    ├── compression-logic.md    # Algoritmo de compresión
    ├── auto-trigger.md        # Sistema de disparador automático
    ├── structured-output.md    # Formato de salida JSON
    └── cli.md                # Referencia CLI
```

---

## Requisitos

- Python 3.8+
- Sin dependencias externas
- Sin base de datos requerida

---

## Licencia

MIT — libre de usar, modificar y distribuir.
