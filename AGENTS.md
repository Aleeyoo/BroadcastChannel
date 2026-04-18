# Repository Guidelines

## Project Structure & Modules

The codebase is built on Astro 4. Routes live in `src/pages`, shared UI in `src/components`, layouts in `src/layouts`, and Telegram/RSS logic in `src/lib`. Styles and static assets sit under `src/styles` and `src/assets`. Everything placed in `public/` is served as-is by Astro. Helper utilities and automation scripts belong to `api/` and `scripts/`. Build artifacts are emitted to `dist/`. Always run `rm -rf dist/` before deploying to avoid stale files.

## Build, Test & Dev Commands

Always use `pnpm`. Run `pnpm install` to install dependencies and register simple-git-hooks when `.git` exists. `pnpm dev`/`pnpm start` launch hot reload on port 4321, `pnpm build` creates the deployable SSR bundle, `pnpm preview` validates the build in the production-like adapter, and `pnpm lint src` / `pnpm lint:fix src` check or auto-fix ESLint issues (注意：lint 作用于所有文件，包含 HTML；用 `src` 限定只检查源码)。

## Code Style & Naming

ESLint relies on `@antfu/eslint-config`, `eslint-plugin-astro`, and `eslint-plugin-format`, using two-space indents, single quotes, and automatic trailing commas per syntax. Name Astro components as `PascalCase.astro`, scripts and helpers as `kebab-case.ts`, environment variables with uppercase snake case, and keep route paths kebab-cased. Make sure `pnpm lint src` is clean before committing, and add brief clarifying comments when Telegram API or RSS handling might confuse readers.

## Testing Guidance

There is no unit-test framework yet. Minimum validation is passing ESLint (`pnpm lint src`) plus a manual `pnpm preview`. When adding modules, prefer injectable pure functions inside `src/lib` to ease future Vitest adoption. List covered user paths in the PR description (e.g., "channel without TAG" or "RSS Beautify disabled") and mention the commands you ran.

## PR & Commit Guidelines

Follow Conventional Commits (`feat:`, `fix:`, `refactor:`, etc.) and explain the reason and impact in the body. Each PR should include: 1) background or linked issue, 2) notes on config/env changes, 3) local validation output or screenshots, and 4) UI screenshots from `pnpm preview` when relevant. Keep PR scope focused; split into multiple commits if it simplifies rollbacks.

## Security & Config Notes

Deployments depend on `.env` settings such as `CHANNEL`, `LOCALE`, Sentry credentials, and social account URLs. Never commit real tokens—use `.env.example` or platform variables (Vercel/Cloudflare/etc.). To try different Telegram proxies, copy `.env` to `.env.local`; Astro will load it during `astro dev`. Cloudflare/Netlify/Vercel Node adapters live in `astro.config.mjs`, so confirm the target platform supports SSR before changing them. Set `SERVER_ADAPTER` env var to override the auto-detected provider.

## Frontend Optimization Guide

For UI/UX improvements, use these skills in order: `typeset` → `colorize` → `adapt` → `animate` → `normalize`. Key CSS files:

- `src/assets/style.css` — 主样式 + CSS 变量定义
- `src/assets/item.css` — Telegram 内容样式
- `src/assets/global.css` — 全局 + 响应式
- `src/layouts/base.astro` — HTML 模板 + Google Fonts
