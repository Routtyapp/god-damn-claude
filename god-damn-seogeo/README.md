# God Damn SEO/GEO Plugin

SEO(검색엔진 최적화) 및 GEO(생성형 엔진 최적화) 전용 플러그인입니다.

## 설치

```bash
cd god-damn-seogeo
claude --plugin-dir .
```

## Skills 목록

### 1. seo-geo
SEO & GEO 통합 최적화 가이드

```
/god-damn-seogeo:seo-geo
```

**기능:**
- 메타 태그 최적화
- 구조화된 데이터 (JSON-LD)
- 시맨틱 HTML 구조
- 이미지 최적화
- Core Web Vitals
- GEO (AI 검색 최적화) 전략
- SEO & GEO 통합 체크리스트

## 사용 예시

### SEO 메타 태그 최적화
```
/god-damn-seogeo:seo-geo meta

블로그 포스트 페이지의 SEO를 최적화해줘.
Open Graph와 구조화된 데이터도 포함해.
```

### GEO 최적화
```
/god-damn-seogeo:seo-geo geo

상품 페이지를 AI 검색에 최적화해줘.
스키마 속성을 풍부하게 설정해줘.
```

### SEO/GEO 체크리스트 검토
```
/god-damn-seogeo:seo-geo checklist

현재 사이트의 SEO/GEO 상태를 체크리스트로 검토해줘.
```

## 디렉토리 구조

```
god-damn-seogeo/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── seo-geo/
│       ├── SKILL.md
│       └── seo-geo-checklist.md
└── README.md
```

## 라이선스

MIT License
