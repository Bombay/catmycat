# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

catmycat is a Korean-language blog built with Hugo and the PaperMod theme, focusing on AI, side jobs, US stocks, and lifestyle information. It's a monetization-focused blog deployed on Cloudflare Pages with automatic deployment from GitHub.

**Live URL**: https://catmycat.com
**Tech Stack**: Hugo Extended v0.152.2, PaperMod theme (Git submodule), Cloudflare Pages, Giscus comments, Google Analytics 4

## Development Commands

### Local Development
```bash
# Start Hugo development server with live reload
hugo server -D

# Access at http://localhost:1313
# Live reload is automatic - file changes trigger browser refresh
```

### Creating Content
```bash
# Create new blog post (Page Bundle approach)
hugo new posts/my-post-title/index.md

# Post structure:
# content/posts/my-post-title/
#   ├── index.md (markdown content)
#   └── image.jpg (co-located images)
```

### Building
```bash
# Production build (minified)
hugo --minify

# Output: public/ directory
```

### Deployment Workflow
```bash
# IMPORTANT: 커밋 전 사용자에게 확인 받기
# 1. 로컬 서버에서 미리보기 (http://localhost:1313)
# 2. 사용자에게 "로컬에서 확인하셨나요? 커밋 진행할까요?" 물어보기
# 3. 사용자 확인 후 진행

# Standard git workflow triggers automatic Cloudflare Pages deployment
git add .
git commit -m "your message"
git push

# Cloudflare Pages automatically builds and deploys (1-2 minutes)
```

## Architecture

### Content Organization (Page Bundle)
- All posts use **Page Bundle** structure: each post is a folder with `index.md` + images
- Images placed in same folder are automatically processed by custom shortcodes
- This enables easy future CDN migration by updating shortcode files only

### Custom Shortcodes
Two custom shortcodes in `layouts/shortcodes/` for optimized media handling:

**`{{< img "image.jpg" "alt text" >}}`**
- Auto-resizes images >1200px wide
- Converts to WebP format (30-40% size reduction)
- Adds lazy loading
- Supports external URLs (http/https) without processing
- Shows helpful error if image not found in Page Bundle

**`{{< youtube "VIDEO_ID" >}}`**
- Privacy-enhanced embeds (youtube-nocookie.com)
- Responsive 16:9 aspect ratio
- Lazy loading

### Theme Customization
PaperMod theme is installed as Git submodule. **Never modify theme files directly**. Instead, override by creating files in project root:

- `layouts/partials/footer.html` - Custom footer with privacy policy link
- `layouts/partials/comments.html` - Giscus integration
- `layouts/shortcodes/*.html` - Custom shortcodes

### Third-Party Integrations

**Google Analytics 4**: Configured via `googleAnalytics = "G-QT5CR66L14"` in hugo.toml. Hugo/PaperMod auto-inject tracking script.

**Giscus Comments**: GitHub Discussions-based comments configured in `layouts/partials/comments.html`. Requires:
- GitHub account to comment
- Repository: Bombay/catmycat
- Discussion category: General
- Theme: auto-adapts to site dark/light mode

**Privacy Policy**: Required for Google AdSense. Located at `content/privacy/index.md` with footer link.

## Configuration (hugo.toml)

Key settings:
- `baseURL`: Must match Cloudflare Pages custom domain
- `googleAnalytics`: GA4 measurement ID
- `params.comments = true`: Enable Giscus
- `hasCJKLanguage = true`: Required for Korean content
- `[markup.goldmark.renderer] unsafe = true`: Allow HTML in markdown
- Theme uses `preferred_color_scheme` for auto dark/light switching

## Git Workflow Notes

- PaperMod theme is a **Git submodule** - don't forget `--recurse-submodules` when cloning fresh
- `task/` folder is gitignored (personal TODO tracking)
- `.frontmatter/` folder is gitignored (Front Matter CMS config)
- All commits trigger Cloudflare Pages auto-deployment

## Korean Language Specific

- `languageCode = "ko-kr"` and `defaultContentLanguage = "ko"`
- `hasCJKLanguage = true` enables proper Korean word counting and summary generation
- Giscus configured with `data-lang="ko"` for Korean UI
- Menu items and UI text are in Korean

## Git Commit Guidelines

**Commit Message Language**: 한글로 작성
- Good: "첫 블로그 포스트 작성"
- Bad: "Add first blog post"

**Commit Message Format**:
- 간결하게 작성 (50자 이내)
- 명령형보다는 완료형 사용 ("추가했다" 보다는 "추가")
- 예시: "블로그 시작 포스트 작성", "Hugo 설정 변경", "이미지 최적화"

## Blog Post Writing Guidelines

블로그 글 작성 시 다음 규칙을 따라주세요:

