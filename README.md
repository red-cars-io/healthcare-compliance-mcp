# Healthcare Compliance MCP Server

> FDA regulatory intelligence for AI agents — medical device MAUDE events, 510(k) clearances, device recalls, and ClinicalTrials.gov data. No API key required.

**[View on Apify](https://apify.com/red.cars/healthcare-compliance-mcp)** | **[MCP Endpoint](https://healthcare-compliance-mcp.apify.actor/mcp)**

---

## What It Does

Give AI agents direct access to FDA and ClinicalTrials.gov data for medical device compliance, adverse event monitoring, and regulatory due diligence.

- **MAUDE adverse events** — search FDA MAUDE database for medical device adverse event reports
- **510(k) clearances** — look up FDA premarket clearance (510(k)) for specific devices
- **Device recalls** — track FDA Class I/II/III medical device recalls by manufacturer or product
- **Clinical trials** — search ClinicalTrials.gov for device trials and intervention studies
- **Compliance reports** — composite compliance assessment with risk scoring

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

---

## Comparison

See how Healthcare Compliance MCP compares to manual FDA research and paid databases like LexisNexis.

**[Read the Comparison →](COMPARISON.md)**

---

## Tools

| Tool | Price | Description |
|------|-------|-------------|
| `search_device_events` | $0.05 | Search FDA MAUDE for medical device adverse event reports |
| `get_device_510k_clearance` | $0.03 | Get 510(k) premarket clearance details |
| `get_device_recalls` | $0.05 | Track FDA device recalls by manufacturer or product |
| `search_clinical_trials` | $0.05 | Search ClinicalTrials.gov for device trials |
| `get_trial_details` | $0.03 | Get detailed clinical trial information |
| `screen_device_compliance` | $0.10 | Composite compliance screen for a device/manufacturer |
| `assess_manufacturer_quality` | $0.08 | Assess manufacturer quality from FDA data |
| `generate_compliance_report` | $0.15 | Full compliance report — MAUDE, recalls, 510(k), trials |

### search_device_events
**When to call:** AI agent investigating medical device safety signals, adverse event patterns, or product liability.
**Example AI prompt:** "Search FDA MAUDE for adverse event reports involving insulin pumps in the last 3 years — show devices with the most serious outcomes."

### get_device_510k_clearance
**When to call:** AI agent verifying FDA clearance status for a medical device before procurement or clinical use.
**Example AI prompt:** "Did Medtronic receive 510(k) clearance for their pacemaker lead model? What's the clearance number and date?"

### get_device_recalls
**When to call:** AI agent tracking device recall status for clinical risk management or procurement.
**Example AI prompt:** "Show me all Class I and Class II recalls for devices made by Johnson & Johnson's DePuy subsidiary in the last 5 years."

### screen_device_compliance
**When to call:** AI agent doing pre-purchase due diligence on a medical device vendor or manufacturer.
**Example AI prompt:** "Screen Boston Scientific's compliance profile — check their MAUDE events, recalls, and 510(k) history."

### generate_compliance_report
**When to call:** AI agent producing a comprehensive medical device compliance assessment for regulatory or investment purposes.
**Example AI prompt:** "Generate a full FDA compliance report for Philips' respiratory device division covering 2019-2024."

---

## Data Sources

| Source | Coverage | What's Available |
|--------|----------|-----------------|
| FDA MAUDE | US | Adverse event reports (mandatory for manufacturers) |
| FDA 510(k) | US | Premarket clearance database |
| FDA Recalls | US | Class I/II/III medical device recalls |
| ClinicalTrials.gov | Global | Device trials, intervention studies |

---

## Pricing

| Tool | Per Call |
|------|----------|
| `search_device_events` | $0.05 |
| `get_device_510k_clearance` | $0.03 |
| `get_device_recalls` | $0.05 |
| `search_clinical_trials` | $0.05 |
| `get_trial_details` | $0.03 |
| `screen_device_compliance` | $0.10 |
| `assess_manufacturer_quality` | $0.08 |
| `generate_compliance_report` | $0.15 |

No subscription. Pay per use via Apify PPE.

---

## Example Calls

### Search MAUDE adverse events

```
search_device_events(device_name="insulin pump", max_results=10)
```

Returns:
```json
{
  "device_name": "insulin pump",
  "total_events": 847,
  "events": [
    {
      "event_id": "20234567890",
      "device_name": "Insulin Infusion Pump",
      "manufacturer": "Medtronic",
      "adverse_event_description": "Battery depletion leading to interrupted therapy",
      "date_of_event": "2023-08-15",
      "source": "FDA MAUDE"
    }
  ]
}
```

### Screen device compliance

```
screen_device_compliance(device_name="coronary stent", manufacturer="Abbott Vascular")
```

Returns:
```json
{
  "device_name": "coronary stent",
  "manufacturer": "Abbott Vascular",
  "maude_events_count": 234,
  "recall_count": 3,
  "510k_clearances": 7,
  "compliance_score": 82,
  "risk_level": "LOW",
  "sources": ["FDA MAUDE", "FDA 510(k)", "FDA Recalls"]
}
```

---

## Connect to AI Agents

### Claude Desktop / Cursor / Windsurf
```json
{
  "mcpServers": {
    "healthcare-compliance-mcp": {
      "url": "https://healthcare-compliance-mcp.apify.actor/mcp"
    }
  }
}
```

---

## SEO Keywords

FDA MAUDE API, medical device adverse events, 510(k) clearance lookup, FDA device recalls, ClinicalTrials.gov API, medical device compliance, AI agent FDA research, medical device due diligence, FDA regulatory intelligence, clinical trial search API, medtech compliance, device safety signals, AI agent healthcare

- [Comparison: vs FDA database, LexisNexis](COMPARISON.md)
