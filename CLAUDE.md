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
- 모바일 요금제 데이터: `../skylife-plans/data/plans.json` (2026-06 키)
- 결과 패널: 정가 합계 / 할인 합계 / 월 청구금액 합계 표시

## 수정 시 주의사항
- 수수료·할인 수치는 실제 정책 기반 — 임의 변경 금지
- `calcPricing()` 반환값: tvPrice, tvOrig, netPrice, netOrig, origTotal, discTotal, total 등
- 모바일 요금제는 fetch로 외부 JSON 로드 → 로컬 서버 필요 (python3 -m http.server 8090)
- max-width: 1280px (다른 페이지 1200px와 다름)
