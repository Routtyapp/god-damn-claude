# SEO & GEO 통합 체크리스트

SEO(Search Engine Optimization)와 GEO(Generative Engine Optimization)를 모두 고려한 체크리스트입니다.

---

## GEO: AI 최적화

### AI가 선호하는 콘텐츠 구조
- [ ] **목록형 콘텐츠 활용** - AI 인용의 50%가 목록형 콘텐츠
- [ ] **표 형식 데이터 사용** - 표 형식은 인용률 2.5배 증가
- [ ] **두괄식 작성** - 각 섹션 첫 40-60단어 내 핵심 답변 제공
- [ ] **독자적 데이터/통계 포함** - 오리지널 데이터는 30~40% 높은 가시성
- [ ] **최신성 유지** - AI 봇 트래픽 65%가 최근 1년 내 콘텐츠로 향함
- [ ] **명확한 경계의 정보 구조** - AI 패턴 매칭에 유리

### AI를 위한 기술적 최적화
- [ ] **풍부한 스키마 속성** 포함:
  - [ ] 이름, 설명, 가격, 통화
  - [ ] 재고 상태, 브랜드, SKU
  - [ ] 카테고리, 이미지
  - [ ] 옵션 (색상, 사이즈, 재질, 스펙 등)
- [ ] **HTML 구조화** - 복잡한 정보는 이미지가 아닌 HTML 표/리스트로
- [ ] **FAQ, How-to 스키마** - 질문-답변 세트 구성
- [ ] **리뷰 키워드 데이터화** - 리뷰 텍스트 핵심 키워드를 스키마에 포함

### GEO 노출 전략
- [ ] **목록형 기사 상위 노출** 목표 - LLM은 검색 랭킹 참조
- [ ] **권위 있는 디렉토리 등록**:
  - [ ] 백과사전/지식 저장소 (브리태니커 등)
  - [ ] 위키피디아, 블룸버그 등 널리 사용되는 자료
- [ ] **성과 명시** - 기업 규모, 고객 기반, 사용 통계 공개
- [ ] **웹사이트 권위도 향상** 노력

### 권위 및 신뢰성 구축
- [ ] **데이터 기반 증명** - 구체적 통계와 근거 제시
- [ ] **메타태그에 쇼핑 의도 반영**:
  - 예: "~용", "~를 찾는 사람용", "~보다 …한"

---

## URL 최적화 (SEO + GEO 공통)

### URL 작성 원칙
- [ ] **하이픈(-) 사용** - 언더스코어(_) 금지
  - 좋음: `/seo-guide` → "seo guide"로 인식
  - 나쁨: `/seo_guide` → "seoguide"로 인식
- [ ] **소문자만 사용** - 대소문자 혼용 금지
  - 일부 서버는 대소문자 구분 → 링크 오류, 점수 분산
- [ ] **영문 URL만 사용** - 한글 등 비영문 URL 금지
  - `/검색엔진최적화` → `/%EA%B2%80%EC%83%89...` 인코딩
  - 공유 시 깨짐, 검색엔진 인식 어려움
- [ ] **설명적 URL** - 내용을 즉시 파악 가능
  - 좋음: `/sushi` → 스시 관련 페이지
  - 나쁨: `/food?id=1124` → 내용 파악 불가
- [ ] **짧고 간결하게** 유지
- [ ] **불필요한 파라미터 제거**

---

## 기술적 SEO

### 크롤링 & 인덱싱
- [ ] robots.txt 파일 존재 및 올바른 설정
- [ ] XML 사이트맵 생성 및 제출
- [ ] 캐노니컬 URL 설정
- [ ] 중복 콘텐츠 처리
- [ ] 404 페이지 커스터마이징
- [ ] 301 리다이렉트 적절히 사용
- [ ] noindex 태그 필요한 페이지에만 적용

### 사이트 구조
- [ ] 깊이 3단계 이내로 모든 페이지 접근 가능
- [ ] 내부 링크 구조 최적화
- [ ] 브레드크럼 구현
- [ ] HTML 사이트맵 페이지

### 모바일
- [ ] 모바일 친화적 디자인 (반응형)
- [ ] 모바일 뷰포트 설정
- [ ] 터치 요소 간격 충분 (최소 48px)
- [ ] 모바일에서 콘텐츠 숨김 없음
- [ ] Google Mobile-Friendly Test 통과

### 성능 (Core Web Vitals)
- [ ] LCP (Largest Contentful Paint) < 2.5초
- [ ] FID (First Input Delay) < 100ms
- [ ] CLS (Cumulative Layout Shift) < 0.1
- [ ] TTFB (Time to First Byte) < 600ms
- [ ] 이미지 최적화 (WebP, 적절한 크기)
- [ ] JavaScript/CSS 최소화
- [ ] Gzip/Brotli 압축
- [ ] 브라우저 캐싱 설정

### 보안
- [ ] HTTPS 사용 (SSL 인증서)
- [ ] HTTP에서 HTTPS 리다이렉트
- [ ] 혼합 콘텐츠 없음
- [ ] 보안 헤더 설정

---

## 온페이지 SEO

### 제목 태그
- [ ] 모든 페이지에 고유한 `<title>` 태그
- [ ] 제목 길이 50-60자
- [ ] 주요 키워드 포함
- [ ] 브랜드명 포함 (뒤쪽에)
- [ ] 클릭 유도 문구

