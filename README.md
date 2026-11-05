# Hyuk_Profile

전북대학교 홍준혁(GitHub: [Hyuk-00](https://github.com/Hyuk-00))의 개인 소개 웹사이트입니다.
2026학년도 초급프로젝트 과제 1로 제작했으며, Hugo 정적 사이트 생성기와 Mana 테마를 사용해 GitHub Pages로 배포합니다.

- **사이트 주소**: https://hyuk-00.github.io/Profile-H/
- **저장소**: https://github.com/Hyuk-00/Profile-H
- **배포 상태**: [Actions 실행 기록](https://github.com/Hyuk-00/Profile-H/actions)

## 사이트 구성

| 메뉴 | 페이지 | 내용 |
|---|---|---|
| (홈) | `/Profile-H/` | 이름과 짧은 소개, GitHub 링크, 글 목록 |
| Study | `/Profile-H/posts/study/` | 관심 분야, 사용 기술과 경험, 학습 목표 |
| Key Activities | `/Profile-H/posts/key_activities/` | 대표 활동(포커 게임)의 목적, 기간·역할·진행 상태, 배운 점 |
| └ Process and Results of Execution | `/Profile-H/posts/process_and_results_of_execution/` | 대표 활동의 수행 과정과 결과물 |

## 필요한 도구와 버전

| 도구 | 버전 | 용도 |
|---|---|---|
| Hugo **Extended** | 0.167.0 | 사이트 빌드·로컬 미리보기 (필수) |
| Git | 최신 버전 | 저장소 받기, commit·push |
| Python | 3.9 이상 (Actions는 3.12 사용) | Actions와 같은 방식으로 빌드할 때만 필요 (선택) |

GitHub Actions 빌드 환경은 `.github/workflows/hugo-pages.yml`에 지정되어 있습니다(Ubuntu, Hugo Extended 0.167.0, Python 3.12, Dart Sass 1.105.0, Node 24, Go 1.27.1). 이 테마는 Sass·PostCSS를 사용하지 않으므로 로컬에서는 Hugo Extended만 있으면 됩니다.

### Hugo 설치

- **Windows**: `winget install Hugo.Hugo.Extended` 또는 [Hugo 릴리스 페이지](https://github.com/gohugoio/hugo/releases/tag/v0.167.0)에서 `hugo_extended_0.167.0_windows-amd64.zip`을 받아 압축을 풀고 PATH에 추가
- **macOS**: `brew install hugo`
- 설치 확인: `hugo version` 실행 시 `v0.167.0`과 `extended`가 표시되어야 합니다.

## 로컬에서 실행하기

모든 명령은 **저장소 최상위 폴더**에서 실행합니다.

```bash
# 1. 저장소 받기
git clone https://github.com/Hyuk-00/Profile-H.git
cd Profile-H

# 2. 로컬 미리보기 (수정하면 자동 새로고침)
hugo server --source site --config pages.json
```

브라우저에서 **http://localhost:1313/Profile-H/** 를 엽니다. 종료는 `Ctrl + C`입니다.

### 빌드만 하기

```bash
hugo --source site --config pages.json -d ../public --minify
```

결과물은 저장소 최상위의 `public/` 폴더에 생성됩니다. `-d ../public`을 빼면 `site/public/`에 생성되며, 두 폴더 모두 `.gitignore`로 제외되어 커밋되지 않습니다.

### Actions와 같은 방식으로 빌드하기 (선택)

```bash
python .github/pages_build.py --build
```

`.github-pages.json`의 `baseURL`을 읽어 빌드하고, GitHub Pages 하위 경로(`/Profile-H/`)에 맞게 주소를 보정합니다. Hugo가 PATH에 있어야 합니다.

## 폴더 구조

```text
Profile-H/
├─ .github/
│  ├─ workflows/hugo-pages.yml   # main push 시 빌드·배포하는 워크플로
│  └─ pages_build.py             # 워크플로에서 쓰는 빌드 스크립트
├─ site/                         # ★ 실제 빌드에 쓰이는 사이트 폴더
│  ├─ pages.json                 # 사이트 설정 (제목, 소개 문장, 메뉴, GitHub 링크)
│  ├─ content/posts/             # 글 (Markdown)
│  ├─ static/images/avatar.png   # 홈 아바타 이미지
│  ├─ assets/css/, assets/js/    # 테마 CSS·JS를 덮어쓰는 수정본
│  └─ themes/template/           # Mana 테마 (일부 템플릿 수정)
├─ .github-pages.json            # 배포 준비 스크립트 설정 (baseURL 등)
├─ prepare_github_pages.py       # 수업 제공 배포 준비 스크립트
├─ LICENSE                       # Mana 테마 라이선스 (MIT)
└─ layouts/, assets/, i18n/ …    # 원본 테마 ZIP 파일 (빌드에는 사용하지 않음)
```

> 사이트는 `site/` 폴더만으로 빌드됩니다. 최상위의 `layouts/`, `assets/` 등은 내려받은 테마 원본이므로, 사이트를 바꾸려면 반드시 `site/` 안의 파일을 수정해야 합니다.

## 수정할 파일 위치

| 바꾸고 싶은 것 | 파일 |
|---|---|
| 사이트 제목, 홈 소개 문장 | `site/pages.json`의 `title`, `params.description` |
| GitHub 링크 | `site/pages.json`의 `params.social.github` |
| 메뉴 항목·순서·하위 메뉴 | `site/pages.json`의 `menus.main` (`pageRef`, `weight`, `parent`) |
| 글 내용 | `site/content/posts/study.md`, `Key_Activities.md`, `Process_and_Results_of_Execution.md` |
| 홈 카드 미리보기 문장 | 각 글 맨 위 front matter의 `summary` |
| 홈 글 목록 순서 | 각 글 맨 위 front matter의 `weight` (작은 숫자가 위) |
| 아바타 이미지 | `site/static/images/avatar.png` |
| 아바타 대체 설명(alt) | `site/themes/template/layouts/_default/home.html` |
| 메뉴 표시 방식(PC에서도 ☰), 앵커 위치, 현재 메뉴 표시 | `site/assets/css/responsive.css` |
| 홈 소개 문장·GitHub 아이콘 배치, 아바타 움직임 | `site/themes/template/layouts/_default/home.html`, `site/assets/css/hero.css` |
| 글의 날짜 표시 (홈 카드 / 글 상세) | `site/themes/template/layouts/partials/post-meta-inline.html` / `post-meta.html` |
| 메뉴 열기·닫기 동작 | `site/assets/js/navigation.js` |

새 글을 추가하려면 `site/content/posts/`에 `.md` 파일을 만들고 맨 위에 아래 내용을 넣습니다.

```yaml
---
title: 글 제목
date: 2026-11-05
draft: false
weight: 3
summary: "홈 카드에 보일 한 줄 요약"
---
```

## 수정 후 재배포하기

1. 위 표의 파일을 수정하고 `hugo server --source site --config pages.json`으로 로컬에서 확인합니다.
2. 변경 내용을 main 브랜치에 커밋하고 push합니다.
   ```bash
   git add <수정한 파일>
   git commit -m "바꾼 내용을 설명하는 메시지"
   git push origin main
   ```
3. 저장소의 [Actions](https://github.com/Hyuk-00/Profile-H/actions) 탭에서 **Deploy Hugo to GitHub Pages** 실행이 자동으로 시작되었는지 확인하고, `build`와 `deploy`가 모두 초록색(성공)이 될 때까지 기다립니다. 보통 1분 안팎이 걸립니다.
4. https://hyuk-00.github.io/Profile-H/ 를 새로고침하여 변경이 반영되었는지 확인합니다. 예전 화면이 보이면 강력 새로고침(`Ctrl + Shift + R`)이나 시크릿 창으로 다시 엽니다.
5. 실행이 실패(빨간색)하면 해당 실행을 눌러 로그에서 오류 위치를 확인하고, 수정 후 다시 push합니다.

> GitHub 설정: Settings → Pages → Build and deployment → Source가 **GitHub Actions**로 되어 있어야 합니다.

## 출처와 이용 조건

| 자료 | 사용 위치 | 구분 | 출처 | 이용 조건 |
|---|---|---|---|---|
| Mana 테마 (Livour) | `site/themes/template/`, `site/assets/`, 최상위 원본 파일 | 외부 자료 (일부 직접 수정) | https://github.com/Livour/hugo-mana-theme · [Hugo Themes](https://themes.gohugo.io/themes/hugo-mana-theme/) | MIT License, 원문은 [`LICENSE`](LICENSE) |
| 배포 준비·빌드 스크립트 | `prepare_github_pages.py`, `.github/` | 외부 자료 (수업 제공) | 초급프로젝트 수업 자료 | 수업 과제 용도 |
| 사이트 글 (소개·활동) | `site/content/posts/`, `site/pages.json` | 직접 작성 | - | - |
| 아바타 이미지 | `site/static/images/avatar.png` | AI 생성 | Claude AI | 개인 과제용으로 생성 |
| 포커 게임 코드 발췌 | `Process_and_Results_of_Execution.md` | 직접 작성 | - | - |
| Google Fonts (Inter 등) | 테마 기본 글꼴 | 외부 자료 | https://fonts.google.com | SIL Open Font License |

### 테마에서 직접 수정한 부분

- 테마 전환 아이콘 제거, 다크 모드 고정, 배경·글자 색 변경
- 검색 버튼·검색 팝업·검색 스크립트와 검색엔진용 검색 안내(JSON-LD) 제거 (`header.html`, `mobile-menu.html`, `baseof.html`, `head/js.html`, `head/json-ld.html`)
- 홈 소개 문장 옆으로 GitHub 아이콘 배치, 소개 문장 단어 단위 줄바꿈 (`home.html`, `hero.css`)
- 홈 아바타의 자동 상하 움직임 제거, 아바타 대체 설명 추가 (`hero.css`, `home.html`)
- 하위 메뉴 출력, PC에서도 ☰ 메뉴 사용, 현재 메뉴 굵기·막대 표시 (`menu.html`, `responsive.css`)
- 앵커 이동 시 고정 헤더에 가리지 않도록 조정 (`responsive.css`)
- 메뉴를 열면 첫 메뉴 항목으로 초점 이동 (`navigation.js`)
- 글의 단어 수·읽기 시간 표시 제거 (`post-meta-inline.html`, `post-meta.html`)
