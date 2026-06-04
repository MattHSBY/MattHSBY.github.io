# Blog Site — GitHub Pages

A static blog served from GitHub Pages. Posts live in another repository ([blog-vault](https://github.com/MattHSBY/blog-vault)) and are fetched at runtime via the GitHub API.

---

```
┌─────────────────────────────┐       ┌─────────────────────────────┐
│  VAULT REPO                 │       │  SITE REPO (this one)       │
│  e.g. username/blog-vault   │       │  e.g. username/username     │
│                             │       │  .github.io                 │
│  hello-world.md             │       │                             │
│  projects/                  │  ←──  │  index.html                 │
│    my-project.md            │  API  │  post.html                  │
│  notes/                     │       │  style.css                  │
│    quick-note.md            │       │  about.html                 │
│                             │       │                             │
│  ← Obsidian Git pushes here │       │  ← You clone/edit this      │
│  ← Never contains HTML/CSS  │       │  ← GitHub Pages serves this │
└─────────────────────────────┘       └─────────────────────────────┘
```

**Why two repos?**
- When writing/publishing: open Obsidian, write, push. Never touch the site repo.
- When editing the site: clone the site repo. Never wade through hundreds of .md files.
- The vault repo stays a clean Obsidian vault — no HTML noise, no config files.

---

## Setup

### Step 1 — Create the vault repo

Create a new **public** GitHub repo (e.g. `blog-vault`). It can be empty to start.

> It must be **public** so the GitHub Contents API can read it without authentication.

### Step 2 — Configure the site

In both `index.html` and `post.html`, find the config block near the top of the `<script>` and fill in your vault repo details:

```js
const VAULT_USER = 'your-github-username';
const VAULT_REPO = 'blog-vault';          // or whatever you named it
```

### Step 3 — Enable GitHub Pages on the site repo

Repo → **Settings → Pages** → Source: `Deploy from a branch` → `main` → `/(root)`.

Your site will be live at `https://YOUR_USERNAME.github.io/YOUR_SITE_REPO/`
(or `https://YOUR_USERNAME.github.io/` if the repo is `username.github.io`).

### Step 4 — Set up Obsidian + Git plugin

1. Create a new Obsidian vault wherever you like locally.
2. Install the **Git** community plugin (by Vinzent03) from the Obsidian plugin browser.
3. In the plugin settings, use **"Initialize a new repo"** or clone the vault repo you created in Step 1.
4. Set up authentication (HTTPS with a personal access token is simplest — see the [plugin docs](https://publish.obsidian.md/git-doc/Authentication)).
5. Push/pull from the command palette or via the plugin's auto-commit feature.

The vault repo root becomes whatever folder Obsidian is pointing at. Every `.md` file in that vault, at any depth, will appear on your site.

---

## Writing posts

Every `.md` file in the vault repo shows up as a post. The only convention:

```markdown
# Your Post Title

*June 2026*

Content starts here...
```

| Element | Purpose |
|---------|---------|
| `# Title` | First line — becomes the post title and card heading |
| `*Month Year*` | Italics-only line immediately after title — parsed as date |
| Everything else | Rendered as normal Markdown |

The date line is optional. Posts without one sort alphabetically after dated posts.

### Organising with folders

Just use folders in Obsidian as you normally would:

```
your-vault/
├── hello-world.md           → no category shown
├── projects/
│   └── my-project.md        → category badge: "projects"
└── notes/
    └── quick-note.md        → category badge: "notes"
```

The site crawls all folders recursively. Folder names become category badges on cards and post pages.

### Files to ignore

By default the site skips: `.obsidian/`, `.git/`, `README.md`. To skip additional files or folders (e.g. templates, attachments), add them to the `IGNORE` set in `index.html` and `post.html`:

```js
const IGNORE = new Set(['.obsidian', '.git', 'README.md', 'readme.md', 'templates', 'attachments']);
```

---

## Customising the site

| What | Where |
|------|-------|
| Colours, fonts, spacing | `style.css` — edit the CSS variables at the top |
| Site name | `<a class="site-title">` in all HTML files |
| Hero text | `.hero` section in `index.html` |
| Nav links | `<nav class="site-nav">` in all HTML files |
| About page | `about.html` |

---

## How it works

Both `index.html` and `post.html` call the **GitHub Contents API** at runtime to discover and read `.md` files from the vault repo. `marked.js` (CDN) renders Markdown to HTML in the browser.

No build step. Changes appear on the site as soon as you push from Obsidian.

**API rate limits**: GitHub allows 60 unauthenticated requests/hour per IP. For a personal blog this is rarely an issue. If you ever hit it, add a `?token=YOUR_READ_ONLY_PAT` parameter or proxy the requests through a small Cloudflare Worker.
