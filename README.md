# Web Search Plus Extra

> [web-search-plus](https://github.com/robbyczgw-cla/web-search-plus) 기반 포크 — **Exa API 최신 스펙 반영** 및 검색 옵션 확장

[![Original](https://img.shields.io/badge/Original-web--search--plus-blue)](https://github.com/robbyczgw-cla/web-search-plus)
[![GitHub](https://img.shields.io/badge/GitHub-web--search--plus--extra-black)](https://github.com/min3754/web-search-plus-extra)

---

## Overview

이 저장소는 [robbyczgw-cla/web-search-plus](https://github.com/robbyczgw-cla/web-search-plus) Skills를 기반으로, **Exa API의 최신 스펙을 반영**하여 확장한 버전입니다.

원본 프로젝트는 Exa의 검색 타입을 `neural`/`keyword`만 지원했으나, 본 프로젝트에서는 Exa API v2 스펙에 맞춰 대대적으로 업그레이드했습니다.

---

## Exa API Changes

### Search Type 확장

| 원본 | 변경 후 |
|------|---------|
| `neural`, `keyword` | `auto`, `fast`, `instant`, `deep` |

```bash
python3 scripts/search.py -p exa -q "AI startups" --exa-type auto
python3 scripts/search.py -p exa -q "transformer papers" --exa-type deep
```

### Contents 구조 도입

요청 본문에 `contents` 블록을 통해 텍스트, 하이라이트, 라이브 크롤링을 세밀하게 제어합니다.

```json
{
  "contents": {
    "text": { "maxCharacters": 1000 },
    "highlights": { "maxCharacters": 300, "query": "guiding query" },
    "livecrawl": "fallback"
  }
}
```

### 새로 추가된 옵션

| 옵션 | 설명 |
|------|------|
| `--exa-type` | 검색 모드: `auto`, `fast`, `instant`, `deep` |
| `--exa-livecrawl` | 라이브 크롤링: `never`, `fallback`, `preferred`, `always` |
| `--exa-max-chars` | 텍스트 콘텐츠 최대 글자 수 (기본: 1000) |
| `--highlights-max-chars` | 하이라이트 최대 글자 수 |
| `--guiding-query` | 하이라이트 관련성 가이드 쿼리 |
| `--include-text` | 결과에 반드시 포함할 텍스트 (최대 5단어) |
| `--exclude-text` | 결과에서 제외할 텍스트 |
| `--additional-queries` | deep 모드용 추가 쿼리 변형 |

### Category 확장

원본의 `company`, `research paper`, `news`, `tweet`, `personal site`에 다음이 추가되었습니다:

- `financial report`
- `people`

---

## Usage

### Auto-Routing (자동 라우팅)

```bash
python3 scripts/search.py -q "companies like Stripe"          # -> Exa
python3 scripts/search.py -q "how does HTTPS work"             # -> Tavily
python3 scripts/search.py -q "iPhone 16 price"                 # -> Serper
```

### Exa 직접 사용

```bash
# Deep 모드 + 라이브 크롤링
python3 scripts/search.py -p exa -q "LLM scaling laws" --exa-type deep --exa-livecrawl fallback

# Similar URL 검색
python3 scripts/search.py -p exa --similar-url "https://stripe.com" --category company

# 텍스트 필터링
python3 scripts/search.py -p exa -q "AI agents" --include-text "open source" --exclude-text "deprecated"

# 하이라이트 가이드 쿼리
python3 scripts/search.py -p exa -q "attention mechanism" --category "research paper" --guiding-query "transformer architecture"

# 추가 쿼리 변형 (deep 모드)
python3 scripts/search.py -p exa -q "YC startups 2024" --exa-type deep --additional-queries "seed funding" "Series A"
```

---

## Setup

```bash
# Exa API 키 설정
export EXA_API_KEY="your-key"    # https://exa.ai

# (선택) 다른 프로바이더 키
export SERPER_API_KEY="your-key"          # https://serper.dev
export TAVILY_API_KEY="your-key"          # https://tavily.com
export YOU_API_KEY="your-key"             # https://api.you.com
export KILOCODE_API_KEY="your-key"        # Perplexity via Kilo Gateway

# 설정 파일 생성
cp config.example.json config.json
```

---

## Supported Providers

| Provider | 용도 | API Key |
|----------|------|---------|
| **Serper** | Google 검색 (쇼핑, 로컬, 뉴스) | `SERPER_API_KEY` |
| **Tavily** | AI 리서치 (심층 분석, 전문 콘텐츠) | `TAVILY_API_KEY` |
| **Exa** | 시맨틱 검색 (유사 페이지, 스타트업 탐색) | `EXA_API_KEY` |
| **Perplexity** | 직접 답변 (Sonar Pro via Kilo) | `KILOCODE_API_KEY` |
| **You.com** | RAG/실시간 정보 | `YOU_API_KEY` |
| **SearXNG** | 프라이버시 메타 검색 (셀프호스팅) | 불필요 |

---

## Credits

- Original: [robbyczgw-cla/web-search-plus](https://github.com/robbyczgw-cla/web-search-plus)

## License

MIT
