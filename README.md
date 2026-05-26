# RMAP Chatbot Slidev Presentation

## GitHub Pages

This project is published on GitHub Pages.

- Repository: `phiwi/chatbot`
- Expected URL: `https://phiwi.github.io/chatbot/`

## CI/CD (GitHub Actions)

Deployment is automated via GitHub Actions.

- Workflow file: `.github/workflows/deploy-pages.yml`
- Trigger: every push to `main` (and manual run via `workflow_dispatch`)
- Build command: `npm run build -- --base /chatbot/`
- Deploy target: GitHub Pages (`github-pages` environment)

After pushing changes to `main`, GitHub builds and publishes the site automatically.

## Run locally

```bash
npm install
npm run dev
```

## Build

```bash
npm run build
```

## Export PDF

```bash
npm run export
```
