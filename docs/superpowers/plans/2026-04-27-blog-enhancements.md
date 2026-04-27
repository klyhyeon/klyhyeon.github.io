# Blog Enhancements Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add giscus comments, GoatCounter analytics, and prepare image replacement hooks to the Hydejack Jekyll blog.

**Architecture:** Hydejack provides `_includes/my-head.html` and `_includes/my-body.html` as customization hooks. Comments use `_includes/comments.html` which is rendered by the theme when `comments: true` is set in front matter. GoatCounter goes in the head. Image replacements are file swaps.

**Tech Stack:** Jekyll, Hydejack theme, giscus, GoatCounter

---

### Task 1: Add GoatCounter Analytics

**Files:**
- Modify: `_includes/my-head.html` (currently empty, 1 line)

- [ ] **Step 1: Add GoatCounter script to my-head.html**

Add the GoatCounter tracking script. This file is included in the `<head>` of every page by the Hydejack theme.

```html
<script data-goatcounter="https://klyhyeon.goatcounter.com/count"
        async src="//gc.zgo.at/count.js"></script>
```

- [ ] **Step 2: Verify locally**

Run: `bundle exec jekyll serve`

Open http://localhost:4000 in browser. View page source and confirm the GoatCounter script tag appears in the `<head>` section.

- [ ] **Step 3: Commit**

```bash
git add _includes/my-head.html
git commit -m "feat: GoatCounter 애널리틱스 추가"
```

---

### Task 2: Add Giscus Comments

**Files:**
- Create: `_includes/comments.html`
- Modify: `_config.yml` (lines 144-148, uncomment comments default for posts)

- [ ] **Step 1: Create comments.html with giscus widget**

Create `_includes/comments.html` with the giscus script. This file is automatically rendered by Hydejack's `post_addons` when a post has `comments: true`.

```html
<div id="giscus-comments">
  <script src="https://giscus.app/client.js"
          data-repo="klyhyeon/klyhyeon.github.io"
          data-repo-id=""
          data-category="Comments"
          data-category-id=""
          data-mapping="pathname"
          data-strict="0"
          data-reactions-enabled="1"
          data-emit-metadata="0"
          data-input-position="top"
          data-theme="preferred_color_scheme"
          data-lang="ko"
          data-loading="lazy"
          crossorigin="anonymous"
          async>
  </script>
</div>
```

**NOTE:** The `data-repo-id` and `data-category-id` values must be filled in. To get these:
1. Go to https://giscus.app
2. Enter repo: `klyhyeon/klyhyeon.github.io`
3. Select category: "Comments"
4. Copy the generated `data-repo-id` and `data-category-id` values

- [ ] **Step 2: Enable comments for all posts in _config.yml**

Uncomment the comments default block in `_config.yml` (around lines 144-148). Change from:

```yaml
  # # You can use the following to enable comments on all posts.
  # - scope:
  #     type:            posts
  #   values:
  #     comments:        true
```

To:

```yaml
  # Enable comments on all posts.
  - scope:
      type: posts
    values:
      comments: true
```

- [ ] **Step 3: Verify locally**

Run: `bundle exec jekyll serve`

Open any blog post (e.g., http://localhost:4000/thinkings/2024-12-21-retrospect/). Scroll to the bottom. The giscus comment widget should appear below the post content (it will show a loading state or error if repo-id isn't set yet — that's expected).

- [ ] **Step 4: Commit**

```bash
git add _includes/comments.html _config.yml
git commit -m "feat: giscus 댓글 시스템 추가"
```

---

### Task 3: Replace Sidebar Background Image

**Files:**
- Replace: `assets/img/sidebar-bg.jpg`

- [ ] **Step 1: Replace the sidebar background image**

Copy the user's new image to `assets/img/sidebar-bg.jpg`, replacing the existing water/rain image. The `accent_image` config already points to this path, so no config changes needed.

```bash
cp /path/to/new-sidebar-image.jpg assets/img/sidebar-bg.jpg
```

**NOTE:** Waiting for user to provide the replacement image.

- [ ] **Step 2: Verify locally**

Run: `bundle exec jekyll serve`

Open http://localhost:4000 and check the sidebar shows the new background image.

- [ ] **Step 3: Commit**

```bash
git add assets/img/sidebar-bg.jpg
git commit -m "chore: 사이드바 배경 이미지 변경"
```

---

### Task 4: Replace Favicon

**Files:**
- Create: `assets/icons/favicon.ico` (overrides theme default)

- [ ] **Step 1: Add custom favicon**

Place the user's favicon at `assets/icons/favicon.ico`. This path overrides the Hydejack theme's default favicon.

```bash
mkdir -p assets/icons
cp /path/to/new-favicon.ico assets/icons/favicon.ico
```

If the user provides a PNG, convert it:
```bash
# If you have ImageMagick installed:
convert /path/to/favicon.png -resize 48x48 assets/icons/favicon.ico
```

**NOTE:** Waiting for user to provide the replacement image.

- [ ] **Step 2: Verify locally**

Run: `bundle exec jekyll serve`

Open http://localhost:4000 in a new incognito tab (favicons are cached). The browser tab should show the new icon instead of "hy".

- [ ] **Step 3: Commit**

```bash
git add assets/icons/favicon.ico
git commit -m "chore: 파비콘 변경"
```