### 톤 & 스타일
- **묵독했을 때 어색하지 않게**: 글을 소리 내어 읽었을 때 자연스럽게 읽히는지 확인
- **간결하고 자연스러운 경험담 필체**: 진솔한 경험을 담되, 불필요한 장식 제거
- **짧은 문장 사용**: 긴 문장은 여러 개의 짧은 문장으로 분리
- **AI가 쓴 것처럼 보이지 않게**:
  - "세 가지 조건을 정했다. 첫째, 둘째, 셋째" 같은 형식적 표현 지양
  - 괄호 남발 지양, 자연스러운 문장으로 풀어씀
  - 과도한 강조 표현 지양

### E-E-A-T 반영
- **경험(Experience)**: 실제로 겪은 것을 구체적으로 서술 ("3일 사용", "1주일 사용")
- **전문성(Expertise)**: 구체적 수치 제시 (PageSpeed 점수, 비용, 속도 등)
- **권위(Authoritativeness)**: 각 도구가 어떤 경우에 적합한지 명확히 제시
- **신뢰성(Trustworthiness)**: 장단점을 균형있게 서술, 과장하지 않음

### 구조
- 명확한 섹션 구분
- 표는 비교가 필요한 경우에만 사용
- 실제 경험과 구체적 수치를 우선적으로 포함

---

## SEO Checklist (커밋 전 필수 점검)

블로그 글을 작성하고 커밋하기 전에 다음 항목들을 **반드시** 확인하세요:

### Front Matter 필수 항목

```yaml
---
title: "제목"                      # ✅ 명확하고 검색 친화적인 제목
slug: "english-slug"               # ✅ 영문 slug (한글 URL 방지)
date: 2025-11-09T11:41:17+09:00   # ✅ 자동 생성
draft: false                       # ✅ 배포 준비 완료 시 false
description: "120-160자 요약"     # ✅ 필수! 검색 결과 스니펫
tags: ["태그1", "태그2", "태그3"] # ✅ 3-5개 권장
categories: ["카테고리"]           # ✅ 1-2개 권장
---
```

### 커밋 전 체크리스트

#### 1. Front Matter 검증
- [ ] **description** 작성 (120-160자, 글의 핵심 요약)
- [ ] **tags** 추가 (3-5개, 구체적이고 검색 가능한 키워드)
- [ ] **categories** 설정 (1-2개, 대분류)
- [ ] **slug** 확인 (영문, 소문자, 하이픈 구분)
- [ ] **draft: false** 확인 (배포할 준비가 되었는지)

#### 2. 이미지 최적화
- [ ] 모든 이미지에 **구체적인 alt 텍스트** 작성
  ```markdown
  # ❌ 나쁜 예
  ![image](screenshot.png)
  ![alt text](photo.jpg)

  # ✅ 좋은 예
  ![Hugo 블로그 PaperMod 테마 메인 화면](screenshot.png)
  ![Google Analytics 4 실시간 트래픽 대시보드](ga4-dashboard.jpg)
  ```
- [ ] 이미지 크기 확인 (1200px 이상은 자동 리사이징됨)
- [ ] 커스텀 shortcode 사용 권장: `{{< img "image.jpg" "설명" >}}`

#### 3. 제목 구조 (Heading)
- [ ] H1은 제목으로 자동 생성 (직접 작성 금지)
- [ ] H2로 주요 섹션 구분 (`## 섹션 제목`)
- [ ] H3로 하위 섹션 구분 (`### 하위 제목`)
- [ ] Heading 순서 건너뛰지 않기 (H2 → H4 ❌)

#### 4. 내부 링크 (글이 여러 개일 때)
- [ ] 관련 글이 있다면 내부 링크 추가
  ```markdown
  [Hugo 셋업 가이드](/2025/11/hugo-setup/)를 참고하세요.
  ```

#### 5. 불필요한 항목 제거
- [ ] **keywords** 필드 삭제 (2025년 기준 완전히 무의미)
- [ ] 테스트용 Lorem Ipsum 텍스트 제거
- [ ] 임시 주석 제거

#### 6. 퇴고 및 문체 점검 (필수!)

글 작성 후 **반드시** 퇴고 시간을 가져야 합니다. 다음 항목들을 점검하세요:

**자연스러움 체크**
- [ ] **묵독했을 때 어색하지 않은지** 확인
  - 글을 처음부터 끝까지 소리 내지 않고 읽어보기
  - 막히거나 어색한 부분이 있으면 수정

**AI 느낌 제거**
- [ ] **AI가 작성한 것 같은 표현** 제거
  - "세 가지를 정리하면 다음과 같다. 첫째, 둘째, 셋째" ❌
  - "이유는 다음과 같다:" ❌
  - 과도한 번호 매기기, 리스트 구조화 ❌
  - 불필요한 **강조** 남발 ❌

- [ ] **자연스러운 경험담으로 풀어쓰기**
  - 리스트는 자연스러운 문장으로 변경 고려
  - "나는 ~했다", "~더라", "~니까" 같은 자연스러운 어미 사용
  - 괄호 남발 지양

**가독성 균형**
- [ ] **과하게 자연스럽게 만들려다 가독성을 해치지 않았는지** 확인
  - 너무 긴 문장은 짧게 분리
  - 중요한 정보는 명확하게 전달되는지 확인
  - 구조가 필요한 부분(비교표, 단계별 설명)은 유지

