# Writing Guide

## 1. Create a New Post
Run this command in your terminal:
```bash
hugo new content/posts/my-new-post.md
```

## 2. Edit Frontmatter
Open the file and update the metadata:
```yaml
---
title: "My New Post"
date: 2024-01-31T12:00:00+08:00
draft: false
---
```

## 3. Preview Locally
Start the local server to see your changes:
```bash
hugo server -D
```
Open [http://localhost:1313](http://localhost:1313) in your browser.

## 4. Publish
When you are ready to publish:
```bash
git add .
git commit -m "feat: add new post"
git push origin main
```
GitHub Actions will automatically build and deploy your site.
