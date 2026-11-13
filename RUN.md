# Running Locally

## Prerequisites

- Node.js (v18+)
- npm

## Install dependencies

```sh
make install
```

## Run the dev server

```sh
make dev
```

The site starts at `http://localhost:4321`.

## Build for production

```sh
make build
```

## Preview the production build

```sh
make preview
```

## Lint

```sh
make lint
```

## Clean build artifacts

```sh
make clean
```

## Deployment

Pushes to `master` automatically deploy to GitHub Pages via the workflow in `.github/workflows/deploy.yml`.

## Project Structure

```text
/
├── public/              # Static assets (images, favicon)
├── src/
│   ├── components/      # Astro components (Hero, About, Services, etc.)
│   ├── content/blog/    # Blog posts (Markdown with Content Collections)
│   ├── layouts/         # Page layouts
│   └── pages/           # Route pages
├── Makefile             # Build and run commands
├── astro.config.mjs     # Astro configuration
├── eslint.config.js     # ESLint configuration
├── jest.config.js       # Jest configuration
└── package.json
```
