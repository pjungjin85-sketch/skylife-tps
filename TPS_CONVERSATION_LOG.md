# TPS 결합 요금 계산기 — 대화 로그

## 2026-06-07

### 주요 개발 내역

#### 1. 정가/할인 합계 브레이크다운 추가
- 결과 패널 총액 위에 "정가 합계 / 할인 합계 / 월 청구금액 합계" 표시
- `calcPricing()`에 `origTotal`, `discTotal` 계산 추가
- 할인이 없을 때(단독 선택)는 브레이크다운 숨김

#### 2. GitHub 배포 (skylife-tps)
- 저장소: `pjungjin85-sketch/skylife-tps`
- GitHub Pages: `https://pjungjin85-sketch.github.io/skylife-tps/`
- 비밀번호 잠금 제거 → 누구나 접근 가능

#### 3. Vercel 배포 연결
- URL: `https://skylife-tps.vercel.app/`
- `vercel.json` 추가 (no-cache 헤더) → 수정 즉시 반영
- skylife-guide와 동일 방식 적용

#### 4. 모바일 요금제 fetch 경로 수정
- 기존: `../skylife-plans/data/plans.json` (로컬 전용)
- 변경: `https://raw.githubusercontent.com/pjungjin85-sketch/skylife-plans/main/data/plans.json`
- Vercel 환경에서 작동하지 않던 문제 해결

#### 5. 모바일 요금제 자동 동기화
- skylife-plans GitHub raw URL에서 직접 fetch
- plans.json 업데이트 시 TPS 별도 작업 없이 자동 반영

#### 6. 월별 키 선택 로직 skylife-plans와 통일
- `YYYY-MM-DD` 비교 → `YYYY-MM` 형식으로 변경
- 모바일 섹션 헤더에 "2026년 6월 기준" 라벨 추가

#### 7. 월별 업데이트 팝업
- `CHANGELOG` 객체로 월별 변경내용 관리 (index.html 내 직접 작성)
- 변경월 1일~6일(D+5)에만 표시
- localStorage로 중복 표시 방지
- 아무 곳 클릭 시 닫힘
- 테스트 모드: `?test-notice` URL 파라미터로 강제 표시

#### 8. 헤더 기준월 자동 표시
- `TV_NET_VERSION` 상수 (TV/인터넷/결합 정책 버전) 추가
- plans.json activeKey와 비교해 최신 월 자동 선택
- 헤더 제목: "2026년 6월 · 결합 요금 계산기" (월 15px, 구분자 ·)

#### 9. TV/인터넷 섹션 기준월 라벨
- TV, 인터넷, 모바일 3개 섹션 모두 기준월 표시
- TV/인터넷: `TV_NET_VERSION` 기준, 모바일: plans.json activeKey 기준

#### 10. skylife-guide 연동
- TPS 계산기 퀵링크를 요금제 리스트 왼쪽(첫 번째)에 추가
- TPS에 kt skylife SVG favicon 추가 (skylife-guide와 동일)

---

### 현재 구조 요약

| 항목 | 내용 |
|------|------|
| URL | https://skylife-tps.vercel.app/ |
| 저장소 | pjungjin85-sketch/skylife-tps |
| 모바일 데이터 | skylife-plans GitHub raw URL (자동 동기화) |
| TV/인터넷 데이터 | index.html 하드코딩 (`TV_NET_VERSION` 버전 관리) |
| 팝업 변경내용 | index.html `CHANGELOG` 객체 |

---

## 2026-06-07 — PDF 정책 반영 (2026년 6월 유선 영업정책)

### 변경 사항

#### 1. IPTV STB 임대료 수정
- 기존: 3,300원 → 수정: **3,000원** (PDF 12페이지 기준)

#### 2. IPTV 2nd TV 추가 기능
- IPTV 선택 시 "2nd TV 추가" 버튼 노출
- 수신료: Basic계열 5,500원 / Plus계열 6,000원 (VAT별도)
- STB 임대료: 1,500원 (프로모션 ~6/30, 이후 3,000원)
- 설치비: 10,000원 (1회성, VAT포함) → 결과 패널에 표시

#### 3. 설치비 1회성 섹션 추가 (결과 패널)
- 인터넷 설치비: 36,000원 (VAT포함)
- 위성TV 설치비: 27,500원 (sky 포인트·초이스는 포인트로 면제)
- IPTV 설치비: 27,500원
- 위성TV + 인터넷 동시설치: 48,500원 (할인 적용)
- IPTV 2nd TV 설치비: 10,000원

