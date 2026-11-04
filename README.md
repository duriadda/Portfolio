# 장현욱 포트폴리오

Hugo 테마 [sada](https://github.com/darshanbaral/sada)로 만든 1페이지 자기소개 사이트입니다.
`main` 브랜치에 push하면 GitHub Actions가 빌드하여 GitHub Pages에 자동 배포합니다.

- 사이트: https://duriadda.github.io/Portfolio/
- 저장소: https://github.com/duriadda/Portfolio
- 배포 실행 기록: https://github.com/duriadda/Portfolio/actions

## 폴더 구조

```text
.
├── site/                       ← 실제 빌드 대상 (수정은 여기서)
│   ├── config.toml             사이트 기본 정보·메뉴·GitHub 링크·아바타
│   ├── pages.json              배포용 설정 (baseURL 등, 스크립트가 생성)
│   ├── data/                   섹션별 내용
│   │   ├── interests.toml      관심 분야
│   │   ├── goals.toml          학습 목표
│   │   ├── activities.toml     대표 활동·동아리·기타 활동
│   │   ├── skills.toml         역량과 적용 사례
│   │   └── school_links.toml   학교 시스템 바로가기 카드
│   ├── layouts/                HTML 템플릿 (index.html, 404.html, partials/)
│   │   └── partials/           섹션별 템플릿과 스크립트
│   │       ├── nav.html            상단 메뉴
│   │       ├── anchor-nav.html     메뉴 클릭 시 부드러운 이동·뒤로 가기 처리
│   │       ├── nav-highlight.html  현재 보고 있는 섹션 메뉴 강조
│   │       ├── reload-top.html     새로고침하면 맨 위 처음 화면
│   │       ├── theme-init.html     첫 화면 테마 결정 (저장값 > 운영체제 설정)
│   │       └── theme-toggle.html   테마 전환 버튼 동작·선택 저장
│   ├── assets/main.css         색상 팔레트·기본 스타일·초점 테두리 (Tailwind CSS v4)
│   └── static/                 이미지·favicon (그대로 복사됨)
├── .github/workflows/hugo-pages.yml   배포 워크플로
├── .github/pages_build.py      Actions에서 쓰는 빌드 스크립트
├── prepare_github_pages.py     배포 준비 스크립트 (수업 제공 원본)
└── (그 밖의 루트 파일)          sada 테마 원본 ZIP 그대로 — 빌드에 쓰이지 않음
```

## 사용 도구와 버전

| 도구 | 버전 | 비고 |
|---|---|---|
| Hugo extended | 0.167.0 (Actions) / 0.168.0 (로컬) | Actions 버전은 `.github/workflows/hugo-pages.yml`에 고정 |
| Node.js / npm | 26.5.0 / 11.17.0 (로컬), 24 (Actions) | Tailwind CSS 실행용 |
| Tailwind CSS | 4.1.10 (`site/package-lock.json`) | |
| Python | 3.13.2 (로컬), 3.12 (Actions) | 배포 준비·빌드 스크립트 |
| Git | 2.54.0 | |
| OS | macOS 27.0.1 (로컬), Ubuntu (Actions) | |

## 로컬에서 실행하기

macOS 기준입니다. Homebrew로 Hugo와 Node.js를 설치합니다.

```bash
brew install hugo node
git clone https://github.com/duriadda/Portfolio.git
cd Portfolio/site
npm ci                                   # Tailwind CSS 설치
hugo server --config config.toml,pages.json
```

브라우저에서 http://localhost:1313/Portfolio/ 을 엽니다.
파일을 저장하면 화면이 자동으로 새로고침됩니다. 종료는 `Ctrl+C`입니다.

> **경로에 한글이 있는 macOS 폴더에서 실행할 때**
> `Can't resolve './tailwind.config.js'` 오류가 나면 아래처럼 실행합니다.
>
> ```bash
> env -u PWD hugo server --config config.toml,pages.json
> ```
>
> macOS 디스크의 한글 폴더 이름(NFD)과 터미널이 넘기는 경로(NFC)의 표기가 달라 Tailwind가 설정 파일을 찾지 못하는 문제입니다.
> `PWD` 환경변수를 지우면 Hugo가 디스크의 실제 경로를 사용합니다. 영문 경로에서는 필요 없습니다.

`--config config.toml,pages.json`을 빼면 Tailwind 실행 허용 설정(`pages.json`의 `security.exec.allow`)이 빠져 빌드가 실패합니다.

## 내용 수정과 재배포

1. 고칠 파일을 찾습니다.

   | 바꾸려는 것 | 파일 |
   |---|---|
   | 이름·한 줄 소개·소개 문단 | `site/config.toml`의 `[params.profile]`, `[params.aboutme]` |
   | 상단 메뉴 | `site/config.toml`의 `[[menu.main]]` |
   | GitHub 등 링크 | `site/config.toml`의 `[[params.social.list]]` |
   | 프로필 이미지 | `site/static/images/avatar.webp` + `config.toml`의 `photo`, `photo_alt` |
   | 관심 분야 / 학습 목표 / 활동·동아리 / 역량 | `site/data/*.toml` (동아리는 `activities.toml`의 `[[clubs]]`) |
   | 학교 시스템 바로가기 카드 | `site/data/school_links.toml` (name, description, url, icon) |
   | 색상 | `site/assets/main.css`의 `@theme` |
   | 섹션 배치 | `site/layouts/index.html` |

2. 로컬에서 확인합니다 (위 "로컬에서 실행하기").
3. 커밋하고 push합니다.

   ```bash
   git add <바꾼 파일>
   git commit -m "변경 내용 설명"
   git push origin main
   ```

4. 저장소의 **Actions** 탭에서 `Deploy Hugo to GitHub Pages` 실행이 초록색 체크(build → deploy)로 끝나는지 확인합니다. 보통 1–2분 걸립니다.
5. https://duriadda.github.io/Portfolio/ 를 강력 새로고침(`Cmd+Shift+R`)해 변경이 반영됐는지 확인합니다.

배포 설정: 저장소 Settings → Pages → Build and deployment → Source = **GitHub Actions**

## 점검과 문제 해결 기록

배포 사이트는 Chrome headless를 DevTools 프로토콜로 조작해 네트워크 응답, Console 오류, 화면 폭별 넘침, Tab 순서를 자동으로 확인했고, 메뉴 이동·새로고침은 Safari에서 직접 확인했습니다.

| 증상 | 원인 | 조치 |
|---|---|---|
| Console에 `/favicon.ico` 404 | 아이콘 지정이 없어 브라우저가 도메인 루트로 요청 | `favicon.svg`·`favicon-32.png` 지정 (`partials/favicon.html`) |
| 로컬 `hugo server`에서 `Can't resolve './tailwind.config.js'` | 한글 경로의 NFD/NFC 표기 차이 | `env -u PWD`로 실행 (위 "로컬에서 실행하기") |
| Safari에서 메뉴를 누르면 목적지 내용이 하얀 상자로 표시 | Safari **확장 프로그램** (개인정보 보호 창에서는 정상) | 사이트 결함 아님. 확인은 확장을 끈 창에서 |
| 섹션으로 이동한 뒤 새로고침하면 그 위치에 머묾 | 브라우저가 본문을 읽으며 주소의 `#섹션`으로 먼저 스크롤 | 새로고침이면 `<head>` 단계에서 `#` 제거 (`partials/reload-top.html`) |

## 출처와 이용 조건

| 항목 | 출처 | 구분 | 이용 조건 |
|---|---|---|---|
| 테마 | [sada](https://github.com/darshanbaral/sada) by Darshan Baral | 외부 | MIT License (`LICENSE` 유지, 사이트 푸터에 표기) |
| 배포 준비·빌드 스크립트 | `prepare_github_pages.py` 3.1 | 수업 제공 | 과제용 |
| 아이콘 | [Bootstrap Icons](https://icons.getbootstrap.com/) 1.13.1 (jsDelivr CDN) | 외부 | MIT License |
| 웹폰트 | [Google Fonts](https://fonts.google.com/) Barlow, Brawler | 외부 | SIL Open Font License |
| 프로필 아바타 `avatar.webp` | [Chiikawa Wallpaper – 세이렌](https://chiikawawallpaper.com/ko/mobile-wallpaper/siren) (비공식 팬 사이트) | 외부 | 캐릭터 「치이카와」의 권리는 원작 권리자에게 있음. 비상업적 과제 사이트의 아바타로만 사용 |
| 학회지 링크 | [한국통신학회 「정보와통신」 Vol.43 No.5](https://www.kics.or.kr/html/?pmode=Bpublication&page=1&smode=view&seq=4850&searchValue=&searchTitle=strTitle) | 외부 링크 | 링크만 사용 |
| 학교 시스템 바로가기 주소 | [전북대학교 홈페이지](https://www.jbnu.ac.kr/web/index.do) 메뉴의 링크 (2026-11-04 접속 확인) | 외부 링크 | 링크만 사용 |
| favicon (`favicon.svg`, `favicon-32.png`) | 직접 제작 (SVG 작성 후 Chrome으로 32px PNG 렌더링) | 직접·AI | — |
| 사이트 문구(`site/data/`, `config.toml`) | 본인 작성 | 직접 | 일부 문장은 AI(Claude) 제안을 검토·수정해 반영 |
| 레이아웃 수정·추가 파일 (메뉴, 섹션 partial, 스크립트, 404, 색상) | 본인 + AI(Claude Code) 협업 | 직접·AI | 변경 내역은 커밋 이력 참고 |

테마 원본의 영문 README는 `site/README.md`에 그대로 남겨 두었습니다.
