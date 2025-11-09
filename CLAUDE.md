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

## Contact & Analytics

- Contact: ojcat0419@gmail.com
- GitHub: https://github.com/Bombay
- GA4: G-QT5CR66L14