#### 4. VAT 안내 문구 수정
- 기존: TV/인터넷 VAT별도, 모바일 VAT포함
- 변경: TV/인터넷 VAT별도 + **설치비: VAT포함 (1회성)** 명시

- 커밋: `aeeb9a5`

---

---

## 2026-06-07 — 약정 표시 및 UX 개선

### 변경 사항

#### 1. 모든 요금 VAT 포함 기준 통일
- TV/인터넷 모든 금액 ×1.1 처리 (기존 VAT별도 → VAT포함)
- 설치비는 이미 VAT포함이었으므로 그대로 유지
- 총액 옆 `(VAT포함)` 표기 추가

#### 2. 위성TV 5년 약정 선택 기능
- 약정 선택 버튼 (3년 / 5년) 추가
- 5년 선택 시 −1,100원/월 할인 반영 (`y5discount` 필드)
- IPTV·인터넷은 3년 고정 → 위성TV 선택 해제 시 자동 3년 복귀

#### 3. 약정 선택을 각 상품 아코디언 내부로 이동
- 위성TV 아코디언 하단: 3년 / 5년 선택 버튼 포함
- IPTV 아코디언 하단: "📋 3년 약정 고정" 문구
- 인터넷 아코디언 하단: "📋 3년 약정 고정" 문구
- 기존 별도 `contract-row` div 제거

#### 4. 상품 버튼에 약정 뱃지 표시
- 위성TV 버튼: 현재 약정에 따라 `3년`(빨간) / `5년`(초록) 뱃지
- 5년 선택 시 버튼 가격도 즉시 갱신 (−1,100원 반영)
- IPTV / 인터넷 버튼: `3년` 뱃지 고정 표시

#### 5. step 칩 및 결과 패널에 약정 기간 표시
- 헤더 step 칩: "sky all(11) · 3년", "sky인터넷 (100M) · 3년", "ipit TV Basic (기본형) · 3년" 등
- 결과 패널 행 라벨: "📺 TV 수신료 — sky all(11) · 3년약정", "🌐 인터넷 — sky인터넷 (100M) · 3년약정"

#### 6. JS 버그 수정
- 삭제된 `contract-row` DOM 참조하던 `_showContractRow()` 제거
- `_resetContractIfNeeded(cat)` 으로 대체 → IPTV 선택 또는 TV 해제 시 5년→3년 자동 복귀
- `resetAll()`의 `contract-row` 참조 제거, `buildSatGrid()` 호출 추가

#### 7. 상품 재클릭 시 선택 해제 토글
- TV / 인터넷 / 모바일 모두 이미 선택된 상품을 다시 클릭하면 선택 해제
- 기존: 다른 상품 클릭 또는 "없음" 버튼만으로 해제 가능

- 커밋: `e7925b9` (약정 표시), `7ccbc7c` (토글 해제)

---

## 2026-06-08 — 인터넷 상품 확장 및 UX 개선

### 변경 사항

#### 1. 참고용 문구 변경
- "* 해당 금액은 참고용으로 정확한 월 청구금액은 각 상품과 결합 정책을 확인해주세요"
- → "계산된 금액은 참고용입니다. 정확한 금액은 상품과 결합 정책을 확인해주세요."

#### 2. Family 인터넷 상품 추가 (PDF 6페이지)
- `FAMILY_NET_PLANS` 배열 추가: sky인터넷 16,500 / 기가200 18,700 / 기가콤팩트 22,000 / 기가인터넷 27,500원 (VAT포함)
- 인터넷 step에 "🏠 Family 인터넷" 아코디언 추가
- Family + TV 결합 시 "⚠ Family 결합 - 요금 확인" 경고 표시 (결합 요금 미확인)

#### 3. 인터넷 설치비 휴일 요금 반영 (PDF 6페이지)
- MASS: 36,000원 / 휴일 45,000원 (`installFeeHoliday` 필드)
- Family: 20,000원 / 휴일 29,000원
- 결과 패널 설치비 섹션에 휴일 요금 병기

#### 4. 인터넷 부가서비스 선택 기능 (PDF 6페이지)
- `NET_ADDONS` 4종: sky키즈안심 4,400 / Sky추가단말서비스 5,500 / Sky케어안심 4,400 / 안심인터넷 1,100원 (VAT포함)
- 인터넷 선택 시 부가서비스 섹션 표시 (멀티 토글)
- 선택한 부가서비스 결과 패널 sub-row 표시 + 월 합계 반영

