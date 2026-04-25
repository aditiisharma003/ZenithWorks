# ZenithWorks AI Employees

Multi-agent AI automation platform that generates professional emails for 4 departments using **CrewAI + Google Gemini 1.5 Flash**.

## Why Gemini?
| | Gemini 1.5 Flash | GPT-4o-mini |
|---|---|---|
| Free tier | ✅ Yes (Google AI Studio) | ❌ No |
| Context window | 1M tokens | 128K tokens |
| Speed | Very fast | Fast |
| Google ecosystem | Native | Requires extra setup |

## Departments

| Department | Input Fields |
|------------|-------------|
| HR | name, position, department, start_date, manager |
| Customer Service | customer_name, issue, product, priority |
| Marketing | product, audience, benefits, offer |
| Accounting | client_name, invoice_number, amount, due_date |

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Set up environment
cp .env.example .env
# Edit .env — add your GEMINI_API_KEY (free from https://aistudio.google.com)

# 3. Run
python app.py
# → http://localhost:8080
```

## Get Your Free Gemini API Key

1. Go to [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with Google
3. Click **Create API Key**
4. Paste into `.env` as `GEMINI_API_KEY=...`

## Docker

```bash
docker-compose up --build
```

## Deploy to Render (free)

1. Push to GitHub
2. Connect repo at [render.com](https://render.com)
3. Set env vars in Render dashboard
4. Deploy — `render.yaml` handles the rest

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check + monitoring stats |
| GET | `/api/departments` | List available departments |
| POST | `/api/process/{dept}` | Generate single email |
| POST | `/api/process/csv` | Batch CSV processing |

## Single Request Example

```bash
curl -X POST http://localhost:8080/api/process/hr \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "position": "Software Engineer",
    "department": "Engineering",
    "start_date": "2025-09-01",
    "manager": "John Smith"
  }'
```

## Batch CSV Example

```bash
curl -X POST http://localhost:8080/api/process/csv \
  -H "Content-Type: application/json" \
  -d '{
    "department": "hr",
    "csv_content": "name,position,department,start_date,manager\nJane,Engineer,Tech,2025-09-01,John",
    "save_to_sheets": false
  }'
```

## Monitoring — /health Response

```json
{
  "status": "healthy",
  "model": "gemini-1.5-flash",
  "monitoring": {
    "total_requests": 47,
    "avg_latency_ms": 1842.3,
    "department_breakdown": {
      "hr": 18,
      "marketing": 12,
      "customer-service": 10,
      "accounting": 7
    },
    "batch_rows_processed": 23,
    "emails_sent": 5,
    "errors": 0
  }
}
```

## Google Sheets Setup (Optional)

1. Create a Google Cloud project
2. Enable the Google Sheets API
3. Create a Service Account and download the JSON key
4. Share your spreadsheet with the service account email
5. Paste the JSON as a single line into `GOOGLE_SHEETS_CREDENTIALS`

Sheet tabs auto-logged:
- `HR_Outputs`, `CS_Outputs`, `Marketing_Outputs`, `Accounting_Outputs`
- `HR_Batch`, `CS_Batch`, `Marketing_Batch`, `Accounting_Batch`
