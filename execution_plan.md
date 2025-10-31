# Execution Plan
# Stock Info Dashboard 개발 실행 계획서

**프로젝트**: 주식 투자자를 위한 기업 기본정보 제공 서비스
**기반 문서**: PRD.md v1.0
**작성일**: 2025-10-31
**총 예상 기간**: 4주 (160 작업시간)

---

## 📋 Executive Summary

본 실행 계획서는 PRD.md에 정의된 Stock Info Dashboard를 개발하기 위한 구체적인 작업 분해 및 실행 순서를 제시합니다. 총 24개의 독립적인 태스크로 분해하였으며, 각 태스크는 200k 토큰 컨텍스트 내에서 완료 가능하도록 설계되었습니다.

**핵심 원칙:**
- 각 태스크는 독립적으로 실행 가능
- 의존성이 있는 태스크는 명확히 표시
- 병렬 실행 가능한 태스크 그룹화
- 단일 컨텍스트 내 완료 가능

---

## 🎯 Project Scope

### In Scope (Phase 1 MVP)
- ✅ 주가지수 조회 (KOSPI, KOSDAQ)
- ✅ 환율 조회 (USD, EUR, JPY, CNY)
- ✅ 금 선물 가격 조회
- ✅ 금리 조회 (기준금리, CD금리, 국고채, 연준금리)
- ✅ 실시간 데이터 갱신 (1~15분 주기)
- ✅ 모바일 반응형 UI
- ✅ 한국어 지원

### Out of Scope (Future Phases)
- ❌ 개별 종목 조회
- ❌ 포트폴리오 관리
- ❌ 알림 기능
- ❌ 커뮤니티 기능
- ❌ 유료 서비스

---

## 📊 Task Decomposition

### Total: 24 Tasks
- Foundation: 4 tasks
- API Integration: 4 tasks
- Backend API Routes: 4 tasks
- Frontend Components: 4 tasks
- Integration: 4 tasks
- Quality & Deployment: 4 tasks

---

## 🏗️ Wave 1: Foundation (병렬 실행 가능)

### TASK-001: 프로젝트 구조 변경 및 리네이밍
**난이도**: 🟢 Low
**예상 시간**: 2시간
**의존성**: None
**병렬 가능**: Yes

**목표:**
- 현재 cryptocurrency 프로젝트를 stock-info-dashboard로 전환
- 디렉토리 구조 재정비

**상세 작업:**
1. `crypto-ranking-board.tsx` → `components/stock-dashboard.tsx` 이동
2. 관련 컴포넌트 파일명 변경
3. Import 경로 수정
4. package.json name 변경: `stock-info-dashboard`
5. README.md 업데이트

**완료 기준:**
- [ ] 모든 파일이 새로운 구조로 정리됨
- [ ] Build 에러 없음
- [ ] `pnpm dev` 정상 실행

**산출물:**
```
components/
  ├── stock-dashboard.tsx
  ├── cards/
  │   ├── stock-index-card.tsx
  │   ├── exchange-rate-card.tsx
  │   ├── gold-price-card.tsx
  │   └── interest-rate-card.tsx
```

---

### TASK-002: 타입 정의 및 인터페이스 설계
**난이도**: 🟢 Low
**예상 시간**: 3시간
**의존성**: None
**병렬 가능**: Yes

**목표:**
- 모든 데이터 타입 정의
- API 응답 인터페이스 설계

**상세 작업:**
1. `lib/types/` 디렉토리 생성
2. 각 기능별 타입 정의:
   - `stock-index.ts`
   - `exchange-rate.ts`
   - `gold-price.ts`
   - `interest-rate.ts`
3. 공통 API 응답 타입 정의 (`api-response.ts`)
4. Zod 스키마 정의 (validation용)

**완료 기준:**
- [ ] 모든 인터페이스가 camelCase 규칙 준수
- [ ] Zod 스키마로 런타임 검증 가능
- [ ] TypeScript strict mode 통과

**산출물:**
```typescript
// lib/types/stock-index.ts
export interface StockIndexData {
  name: string
  code: string
  currentPrice: number
  change: number
  changePercent: number
  high: number
  low: number
  volume: number
  tradingValue: number
  updatedAt: string
}

// lib/types/api-response.ts
export interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: ApiError
  timestamp: string
}
```

