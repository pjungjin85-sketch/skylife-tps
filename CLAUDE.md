# TPS 결합 요금 계산기 프로젝트 규칙

공통 규칙은 상위 디렉토리 `/Users/jaypark/workspace/CLAUDE.md` 참고.

## 이 페이지 정보
- **용도**: KT 스카이라이프 TPS(TV+인터넷+모바일) 결합 요금 계산기 (대리점 전용)
- **파일**: `index.html` (단일 파일)
- **GitHub Pages URL**: https://pjungjin85-sketch.github.io/skylife-tps/
- **비밀번호**: SHA-256 해시로 잠금 (`CORRECT_HASH` 상수)

## 기능 구조
- TV / 인터넷 / 모바일(1~2회선) 선택 → 결합 요금 자동 계산
- 결합 유형: 30% 홈결합, 기가 안심 홈결합, 일반 결합할인, IPTV 결합
- 유무선결합 TV 할인: 모바일 1회선 10%, 2회선 20%
- 모바일 요금제 데이터: `skylife-plans` 레포의 `data/plans.json`을 raw.githubusercontent.com에서 fetch (월별 키 구조)
- 결과 패널: 정가 합계 / 할인 합계 / 월 청구금액 합계 표시

## 월별 데이터 구조 (2026-07~ 리팩토링)
- TV/인터넷 카탈로그(`SAT_TV`, `IPTV_PLANS`, `NET_PLANS`, `FAMILY_NET_PLANS`, `IPTV_COMBO`, `NET_ADDONS`)는
  `TV_NET_CATALOG = {'YYYY-MM': {...}}` 형태로 월별 키에 저장됨. **기존 월 키를 덮어쓰지 말고 새 키를 추가**할 것.
- `initCatalog()`(옛 `loadMobPlans()`)가 페이지 로드 시:
  1. 모바일 `plans.json`을 fetch
  2. `TV_NET_CATALOG`의 키와 모바일 데이터의 키의 **교집합** 중, 오늘 날짜 이하인 가장 최근 월을 "활성 월"로 채택
  3. TV/인터넷/모바일 전체를 이 활성 월 데이터로 렌더링
- 즉 **TV·인터넷·모바일 중 하나라도 해당 월 데이터가 없으면, 셋 다 자동으로 이전 활성 월을 계속 표시**함
  (예: 8월 유선 정책을 반영해 `TV_NET_CATALOG`에 `'2026-08'`을 추가해도, `skylife-plans`가 아직 8월 요금제를 안 올렸다면
  사이트는 계속 7월 기준으로 노출되고, 모바일 쪽 8월 데이터가 올라오는 순간 자동으로 전체가 8월로 전환됨)
- **8월 유선 정책 파일을 반영할 때**: `TV_NET_CATALOG`에 `'2026-08'` 키를 새로 추가 (2026-07 키는 그대로 둠).
  `TV_NET_VERSION`은 더 이상 수동 상수가 아니라 `initCatalog()`에서 활성 월로 자동 대입됨 — 직접 값을 지정하지 말 것.

## 수정 시 주의사항
- 수수료·할인 수치는 실제 정책 기반 — 임의 변경 금지
- `calcPricing()` 반환값: tvPrice, tvOrig, netPrice, netOrig, origTotal, discTotal, total 등
- 모바일 요금제 캐시: network-first + localStorage fallback (`fetchPlansData()`), 24h TTL 캐시 아님
- 로컬 확인 시 로컬 서버 필요 (python3 -m http.server 8090) — file://로 열면 외부 fetch가 막힐 수 있음
- max-width: 1280px (다른 페이지 1200px와 다름)
