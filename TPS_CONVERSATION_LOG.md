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

### 다음 달 업데이트 체크리스트

- [ ] `TV_NET_VERSION` 값 갱신 (TV/인터넷/결합 정책 변경 시)
- [ ] `CHANGELOG`에 해당 월 항목 추가
- [ ] SAT_TV, NET_PLANS, IPTV_COMBO 데이터 수정
- [ ] IPTV 2nd TV STB 프로모션 종료 시 1,500원 → 3,000원 변경 (7월 이후)
- [ ] `git push && npx vercel --prod` 배포
