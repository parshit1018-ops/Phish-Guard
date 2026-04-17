# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Contains a full-stack Fraud Detection Web App (FraudShield).

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Frontend**: React + Vite + Tailwind CSS + shadcn/ui
- **Charts**: Recharts
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Logging**: pino (structured JSON)

## Artifacts

- **`artifacts/fraud-detection`** — React + Vite frontend (previewPath: `/`)
- **`artifacts/api-server`** — Express 5 API server (previewPath: `/api`)

## FraudShield App Features

### Frontend Pages
- `/dashboard` — Security command center: fraud stats, trend charts, pie chart, recent alerts
- `/predict` — Transaction fraud prediction form with probability score and feature explanations
- `/transactions` — Paginated transaction log with fraud/safe color coding and filter
- `/model-insights` — ML model comparison (LR vs Random Forest): accuracy, confusion matrices, feature importance chart

### Backend Routes
- `POST /api/fraud/predict` — Predict fraud for a single transaction (Ensemble LR + RF)
- `POST /api/fraud/batch-predict` — Batch prediction
- `GET /api/fraud/model-metrics` — Pre-computed model performance metrics
- `GET /api/fraud/feature-importance` — Feature importance from Random Forest
- `GET /api/transactions` — Paginated transaction list with fraud filter
- `POST /api/transactions` — Create + evaluate a new transaction
- `GET /api/transactions/:id` — Single transaction
- `GET /api/dashboard/summary` — Aggregated stats
- `GET /api/dashboard/fraud-trends` — Daily fraud trend data
- `GET /api/dashboard/heatmap` — Fraud by location + type
- `GET /api/dashboard/recent-alerts` — Latest fraud alerts
- `POST /api/dashboard/simulate` — Simulate a random transaction

### ML Model (TypeScript)
- Logistic Regression with pre-trained weights
- Random Forest (100-tree ensemble with deterministic pseudo-random trees)
- Ensemble: 40% LR + 60% RF
- Feature extraction: amount, time-of-day, device, transaction type, location risk, fraud history
- Feature importance and SHAP-style explanations

## Database Schema
- `transactions` table: amount, transaction_type, location, hour, device_type, previous_fraud_history, is_fraud, fraud_probability, risk_level, created_at

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
- `pnpm --filter @workspace/fraud-detection run dev` — run frontend locally
