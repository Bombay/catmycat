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

## Contact & Analytics

- Contact: ojcat0419@gmail.com
- GitHub: https://github.com/Bombay
- GA4: G-QT5CR66L14