---

### TASK-003: API 라우트 구조 설계
**난이도**: 🟢 Low
**예상 시간**: 2시간
**의존성**: TASK-002
**병렬 가능**: Yes

**목표:**
- Next.js API Routes 구조 설계
- 공통 유틸리티 함수 작성

**상세 작업:**
1. `app/api/` 디렉토리 구조 설계
2. API 응답 래퍼 함수 작성 (`lib/api-utils.ts`)
3. 에러 핸들러 작성
4. Rate limiting 미들웨어 설계 (향후 적용)

**완료 기준:**
- [ ] API 라우트 파일 스캐폴딩 완료
- [ ] 공통 응답 형식 적용 가능

**산출물:**
```
app/api/
  ├── stocks/route.ts
  ├── exchange/route.ts
  ├── gold/route.ts
  └── interest-rates/route.ts

lib/
  └── api-utils.ts
```

---

### TASK-004: UI 컴포넌트 라이브러리 구축
**난이도**: 🟡 Medium
**예상 시간**: 4시간
**의존성**: None
**병렬 가능**: Yes

**목표:**
- 재사용 가능한 UI 컴포넌트 작성
- 디자인 시스템 정립

**상세 작업:**
1. InfoCard 기본 컴포넌트 (4개 카드의 부모 컴포넌트)
2. PriceDisplay 컴포넌트 (숫자 포맷팅)
3. ChangeIndicator 컴포넌트 (등락 표시)
4. LoadingCard, ErrorCard 컴포넌트
5. Tailwind 테마 색상 정의 (빨강/파랑)

**완료 기준:**
- [ ] 모든 컴포넌트가 shadcn/ui 패턴 따름
- [ ] Storybook 또는 개발 페이지에서 확인 가능
- [ ] 다크 모드 지원

**산출물:**
```typescript
// components/ui/info-card.tsx
export function InfoCard({ title, children, status }: InfoCardProps) {
  return (
    <Card className={cn("p-6", statusColors[status])}>
      <CardHeader>
        <CardTitle>{title}</CardTitle>
      </CardHeader>
      <CardContent>{children}</CardContent>
    </Card>
  )
}
```

---

## 🔌 Wave 2: External API Integration (병렬 실행 가능)

### TASK-005: 주가지수 API 연동
**난이도**: 🔴 High
**예상 시간**: 8시간
**의존성**: TASK-002
**병렬 가능**: Yes

**목표:**
- KOSPI, KOSDAQ 실시간 데이터 가져오기

**API 후보:**
1. **네이버 금융 크롤링** (비공식)
2. **한국투자증권 Open API** (공식, 인증 필요)
3. **KIS Developers API** (공식, 무료 티어 제한)

**상세 작업:**
1. API 선정 및 가입/인증
2. `lib/external-api/stock-api.ts` 작성
3. 데이터 파싱 및 타입 변환
4. 에러 핸들링 (API 장애 시 대응)
5. 캐싱 전략 (1분 TTL)

**완료 기준:**
- [ ] KOSPI, KOSDAQ 현재가 조회 성공
- [ ] 전일 대비 등락률 계산 정확
- [ ] 장 마감 시간 처리 (종가 고정)
- [ ] API 호출 실패 시 Fallback 데이터

**산출물:**
```typescript
// lib/external-api/stock-api.ts
export async function fetchStockIndices(): Promise<StockIndexData[]> {
  const response = await fetch('https://api.example.com/indices')
  const data = await response.json()
  return data.map(parseStockIndex)
}
```

**리스크:**
- 공식 API 비용 발생 가능
- 크롤링 시 차단 위험

---

### TASK-006: 환율 API 연동
**난이도**: 🟡 Medium
**예상 시간**: 5시간
**의존성**: TASK-002
**병렬 가능**: Yes

**목표:**
- USD, EUR, JPY, CNY 환율 데이터 가져오기

**API 후보:**
1. **한국수출입은행 환율 API** (공식, 무료, 추천)
2. **Exchange Rates API** (글로벌, 무료 티어)

