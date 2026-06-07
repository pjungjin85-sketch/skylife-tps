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

### 다음 달 업데이트 체크리스트

- [ ] `TV_NET_VERSION` 값 갱신 (TV/인터넷/결합 정책 변경 시)
- [ ] `CHANGELOG`에 해당 월 항목 추가
- [ ] SAT_TV, NET_PLANS, IPTV_COMBO 데이터 수정
- [ ] `git push && vercel --prod` 배포
