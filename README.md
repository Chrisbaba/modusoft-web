# modusoft-web

modusoft.kr — 모두소프트 앱 소개 허브 (정적 사이트).

- 빌드 없음. `index.html` 과 `assets/` 를 그대로 서빙한다.
- 앱별 페이지는 **경로**다 (서브도메인·별도 도메인 아님 — 스토어에 적은 URL 이 앱이 사는 동안
  살아 있어야 하는데, 경로는 프로젝트·인증서·DNS 하나로 전부를 지킨다).

| 경로 | 내용 | 스토어 |
|---|---|---|
| `/jeomjeom/` | 점점 소개 | 마케팅 URL (선택) |
| `/jeomjeom/privacy/` | 점점 개인정보처리방침 | **Apple·Play 필수** |
| `/jeomjeom/support/` | 점점 지원·FAQ | Apple 필수 |

  원고의 정본은 앱 저장소 `jeomjeom/docs/WEB.md`·`docs/STORE_LISTING.md` 다. 방침 문장은
  앱 안 「개인정보 안내」 화면과 같은 사실을 말해야 한다(둘 중 하나만 고치지 않는다).
- 배포: Cloudflare Pages (`main` 브랜치 푸시 → 자동 배포)
  - Framework preset: `None`
  - Build command: `exit 0`
  - Build output directory: `/`

## 🔴 손대면 안 되는 것 (DNS)

modusoft.kr 의 DNS 는 Cloudflare 에 있고, 메일이 카페24 에 남아 있다.
Pages 커스텀 도메인을 붙일 때 아래 레코드는 **절대 지우거나 프록시(주황 구름)로 바꾸지 않는다.**

| 레코드 | 값 | 비고 |
|---|---|---|
| A `webmail` | 183.111.242.70 | **DNS only(회색) 영구 고정** — IMAP 993 은 프록시되지 않는다 |
| MX `@` | spam.cafe24.com (10) | 수신 |
| TXT `@` | `v=spf1 include:spf.cafe24.com ~all` | 발신 인증 |
| A `dev` / A `web` | 211.37.148.118 | |
| CNAME `*` | modusoft.kr | |

Pages 가 교체하는 것은 apex A(222.237.76.44)와 `www` CNAME **둘뿐**이다.