**상세 작업:**
1. 한국수출입은행 API 키 발급
2. `lib/external-api/exchange-api.ts` 작성
3. 4개 통화 데이터 동시 조회
4. 매매기준율 파싱
5. 캐싱 (10분 TTL)

**완료 기준:**
- [ ] 4개 통화 데이터 정상 조회
- [ ] 전일 대비 등락 계산
- [ ] 주말/공휴일 데이터 처리

**산출물:**
```typescript
// lib/external-api/exchange-api.ts
export async function fetchExchangeRates(): Promise<ExchangeRateData[]> {
  const currencies = ['USD', 'EUR', 'JPY', 'CNY']
  const promises = currencies.map(currency =>
    fetch(`https://www.koreaexim.go.kr/site/program/financial/exchangeJSON?authkey=${API_KEY}&data=AP01`)
  )
  const results = await Promise.all(promises)
  return results.map(parseExchangeRate)
}
```

---

### TASK-007: 금 선물 API 연동
**난이도**: 🟡 Medium
**예상 시간**: 5시간
**의존성**: TASK-002
**병렬 가능**: Yes

**목표:**
- 국제 금 선물 가격 (USD/oz) 가져오기

**API 후보:**
1. **Alpha Vantage** (무료 티어: 25 calls/day)
2. **Yahoo Finance API** (비공식)
3. **Metals-API** (유료)

**상세 작업:**
1. API 선정 및 키 발급
2. `lib/external-api/gold-api.ts` 작성
3. COMEX 금 선물 가격 파싱
4. 캐싱 (15분 TTL)

**완료 기준:**
- [ ] 금 선물 현재가 조회
- [ ] 전일 대비 등락 계산
- [ ] USD/oz 단위 표시

**산출물:**
```typescript
// lib/external-api/gold-api.ts
export async function fetchGoldPrice(): Promise<GoldPriceData> {
  const response = await fetch(`https://www.alphavantage.co/query?function=COMMODITY&symbol=GOLD&apikey=${API_KEY}`)
  const data = await response.json()
  return parseGoldPrice(data)
}
```

---

### TASK-008: 금리 API 연동
**난이도**: 🟡 Medium
**예상 시간**: 6시간
**의존성**: TASK-002
**병렬 가능**: Yes

**목표:**
- 한국 기준금리, CD금리, 국고채 3년, 미국 기준금리 조회

**API 후보:**
1. **한국은행 경제통계시스템 (ECOS)** (공식, 무료)
2. **FRED API** (미국 연준, 무료)

**상세 작업:**
1. ECOS API 인증키 발급
2. `lib/external-api/interest-rate-api.ts` 작성
3. 4개 금리 지표 동시 조회
4. 최신 데이터 파싱 (금통위 회의 결과)
5. 캐싱 (1일 TTL, 금리는 자주 변하지 않음)

**완료 기준:**
- [ ] 한국 기준금리 조회
- [ ] CD금리, 국고채 3년 조회
- [ ] 미국 기준금리 조회
- [ ] 변동 날짜 표시

**산출물:**
```typescript
// lib/external-api/interest-rate-api.ts
export async function fetchInterestRates(): Promise<InterestRateData[]> {
  const koreanRates = await fetchFromECOS()
  const usRate = await fetchFromFRED()
  return [...koreanRates, usRate]
}
```

---

## 🛠️ Wave 3: Backend API Routes (병렬 실행 가능)

### TASK-009: /api/stocks 엔드포인트 개발
**난이도**: 🟡 Medium
**예상 시간**: 4시간
**의존성**: TASK-003, TASK-005
**병렬 가능**: Yes

**목표:**
- 주가지수 데이터를 제공하는 API 엔드포인트

**상세 작업:**
1. `app/api/stocks/route.ts` 작성
2. TASK-005에서 작성한 함수 호출
3. 응답 캐싱 (1분)
4. 에러 처리

**완료 기준:**
- [ ] `GET /api/stocks` 응답 성공
- [ ] 표준 ApiResponse 형식 준수
- [ ] Cache-Control 헤더 설정

**산출물:**
```typescript
// app/api/stocks/route.ts
export async function GET() {
  try {
    const data = await fetchStockIndices()
    return NextResponse.json({
      success: true,
      data,
      timestamp: new Date().toISOString()
    }, {
      headers: { 'Cache-Control': 'public, max-age=60' }
    })
  } catch (error) {
    return handleApiError(error)
  }
}
```

---

### TASK-010: /api/exchange 엔드포인트 개발
**난이도**: 🟢 Low
**예상 시간**: 3시간
**의존성**: TASK-003, TASK-006
**병렬 가능**: Yes

**목표:**
- 환율 데이터 제공 API

**상세 작업:**
1. `app/api/exchange/route.ts` 작성
2. 4개 통화 데이터 반환
3. 캐싱 (10분)

**완료 기준:**
- [ ] `GET /api/exchange` 정상 동작
- [ ] 4개 통화 모두 포함

**산출물:**
```typescript
// app/api/exchange/route.ts
export async function GET() {
  const data = await fetchExchangeRates()
  return createApiResponse(data, 600) // 10분 캐시
}
```

---

### TASK-011: /api/gold 엔드포인트 개발
**난이도**: 🟢 Low
**예상 시간**: 3시간
**의존성**: TASK-003, TASK-007
**병렬 가능**: Yes

**목표:**
- 금 선물 가격 제공 API

**완료 기준:**
- [ ] `GET /api/gold` 정상 동작
- [ ] 15분 캐시 설정

---

### TASK-012: /api/interest-rates 엔드포인트 개발
**난이도**: 🟢 Low
**예상 시간**: 3시간
**의존성**: TASK-003, TASK-008
**병렬 가능**: Yes

**목표:**
- 금리 데이터 제공 API

**완료 기준:**
- [ ] `GET /api/interest-rates` 정상 동작
- [ ] 1일 캐시 설정

---

## 🎨 Wave 4: Frontend Components (병렬 실행 가능)

### TASK-013: 주가지수 카드 컴포넌트
**난이도**: 🟡 Medium
**예상 시간**: 5시간
**의존성**: TASK-004, TASK-009
**병렬 가능**: Yes

**목표:**
- KOSPI, KOSDAQ 표시 카드 UI

**상세 작업:**
1. `components/cards/stock-index-card.tsx` 작성
2. SWR로 `/api/stocks` 데이터 페칭
3. 등락률에 따라 빨강/파랑 색상 표시
4. 로딩/에러 상태 처리

**완료 기준:**
- [ ] 현재가, 등락률, 거래량 표시
- [ ] 상승 빨강, 하락 파랑
- [ ] 1분마다 자동 갱신

**산출물:**
```tsx
// components/cards/stock-index-card.tsx
export function StockIndexCard() {
  const { data, error, isLoading } = useSWR('/api/stocks', fetcher, {
    refreshInterval: 60000 // 1분
  })

  if (isLoading) return <LoadingCard />
  if (error) return <ErrorCard message="주가 데이터를 불러올 수 없습니다" />

  return (
    <InfoCard title="주가지수">
      {data.map(index => (
        <div key={index.code}>
          <h3>{index.name}</h3>
          <PriceDisplay value={index.currentPrice} />
          <ChangeIndicator change={index.changePercent} />
        </div>
      ))}
    </InfoCard>
  )
}
```

---

### TASK-014: 환율 카드 컴포넌트
**난이도**: 🟢 Low
**예상 시간**: 4시간
**의존성**: TASK-004, TASK-010
**병렬 가능**: Yes

**목표:**
- 4개 통화 환율 표시

**완료 기준:**
- [ ] USD, EUR, JPY, CNY 표시
- [ ] 10분 자동 갱신

---

### TASK-015: 금 선물 카드 컴포넌트
**난이도**: 🟢 Low
**예상 시간**: 4시간
**의존성**: TASK-004, TASK-011
**병렬 가능**: Yes

**목표:**
- 금 가격 표시 (USD/oz)

**완료 기준:**
- [ ] 금 아이콘 표시
- [ ] 15분 자동 갱신

---

### TASK-016: 금리 카드 컴포넌트
**난이도**: 🟢 Low
**예상 시간**: 4시간
**의존성**: TASK-004, TASK-012
**병렬 가능**: Yes

**목표:**
- 4개 금리 지표 표시

**완료 기준:**
- [ ] 기준금리, CD금리, 국고채, 연준금리
- [ ] 변동 날짜 표시

---

## 🔗 Wave 5: Integration (순차 실행)

### TASK-017: 메인 대시보드 통합
**난이도**: 🟡 Medium
**예상 시간**: 6시간
**의존성**: TASK-013~016
**병렬 가능**: No

**목표:**
- 4개 카드를 하나의 대시보드에 통합

**상세 작업:**
1. `components/stock-dashboard.tsx` 완성
2. 2x2 그리드 레이아웃 (데스크톱)
3. 세로 스크롤 레이아웃 (모바일)
4. 전체 새로고침 기능

**완료 기준:**
- [ ] 4개 카드 모두 표시
- [ ] 반응형 레이아웃
- [ ] Pull-to-refresh (모바일)

**산출물:**
```tsx
// components/stock-dashboard.tsx
export function StockDashboard() {
  return (
    <div className="container mx-auto p-4">
      <h1 className="text-3xl font-bold mb-6">주식 투자 대시보드</h1>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <StockIndexCard />
        <ExchangeRateCard />
        <GoldPriceCard />
        <InterestRateCard />
      </div>
      <footer className="mt-8 text-center text-sm text-muted-foreground">
        마지막 업데이트: {new Date().toLocaleString('ko-KR')} | 1분마다 자동 갱신
      </footer>
    </div>
  )
}
```

---

## 🎯 Wave 6: Polish (병렬 실행 가능)

### TASK-018: 실시간 데이터 갱신 (SWR 설정)
**난이도**: 🟢 Low
**예상 시간**: 3시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- SWR 전역 설정 최적화

**상세 작업:**
1. `app/providers.tsx`에 SWRConfig 추가
2. refreshInterval 설정
3. revalidateOnFocus 설정
4. onError 핸들러

**완료 기준:**
- [ ] 자동 갱신 동작 확인
- [ ] 탭 전환 시 재검증

---

### TASK-019: 에러 처리 및 로딩 상태
**난이도**: 🟡 Medium
**예상 시간**: 4시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- 모든 에러 케이스 처리

**상세 작업:**
1. API 장애 시 Fallback UI
2. 네트워크 오류 메시지
3. Skeleton 로딩 상태
4. React Error Boundary

**완료 기준:**
- [ ] 모든 API 실패 케이스 처리
- [ ] 사용자 친화적 에러 메시지

---

### TASK-020: 반응형 디자인 최적화
**난이도**: 🟡 Medium
**예상 시간**: 5시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- 모바일/태블릿/데스크톱 최적화

**상세 작업:**
1. 브레이크포인트 테스트 (375px, 768px, 1024px, 1440px)
2. 폰트 크기 조정
3. 터치 인터랙션 개선
4. 다크 모드 색상 조정

**완료 기준:**
- [ ] 모든 디바이스에서 깨지지 않음
- [ ] 터치 타겟 44x44px 이상
- [ ] 다크 모드 정상 동작

---

## ✅ Wave 7: Quality & Deployment (병렬 실행 가능)

### TASK-021: 테스트 작성
**난이도**: 🔴 High
**예상 시간**: 10시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- Unit + Integration 테스트

**상세 작업:**
1. API 라우트 테스트 (Mock 데이터)
2. 컴포넌트 렌더링 테스트
3. 데이터 페칭 테스트
4. E2E 테스트 (Playwright, Optional)

**완료 기준:**
- [ ] 핵심 기능 80% 커버리지
- [ ] CI에서 자동 실행

**산출물:**
```typescript
// __tests__/api/stocks.test.ts
describe('GET /api/stocks', () => {
  it('should return stock indices data', async () => {
    const response = await fetch('/api/stocks')
    const data = await response.json()
    expect(data.success).toBe(true)
    expect(data.data).toHaveLength(2)
  })
})
```

---

### TASK-022: 성능 최적화
**난이도**: 🟡 Medium
**예상 시간**: 6시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- Lighthouse 성능 점수 90+ 달성

**상세 작업:**
1. Next.js Image 최적화
2. 불필요한 re-render 방지 (React.memo)
3. Bundle 크기 분석 및 최적화
4. Code Splitting

**완료 기준:**
- [ ] Lighthouse Performance 90+
- [ ] FCP < 1.8s
- [ ] LCP < 2.5s

---

### TASK-023: 접근성(A11y) 개선
**난이도**: 🟡 Medium
**예상 시간**: 4시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- WCAG 2.1 AA 준수

**상세 작업:**
1. 모든 이미지에 alt 속성
2. 키보드 네비게이션 지원
3. ARIA 라벨 추가
4. 색상 대비 4.5:1 이상

**완료 기준:**
- [ ] Lighthouse Accessibility 95+
- [ ] 스크린 리더 호환

---

### TASK-024: 배포 및 모니터링 설정
**난이도**: 🟡 Medium
**예상 시간**: 5시간
**의존성**: TASK-017
**병렬 가능**: Yes

**목표:**
- Vercel 배포 및 모니터링

**상세 작업:**
1. Vercel 프로젝트 연결
2. 환경 변수 설정 (API 키들)
3. Analytics 설정
4. Error Tracking (Sentry, Optional)
5. 커스텀 도메인 연결 (Optional)

**완료 기준:**
- [ ] Production 배포 성공
- [ ] 모든 API 정상 동작
- [ ] Analytics 데이터 수집 시작

---

## 📅 Timeline & Milestones

### Week 1: Foundation & API Integration
- **Day 1-2**: TASK-001~004 (Foundation)
- **Day 3-5**: TASK-005~008 (API Integration)

**Milestone 1**: 모든 외부 API 연동 완료, 데이터 조회 성공

### Week 2: Backend & Frontend
- **Day 6-7**: TASK-009~012 (API Routes)
- **Day 8-10**: TASK-013~016 (Frontend Cards)

**Milestone 2**: 4개 카드 개별 동작 확인

### Week 3: Integration & Polish
- **Day 11-12**: TASK-017 (Dashboard 통합)
- **Day 13-15**: TASK-018~020 (데이터 갱신, 에러 처리, 반응형)

**Milestone 3**: MVP 완성, 내부 테스트 가능

### Week 4: Quality & Launch
- **Day 16-18**: TASK-021~023 (테스트, 성능, 접근성)
- **Day 19**: TASK-024 (배포)
- **Day 20**: 버그 수정 및 최종 검토

**Milestone 4**: 정식 출시 🎉

---

## 🔄 Parallel Execution Strategy

### 최대 병렬화 가능 구간:

**Sprint 1 (Day 1-2)**: 4 tasks 동시
```
TASK-001 ─┐
TASK-002 ─┼─ 4명이 동시 작업 가능
TASK-003 ─┤
TASK-004 ─┘
```

**Sprint 2 (Day 3-5)**: 4 tasks 동시
```
TASK-005 (주가) ─┐
TASK-006 (환율) ─┼─ 4명이 독립적으로 작업
TASK-007 (금)   ─┤
TASK-008 (금리) ─┘
```

**Sprint 3 (Day 6-10)**: 8 tasks 동시
```
TASK-009~012 (Backend) ─┬─ 백엔드 개발자 4명
TASK-013~016 (Frontend) ─┴─ 프론트엔드 개발자 4명
```

**Sprint 4 (Day 16-19)**: 4 tasks 동시
```
TASK-021 (테스트)   ─┐
TASK-022 (성능)     ─┼─ QA, DevOps 팀 병렬 작업
TASK-023 (접근성)   ─┤
TASK-024 (배포)     ─┘
```

---

## ⚠️ Risk Management

### High Priority Risks

| 리스크 | 확률 | 영향 | 대응 방안 |
|--------|------|------|-----------|
| **외부 API 비용 초과** | 🟡 Medium | 🔴 High | 무료 티어 확인, Fallback API 준비 |
| **API Rate Limiting** | 🟡 Medium | 🟡 Medium | 캐싱 강화, 여러 API 분산 사용 |
| **주가 API 크롤링 차단** | 🟢 Low | 🔴 High | 공식 API 사용 (한국투자증권) |
| **실시간 데이터 지연** | 🟡 Medium | 🟡 Medium | "마지막 업데이트 시간" 명시 |

### Mitigation Strategies

1. **API 백업 플랜**:
   - 주가: 네이버금융 → 한국투자 API → KRX 공식
   - 환율: 수출입은행 → Exchange Rates API
   - 금: Alpha Vantage → Yahoo Finance → Metals-API

2. **Cost Control**:
   - API 호출 횟수 모니터링 (Vercel Analytics)
   - 캐싱 적극 활용 (1분~1일)
   - 무료 티어 한도 알림 설정

3. **Performance Degradation**:
   - CDN 캐싱 (Vercel Edge)
   - Static Generation 부분 적용
   - Fallback 데이터 (stale-while-revalidate)

---

## 📊 Success Criteria (Definition of Done)

### Each Task DoD:
- [ ] 코드 리뷰 완료
- [ ] TypeScript 에러 없음
- [ ] Build 성공
- [ ] 관련 테스트 통과
- [ ] 문서화 (README 또는 주석)

### Phase 1 MVP DoD:
- [ ] 4개 기능 모두 정상 동작
- [ ] 모바일/데스크톱 반응형 완성
- [ ] Lighthouse 점수: Performance 90+, Accessibility 95+
- [ ] 실제 API 데이터 연동 (Mock 아님)
- [ ] 1분 간격 자동 갱신 동작
- [ ] 에러 처리 완비
- [ ] Production 배포 완료
- [ ] 5명 이상 베타 테스터 피드백 수집

---

## 🛠️ Tech Stack Summary

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Framework** | Next.js 15.2.4 | SSR, API Routes |
| **Frontend** | React 19 | UI 컴포넌트 |
| **Styling** | Tailwind CSS | 유틸리티 CSS |
| **UI Library** | shadcn/ui | 재사용 컴포넌트 |
| **Data Fetching** | SWR | 실시간 갱신, 캐싱 |
| **Validation** | Zod | 런타임 타입 검증 |
| **Testing** | Jest + Testing Library | 단위/통합 테스트 |
| **Deployment** | Vercel | 호스팅, CI/CD |
| **Analytics** | Vercel Analytics | 사용자 추적 |

---

## 📞 Communication Plan

### Daily Standups:
- 매일 오전 10시
- 진행 상황, 블로커 공유
- 15분 이내

### Weekly Reviews:
- 매주 금요일 오후 4시
- Milestone 달성 확인
- 다음 주 계획 조정

### Issue Tracking:
- GitHub Issues 사용
- 태그: `bug`, `feature`, `api`, `ui`, `testing`
- 우선순위: `P0` (긴급) ~ `P3` (낮음)

---

## 📝 Appendix

### A. API Keys Checklist
- [ ] 한국수출입은행 환율 API 키
- [ ] 한국은행 ECOS API 키
- [ ] Alpha Vantage API 키
- [ ] 한국투자증권 API 키 (또는 대체)
- [ ] FRED API 키 (미국 금리)

### B. Environment Variables
```bash
# .env.local
NEXT_PUBLIC_APP_NAME="주식 투자 대시보드"

