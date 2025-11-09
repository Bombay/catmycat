---
title: "비개발자도 가능한 Hugo 블로그 - Claude Code와 30분 만에 완성"
slug: "hugo-setup-with-claude-code"
date: 2025-11-09T22:16:14+09:00
draft: false
tags: ["Hugo", "Claude Code", "AI", "블로그", "PaperMod", "SEO"]
categories: ["웹개발"]
description: "Hugo 블로그 설정을 Claude Code와 함께 진행한 실제 경험. PaperMod 테마의 SEO 자동화, Front Matter CMS 이미지 붙여넣기, MCP 활용까지 - 비개발자도 30분이면 가능하다."
---

[지난 글](/2025/11/blog-start/)에서 Hugo를 선택한 이유를 적었다.
이번엔 실제로 어떻게 설정했는지 적어본다.

## 개발 환경 설정의 두려움

터미널, 명령어, Git.
비개발자에겐 이 단어들이 벽처럼 느껴진다.

나도 처음엔 막막했다.
Hugo 공식 문서를 읽어봐도 뭐가 뭔지 모르겠더라.

그런데 Claude Code와 함께하니 달랐다.
"Hugo 블로그 시작하고 싶어" 한마디면 알아서 진행됐다.

실제 걸린 시간:
- 설정: 30분
- 첫 배포: 1시간

## Hugo 설치와 첫 서버 실행

Claude Code에게 물어봤다.
"Hugo 블로그 어떻게 시작해?"

알아서 Homebrew로 설치하고,
새 프로젝트 만들고,
로컬 서버까지 띄워줬다.

```bash
hugo server -D
```

이 명령어를 외울 필요도 없었다.
필요할 때마다 물어보면 됐으니까.

{{< img "screenshot-local-blog.png" "Hugo 로컬 서버 실행 화면 - catmycat 블로그 메인페이지" >}}

localhost:1313에서 바로 확인 가능했다.
저장하면 브라우저가 자동으로 새로고침됐다.

## 테마 선택: 300개 중에서 고르기