### 메타 설명
- [ ] 모든 페이지에 고유한 meta description
- [ ] 길이 150-160자
- [ ] 주요 키워드 자연스럽게 포함
- [ ] 행동 유도 문구 (CTA)
- [ ] 검색/쇼핑 의도에 맞는 내용

### 헤딩 구조
- [ ] 페이지당 H1 하나만 사용
- [ ] H1에 주요 키워드 포함
- [ ] 헤딩 계층 구조 (H1 > H2 > H3) 준수
- [ ] 헤딩에 관련 키워드 분산

### 콘텐츠
- [ ] 고유하고 가치 있는 콘텐츠
- [ ] 적절한 콘텐츠 길이 (주제에 맞게)
- [ ] 키워드 자연스럽게 분포
- [ ] 읽기 쉬운 문단 구조
- [ ] 멀티미디어 활용 (이미지, 비디오)
- [ ] **[GEO]** 목록, 표 형식 적극 활용
- [ ] **[GEO]** 핵심 답변 먼저 제시 (두괄식)

### 이미지
- [ ] 모든 이미지에 설명적인 alt 텍스트
- [ ] 파일명에 키워드 포함 (예: red-sneakers.jpg)
- [ ] 이미지 크기 최적화
- [ ] 차세대 포맷 사용 (WebP, AVIF)
- [ ] lazy loading 적용
- [ ] **[GEO]** 복잡한 정보는 이미지 대신 HTML로

### 내부 링크
- [ ] 관련 페이지로 내부 링크
- [ ] 앵커 텍스트에 키워드 포함
- [ ] 깨진 링크 없음
- [ ] 적절한 링크 수

---

## 구조화된 데이터

### 필수 스키마
- [ ] Organization 스키마
- [ ] WebSite 스키마 (검색 기능 포함)
- [ ] BreadcrumbList 스키마

### 페이지 유형별 스키마
- [ ] Article (블로그/뉴스)
- [ ] Product (상품) - **[GEO]** 풍부한 속성 필수
- [ ] LocalBusiness (로컬 비즈니스)
- [ ] FAQPage (FAQ) - **[GEO]** AI 추출에 유리
- [ ] HowTo (가이드) - **[GEO]** AI 추출에 유리
- [ ] Event (이벤트)
- [ ] Review (리뷰) - **[GEO]** 키워드 포함

### 검증
- [ ] Google Rich Results Test 통과
- [ ] Schema Markup Validator 통과

---

## 소셜 미디어

- [ ] Open Graph 태그 설정
- [ ] Twitter Card 태그 설정
- [ ] 소셜 공유 이미지 최적화 (1200x630px)
- [ ] 소셜 미디어 프로필 링크

---

## 로컬 SEO (해당 시)

- [ ] Google Business Profile 등록
- [ ] NAP (이름, 주소, 전화번호) 일관성
- [ ] 로컬 스키마 마크업
- [ ] 지역 키워드 타겟팅
- [ ] 로컬 리뷰 관리

---

## 모니터링 & 분석

### 필수 설정
- [ ] Google Search Console 설정
- [ ] Google Analytics 설정
- [ ] 사이트맵 제출

### 정기 모니터링
- [ ] 크롤링 오류 모니터링
- [ ] 키워드 순위 추적
- [ ] 백링크 모니터링
- [ ] Core Web Vitals 모니터링
- [ ] **[GEO]** AI 챗봇 인용 모니터링

---

## 설정 파일 예시

### robots.txt
```
User-agent: *
Allow: /

# AI 크롤러 허용 (GEO 최적화)
User-agent: GPTBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: Claude-Web
Allow: /

User-agent: PerplexityBot
Allow: /

# 관리자 영역 차단
Disallow: /admin/
Disallow: /api/

# 검색 결과 페이지 차단
Disallow: /search

# 사이트맵 위치
Sitemap: https://example.com/sitemap.xml
```

### sitemap.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2024-01-15</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/about</loc>
    <lastmod>2024-01-10</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

---

## SEO vs GEO 비교

| 항목 | SEO (검색엔진) | GEO (AI/LLM) |
|------|---------------|--------------|
| 대상 | Google, Bing 등 | ChatGPT, Claude, Perplexity 등 |
| 목표 | 검색 순위 상승 | AI 응답에 인용/추천 |
| 콘텐츠 | 키워드 최적화 | 구조화된 정보 (목록, 표) |
| 신뢰도 | 백링크, 도메인 권위 | 권위 있는 출처 등록, 데이터 기반 |
| 최신성 | 중요 | 매우 중요 (1년 내 콘텐츠 선호) |
| 스키마 | 기본 속성 | 풍부한 속성 필수 |

---

## SEO/GEO 도구

### 필수 도구
- Google Search Console
- Google Analytics
- Google PageSpeed Insights

### 권장 도구
- Ahrefs / SEMrush / Moz (키워드/백링크)
- Screaming Frog (크롤링)
- Schema Markup Validator
- Google Mobile-Friendly Test
- Google Rich Results Test

### GEO 모니터링
- AI 챗봇에서 브랜드/제품 검색 테스트
- 인용 여부 및 정확도 확인
