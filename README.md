# Healthcare Compliance MCP Server

> **[View on Apify](https://apify.com)** | **[Use on Apify Store](https://apify.com)**

---

## Quick Start

Add to your MCP client:

```json
{
  "mcpServers": {
    "healthcare-compliance-mcp": {
      "url": "https://healthcare-compliance-mcp.apify.actor/mcp"
    }
  }
}
```

AI agents can now search medical device compliance data, screen devices for FDA approval status, and generate due diligence reports.

---

## What healthcare compliance data can you access?

| Data Type | Source | Example |
|----------|--------|---------|
| Adverse Event Reports | FDA MAUDE | Device malfunctions, injuries, deaths |
| 510(k) Clearances | FDA | Premarket notification clearances |
| Device Recalls | FDA Enforcement | Class I/II/III recall notices |
| Clinical Trials | ClinicalTrials.gov | Trial phases, recruitment status |

---

## Why use Healthcare Compliance MCP?

**The problem:** Medical device compliance research requires searching multiple FDA databases, clinical trial registries, and enforcement databases — then synthesizing findings into actionable intelligence. This takes hours of manual research.

**The solution:** AI agents use Healthcare Compliance MCP to get instant, structured compliance intelligence on any medical device or manufacturer.

### Key benefits:

- **FDA MAUDE search** — Search adverse event reports across all medical device categories
- **510(k) clearance lookup** — Verify premarket notification status for any device
- **Recall tracking** — Monitor Class I/II/III recalls by manufacturer or product code
- **Clinical trial intelligence** — Research trial phases, recruitment status, and completion dates
- **Compliance scoring** — Composite risk scores combining adverse events, recalls, and enforcement
- **Parallel data fetching** — All FDA sources queried simultaneously for fast responses

---

## Features

**Comprehensive FDA Coverage**
Access all major FDA medical device databases: MAUDE adverse events, 510(k) premarket clearances, and enforcement reports.

**Clinical Trial Intelligence**
Search ClinicalTrials.gov for trial phases, recruitment status, intervention types, and sponsor information.

**Compliance Risk Scoring**
Composite scores (0-100) combining adverse event rates, recall history, and enforcement actions with risk level assignments (LOW/MEDIUM/HIGH/CRITICAL).

**Manufacturer Quality Assessment**
Multi-source quality assessment scoring across all device categories for a given manufacturer.

**Full Compliance Reports**
Generate comprehensive compliance intelligence reports combining all data sources.

**Parallel API Execution**
All FDA and ClinicalTrials.gov sources fetched simultaneously using Promise.all() for fast tool responses.

---

## Use cases for healthcare compliance

### Medical Device Buyer Research
*Persona: Healthcare procurement specialist using AI to evaluate vendor risk*

```
AI agent: "Screen the 'Acuson ultrasound system' from Siemens for compliance risk"
MCP call: screen_device_compliance({ device_name: "Acuson", manufacturer: "Siemens" })
Returns: compliance_score, risk_level, adverse_event_count, recall_count, 510k_status
```

### Clinical Trial Landscape Analysis
*Persona: Biotech investor researching GLP-1 agonist trial landscape*

```
AI agent: "Find all Phase 3 trials for GLP-1 agonists recruiting in 2026"
MCP call: search_clinical_trials({ condition: "GLP-1", phase: "PHASE3", status: "RECRUITING" })
Returns: trials with nct_id, title, sponsor, enrollment, start/completion dates
```

### Manufacturer Due Diligence
*Persona: VC analyst conducting due diligence on medical device startup*

```
AI agent: "Assess Medtronic's quality track record across all device categories"
MCP call: assess_manufacturer_quality({ manufacturer_name: "Medtronic" })
Returns: quality_score, adverse_event_rate, recall_count, device_categories, verdict
```

### Regulatory Compliance Monitoring
*Persona: Compliance officer monitoring vendor compliance*

```
AI agent: "Generate compliance report for 'insulin pump' devices from all manufacturers"
MCP call: generate_compliance_report({ device_name: "insulin pump", include_clinical_trials: true })
Returns: executive_summary, sections for 510k/recalls/events/trials, data sources
```

### 510(k) Clearance Verification
*Persona: FDA consultant verifying device clearance status*

```
AI agent: "Check if Boston Scientific has 510(k) clearance for coronary stents"
MCP call: get_device_510k_clearance({ applicant: "Boston Scientific", device_name: "coronary stent" })
Returns: k_number, decision_date, product_code, clearance status
```

---

## How to connect Healthcare Compliance MCP Server to your AI client

### Step 1: Get your Apify API token

Sign up at [apify.com](https://apify.com) and copy your API token from the console.

### Step 2: Add the MCP server to your client

**Claude Desktop:**
Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "healthcare-compliance-mcp": {
      "url": "https://healthcare-compliance-mcp.apify.actor/mcp"
    }
  }
}
```

**Cursor/Windsurf:**
Add to MCP settings:
```json
{
  "mcpServers": {
    "healthcare-compliance-mcp": {
      "url": "https://healthcare-compliance-mcp.apify.actor/mcp"
    }
  }
}
```

### Step 3: Start querying

```
AI agent: "Screen pacemaker compliance for Medtronic devices"
```

### Step 4: Retrieve results

The MCP returns structured JSON with compliance scores, risk levels, and source data.

---

## MCP tools

| Tool | Price | Description |
|------|-------|-------------|
| search_device_events | $0.05 | Search FDA MAUDE database for adverse event reports |
| get_device_510k_clearance | $0.03 | Get 510(k) premarket clearance details |
| get_device_recalls | $0.05 | Search FDA enforcement reports for recalls |
| search_clinical_trials | $0.05 | Search ClinicalTrials.gov registry |
| get_trial_details | $0.03 | Get detailed information for a specific trial |
| screen_device_compliance | $0.10 | Composite compliance risk score for a device |
| assess_manufacturer_quality | $0.08 | Multi-source quality assessment for manufacturer |
| generate_compliance_report | $0.15 | Full compliance intelligence report |

---

## Tool parameters

### search_device_events

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| device_name | string | No | Device name (e.g., 'pacemaker') |
| manufacturer | string | No | Manufacturer name |
| product_code | string | No | FDA product code |
| date_from | string | No | Start date YYYYMMDD |
| date_to | string | No | End date YYYYMMDD |
| max_results | integer | No | Maximum results (default: 10) |

### get_device_510k_clearance

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| applicant | string | No | Applicant/manufacturer name |
| product_code | string | No | FDA product code |
| device_name | string | No | Device name |
| date_from | string | No | Start date YYYYMMDD |
| date_to | string | No | End date YYYYMMDD |
| max_results | integer | No | Maximum results (default: 10) |

### get_device_recalls

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| recalling_firm | string | No | Recalling firm name |
| product_code | string | No | FDA product code |
| classification | string | No | Recall classification (Class I, II, III) |
| date_from | string | No | Start date YYYYMMDD |
| date_to | string | No | End date YYYYMMDD |
| max_results | integer | No | Maximum results (default: 10) |

### search_clinical_trials

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| condition | string | No | Medical condition |
| intervention | string | No | Intervention name or type |
| sponsor | string | No | Sponsor name |
| phase | string | No | Trial phase (PHASE1, PHASE2, PHASE3, PHASE4) |
| status | string | No | Trial status (RECRUITING, COMPLETED, etc.) |
| max_results | integer | No | Maximum results (default: 10) |

### get_trial_details

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| nct_id | string | Yes | ClinicalTrials.gov NCT ID (e.g., 'NCT000001') |

### screen_device_compliance

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| device_name | string | Yes | Device name |
| manufacturer | string | No | Manufacturer name |

### assess_manufacturer_quality

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| manufacturer_name | string | Yes | Manufacturer name |

### generate_compliance_report

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| device_name | string | Yes | Device name |
| manufacturer | string | No | Manufacturer name |
| include_clinical_trials | boolean | No | Include clinical trial data (default: false) |

---

## Connection examples

### cURL

```bash
curl -X POST "https://healthcare-compliance-mcp.apify.actor/mcp" \
  -H "Authorization: Bearer YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "search_device_events",
    "params": { "device_name": "pacemaker", "max_results": 5 }
  }'
