# Building a Static Blog with Obsidian and GitHub Pages

*June 2026 · Category: Projects*

Here's how I set up a zero-dependency blog that publishes straight from my Obsidian vault.

## The Stack

- **Obsidian** for writing
- **Git plugin** for syncing
- **GitHub Pages** for hosting
- **Vanilla JS** for rendering

No build step. No npm. No framework.

## How It Works

Every `.md` file I write in my vault gets committed and pushed to a `posts/` folder in my GitHub repo. A small JavaScript function crawls the GitHub API to find all those files, reads their content, and renders them with `marked.js`.

The result is a fully static site that updates whenever I push from Obsidian.

## Lessons Learned

Keeping the vault separate from the site scaffolding was the right call. My Obsidian experience stays clean — no HTML noise, no config files polluting my notes.

The only convention I follow: put a `# Title` as the first line, and optionally a date on the second line with `*Month Year*`.
