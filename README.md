# Data Transform Engine

A microservices application for processing and aggregating stock market data.

## Architecture

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│ Frontend │────▶│ Backend  │────▶│Go Worker │
│ (Next.js)│     │ (FastAPI)│     │ (Go)     │
│ :3000    │     │ :8000    │     │ :8080    │
└──────────┘     └──────────┘     └──────────┘
```

## Services

| Service | Tech | Port | Purpose |
|---------|------|------|---------|
| [Frontend](https://github.com/obj809/frontend-data-transform-engine) | Next.js, React 19, TypeScript | 3000 | File upload UI, results display |
| [Backend](https://github.com/obj809/backend-data-transform-engine) | FastAPI, Python | 8000 | API gateway, validation, orchestration |
| [Go Worker](https://github.com/obj809/go-worker-data-transform-engine) | Go | 8080 | Data processing, aggregation |

## Quick Start

```bash
# Terminal 1 - Go Worker
cd go-worker-data-transform-engine
go run main.go

# Terminal 2 - Backend
cd backend-data-transform-engine
source venv/bin/activate
uvicorn main:app --reload

# Terminal 3 - Frontend
cd frontend-data-transform-engine
npm run dev
```

## Data Flow

1. User uploads JSON file via frontend
2. Backend validates file format
3. Backend sends records to Go worker for processing
4. Worker computes averages across all records
5. Result returned to frontend for display

## Configuration

| Variable | Service | Default | Description |
|----------|---------|---------|-------------|
| `WORKER_URL` | Backend | `http://localhost:8080` | Go worker URL |
| `NEXT_PUBLIC_API_URL` | Frontend | - | Backend API URL |
| `PORT` | Go Worker | `8080` | Worker server port |

## Sample Data

- `synthetic-data.json` - Sample input (50 stock records)
- `synthetic-data-result.json` - Expected output
