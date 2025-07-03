# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a benchmarking suite that compares the performance of popular monorepo build tools: Nx, Turbo, Lerna, and Lage. It simulates a medium-sized enterprise monorepo with ~26,000 components across 5 Next.js apps and 105 library packages.

## Essential Commands

### Benchmarking
```bash
# Run benchmark with console output
pnpm run benchmark

# Run benchmark with JSON output (for automation)
pnpm run benchmark:json

# TypeScript development version
pnpm run benchmark:json:ts
```

### Development
```bash
# Install dependencies
pnpm install

# Build TypeScript scripts
pnpm run build:scripts

# Watch mode for script development
pnpm run build:scripts:watch

# Run automation tests
pnpm run test:automation:ts

# Compare results and update README
pnpm run compare:results:ts
```

## Architecture

### Directory Structure
- `/apps/` - 5 Next.js applications (crew, flight-simulator, navigation, ticket-booking, warp-drive-manager)
- `/packages/shared/` - 5 shared libraries with 250 components each
- `/packages/[app-name]/` - App-specific libraries (20 per app, 250 components each)
- `/scripts/` - TypeScript automation scripts for benchmarking and maintenance
- `/.github/workflows/` - GitHub Actions for daily automation

### Key Files
- `/scripts/run-benchmarks.ts` - Main benchmarking logic
- `/scripts/compare-results.ts` - Results comparison and README updates
- `/scripts/create-release.ts` - GitHub release creation
- `previous-benchmark-results.json` - Historical benchmark data
- Build tool configs: `nx.json`, `turbo.json`, `lerna.json`, `lage.config.js`

## Development Guidelines

### When Working with Benchmarks
1. The benchmark process warms up caches before measuring cache restoration performance
2. Each tool runs 10 iterations for statistical accuracy
3. Results are automatically saved to `previous-benchmark-results.json`
4. Performance regressions >10% trigger automated issue creation

### When Modifying Scripts
1. All automation scripts are in TypeScript under `/scripts/`
2. Use `pnpm run build:scripts:watch` during development
3. Test changes with `pnpm run test:automation:ts`
4. Scripts interact with GitHub API for releases and issues

### Important Context
- This repo tests cache restoration performance, not full builds or incremental builds
- Daily GitHub Actions update dependencies and run benchmarks at 6 AM UTC
- Version tags follow format: `benchmark-nx[version]-turbo[version]-lerna[version]-lage[version]`
- The repo is designed to be fair to all tools, using Next.js which all handle well