[Hugo Themes](https://themes.gohugo.io)에 들어가봤다.
300개가 넘는 테마가 있더라.

{{< img "screenshot-hugo-themes.png" "Hugo Themes 갤러리 - 300개 이상의 테마 선택 가능" >}}

블로그, 포트폴리오, 문서 사이트.
용도별로 다양했다.

나는 PaperMod를 골랐다.
이유는 세 가지:

**빠른 속도**
PageSpeed 95점 이상.
광고 수익률에 직결된다.

**깔끔한 디자인**
다크모드 기본 지원.
힙스터 취향에 딱.

**SEO 자동화**
여기가 핵심이다.

{{< img "screenshot-papermod-demo.png" "PaperMod 테마 데모 사이트 - 깔끔한 디자인과 다크모드" >}}

### PaperMod가 자동으로 해주는 것들

WordPress 플러그인 찾아 헤맬 필요 없다.
PaperMod는 기본으로 다 해준다:

**Open Graph 메타태그**
- 카카오톡 공유 시 미리보기
- 페이스북 공유 시 썸네일
- 이미지 자동 추출 (설정 안 해도 본문 첫 이미지 사용)

**Twitter Cards**
- 트위터 공유 시 카드 표시
- 이미지 크기 자동 조정

**JSON-LD 구조화 데이터**
- 구글 검색 크롤링 최적화
- 검색 결과 썸네일 표시
- 작성일, 수정일 자동 포함

**Sitemap & Robots.txt**
- 검색엔진 크롤러 가이드
- 자동 생성 및 업데이트

이게 다 자동이다.
플러그인 하나 설치 안 했다.

### Claude Code가 SEO를 대신한다

진짜 장점은 여기서 나온다.

블로그 글 쓸 때마다 체크해야 할 게 많다:
- description 120-160자
- slug는 영문으로
- 이미지마다 alt 텍스트
- tags 3-5개
- 1200px 이상 이미지 리사이징

이걸 Claude Code가 다 챙겨준다.

커밋 전에 물어보면:
- "description 빠졌어요"
- "이미지 alt가 'image'로 돼있어요. 구체적으로 바꿀까요?"
- "이미지 크기가 2400px인데 리사이징할까요?"

**결국 글쓰기에만 집중하면 된다.**

## Front Matter CMS 설치 (필수!)

VS Code 확장이다.
글쓰기가 훨씬 편해진다.

**이미지 붙여넣기**
이게 가장 좋았다.

1. 스크린샷 찍기 (Cmd+Shift+4)
2. 마크다운 편집기에서 Cmd+V
3. 파일명 입력
4. 자동으로 저장되고 경로 입력됨

```markdown
![](screenshot.png)
```

이렇게 바로 들어간다.
일일이 경로 타이핑할 필요 없다.

**Front Matter 편집 UI**
title, description, tags를 GUI로 편집.
날짜/시간 picker 제공.

터미널 열 필요 없이 VS Code에서 다 된다.

### 실제 워크플로우

```
1. 이미지 캡처 (Cmd+Shift+4)
2. Cmd+V로 붙여넣기
3. Claude Code에게 "alt 텍스트 추가해줘"
4. 구체적인 설명 자동 생성
5. SEO 체크리스트 자동 점검
```

글 하나 쓰는데 5분.
WordPress였다면 30분 걸렸을 것이다.

## Page Bundle로 이미지 관리

Hugo의 콘텐츠 구조는 독특하다.

```
content/posts/my-post/
├── index.md
└── image.png
```

각 글이 폴더 단위다.
이미지를 같은 폴더에 저장한다.

**장점**
나중에 CDN 이전할 때 경로 변경이 쉽다.
글 하나가 하나의 패키지처럼 관리된다.

### 커스텀 shortcode로 자동 최적화

`{{</* img "image.jpg" "설명" */>}}`

이 한 줄이면:
- 1200px 이상 자동 리사이징
- WebP 변환 (용량 30-40% 감소)
- Lazy loading 자동 적용
- figcaption으로 이미지 설명 표시

첫 번째 글에 있던 2.4MB 이미지가 800KB로 줄었다.
페이지 로딩 속도가 눈에 띄게 빨라졌다.

## Cloudflare Pages 배포

GitHub 연결만 하면 끝이다.

1. Cloudflare Pages에서 New Project
2. GitHub 저장소 선택
3. Build command: `hugo --minify`
4. Publish directory: `public`

**실제 배포 시간**:
- 첫 배포: 1분 32초
- 이후 배포: 평균 45초

월 비용: **0원** (무료 플랜)

도메인 연결도 쉽다.
Cloudflare에서 DNS 설정 두 줄 추가하면 끝.

이제 `git push`만 하면 자동으로 배포된다.
Netlify, Vercel보다 빠르다는 느낌을 받았다.

## AI와 함께하는 글쓰기 환경

환경 구축 후의 진짜 장점이 여기 있다.

Hugo + Claude Code = 조사-작성-편집 자동화

**MCP로 확장 가능**

Playwright MCP:
- 경쟁 블로그 분석 스크린샷
- "상위 블로그 10개 레이아웃" → 5분 완성
- (이 글의 스크린샷도 Playwright로 자동 수집했다)

Filesystem MCP:
- 이미지 일괄 리사이징
- 50개 이미지를 한번에 WebP 변환

GitHub MCP:
- 코드 예제 실시간 가져오기
- 최신 버전 자동 반영

**실제 글쓰기 플로우**

```
1. Claude Code에게 "블로그 글 초안 작성해줘"
2. Front Matter CMS로 이미지 붙여넣기
3. Claude Code가 SEO 체크리스트 점검
4. git push → Cloudflare Pages 자동 배포
```

단순 글쓰기를 넘어 전체 자동화가 가능하다.

## 비개발자도 가능할까?

솔직히 말하면, **가능하다.**

**필요한 것**:
- 터미널에 대한 거부감만 없으면 됨
- 명령어는 Claude Code가 다 알려줌
- Git 커밋도 Claude Code가 대신

**시간**:
- 처음 설정: 1-2시간 (Claude Code와 대화하며)
- 글 하나 발행: 5분
- WordPress였다면 플러그인 찾느라 하루

**학습 곡선**:
- Day 1: Claude Code에게 모든 걸 물어봄
- Day 3: 기본 명령어 익숙해짐
- Day 7: 혼자서도 글 발행 가능
- Week 2: MCP 연결하며 확장 중

중요한 건 **AI에게 물어보는 걸 두려워하지 않는 것**이다.

## 1주일 사용 후기

PageSpeed 95점이 뭔지 알게 됐다.
광고 수익률에 직접 영향을 준다.

월 0원 비용이 뭔지 알게 됐다.
네이버/티스토리 제약 없이 자유롭다.

Claude Code 덕분에 기술 장벽이 완전히 제거됐다.
요즘 같이 [Cursor](https://cursor.com)를 쓰는 시대엔 더 쉬워질 것이다.

**다음 글 예고**
실제로 글 쓰며 사용하는 워크플로우와 수익화 전략을 적어볼까 한다.
