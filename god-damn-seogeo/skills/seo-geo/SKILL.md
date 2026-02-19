# SEO & GEO Skill

검색 엔진 최적화(SEO) 및 생성형 엔진 최적화(GEO) 설계를 위한 가이드입니다.

## 사용 시점

- 웹사이트의 SEO를 최적화할 때
- AI/LLM 검색 결과에 노출되기 위한 GEO 최적화할 때
- 검색 엔진에 구조화된 데이터를 제공할 때

## SEO 핵심 요소

### 1. 메타 태그

#### 필수 메타 태그
```html
<head>
  <!-- 문자 인코딩 -->
  <meta charset="UTF-8">

  <!-- 뷰포트 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 페이지 제목 (50-60자 권장) -->
  <title>주요 키워드 - 브랜드명</title>

  <!-- 메타 설명 (150-160자 권장) -->
  <meta name="description" content="페이지 내용을 명확하게 요약한 설명. 검색 결과에 표시됩니다.">

  <!-- 캐노니컬 URL -->
  <link rel="canonical" href="https://example.com/page">

  <!-- 로봇 지시문 -->
  <meta name="robots" content="index, follow">
</head>
```

#### Open Graph (소셜 공유)
```html
<meta property="og:type" content="website">
<meta property="og:title" content="페이지 제목">
<meta property="og:description" content="페이지 설명">
<meta property="og:image" content="https://example.com/og-image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:site_name" content="사이트명">
<meta property="og:locale" content="ko_KR">
```

#### Twitter Card
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="페이지 제목">
<meta name="twitter:description" content="페이지 설명">
<meta name="twitter:image" content="https://example.com/twitter-image.jpg">
<meta name="twitter:site" content="@username">
```

### 2. 시맨틱 HTML 구조

```html
<!DOCTYPE html>
<html lang="ko">
<head>...</head>
<body>
  <header>
    <nav aria-label="주 내비게이션">
      <a href="/">홈</a>
      <a href="/about">소개</a>
    </nav>
  </header>

  <main>
    <article>
      <h1>메인 제목 (페이지당 하나)</h1>
      <section>
        <h2>섹션 제목</h2>
        <p>내용...</p>
      </section>
      <section>
        <h2>다른 섹션</h2>
        <h3>하위 제목</h3>
      </section>
    </article>

    <aside aria-label="관련 콘텐츠">
      <h2>관련 글</h2>
    </aside>
  </main>

  <footer>
    <nav aria-label="푸터 내비게이션">...</nav>
  </footer>
</body>
</html>
```

### 3. 구조화된 데이터 (JSON-LD)

#### Organization
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "회사명",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+82-2-1234-5678",
    "contactType": "customer service",
    "availableLanguage": ["Korean", "English"]
  },
  "sameAs": [
    "https://www.facebook.com/example",
    "https://twitter.com/example"
  ]
}
</script>
```

#### WebSite (사이트 검색)
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "사이트명",
  "url": "https://example.com",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "https://example.com/search?q={search_term_string}",
    "query-input": "required name=search_term_string"
  }
}
</script>
```

#### Article (블로그/뉴스)
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "기사 제목",
  "description": "기사 요약",
  "image": "https://example.com/article-image.jpg",
  "author": {
    "@type": "Person",
    "name": "작성자명"
  },
  "publisher": {
    "@type": "Organization",
    "name": "발행사명",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png"
    }
  },
  "datePublished": "2024-01-15",
  "dateModified": "2024-01-16"
}
</script>
```

#### Product (상품)
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "상품명",
  "description": "상품 설명",
  "image": "https://example.com/product.jpg",
  "brand": {
    "@type": "Brand",
    "name": "브랜드명"
  },
  "offers": {
    "@type": "Offer",
    "price": "29900",
    "priceCurrency": "KRW",
    "availability": "https://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "판매자명"
    }
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.5",
    "reviewCount": "128"
  }
}
</script>
```

#### FAQ
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "질문 1?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "답변 1"
      }
    },
    {
      "@type": "Question",
      "name": "질문 2?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "답변 2"
      }
    }
  ]
}
</script>
```

#### BreadcrumbList (탐색경로)
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "홈",
      "item": "https://example.com"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "카테고리",
      "item": "https://example.com/category"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "현재 페이지"
    }
  ]
}
</script>
```

### 4. 이미지 최적화

```html
<!-- alt 텍스트 필수 -->
<img
  src="product.webp"
  alt="빨간색 운동화 측면 사진"
  width="800"
  height="600"
  loading="lazy"
  decoding="async"
/>

<!-- 반응형 이미지 -->
<picture>
  <source
    media="(min-width: 800px)"
    srcset="large.webp"
    type="image/webp"
  >
  <source
    media="(min-width: 400px)"
    srcset="medium.webp"
    type="image/webp"
  >
  <img
    src="small.jpg"
    alt="제품 이미지"
    width="400"
    height="300"
  >
</picture>
```

### 5. 성능 최적화 (Core Web Vitals)

```html
<!-- 리소스 힌트 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="dns-prefetch" href="https://analytics.example.com">
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>

<!-- 중요 CSS 인라인 -->
<style>
  /* Critical CSS */
</style>

<!-- 나머지 CSS 비동기 로드 -->
<link rel="preload" href="/styles/main.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

## 참조 문서

- [SEO & GEO 통합 체크리스트](seo-geo-checklist.md)
