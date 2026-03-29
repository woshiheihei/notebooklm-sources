# article-hub

Public GitHub Pages site for publishing important articles and visual artifacts as clean static pages.

## What this is for

- Turn local `.md` files into public single-page HTML documents
- Publish hand-tuned local `.html` files directly as public static pages
- Turn local images into simple public viewer pages
- Produce stable public URLs that can be pasted into NotebookLM as web sources

## Local publishing

This repo is managed from the main OpenClaw workspace.

Use the workspace helper:

```bash
/Users/weibao/.openclaw/workspace/scripts/publish-github-pages <path-to-md-html-or-image>
```

Examples:

```bash
scripts/publish-github-pages translate/aakash-karpathy-prompt-loop-timeline-zh-CN/translation.md
scripts/publish-github-pages output/html/x-thread-dossier-2037200624450936940.html --related translate/x-thread-dossier-2037200624450936940/delivery.md --related output/ljg-downloads/x-thread-dossier-2037200624450936940.png
scripts/publish-github-pages output/cards/example.png --title "Example card"
```

The publisher will:

1. Convert Markdown into static HTML, or publish tuned HTML directly
2. Copy local referenced images into the public page folder
3. Archive original source files and related artifacts under `sources/<slug>/`
4. Update the site index
5. Commit and push changes to GitHub

## GitHub Pages

This repo is intended to publish from the `main` branch, `/docs` folder.