# API Keys
KOREAEXIM_API_KEY=your_key_here
ECOS_API_KEY=your_key_here
ALPHAVANTAGE_API_KEY=your_key_here
KIS_APP_KEY=your_key_here
KIS_APP_SECRET=your_secret_here
FRED_API_KEY=your_key_here

# Feature Flags
NEXT_PUBLIC_ENABLE_GOLD=true
NEXT_PUBLIC_ENABLE_US_RATES=true
```

### C. Useful Resources
- 한국거래소 KRX: https://www.krx.co.kr
- 한국은행 ECOS: https://ecos.bok.or.kr
- 한국수출입은행 API: https://www.koreaexim.go.kr/site/program/financial/exchangeJSON
- Alpha Vantage: https://www.alphavantage.co/documentation
- FRED API: https://fred.stlouisfed.org/docs/api/

---

## ✅ Sign-off

| Role | Name | Approval | Date |
|------|------|----------|------|
| Project Manager | - | ☐ | - |
| Tech Lead | - | ☐ | - |
| Product Owner | - | ☐ | - |

---

**Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-10-31 | Design Team | Initial execution plan |

---

**Next Steps:**
1. Kick-off meeting 일정 잡기
2. API 키 발급 시작
3. GitHub Project Board 설정
4. TASK-001 착수

**목표 완료일**: 2025-11-28 (4주 후)
