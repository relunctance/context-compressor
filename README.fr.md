# 🦅 Context Compressor

> **Moteur de Compression de Contexte Pur** — pas un gestionnaire de mémoire, un sauveur.

Quand le contexte de conversation IA est sur le point d'exploser, Context Compressor le compresse instantanément. 180k → 35k. Continuez à discuter comme si de rien n'était.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## Quelle est la différence avec context-hawk ?

| Outil | Ce qu'il fait | Quand l'utiliser |
|-------|---------------|-----------------|
| **Context Compressor** | Compresse la conversation **actuelle** | Maintenant, quand le contexte est plein |
| **context-hawk** | Gère la mémoire **persistante** entre les sessions | Au quotidien, entre les conversations |

**Context Compressor = Secours d'urgence. context-hawk = Entretien quotidien.**

---

## Comment ça fonctionne

### Avant compression (180k tokens — en explosion)

```
[Historique complet de conversation — 180k tokens — à 88%]
System: You are a helpful assistant...
User: Première question...
Assistant: Première réponse...
User: Deuxième question...
... (de plus en plus long, de plus en plus cher, de plus en plus lent)
```

### Après compression (35k tokens — propre)

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[Invite système préservée en permanence]", "status": "preserved"},
    {"role": "user", "content": "[Question récente complète]", "status": "preserved"},
    {"role": "assistant", "content": "[Réponse récente complète]", "status": "preserved"},
    {"role": "summary", "content": "[Résumé des 45 premiers messages]", "status": "summarized"}
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

## ✨ Fonctionnalités principales

| Fonctionnalité | Description |
|-----------------|-------------|
| **Déclenchement auto** | Compresse automatiquement à 70% du seuil de contexte |
| **4 niveaux de compression** | light / normal / heavy / emergency |
| **Sortie JSON structurée** | Stats complètes : tokens, ratio, comptes |
| **Invite système préservée** | Les définitions de rôle ne sont jamais compressées |
| **Filtrage par importance** | Jette le bruit, garde décisions/règles/code |
| **Déduplication de messages** | Fusionne les confirmations répétées |
| **Réduction de code** | Les blocs de code longs sont repliés en métadonnées |
| **Python pur** | Pas de base de données, pas de dépendances |
| **Écrit dans la mémoire** | Historique de compression sauvegardé dans `memory/today.md` |

---

## 🚀 Démarrage rapide

```bash
# Installer
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# Compresser la conversation actuelle (détection auto du niveau)
hawk-compress

# Compresser avec un niveau spécifique
hawk-compress --level heavy

# Aperçu sans écrire
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

## Niveaux de compression

| Niveau | Quand | Effet |
|---------|-------|-------|
| `light` | 60-70% | Résumer les messages de plus de 30 jours |
| `normal` | 70-85% | Résumer + garder les 10 récents ← **défaut** |
| `heavy` | 85-95% | Garder uniquement les 5 récents |
| `emergency` | > 95% | Garder uniquement les 3 récents |

---

## Déclenchement automatique

Quand le contexte atteint 70%, chaque réponse inclut :

```
[🦅 Context: 72%] Compression recommandée : /hawk-compress
  148k → ~35k | Économisez 113k tokens
```

À 85% et plus, une confirmation forcée est requise avant de continuer.

---

## Structure des fichiers

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Outil CLI Python
└── references/
    ├── compression-logic.md    # Algorithme de compression
    ├── auto-trigger.md        # Système de déclenchement auto
    ├── structured-output.md    # Format de sortie JSON
    └── cli.md                # Référence CLI
```

---

## Prérequis

- Python 3.8+
- Aucune dépendance externe
- Aucune base de données requise

---

## Licence

MIT — libre d'utilisation, de modification et de distribution.
