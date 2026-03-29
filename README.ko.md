# 🦅 Context Compressor

> **순수 컨텍스트 압축 엔진** — 메모리 관리자가 아니라 구명복입니다.

AI 대화 컨텍스트가 폭발하려 할 때, Context Compressor가 즉석에서 압축합니다. 180k → 35k. 아무 일도 없던 것처럼 대화를 계속하세요.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-2026.3%2B-brightgreen)](https://github.com/openclaw/openclaw)

**English** | [中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Italiano](README.it.md) | [Русский](README.ru.md) | [Português (Brasil)](README.pt-BR.md)

---

## context-hawk와 무엇이 다른가요?

| 도구 | 하는 일 | 사용 시기 |
|------|--------|----------|
| **Context Compressor** | **현재** 대화 압축 | 지금, 컨텍스트가 꽉 찼을 때 |
| **context-hawk** | 세션을またぐ**지속적** 메모리 관리 | 일상적, 대화 사이 |

**Context Compressor = 긴급 구조. context-hawk = 일상 유지보수.**

---

## 작동 원리

### 압축 전（180k tokens — 폭발 직전）

```
[전체 대화 기록 — 180k tokens — 88%]
System: You are a helpful assistant...
User: 첫 번째 질문...
Assistant: 첫 번째 답변...
User: 두 번째 질문...
...（점점 길어지고, 비용도 많이 들고, 속도도 느려짐）
```

### 압축 후（35k tokens — 깔끔）

```json
{
  "compressed_prompt": [
    {"role": "system", "content": "[영구 보존할 시스템 프롬프트]", "status": "preserved"},
    {"role": "user", "content": "[최신 질문 원문 전체]", "status": "preserved"},
    {"role": "assistant", "content": "[최신 답변 원문 전체]", "status": "preserved"},
    {"role": "summary", "content": "[이른 45개 메시지 요약]", "status": "summarized"}
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

## ✨ 핵심 기능

| 기능 | 설명 |
|---------|-------|
| **자동 트리거** | 컨텍스트 70% 임계값에서 자동 압축 |
| **4단계 압축 레벨** | light / normal / heavy / emergency |
| **구조화된 JSON 출력** | 완전한 통계: tokens, 비율, 건수 |
| **시스템 프롬프트 보존** | 역할 정의는 절대 압축 안 함 |
| **중요도 필터링** | 노이즈 버리고 결정/규칙/코드 유지 |
| **메시지 중복 제거** | 반복 확인 메시지 병합 |
| **코드 접기** | 긴 코드 블록은 메타 정보로 접기 |
| **순수 Python** | 데이터베이스 없음, 의존성 없음 |
| **메모리 기록** | 압축 이력은 `memory/today.md`에 저장 |

---

## 🚀 빠른 시작

```bash
# 설치
chmod +x scripts/hawk-compress
ln -s scripts/hawk-compress /usr/local/bin/hawk-compress

# 현재 대화 압축（자동 레벨 감지）
hawk-compress

# 특정 레벨로 압축
hawk-compress --level heavy

# 기록 없이 미리보기
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

## 압축 레벨

| 레벨 | 타이밍 | 효과 |
|--------|-------|------|
| `light` | 60-70% | 30일 이상 된 메시지 요약 |
| `normal` | 70-85% | 요약 + 최근 10개 유지 ← **기본값** |
| `heavy` | 85-95% | 최근 5개만 유지 |
| `emergency` | > 95% | 최근 3개만 유지 |

---

## 자동 트리거

컨텍스트가 70%에 도달하면 모든 답변에 다음이 포함됩니다:

```
[🦅 Context: 72%] 압축 권장: /hawk-compress
  148k → ~35k | 113k tokens 절약
```

85% 이상에서는 계속하기 전 강제 확인이 발생합니다.

---

## 파일 구조

```
context-compressor/
├── SKILL.md
├── README.md
├── LICENSE
├── scripts/
│   └── hawk-compress       # Python CLI 도구
└── references/
    ├── compression-logic.md    # 압축 알고리즘
    ├── auto-trigger.md        # 자동 트리거 시스템
    ├── structured-output.md    # JSON 출력 형식
    └── cli.md                # CLI 참조
```

---

## 요구사항

- Python 3.8 이상
- 외부 의존성 없음
- 데이터베이스 불필요

---

## 라이선스

MIT — 무료로 사용, 수정, 배포 가능.
