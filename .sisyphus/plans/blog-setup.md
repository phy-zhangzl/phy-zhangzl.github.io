# Blog Beautification and Automation Setup

## TL;DR

> **Quick Summary**: Configure Hugo PaperMod theme (Profile Mode, Dark Mode, Search) and set up automated GitHub Pages deployment via GitHub Actions.
> 
> **Deliverables**:
> - Configured `hugo.toml` (Profile Mode, Dark Mode, Search)
> - GitHub Actions Workflow (`.github/workflows/deploy.yml`)
> - Documentation (`WRITING.md`)
> - Initial Content (`hello-world.md`, `search.md`)
> 
> **Estimated Effort**: Short
> **Parallel Execution**: YES - 2 waves
> **Critical Path**: Config → Actions → Docs

---

## Context

### Original Request
User wants to "beautify" their Hugo blog (using PaperMod) and needs guidance on writing/publishing. Currently missing automation and content.

### Interview Summary
**Key Discussions**:
- **Theme**: Stick with PaperMod, optimize for "Profile Mode".
- **Identity**: "Prometheus's fire" by "zeal". Contact: phyzeal@linux.do.
- **Workflow**: Automated build via GitHub Actions (target: `gh-pages`).
- **Site Type**: Personal User Site (`phy-zhangzl.github.io`).
- **Features**: Dark Mode (Auto), Search enabled.

### Metis Review
**Identified Gaps** (addressed):
- **Base URL**: Confirmed as user site root.
- **Content**: `content/` is empty. Added task to create "Hello World" post.
- **Verification**: Defined executable `curl/grep` tests for profile and search.

---

## Work Objectives

### Core Objective
Transform a raw Hugo repo into a configured, deployable blog with a clear writing workflow.

### Concrete Deliverables
- `hugo.toml`: Complete configuration.
- `.github/workflows/deploy.yml`: Deployment script.
- `content/posts/hello-world.md`: Sample post.
- `content/search.md`: Search page.
- `WRITING.md`: Guide for the user.

### Definition of Done
- [ ] `hugo --gc --minify` builds successfully.
- [ ] Generated HTML contains profile info ("Prometheus's fire", "zeal").
- [ ] GitHub Actions workflow passes valid YAML check.

### Must Have
- Profile Mode with Avatar (placeholder) and Social Icons.
- Search functionality working (JSON output).
- Auto-deployment to `gh-pages` branch.

### Must NOT Have (Guardrails)
- Custom CSS/JS files (config only).
- Third-party analytics/comments (unless requested later).
- Changes to core theme files (`themes/PaperMod/*`).

---

## Verification Strategy

> **CRITICAL PRINCIPLE: ZERO USER INTERVENTION**
> All verification MUST be automated.

### Verification Tools
- **Config/Content**: `grep` on built `public/` files.
- **Workflow**: `yq` or basic file existence check.
- **Build**: `hugo` command exit code.

---

## Execution Strategy

### Parallel Execution Waves

```
Wave 1 (Start Immediately):
├── Task 1: Basic Content Setup (Hello World + Search)
└── Task 2: GitHub Actions Workflow

Wave 2 (After Wave 1):
├── Task 3: Hugo Configuration (PaperMod)
└── Task 4: Documentation (WRITING.md)

Critical Path: Task 1 → Task 3 → Verification
```

### Agent Dispatch Summary

| Wave | Tasks | Recommended Agents |
|------|-------|-------------------|
| 1 | 1, 2 | delegate_task(category="quick", load_skills=["git-master"], run_in_background=true) |
| 2 | 3, 4 | delegate_task(category="quick", load_skills=["git-master"], run_in_background=true) |

---

## TODOs

- [ ] 1. [Create Initial Content]

  **What to do**:
  - Create `content/posts/hello-world.md` (Standard frontmatter).
  - Create `content/search.md` (Layout: search, hidden: true).
  - Ensure `content/posts` directory exists.

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: [`git-master`] (Basic file creation)

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: Task 3 (Config needs content to build)

  **References**:
  - **PaperMod Search**: `https://github.com/adityatelange/hugo-PaperMod/wiki/Features#search-page` - Requires layout: search
  - **Hugo Content**: `content/` folder structure.

  **Acceptance Criteria**:
  - [ ] `test -f content/posts/hello-world.md`
  - [ ] `test -f content/search.md`
  - [ ] `grep "layout: search" content/search.md`

  **Commit**: YES
  - Message: `feat(content): add hello world post and search page`

- [ ] 2. [Create GitHub Actions Workflow]

  **What to do**:
  - Create `.github/workflows/deploy.yml`.
  - Use `peaceiris/actions-hugo` and `peaceiris/actions-gh-pages`.
  - Pin Hugo version to `extended`.
  - Target branch: `gh-pages`.

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: [`git-master`]

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: None

  **References**:
  - **Hugo Actions**: `https://github.com/peaceiris/actions-hugo` - Standard workflow

  **Acceptance Criteria**:
  - [ ] `test -f .github/workflows/deploy.yml`
  - [ ] `grep "peaceiris/actions-hugo" .github/workflows/deploy.yml`
  - [ ] `grep "publish_branch: gh-pages" .github/workflows/deploy.yml`

  **Commit**: YES
  - Message: `ci: add github actions workflow for gh-pages`

- [ ] 3. [Configure Hugo & PaperMod]

  **What to do**:
  - Modify `hugo.toml`:
    - Title: "Prometheus's fire"
    - baseURL: "https://phy-zhangzl.github.io"
    - params.defaultTheme: "auto"
    - params.profileMode: enabled (title: "zeal", subtitle: "This is zeal's blog...")
    - params.socialIcons: Email (phyzeal@linux.do)
    - outputs.home: ["HTML", "RSS", "JSON"] (for Search)

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: [`git-master`]

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2
  - **Blocks**: None
  - **Blocked By**: Task 1 (Needs content to verify build)

  **References**:
  - **Config**: `hugo.toml` (existing file)
  - **PaperMod Config**: `https://github.com/adityatelange/hugo-PaperMod/wiki/Installation#sample-configyml`

  **Acceptance Criteria**:
  - [ ] `hugo --gc --minify` exits with 0
  - [ ] `grep "outputs" hugo.toml | grep "JSON"` (Search enabled)
  - [ ] `grep "profileMode" hugo.toml`

  **Commit**: YES
  - Message: `chore(config): configure papermod profile and search`

- [ ] 4. [Create WRITING.md Guide]

  **What to do**:
  - Create `WRITING.md`.
  - Include sections:
    - How to create post (`hugo new ...`)
    - Frontmatter example.
    - Local preview (`hugo server`).
    - Publish (git push).

  **Recommended Agent Profile**:
  - **Category**: `writing`
  - **Skills**: [`git-master`]

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2
  - **Blocks**: None

  **Acceptance Criteria**:
  - [ ] `test -f WRITING.md`
  - [ ] `grep "hugo new" WRITING.md`

  **Commit**: YES
  - Message: `docs: add writing guide`

---

## Success Criteria

### Verification Commands
```bash
hugo --gc --minify && echo "Build Success"
grep "Prometheus's fire" public/index.html
```

### Final Checklist
- [ ] Build succeeds locally.
- [ ] Search JSON is generated.
- [ ] Profile info appears in HTML.
- [ ] Workflow file exists.
