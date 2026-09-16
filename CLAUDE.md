# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

modusoft.kr — 모두소프트 앱 소개 허브. **순수 정적 사이트**다: 빌드·패키지 매니저·린트·테스트가 없다.
저장소 루트가 그대로 서빙 루트다.

- 로컬 미리보기: `python3 -m http.server 8000` (루트 절대경로 `/assets/...` 를 쓰므로 `file://` 로 열면 깨진다)
- 배포: `main` 푸시 → Cloudflare Pages 자동 배포 (preset `None`, build `exit 0`, output `/`)
- `_headers` 는 Cloudflare Pages 헤더 규칙이다. `/assets/*` 는 1년 `immutable` 캐시이므로
  **자산 파일을 같은 이름으로 덮어쓰면 방문자에게 반영되지 않는다** — 내용을 바꾸면 파일 이름을 바꾼다.
  HTML 은 캐시하지 않는다.

## 구조와 규칙

- 앱별 페이지는 서브도메인이 아니라 **경로**다 (`/jeomjeom/`, `/jeomjeom/privacy/`, `/jeomjeom/terms/`,
  `/jeomjeom/support/`). 스토어에 등록된 URL 이므로 경로를 바꾸거나 지우지 않는다.
  각 페이지는 `<dir>/index.html` 한 장이다.
- **공유 CSS·템플릿이 없다.** 모든 페이지가 `<style>` 을 인라인으로 갖고, 헤더·푸터·디자인 토큰(`:root`)을
  복사해 쓴다. 헤더 메뉴, 푸터 사업자 정보(대표·사업자등록번호·메일), 토큰 값을 바꿀 때는
  모든 `index.html` 을 함께 고친다 (`grep -r` 로 찾기).
- 점점 페이지들의 상단 메뉴와 링크 순서는 **점점 · 이용약관 · 개인정보처리방침 · 지원** 으로 통일돼 있다.
- 디자인: 허브(`index.html`)는 무채색(ink/stone) 판이고 브랜드색은 앱 카드(`--accent`)만 갖는다.
  문서 페이지는 앱 브랜드색을 링크 밑줄에만 쓴다.
- 카드 배지: 「출시」(`.badge.on`) / 「심사·준비 중」(`.soon`) / 「개발 중」(`.wip`) 을 구분한다.
  스토어 배지·링크는 **심사 승인 후에만** 건다 (404 링크 금지).

## 페이지를 추가·수정할 때 함께 챙길 것

- `<head>`: `canonical`, `og:url`, `og:title`, `og:description`, `og:image`(1200×630, **절대 URL**) 를 페이지마다 맞춘다.
  `og:image` 가 없으면 카카오톡이 첫 큰 이미지(뭐무 아이콘)를 집는다. 이미지를 바꾸면
  카카오 공유 디버거(developers.kakao.com/tool/debugger/sharing)에서 캐시를 지워야 한다.
- 새 경로는 `sitemap.xml` 에 추가하고, 바꾼 페이지의 `lastmod` 를 갱신한다.
- 방침·약관·지원 원고의 정본은 앱 저장소 `jeomjeom/docs/WEB.md`·`docs/STORE_LISTING.md` 다.
  방침·약관 문장은 앱 안 화면(「개인정보 안내」, 설정 › 이용약관)과 같은 사실을 말해야 하므로
  웹만 고치지 않는다 — 앱 쪽도 고쳐야 하는지 사용자에게 알린다.

## 🔴 DNS (Cloudflare) — 건드리지 않는다

메일이 카페24 에 남아 있다. Pages 커스텀 도메인 작업 시 교체 대상은 apex A 와 `www` CNAME **둘뿐**이다.
`webmail` A(183.111.242.70, DNS only 고정), MX `spam.cafe24.com`, SPF TXT, `dev`/`web` A, `*` CNAME 은
지우거나 프록시로 바꾸지 않는다. 상세 표는 README.md 참고.
