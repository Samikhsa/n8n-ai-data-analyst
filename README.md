Step 1 — Export the workflow JSONs from n8n
Open Data-Analyst-Upload in the editor
Click the ⋯ menu (top right) → Download
Save the file as data-analyst-upload.workflow.json
Repeat for Data-Analyst-Chat → data-analyst-chat.workflow.json
Step 2 — Create a GitHub repository
Go to github.com/new
Name it something like n8n-ai-data-analyst
Set it to Public (so recruiters can see it)
Check Add a README file
Click Create repository
Step 3 — Structure your repo
Create this folder layout locally:

n8n-ai-data-analyst/
├── README.md
├── workflows/
│   ├── data-analyst-upload.workflow.json
│   └── data-analyst-chat.workflow.json
├── demo/
│   └── demo.gif   ← (optional screen recording)
└── docs/
    └── architecture.md   ← (optional diagram)
Step 4 — Write a strong README.md
Here's a template tailored to your project:

# AI Data Analyst — n8n Automation

An AI-powered data analysis system built with n8n, OpenAI, and a persistent data cache.

## Features
- **CSV/XLSX Upload**: Validates files, profiles data, detects outliers, computes correlations
- **AI Chat Interface**: Ask plain-English questions about your uploaded dataset
- **Chart Suggestions**: GPT-powered chart type recommendations with Quickchart.io URLs
- **Data Persistence**: Results cached in n8n Data Tables for instant retrieval

## Architecture

| Workflow | Trigger | Purpose |
|---|---|---|
| `Data-Analyst-Upload` | `POST /webhook/upload-dataset` | Ingest, profile, and cache a CSV/XLSX file |
| `Data-Analyst-Chat` | `POST /webhook/chat-dataset` | Answer questions about a cached dataset |

## How to Import

1. Install [n8n](https://n8n.io) (cloud or self-hosted)
2. Go to **Workflows** → **Import from file**
3. Import `workflows/data-analyst-upload.workflow.json`
4. Import `workflows/data-analyst-chat.workflow.json`
5. Add your **OpenAI API key** in credentials
6. Create the `dataset_cache` Data Table with the schema below
7. Activate both workflows

## dataset_cache Schema

| Column | Type |
|---|---|
| dataset_id | string |
| filename | string |
| n_rows | number |
| n_cols | number |
| duplicate_count | number |
| numeric_cols | string |
| categorical_cols | string |
| datetime_cols | string |
| profile_json | string |
| outlier_report_json | string |
| descriptive_stats_json | string |
| correlation_matrix_json | string |
| cleaned_rows_json | string |

## Example Requests

Upload a file:
```bash
curl -X POST https://<your-n8n>/webhook/upload-dataset \
  -F "file=@sales_data.csv"
Ask a question:

curl -X POST https://<your-n8n>/webhook/chat-dataset \
  -H "Content-Type: application/json" \
  -d '{"dataset_id":"ds-abc123","chatInput":"What are the top 3 trends?"}'
Tech Stack
n8n — workflow automation
OpenAI GPT-4o — AI analysis & chart suggestions
Quickchart.io — chart rendering
n8n Data Tables — persistent cache

---

## Step 5 — Push to GitHub

```bash
# Clone your new repo
git clone https://github.com/<your-username>/n8n-ai-data-analyst.git
cd n8n-ai-data-analyst

# Add your files
cp ~/Downloads/data-analyst-upload.workflow.json workflows/
cp ~/Downloads/data-analyst-chat.workflow.json workflows/

# Commit and push
git add .
git commit -m "Initial commit: AI Data Analyst workflows"
git push origin main
Step 6 — Add a demo (highly recommended for resumes)
Record a 60–90 second Loom or screen recording: upload a CSV → ask a question → see the chart URL output
Drop the .gif or link in your README under a Demo section
This is what makes recruiters stop scrolling
