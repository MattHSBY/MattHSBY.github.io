# Your Blog — GitHub Pages Site

A static blog powered by GitHub Pages. Posts are raw `.md` files in the `posts/` folder — written in Obsidian, published via Git.

## Repo Structure

```
your-repo/
├── index.html        ← homepage (lists all posts as cards)
├── post.html         ← post renderer (handles any .md file)
├── style.css         ← shared styles
├── about.html        ← about page
├── posts/            ← ALL your blog posts live here
│   ├── hello-world.md
│   └── some-category/
│       └── a-nested-post.md
└── README.md
```

## Setup

### 1. Configure your GitHub username and repo name

In **both** `index.html` and `post.html`, find these two lines near the top of the `<script>` block and update them:

```js
const GITHUB_USER = 'YOUR_GITHUB_USERNAME';
const GITHUB_REPO = 'YOUR_REPO_NAME';
```

### 2. Enable GitHub Pages

In your repo → **Settings → Pages**, set the source to `Deploy from a branch` → `main` → `/ (root)`.

Your site will be live at `https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/`.

### 3. Personalise

- Replace `Your Name` in all HTML files with your name
- Edit `about.html` with your bio
- Optionally add a favicon: `<link rel="icon" href="favicon.ico">`

---

## Writing Posts (Obsidian Setup)

### Recommended Obsidian vault structure

Your Obsidian vault should point directly at the `posts/` folder. Install the **Obsidian Git** community plugin and configure it to use this repo (or a fork), with the vault root set to `posts/`.

Alternatively, keep your vault entirely separate and just drag/copy finished `.md` files into `posts/` before committing.

### Post format

Start every post with:

```markdown
# Your Post Title

*Month Year*

Your content starts here...
```

- The `# Title` line becomes the post title in the card and on the post page.
- The `*Month Year*` (an italics-only line immediately after the title) is parsed as the date and shown in the sidebar and card.  
  You can be more specific: `*12 June 2026*` or `*June 2026*` — both work.
- Everything else is rendered as normal Markdown.

### Organising posts into categories

Just use folders inside `posts/`:

```
posts/
├── projects/
│   └── my-project.md    ← shows as category "projects"
├── notes/
│   └── quick-note.md    ← shows as category "notes"
└── standalone-post.md   ← no category shown
```

The site recursively discovers all `.md` files at any folder depth.

---

## Customisation

| What | Where |
|------|-------|
| Colours, fonts, spacing | `style.css` — edit CSS variables at the top |
| Site name | `<a class="site-title">` in all HTML files |
| Nav links | `<nav class="site-nav">` in all HTML files |
| Hero text | `.hero` section in `index.html` |
| About page | `about.html` |

---

## How It Works

The site uses the **GitHub Contents API** to discover and read `.md` files at runtime — no build step, no static site generator. `marked.js` (loaded from a CDN) renders Markdown to HTML in the browser.

Because it reads directly from the GitHub API, changes appear on the site as soon as you push to `main`.

**Note**: GitHub's API has a rate limit of 60 requests/hour for unauthenticated requests. For a personal blog this is rarely an issue, but if you have many posts and many simultaneous visitors you may hit it. If this becomes a problem, consider adding a GitHub personal access token (read-only, public repos only) as a query parameter, or switching to a lightweight build step.
