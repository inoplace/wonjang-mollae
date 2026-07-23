# 원장님몰래 — 자동 발행 시스템

미용인 정보지 '원장님몰래'(@wonjang.mollae)의 완전 자동 발행 저장소.
월·수·금 07:30 콘텐츠 자동 생성 → 10:00 인스타 자동 게시 (그 사이 이슈에 `stop` 댓글로 거부 가능).

## 대표님 1회 설정 (약 15분)

### 1. 저장소 만들기
1. github.com → New repository → 이름 `wonjang-mollae` → **Public** → Create
2. 이 패키지의 모든 파일을 업로드 (웹에서 "uploading an existing file" 드래그 가능. `.github` 폴더 포함 확인)

### 2. 셀프체크 툴 호스팅 (GitHub Pages)
Settings → Pages → Source: `main` 브랜치 / `/ (root)` → Save
→ 몇 분 뒤 `https://<아이디>.github.io/wonjang-mollae/` 에서 접속됨 → **이 주소를 인스타 프로필 링크에 등록**

### 3. 시크릿 등록 (Settings → Secrets and variables → Actions → New repository secret)
| 이름 | 값 | 발급처 |
|---|---|---|
| `ANTHROPIC_API_KEY` | Claude API 키 | console.anthropic.com → API Keys |
| `IG_USER_ID` | 인스타 비즈니스 계정 숫자 ID | Meta 개발자 앱 세팅 시 확인 (별도 안내) |
| `IG_ACCESS_TOKEN` | 장기 액세스 토큰 (60일) | Meta 그래프 API 탐색기 (별도 안내) |

♠️ IG 시크릿 2개는 페이지-인스타 연동이 풀린 뒤 발급 가능. 그 전까지는 생성만 자동으로 돌고 게시 단계에서 실패 알림이 남습니다.

### 4. Actions 켜기
Actions 탭 → 워크플로 활성화. `콘텐츠 생성` 워크플로에서 "Run workflow"로 수동 테스트 1회 권장.

## 운영 흐름
1. **월·수·금 07:30** — `generate.yml`: topics.json의 다음 주제로 카드 5장+캡션 생성, 저장소 커밋, **[발행 예정] 이슈** 생성 (카드 미리보기 포함, GitHub 앱 알림)
2. **~10:00 사이** — 마음에 안 들면 그 이슈에 `stop` 댓글 (거부권)
3. **10:00** — `publish.yml`: stop 없으면 Graph API로 캐러셀 자동 게시 → 이슈에 ✅ 완료 댓글
4. 주제 큐가 떨어지면 topics.json에 행 추가 (Claude에게 "주제 큐 12개 리필해줘" 요청)

## 유지보수
- IG 토큰은 60일 만료 — 만료 시 게시 실패 이슈 댓글로 알림됨 → 토큰 재발급 후 시크릿 갱신
- 톤·가드레일 수정: `guide.md` 편집
- 카드 디자인 수정: `make_cards.js` (색상표 SERIES 상수)
