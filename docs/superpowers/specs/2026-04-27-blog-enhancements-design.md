# Blog Enhancements Design

## Overview

Add comments, analytics, and visual updates to the klyhyeon.github.io Jekyll blog (Hydejack theme).

## Features

### 1. Giscus Comments (GitHub Discussions)

**What:** Add comment sections to all blog posts using giscus, which maps comments to GitHub Discussions on the repo.

**Implementation:**
- Create `_includes/comments.html` with the giscus `<script>` widget
- The giscus script loads a `<script>` tag that renders an iframe at the insertion point
- Hydejack's `post_addons` config already includes `comments` — it renders `_includes/comments.html` when a post has `comments: true`
- Add `comments: true` to the front matter defaults for all posts in `_config.yml`
- Configuration: `repo: klyhyeon/klyhyeon.github.io`, `data-mapping: pathname`, `data-theme: preferred_color_scheme`

**Prerequisites (manual):**
1. Enable GitHub Discussions on the `klyhyeon.github.io` repository (Settings > General > Features > Discussions)
2. Install the [giscus GitHub App](https://github.com/apps/giscus) on the repository
3. Create a "Comments" category in Discussions (Announcements type recommended)

### 2. GoatCounter Analytics

**What:** Add privacy-friendly visitor analytics with per-page view counts.

**Implementation:**
- Add GoatCounter script tag to `_includes/my-head.html`
- Single `<script>` tag: `<script data-goatcounter="https://SITECODE.goatcounter.com/count" async src="//gc.zgo.at/count.js"></script>`
- No cookies, GDPR-friendly, no consent banner needed

**Prerequisites (manual):**
1. Sign up at [goatcounter.com](https://www.goatcounter.com) (free for non-commercial)
2. Get site code (e.g., `klyhyeon`)

### 3. Sidebar Background Image

**What:** Replace the current water/rain sidebar background image.

**Implementation:**
- Replace `/assets/img/sidebar-bg.jpg` with the new image
- No config changes needed — `accent_image` already points to this path

**Prerequisites (manual):**
1. User provides a replacement image

### 4. Favicon

**What:** Replace the default Hydejack "hy" favicon with a custom one.

**Implementation:**
- Place custom `favicon.ico` at `assets/icons/favicon.ico` to override the theme default
- Optionally add `apple-touch-icon.png` at `assets/icons/icon-192x192.png`

**Prerequisites (manual):**
1. User provides a favicon image (recommended: 32x32 or 48x48 PNG, will be converted to .ico)

## Files to Create/Modify

| File | Action | Purpose |
|------|--------|---------|
| `_includes/comments.html` | Create | Giscus script widget |
| `_includes/my-head.html` | Modify | Add GoatCounter script |
| `_config.yml` | Modify | Add `comments: true` default for posts |
| `assets/img/sidebar-bg.jpg` | Replace | New sidebar background (when ready) |
| `assets/icons/favicon.ico` | Create | Custom favicon (when ready) |

## Out of Scope

- GoatCounter embedded page view counts on individual posts (can be added later)
- Custom giscus theme CSS
- Removing the old hits.seeyoufarm.com commented-out code (low priority cleanup)