#### 5. 인터넷 상품별 AP 모델 표기 (PDF 7페이지)
- `apName` 필드 추가: 100M → sky WiFi (1,100원), 기가 상품 → sky GiGA WiFi (무료)
- Family 100M → sky WiFi (무료), Family 기가 → sky GiGA WiFi (무료)
- 상품 버튼에 `📶 sky GiGA WiFi · 무료` 형식으로 표시
- 결과 패널 AP 임대료 행에 모델명 반영

#### 6. IPTV 인터넷 결합 필수 안내 (PDF 12페이지)
- IPTV 선택 시 STEP 2 헤더에 `⚠ IPTV 필수` 황색 배지 표시
- IPTV 선택 + 인터넷 미선택 시 결과 패널에 경고 메시지
- IPTV 선택 시 인터넷 "없음" 버튼 숨김

#### 7. 결과 패널 약정 안내 문구 삭제
- "* 인터넷·IPTV 3년 약정 기준" 삭제 (각 행에 이미 약정 표기됨, 5년 선택 시 오해 소지)

- 커밋: `d9e6deb` ~ `43334d6`

---

## 2026-06-08 — 인터넷 상품 구조 개편 (MASS Direct / Family 분리)

### 변경 사항

#### 1. MASS 인터넷 Direct(단독) 가격 추가
- `NET_PLANS`에 `direct` 필드 추가 (VAT포함, PDF p.6 기준)
  · 100M: 23,100원 / 200M: 25,300원 / 500M: 30,800원 / 1G: 36,300원
- 인터넷 단독(TV 결합 없음) 시 `direct` 가격 적용 (`standalone` → `direct`)
- `standalone` (일반 금액)은 기존대로 TV결합 기준 요금으로 유지
- 인터넷 상품 버튼에 "단독 23,100원/월" + "TV결합 기준 28,050원" 병기

#### 2. Family 인터넷 구조 변경
- 기존: 독립 아코디언 ("🏠 Family 인터넷") → 선택 시 MASS와 동등한 인터넷으로 취급
- 변경: MASS 인터넷 선택 후에만 "🏠 Family 인터넷 추가" 버튼 표시
  · MASS 1회선 + Family 최대 2회선 = 총 3회선 구조
  · Family 1 / Family 2 각각 독립 그리드로 선택
  · 각 Family 회선 선택 해제 가능 (토글)
  · Family 열면 닫을 때 선택 자동 초기화
- 설치비 결과 패널에 Family 1/2 설치비 별도 표시
- 월 청구금액에 Family 요금 포함

- 커밋: `cf9d443`

---

## 2026-06-09 — 버그 점검 및 모바일 헤더 개선

### 변경 사항

#### 1. 위성TV 5년 약정 결합 요금 오계산 수정
- `y5discountCombo:550` 필드 추가 (결합 시 3년 대비 −550원/월, VAT포함)
- `y5discount:1100`은 단독 기준으로 유지
- `atv9` (알뜰TV): `y5discount:0` — 5년 약정 미제공
- `calcPricing()`에서 결합 여부에 따라 `y5discountCombo` / `y5discount` 분기 적용

#### 2. 30%/기가안심 홈결합 + 5년 약정 자동 해제
- 위성TV 선택 후 100M 인터넷(홈결합 대상) 선택 시 `selContract` 자동 '3yr' 복귀
- 홈결합은 3년 약정 고정 정책 반영

#### 3. 결합 notes "별도" 문구 오류 수정
- AP/STB 요금은 이미 월 합계에 포함 → "별도" 표현 전부 제거
- IPTV TV 결합 할인 notes: 2,000원 → 2,200원 (VAT포함 기준 수정)

#### 4. 5년 약정 버튼 비활성 로직 추가 (`_update5yrBtnState()`)
- IPTV 또는 알뜰TV 선택 시 5년 버튼 dim + 클릭 불가
- `resetAll()` 호출 시 버튼 상태도 초기화

#### 5. 2nd TV STB 임대료 동적 처리
- 프로모션 기간(~6/30): 1,650원 / 종료 후: 3,300원 — 날짜 기반 자동 전환
- D-day 카운터 종료 시 info-box 및 버튼 tip 금액도 동적 갱신
- HTML에 `id="tv2nd-stb-tip-amt"`, `id="tv2nd-stb-info-amt"` 추가

