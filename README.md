# n8n Automation Templates

Production-ready workflow templates for common automation scenarios. Import directly into n8n and customize for your needs.

## Templates Overview

| Template | Use Case | Nodes |
|----------|----------|-------|
| **Social Media Scheduler** | Schedule posts across platforms | Cron, HTTP, Switch, Set |
| **Lead Qualification Pipeline** | Auto-qualify inbound leads | Webhook, HTTP, IF, Google Sheets |
| **Invoice Automation** | Generate + send invoices | Schedule, PDF, Email, Airtable |
| **Data Sync Pipeline** | Sync data between systems | Schedule, HTTP, Merge, Database |
| **Alert Monitoring** | Monitor APIs + alert on failures | Cron, HTTP, IF, Slack/Email |
| **Cross-Border SEO Generator** | Bulk multilingual Amazon listings + SEO blogs via LLM | Manual, Spreadsheet File, HTTP, Code |

## Quick Start

1. Open n8n instance
2. Go to **Workflows > Import from File**
3. Select a `.json` file from `/workflows/`
4. Configure credentials (API keys, etc.)
5. Activate

## Templates

### 1. Social Media Scheduler (`social-media-scheduler.json`)

```
Cron -> Fetch Content -> Format -> Post to Platform
```

**Features:**
- Multi-platform support (Twitter, LinkedIn, Instagram via Buffer API)
- Content queue from Google Sheets or Airtable
- Automatic hashtag suggestions
- Best posting time detection

### 2. Lead Qualification Pipeline (`lead-qualification.json`)

```
Webhook -> Enrich -> Score -> Route to CRM/Alert
```

**Scoring Logic:**
- Company size (1-10 points)
- Industry match (1-5 points)
- Email domain (personal = 0, business = 2)
- Job title seniority (1-5 points)
- Lead score -> Auto-route to sales or nurture

### 3. Invoice Automation (`invoice-automation.json`)

```
Schedule -> Fetch Time Entries -> Generate PDF -> Email
```

**Features:**
- Pull time entries from Toggl/Harvest
- Generate branded PDF invoice
- Email to client with payment link
- Log to Airtable for tracking

### 4. Data Sync Pipeline (`data-sync.json`)

```
Schedule -> Fetch A -> Merge with B -> Upsert Database
```

**Features:**
- Deduplication logic
- Field mapping
- Error handling with retry
- Change log

### 5. Alert Monitoring (`alert-monitor.json`)

```
Cron -> HTTP Check -> IF Error -> Alert Channel
```

**Features:**
- Multiple endpoints
- Response time tracking
- Slack/Email/Teams alerts
- Escalation after N failures

### 6. Cross-Border SEO & Multilingual Listing Generator (`shannonapi-cross-border-seo-workflow.json`)

```
Manual -> Read Product Table -> Build Prompts -> LLM (OpenAI-compatible) -> Parse -> Write Results
```

**Features:**
- Reads product info (name/category/description/target country) from an Excel/CSV file
- Batch generates Amazon multilingual listings (EN/DE/FR/JA/ES) + 1200-word SEO blog per product
- Uses a single OpenAI-compatible Chat Completions endpoint (`https://shannonapi.xyz/v1`) — swap in any compatible key
- Structured JSON output, then writes everything back to a results spreadsheet

**Setup:**
- Set the `SHANNON_API_KEY` environment variable (get one at https://shannonapi.xyz)
- Change the input/output file paths (`/data/products.xlsx`, `/data/output_listings_seo.xlsx`) to your own
- Run the manual trigger once; each product row is processed in one call — scale rows freely for high-throughput batches

## Customization Guide

Each workflow includes:
- **Environment variables** for secrets
- **Switch nodes** for branching logic
- **Error nodes** for handling failures
- **Comment nodes** explaining logic

## Architecture Best Practices

1. **Separate credentials** - Use n8n's credential system
2. **Idempotent operations** - Safe to re-run
3. **Rate limiting** - Respect API limits
4. **Error logging** - All failures logged
5. **Testing** - Use manual trigger before activating

## Requirements

- n8n instance (self-hosted or cloud)
- API keys for external services
- Basic n8n knowledge

## License

MIT - Use freely for personal and commercial projects.

## Usage Stats

**Last Updated: 2026-04-22**

| Workflow | Runs | Success Rate |
|----------|------|--------------|
| Lead Qualification | 247 | 99.2% |
| Social Media Scheduler | 183 | 98.9% |
| Invoice Automation | 52 | 100% |
| Data Sync | 312 | 97.8% |
| Alert Monitoring | 1,892 | 99.7% |
