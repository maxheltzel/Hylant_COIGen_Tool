# COI Generation Pipeline

An AI-driven Certificate of Insurance (COI) generation system that automatically processes email requests and produces Acord 25 PDFs.

## Features

- 🚀 Automated email monitoring via Microsoft Graph API
- 🤖 AI-powered policy document parsing with RAG
- 📄 Acord 25 PDF generation
- 🔍 Smart validation and human review flags
- 📊 Real-time dashboard with React/Tailwind UI
- 🔐 Enterprise-ready with audit logging

## Tech Stack

- **Backend**: Python 3.11, FastAPI, SQLAlchemy
- **AI/ML**: OpenAI GPT-4o-mini, LangChain, FAISS
- **Queue**: Celery + Redis
- **Frontend**: React 18, Vite, Tailwind CSS, shadcn/ui
- **Infrastructure**: Docker, GitHub Actions

## Quick Start

### Local Development

1. Clone the repository:
```bash
git clone https://github.com/hylant/coi-pipeline.git
cd coi-pipeline
```

2. Copy environment template:
```bash
cp .env.example .env
# Edit .env with your credentials
```

3. Install dependencies:
```bash
# Backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Frontend
cd app/ui/frontend
npm install
```

4. Start services:
```bash
# Terminal 1: Redis
redis-server

# Terminal 2: Celery
celery -A app.tasks worker --loglevel=info

# Terminal 3: Backend
uvicorn app.main:app --reload

# Terminal 4: Frontend
cd app/ui/frontend
npm run dev
```

### Docker Setup

```bash
docker-compose up --build
```

Access:
- API: http://localhost:8000
- UI: http://localhost:5173
- API Docs: http://localhost:8000/docs

## Configuration

Required environment variables:

```env
# Microsoft Graph
GRAPH_CLIENT_ID=your-client-id
GRAPH_CLIENT_SECRET=your-secret
GRAPH_TENANT_ID=your-tenant-id
GRAPH_USER_EMAIL=coi-requests@hylant.com

# Epic Database
EPIC_DB_HOST=your-epic-server
EPIC_DB_NAME=EpicDB
EPIC_DB_USER=your-user
EPIC_DB_PASSWORD=your-password

# OpenAI
OPENAI_API_KEY=sk-your-key

# Redis
REDIS_URL=redis://localhost:6379

# Email
SMTP_HOST=smtp.office365.com
SMTP_PORT=587
SMTP_USER=noreply@hylant.com
SMTP_PASSWORD=your-password
```

## Architecture

See `docs/architecture.md` for detailed system design and data flow diagrams.

## API Endpoints

Import `postman/COI_Pipeline.postman_collection.json` for full API documentation.

Key endpoints:
- `POST /api/process-email` - Manually trigger email processing
- `GET /api/requests` - List all COI requests
- `GET /api/requests/{id}/status` - Check request status
- `POST /api/requests/{id}/retry` - Retry failed request

## Testing

```bash
# Run all tests
pytest

# With coverage
pytest --cov=app --cov-report=html

# Specific module
pytest tests/test_parser.py -v
```

## Production Deployment

1. Build Docker image:
```bash
docker build -t hylant/coi-pipeline:latest .
```

2. Deploy to Kubernetes:
```bash
kubectl apply -f k8s/
```

3. Configure monitoring:
- Prometheus metrics at `/metrics`
- Grafana dashboards in `monitoring/dashboards/`

## Contributing

1. Create feature branch from `main`
2. Add tests for new functionality
3. Ensure 90%+ test coverage
4. Submit PR with description

## License

Proprietary - Hylant Group, Inc.
