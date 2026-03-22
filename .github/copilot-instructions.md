# Copilot Instructions — Zentto Support

## Context
This is the support repository for Zentto ERP. Issues here are bug reports, feature requests, and questions from customers.

## Zentto ERP Stack
- **Backend**: Node.js 20, Express, TypeScript
- **Frontend**: Next.js 15, React, Tailwind, Material UI (13 micro-frontend apps)
- **Database**: Dual-engine — SQL Server + PostgreSQL (every SP exists in both)
- **Code repo**: zentto-erp/zentto-web
- **SP naming**: `usp_[Schema]_[Entity]_[Action]`
- **Query helpers**: `callSp()`, `callSpOut()`, `callSpTx()` in `web/api/src/db/query.ts`
- **All dates**: UTC-0

## When analyzing issues
1. Identify the module (ventas, inventario, contabilidad, etc.)
2. Check if it's API, frontend, or database related
3. Reference exact file paths in zentto-web repo
4. Remember: SQL changes MUST be in BOTH sqlweb/ and sqlweb-pg/
5. Suggest minimal, focused fixes

## Labels
- `bug`, `feature`, `question` — issue type
- `modulo:*` — affected module
- `urgent` — critical priority
- `ai-fix` — triggers Claude auto-analysis
- `ai-pr` — triggers Claude auto-PR creation
