# 🦅 Context Compressor

> **Чистый движок сжатия контекста** — не менеджер памяти, а спасательный круг.

Когда контекст AI-диалога вот-вот взорвётся, Context Compressor сжимает его мгновенно. 180k → 35k. Продолжайте общение, будто ничего не произошло.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## Чем это отличается от context-hawk?

| Инструмент | Что делает | Когда использовать |
|------------|-----------|-------------------|
| **Context Compressor** | Сжимает **текущий** диалог | Прямо сейчас, когда контекст заполнен |
| **context-hawk** | Управляет **постоянной** памятью между сессиями | Ежедневно, между диалогами |

**Context Compressor = Экстренная помощь. context-hawk = Ежедневное обслуживание.**

---

## Как это работает

### До сжатия (180k токенов — взрыв)

```
[Полная история диалога — 180k токенов — на 88%]
System: You are a helpful assistant...
User: Первый вопрос...
Assistant: Первый ответ...
User: Второй вопрос...
... (всё длиннее, дороже, медленнее)
```

### После сжатия (35k токенов — чисто)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Постоянно сохраняемый системный промпт]", "status": "preserved"},
    {"role": "user", "content": "[Последний вопрос целиком]", "status": "preserved"},
    {"role": "assistant", "content": "[Последний ответ целиком]", "status": "preserved"},
    {"role": "summary", "content": "[Резюме первых 45 сообщений]", "status": "summarized"}
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

## ✨ Основные функции

| Функция | Описание |
|---------|---------|
| **Автоматический запуск** | Сжимает автоматически при 70% пороге контекста |
| **4 уровня сжатия** | light / normal / heavy / emergency |
| **Структурированный JSON-вывод** | Полная статистика: токены, коэффициент, количество |
| **Системный промпт сохранён** | Определения ролей никогда не сжимаются |
| **Фильтрация по важности** | Отбрасывает шум, сохраняет решения/правила/код |
| **Дедупликация сообщений** | Объединяет повторные подтверждения |
| **Свёртывание кода** | Длинные блоки кода свёртываются в метаданные |
| **Чистый Python** | Без базы данных, без зависимостей |
| **Запись в память** | История сжатия сохраняется в `memory/today.md` |

---

## 🚀 Быстрый старт

```bash
# Установить
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Сжать текущий диалог (автоопределение уровня)
hawk-compress

# Сжать с определённым уровнем
hawk-compress --level heavy

# Предпросмотр без записи
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

## Уровни сжатия

| Уровень | Когда | Эффект |
|---------|-------|--------|
| `light` | 60-70% | Резюмировать сообщения старше 30 дней |
| `normal` | 70-85% | Резюмировать + сохранить последние 10 ← **по умолчанию** |
| `heavy` | 85-95% | Сохранить только последние 5 |
| `emergency` | > 95% | Сохранить только последние 3 |

---

## Автоматический запуск

Когда контекст достигает 70%, каждый ответ включает:

```
[🦅 Context: 72%] Рекомендуется сжатие: /hawk-compress
  148k → ~35k | Экономия 113k токенов
```

При 85% и выше требуется принудительное подтверждение перед продолжением.

---

## Структура файлов

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLI-инструмент
└── references/
    ├── compression-logic.md    # Алгоритм сжатия
    ├── auto-trigger.md        # Система автозапуска
    ├── structured-output.md    # Формат JSON-вывода
    └── cli.md                # CLI-справочник
```

---

## Требования

- Python 3.8+
- Без внешних зависимостей
- База данных не требуется

---

## Лицензия

MIT — свободно для использования, изменения и распространения.
