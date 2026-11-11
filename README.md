# elalwany.de

A modern, minimalist portfolio website [elalwany.de](https://elalwany.de) built with Astro, TypeScript, and Tailwind CSS.

## Tech Stack

- **[Astro](https://astro.build)** - Static site generator
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Utility-first CSS framework
- **Jest** - Testing framework
- **ESLint** - Code linting and formatting

## Getting Started

### Prerequisites

- Node.js (v18+)
- npm

### Install dependencies

```sh
make install
```

### Run locally

```sh
make dev
```

The dev server starts at `http://localhost:4321`.

### Build for production

```sh
make build
```

### Preview the production build

```sh
make preview
```

### Lint

```sh
make lint
```

### Clean build artifacts

```sh
make clean
```

## Project Structure

```text
/
├── public/          # Static assets (images, fonts, etc.)
├── src/
│   ├── components/  # Reusable UI components
│   ├── layouts/     # Page layouts
│   ├── pages/       # Route pages
│   └── styles/      # Global styles and Tailwind config
├── Makefile         # Build and run commands
├── eslint.config.js # ESLint configuration
├── jest.config.ts   # Jest testing configuration
└── package.json