#### 6. `toggleLine2()` / `resetAll()` querySelector 버그 수정
- `document.querySelector('.mob-add-btn')` → DOM 첫 번째 `.mob-add-btn`(IPTV 버튼) 잘못 선택
- 모바일 2회선 토글 버튼에 `id="mob-line2-add-btn"` 추가 → `getElementById`로 변경

#### 7. 모바일 헤더 반응형 레이아웃 수정
- 375px 이하에서 헤더 텍스트 줄바꿈 → 총액이 아래로 밀리는 문제 해결
- `.logo-text`: `font-size:clamp(13px,4.2vw,18px)`
- `.header-title`: `clamp(9px,2.6vw,14px)` + `white-space:nowrap` + `flex-shrink:1` + `text-overflow:ellipsis`
- `.header-total`: `clamp(17px,5.2vw,28px)` 반응형 금액 폰트
- `@media(max-width:480px)`: 패딩/간격/뱃지 소형화

- 커밋: `dd7bb9d`

---

### 다음 달 업데이트 체크리스트

- [ ] `TV_NET_VERSION` 값 갱신 (TV/인터넷/결합 정책 변경 시)
- [ ] `CHANGELOG`에 해당 월 항목 추가
- [ ] SAT_TV, NET_PLANS, IPTV_COMBO 데이터 수정
- [ ] IPTV 2nd TV STB 프로모션 종료 시 1,500원 → 3,000원 변경 (7월 이후)
- [ ] `git push && npx vercel --prod` 배포

---

## 2026-06-09 — 모바일 요금제 선택 UX 개선

#### 1. 선택된 요금제 카테고리 자동 오픈
- `renderMobAcc()`: 선택된 요금제 속한 카테고리 자동 펼침 (TV/인터넷과 동일 UX)
- `hasSelected = plans.some(p => curSel && curSel.code === p.code)` 조건 추가

#### 2. 모바일 step 칩 요금제명 표시
- 기존: "1회선" / "2회선" → 변경: 실제 요금제명 (2회선: "요금제1 / 요금제2")

- 커밋: `a98ddc0`

---

## 2026-06-09 — PDF 누락 항목 리스트업 / 멀티룸 추가 후 삭제

### PDF 12~15페이지 기준 TPS 미반영 항목

| 항목 | 상태 |
|---|---|
| 멀티룸 (sky A 11/13, 성인팩, VIKI 프로모션) | ❌ 미반영 (추후 추가) |
| sky M 상품 (1/2/3년, M+T/M+M+T 구조) | ❌ 미반영 |
| UHD/HD 상품 (UHD sky all, UHD 알뜰TV, HD sky 베이직) | ❌ 미반영 |

### 멀티룸 추가 후 즉시 삭제
- 추가 커밋: `e356009` → 삭제 커밋: `51250e1`
- 재추가 시 참고: sky A 11 (7,700원) / sky A 13 (9,900원) / 성인팩 / 5년 −1,100원 / 설치비 10,000원

### Vercel 수동 배포 이슈 확인
- TPS는 GitHub push → Vercel 자동 배포 연결 안 됨
- 배포 시 `git push` + `npx vercel deploy --prod` 두 단계 필수

---

## 2026-06-11

### 작업 내용
- `<meta name="robots" content="noindex, nofollow">` 추가 (검색엔진 차단)
- fetch 에러 처리: `loadMobPlans()` named function + `!res.ok` 체크 + 다시 시도 버튼
- localStorage 캐시 추가: `skylife_plans_v1` 키, 24h TTL (GitHub raw URL fetch 횟수 감소)
- 브랜드 표기 통일: footer `kt SkyLife` → `kt skylife`
- footer 표기 통일: `Created by 박정진 | TPS 결합 요금 계산기`
- footer 스타일 통일 (FAQ 기준): `position:fixed;bottom:0;padding:10px;color:#666666;z-index:50`
- 모바일에서 `#mobile-bar` + footer 겹침 수정: `@media(max-width:640px){ footer{display:none;} }`

### 버그 수정 (코드 리뷰)
- 프로모션 D-day 계산 로직 3중복 제거: `PROMO_END` 상수 + `isPromoActive()`, `promoDday()`, `getPromoSTBFee()` 헬퍼 함수 추출
