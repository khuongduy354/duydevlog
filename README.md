# Duy's Devlog

A personal blog built with [Hugo](https://gohugo.io/) using the [Hugo Winston Theme](https://github.com/zerostaticthemes/hugo-winston-theme).

## 🚀 Quick Start

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.55.0 or higher)
- Git

### Running the Development Server

```bash
hugo server -D
```

This will start a local development server at `http://localhost:1313/`. The `-D` flag includes draft posts in the preview.

For development without drafts:
```bash
hugo server
```

The server supports live reload - any changes to content, layouts, or configuration will automatically refresh the browser.

## ✍️ Writing Content

### Creating a New Post

Use Hugo's archetype system to create a new blog post:

```bash
hugo new posts/your-post-title.md
```

This creates a new file in `content/posts/` with the following frontmatter template:

```yaml
---
title: "Your Post Title"
date: 2026-01-05T12:00:00+07:00
description: 'A brief description of your post'
image: images/featured.jpeg
draft: true
---
```

### Post Frontmatter Fields

- **title**: The post title (required)
- **date**: Publication date (auto-generated)
- **description**: Short description for SEO and previews
- **image**: Featured image path (relative to `static/`)
- **draft**: Set to `false` when ready to publish

### Writing Tips

1. **Set draft to false** when your post is ready to publish
2. **Add images** to the `static/images/` directory
3. **Use markdown** for content formatting
4. **Add tags** if needed for categorization
5. Posts are located in `content/posts/`
6. Pages (like About) are in `content/pages/`

### Creating a Series

You can organize posts into a series by creating a subdirectory:

```bash
mkdir content/posts/series-name
hugo new posts/series-name/part-1.md
```

See `content/posts/series-autochess/` for an example.

### Content Organization

```
content/
├── _index.md           # Homepage content
├── posts/
│   ├── _index.md       # Blog listing page
│   └── your-post.md    # Individual posts
└── pages/
    └── about.md        # Static pages
```

## 🏗️ Building for Production

To build the static site:

```bash
hugo --gc --minify
```

This generates the static site in the `public/` directory with:
- `--gc`: Garbage collection of unused cache files
- `--minify`: Minification of HTML, CSS, and JS

## 📦 Deployment

### GitHub Pages (Automatic)

This blog is automatically deployed to **GitHub Pages** via GitHub Actions.

**Published URL**: https://khuongduy354.github.io/duydevlog/

**How it works**:
1. Push changes to the `master` branch
2. GitHub Actions workflow (`.github/workflows/hugo.yaml`) automatically:
   - Builds the Hugo site
   - Deploys to GitHub Pages
3. Your site is live within a few minutes

**Manual Deployment Trigger**:
You can also trigger deployment manually from the GitHub Actions tab.

### Configuration

The deployment is configured in:
- `.github/workflows/hugo.yaml` - GitHub Actions workflow
- `config.toml` - Hugo site configuration

## 📝 Configuration

Main configuration file: `config.toml`

Key settings:
```toml
baseURL = "/"
title = "Duy's Devlog"
theme = "hugo-winston-theme"
paginate = 5

[params]
  showAuthorOnHomepage = true
  showAuthorOnPosts = true
  limitPostsOnHomepage = 3
  highlightColor = '#7b16ff'
```

### Customizing Author & Social Links

Edit these files:
- `data/author.json` - Author information
- `data/social.json` - Social media links

## 🎨 Theme

This blog uses the [Hugo Winston Theme](https://github.com/zerostaticthemes/hugo-winston-theme), located in `themes/hugo-winston-theme/`.

To customize styling, you can override theme files or modify the SCSS variables in the theme directory.

## 📚 Useful Commands

```bash
# Start dev server with drafts
hugo server -D

# Start dev server without drafts
hugo server

# Create new post
hugo new posts/my-post.md

# Build for production
hugo --gc --minify

# Check Hugo version
hugo version
```

## 📂 Project Structure

```
.
├── archetypes/          # Content templates
├── content/             # Your blog posts and pages
├── data/                # Data files (author, social)
├── static/              # Static files (images, etc.)
├── themes/              # Hugo Winston theme
├── config.toml          # Site configuration
└── .github/workflows/   # GitHub Actions for deployment
```

## 🔍 Tips

- Always preview your posts with `hugo server -D` before publishing
- Set `draft: false` in frontmatter when ready to publish
- Push to `master` branch to trigger automatic deployment
- Check GitHub Actions tab for deployment status
- Images should be placed in `static/images/`

## 📄 License

This blog's content is personal. The Hugo Winston Theme has its own license (see `themes/hugo-winston-theme/LICENSE`).