**이미지 alt 텍스트 점검**
- [ ] **모든 이미지에 alt 텍스트가 있는지** 확인
  - `![](image.png)` ❌ (빈 alt)
  - `![alt text](image.png)` ❌ (의미 없는 alt)
  - `![Hugo 로컬 서버 실행 화면](image.png)` ✅ (구체적인 설명)
- [ ] **alt 텍스트가 구체적이고 설명적인지** 확인
  - 이미지가 무엇을 보여주는지 명확하게 서술
  - SEO와 접근성 모두에 중요

**퇴고 프로세스**
```bash
# 1. 글 작성 완료 후
# 2. 전체 글을 묵독하며 어색한 부분 체크
# 3. AI 느낌 나는 표현 찾아서 자연스럽게 수정
# 4. 이미지 alt 텍스트 확인 (비어있거나 'alt text'면 수정)
# 5. 수정 후 다시 한번 묵독 (최소 2회 반복)
# 6. 가독성 확인 (정보 전달이 명확한지)
# 7. SEO 체크리스트 진행
```

**퇴고 예시**

❌ **수정 전 (AI 느낌)**
```markdown
블로그 플랫폼을 선택할 때 고려해야 할 요소는 다음과 같다:
1. **속도**: PageSpeed 점수
2. **비용**: 월 유지비
3. **자유도**: 커스터마이징 가능 여부
```

✅ **수정 후 (자연스러움)**
```markdown
블로그 플랫폼을 고를 때 속도를 먼저 봤다.
PageSpeed 점수가 높아야 광고 수익률도 올라가니까.
비용도 중요했다. 월 유지비가 부담되면 오래 못 간다.
커스터마이징도 자유롭게 할 수 있어야 했다.
```

### SEO 자동 점검 명령어

커밋 전에 다음을 실행하여 점검하세요:

```bash
# Front matter 확인
head -20 content/posts/my-post/index.md

# description 존재 여부 확인
grep "description:" content/posts/my-post/index.md

# alt 텍스트 없는 이미지 찾기
grep -n "!\[\]" content/posts/my-post/index.md
grep -n "!\[image\]" content/posts/my-post/index.md
grep -n "!\[alt text\]" content/posts/my-post/index.md
```

### 잘못된 예시 vs 올바른 예시

#### ❌ SEO에 나쁜 Front Matter
```yaml
---
title: "블로그 포스트"
date: 2025-11-09
draft: false
---
```
**문제점:**
- slug 없음 (한글 제목이 URL에 인코딩됨)
- description 없음 (검색 결과 스니펫 미흡)
- tags/categories 없음 (분류 안 됨)

#### ✅ SEO에 좋은 Front Matter
```yaml
---
title: "Hugo로 수익형 블로그 만들기 완전 가이드"
slug: "hugo-monetization-blog-guide"
date: 2025-11-09T15:30:00+09:00
draft: false
description: "Hugo와 PaperMod 테마로 수익형 블로그를 만드는 전체 과정. WordPress, Ghost 비교와 Cloudflare Pages 배포 방법까지 실전 경험을 바탕으로 설명합니다."
tags: ["Hugo", "블로그", "수익형블로그", "Cloudflare Pages", "PaperMod"]
categories: ["웹개발"]
---
```

### 커밋 전 최종 확인

```bash
# 1. 로컬에서 확인
hugo server -D
# → http://localhost:1313 에서 확인

# 2. SEO 필수 항목 점검
# - description 있는지
# - tags 있는지
# - 이미지 alt 텍스트 있는지

# 3. ⚠️ IMPORTANT: 사용자 확인 받기
# Claude Code는 반드시 사용자에게 물어보기:
# "로컬 서버에서 확인하셨나요? 커밋을 진행할까요?"
# 사용자가 "예" 또는 확인을 주면 다음 단계 진행

# 4. 사용자 확인 후 커밋
git add content/posts/my-post/
git commit -m "글 제목"
git push
```

### 주요 원칙

1. **description은 필수** - 없으면 SEO 효과 50% 감소
2. **slug는 영문** - 한글 URL은 검색엔진에 불리
3. **이미지 alt는 구체적으로** - "image", "alt text" 금지
4. **tags는 3-5개** - 너무 많으면 역효과
5. **keywords 필드는 절대 사용하지 말 것** - 2009년부터 무의미

---

## Contact & Analytics

- Contact: ojcat0419@gmail.com
- GitHub: https://github.com/Bombay
- GA4: G-QT5CR66L14

## Claude Code 작업 규칙

### 커밋 전 필수 사항
1. **사용자 확인 받기**: 커밋/푸시 전에 반드시 사용자에게 "로컬에서 확인하셨나요? 커밋을 진행할까요?" 물어보기
2. **커밋 메시지**: 블로그 글을 작성했다면, 글 제목을 커밋 메시지로 사용
3. **한글 사용**: 커밋 메시지는 항상 한글로 작성