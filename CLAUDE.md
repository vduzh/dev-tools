# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Shared development tools running as Docker services. Not tied to any specific project — serves all repositories.

## Setup

```bash
cp .env.example .env   # copy and adjust environment variables
docker compose up -d   # start services
docker compose down    # stop (data preserved)
docker compose down -v # stop and delete all data
```

## Services

- **Nexus** (sonatype/nexus3) — Maven repository + Docker registry
  - Web UI / Maven: port `NEXUS_PORT` (default 8081)
  - Docker registry: port `NEXUS_DOCKER_PORT` (default 8082)
  - Data persisted in `nexus-data` named volume

## Configuration

- `.env.example` — tracked by git, contains all available variables with defaults
- `.env` — gitignored, local copy with actual values (created from `.env.example`)

Docker Compose uses `${VAR:-default}` syntax for fallbacks.

## Commit Messages

Conventional Commits format: `<type>(<scope>): <summary>`

Allowed types: feat, fix, docs, style, refactor, perf, test, chore. Summary must be imperative and short.

## Commit Workflow

When asked to commit:
1. Show the proposed commit message and ask "OK?"
2. Wait for confirmation
3. Stage **all** changed files (not just what's already in stage) and commit