```

### Node.js

```javascript
const response = await fetch('https://healthcare-compliance-mcp.apify.actor/mcp', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_APIFY_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    tool: 'screen_device_compliance',
    params: { device_name: 'insulin pump', manufacturer: 'Medtronic' }
  })
});
const data = await response.json();
console.log(data.result.compliance_score);
```

---

## Output example

```json
{
  "status": "success",
  "result": {
    "device_name": "Insertable Cardiac Pacemaker",
    "manufacturer": "Medtronic",
    "compliance_score": 85,
    "risk_level": "LOW",
    "signals": {
      "adverse_event_rate": { "level": "LOW", "label": "Adverse events rate below average" },
      "recall_history": { "level": "NONE", "label": "No recalls" },
      "enforcement_actions": { "level": "NONE", "label": "No enforcement actions" }
    },
    "adverse_events_count": 127,
    "recall_count": 0,
    "510k_clearances": 12,
    "last_510k_date": "2024-01-15",
    "verdict": "Device has clean regulatory history with no recalls or major adverse events",
    "source": "FDA MAUDE + 510(k) + Enforcement"
  }
}
```

---

## Output fields

| Field | Description |
|-------|-------------|
| compliance_score | Composite score 0-100 based on adverse events, recalls, enforcement |
| risk_level | LOW (80+), MEDIUM (60-79), HIGH (40-59), CRITICAL (<40) |
| signals | Detailed signal breakdown for each risk factor |
| adverse_events_count | Total MAUDE adverse event reports for device |
| recall_count | Total FDA enforcement/recalactions for device/manufacturer |
| 510k_clearances | Count of 510(k) premarket clearances |
| last_510k_date | Date of most recent 510(k) clearance |
| verdict | Human-readable compliance assessment |
| source | Data sources queried |

---

## How much does it cost to run Healthcare Compliance MCP?

**PPE (Pay-Per-Event) pricing — $0.03 to $0.15 per tool call.**

| Tool | Price |
|------|-------|
| search_device_events | $0.05 |
| get_device_510k_clearance | $0.03 |
| get_device_recalls | $0.05 |
| search_clinical_trials | $0.05 |
| get_trial_details | $0.03 |
| screen_device_compliance | $0.10 |
| assess_manufacturer_quality | $0.08 |
| generate_compliance_report | $0.15 |

No subscription. No monthly fee. Pay only when AI agents use the tools.

---

## How Healthcare Compliance MCP works

### Phase 1: Request parsing
AI agent sends tool call via MCP protocol. Server parses tool name and parameters.

### Phase 2: Parallel data fetching
For composite tools (screen_device_compliance, assess_manufacturer_quality, generate_compliance_report), all data sources are queried simultaneously:
- FDA MAUDE adverse events
- FDA 510(k) clearances
- FDA device enforcement reports
- ClinicalTrials.gov (if enabled)

### Phase 3: Scoring and synthesis
Composite tools apply scoring algorithms:
- Adverse event rate scoring
- Recall history weighting
- Enforcement action detection
- Risk level classification

### Phase 4: Response formatting
All results returned as structured JSON with compliance scores, risk levels, signals, and source attribution.

---

## Tips for best results

1. **Use specific device names** — More specific queries (e.g., "insertable cardiac pacemaker") return better results than generic ("pacemaker")

2. **Include manufacturer when known** — Combining device name + manufacturer improves accuracy for compliance screening

3. **Filter by date for trending** — Use date_from/date_to to track compliance over specific time periods

4. **Use composite tools for due diligence** — screen_device_compliance and assess_manufacturer_quality provide holistic risk views

5. **Include clinical trials for device research** — Setting include_clinical_trials: true adds trial intelligence to compliance reports

6. **Check 510(k) status first** — For new devices, verify 510(k) clearance before deeper compliance research

---

## Combine with other Apify actors

**For comprehensive healthcare AI workflows:**

- **Clinical Research MCP** — Find papers, grants, and institutional research profiles
- **Company Intelligence MCP** — Enrich company data with SEC filings, news, and organizational data
- **Lead Enrichment MCP** — Build healthcare professional contact lists for B2B sales

---

## License

Apache 2.0
