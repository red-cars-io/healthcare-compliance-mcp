# Add Healthcare Compliance Intelligence to Your AI Agent in 5 Minutes

A practical guide for AI agent developers (LangChain, AutoGen, CrewAI) to add healthcare and FDA regulatory intelligence — device approvals, adverse events, 510(k) clearances, clinical trials, and risk assessments — to their agents in minutes. No API keys required beyond your Apify token.

## What We're Building

An AI agent that can:
1. Search FDA device approvals and clearance status
2. Monitor FDA MAUDE adverse event reports
3. Find 510(k) premarket notification clearances
4. Research clinical trials on ClinicalTrials.gov
5. Assess medical device risk with composite scoring
6. Search the UDI (Unique Device Identification) database
7. Track FDA device recalls

## Prerequisites

- Node.js 18+
- An Apify API token ([free account works](https://console.apify.com/settings/integrations))
- An AI agent framework: LangChain, AutoGen, or CrewAI

## The MCPs We're Using

| MCP | Purpose | Cost | Endpoint |
|-----|---------|------|----------|
| `healthcare-compliance-mcp` | FDA device approvals, MAUDE, 510(k), ClinicalTrials, recalls | $0.03-0.15/call | `red-cars--healthcare-compliance-mcp.apify.actor` |
| `drug-intelligence-mcp` | FDA drug labels, adverse events, drug interactions, recalls | $0.03-0.08/call | `red-cars--drug-intelligence-mcp.apify.actor` |
| `academic-research-mcp` | Paper search, clinical trial publications | $0.01-0.10/call | `academic-research-mcp.apify.actor` |
| `tech-scouting-report-mcp` | Technology readiness, funding validation | $0.05-0.10/call | `tech-scouting-report-mcp.apify.actor` |

**Note:** `healthcare-compliance-mcp` covers medical devices. Chain it with `drug-intelligence-mcp` for pharmaceutical intelligence (drugs, not devices), and `academic-research-mcp` for clinical trial publication data.

## Step 1: Add the MCP Servers

### MCP Server Configuration

```json
{
  "mcpServers": {
    "healthcare-compliance": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-apify", "red-cars--healthcare-compliance-mcp"],
      "env": {
        "APIFY_API_TOKEN": "${APIFY_API_TOKEN}"
      }
    },
    "drug-intelligence": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-apify", "red-cars--drug-intelligence-mcp"],
      "env": {
        "APIFY_API_TOKEN": "${APIFY_API_TOKEN}"
      }
    },
    "academic-research": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-apify", "academic-research-mcp"],
      "env": {
        "APIFY_API_TOKEN": "${APIFY_API_TOKEN}"
      }
    },
    "tech-scouting": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-apify", "tech-scouting-report-mcp"],
      "env": {
        "APIFY_API_TOKEN": "${APIFY_API_TOKEN}"
      }
    }
  }
}
```

### LangChain Configuration

```javascript
import { ApifyAdapter } from "@langchain/community/tools/apify";
import { ChatOpenAI } from "@langchain/openai";
import { initializeAgentExecutorWithOptions } from "langchain/agents";

const tools = [
  new ApifyAdapter({
    token: process.env.APIFY_API_TOKEN,
    actorId: "red-cars--healthcare-compliance-mcp",
  }),
  new ApifyAdapter({
    token: process.env.APIFY_API_TOKEN,
    actorId: "red-cars--drug-intelligence-mcp",
  }),
  new ApifyAdapter({
    token: process.env.APIFY_API_TOKEN,
    actorId: "academic-research-mcp",
  }),
  new ApifyAdapter({
    token: process.env.APIFY_API_TOKEN,
    actorId: "tech-scouting-report-mcp",
  }),
];

const agent = await initializeAgentExecutorWithOptions(tools, new ChatOpenAI({
  model: "gpt-4",
  temperature: 0
}), { agentType: "openai-functions" });
```

### AutoGen Configuration

```javascript
import { MCPAgent } from "autogen-mcp";

const healthcareAgent = new MCPAgent({
  name: "healthcare-compliance",
  mcpServers: [
    {
      name: "healthcare-compliance",
      command: "npx",
      args: ["-y", "@modelcontextprotocol/server-apify", "red-cars--healthcare-compliance-mcp"],
    },
    {
      name: "drug-intelligence",
      command: "npx",
      args: ["-y", "@modelcontextprotocol/server-apify", "red-cars--drug-intelligence-mcp"],
    },
    {
      name: "academic-research",
      command: "npx",
      args: ["-y", "@modelcontextprotocol/server-apify", "academic-research-mcp"],
    },
    {
      name: "tech-scouting",
      command: "npx",
      args: ["-y", "@modelcontextprotocol/server-apify", "tech-scouting-report-mcp"],
    }
  ]
});
```

### CrewAI Configuration

```yaml
# crewai.yaml
tools:
  - name: healthcare_compliance
    type: apify
    actor_id: red-cars--healthcare-compliance-mcp
    api_token: ${APIFY_API_TOKEN}

  - name: drug_intelligence
    type: apify
    actor_id: red-cars--drug-intelligence-mcp
    api_token: ${APIFY_API_TOKEN}

  - name: academic_research
    type: apify
    actor_id: academic-research-mcp
    api_token: ${APIFY_API_TOKEN}

  - name: tech_scouting
    type: apify
    actor_id: tech-scouting-report-mcp
    api_token: ${APIFY_API_TOKEN}
```

## Step 2: Healthcare Compliance Queries

### Search FDA Device Approvals

```javascript
const result = await healthcareAgent.execute({
  action: "search_fda_approvals",
  searchTerm: "cardiac pacemaker",
  deviceState: "Approved",
  dateFrom: "2020-01-01",
  max_results: 10
});

console.log(result);
// Returns: FDA device approvals with 510(k) number,
// decision date, product code, applicant, device name
```

### Search MAUDE Adverse Event Reports

```javascript
const result = await healthcareAgent.execute({
  action: "search_maude_reports",
  manufacturer: "Medtronic",
  deviceName: "insulin pump",
  dateFrom: "2023-01-01",
  max_results: 20
});

console.log(result);
// Returns: adverse event reports with event type,
// injury/death outcomes, manufacturer, device problems
```

### Search 510(k) Clearances

```javascript
const result = await healthcareAgent.execute({
  action: "search_510k",
  searchTerm: "coronary stent",
  productCode: "D",
  dateFrom: "2020-01-01",
  max_results: 10
});

console.log(result);
// Returns: 510(k) clearances with k_number, decision_date,
// product_code, applicant, indication_for_use
```

### Search Clinical Trials

```javascript
const result = await healthcareAgent.execute({
  action: "search_clinical_trials",
  condition: "GLP-1 agonist",
  intervention: "semaglutide",
  phase: "PHASE3",
  status: "RECRUITING",
  max_results: 10
});

console.log(result);
// Returns: clinical trials with nct_id, title, sponsor,
// phase, recruitment status, enrollment, dates
```

### Assess Medical Device Risk

```javascript
const result = await healthcareAgent.execute({
  action: "assess_medical_device_risk",
  device_name: "insulin pump",
  manufacturer: "Medtronic",
  risk_factors: ["adverse_events", "recalls", "enforcement"]
});

console.log(result);
// Returns: risk assessment with composite score (0-100),
// risk_level (LOW/MEDIUM/HIGH/CRITICAL), signal breakdown
```

### Search UDI Database

```javascript
const result = await healthcareAgent.execute({
  action: "search_udi_database",
  device_name: "pacemaker",
  max_results: 10
});

console.log(result);
// Returns: UDI records with device identifier, company,
// brand name, device type, expiration date
```

### Search Device Recalls

```javascript
const result = await healthcareAgent.execute({
  action: "search_recalls",
  recalling_firm: "Boston Scientific",
  classification: "Class I",
  dateFrom: "2022-01-01",
  max_results: 10
});

console.log(result);
// Returns: recall notices with classification, firm_name,
// product_description, recall_initiation_date, status
```

## Step 3: Chain Healthcare + Drug Intelligence

### Full Example: Medical Device Due Diligence

```javascript
import { ApifyClient } from 'apify';

const apify = new ApifyClient({ token: process.env.APIFY_API_TOKEN });

async function buildMedicalDeviceDueDiligence(deviceName, manufacturer) {
  console.log(`=== Medical Device Due Diligence: ${deviceName} ===\n`);

  // Step 1: Check FDA approvals
  console.log('[1/6] Checking FDA approval status...');
  const approvals = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'search_fda_approvals',
    searchTerm: deviceName,
    deviceState: 'Approved'
  });

  // Step 2: Search MAUDE adverse events
  console.log('[2/6] Analyzing adverse event reports...');
  const adverseEvents = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'search_maude_reports',
    manufacturer: manufacturer,
    deviceName: deviceName,
    dateFrom: '2023-01-01',
    max_results: 100
  });

  // Step 3: Check 510(k) clearances
  console.log('[3/6] Verifying 510(k) status...');
  const clearances = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'search_510k',
    searchTerm: deviceName,
    max_results: 10
  });

  // Step 4: Search recalls
  console.log('[4/6] Checking recall history...');
  const recalls = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'search_recalls',
    recalling_firm: manufacturer,
    max_results: 10
  });

  // Step 5: Assess device risk
  console.log('[5/6] Assessing risk profile...');
  const riskAssessment = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'assess_medical_device_risk',
    device_name: deviceName,
    manufacturer: manufacturer
  });

  // Step 6: Get clinical trials if applicable
  console.log('[6/6] Checking clinical trial status...');
  const trials = await apify.call('red-cars--healthcare-compliance-mcp', {
    action: 'search_clinical_trials',
    condition: deviceName,
    phase: 'PHASE3',
    max_results: 5
  });

  // Build report
  const report = {
    device: deviceName,
    manufacturer: manufacturer,
    approvals: {
      total: approvals.data?.total || 0,
      latestApproval: approvals.data?.devices?.[0] || null
    },
    adverseEvents: {
      total: adverseEvents.data?.total || 0,
      seriousEvents: adverseEvents.data?.events?.filter(e => e.mandalar === 'Death' || e.mandalar === 'Injury').length || 0
    },
    clearances: {
      total: clearances.data?.total || 0,
      kNumbers: clearances.data?.clearances?.map(c => c.k_number) || []
    },
    recalls: {
      total: recalls.data?.total || 0,
      classICount: recalls.data?.recalls?.filter(r => r.classification === 'Class I').length || 0,
      activeRecalls: recalls.data?.recalls?.filter(r => r.status === 'Ongoing').length || 0
    },
    risk: {
      score: riskAssessment.data?.risk_score || 0,
      level: riskAssessment.data?.risk_level || 'UNKNOWN',
      signals: riskAssessment.data?.signals || []
    },
    clinicalTrials: {
      phase3Count: trials.data?.total || 0,
      recruiting: trials.data?.trials?.filter(t => t.status === 'RECRUITING').length || 0
    }
  };

  console.log('\n=== DUE DILIGENCE SUMMARY ===');
  console.log(`Device: ${report.device}`);
  console.log(`Manufacturer: ${report.manufacturer}`);
  console.log(`FDA Approvals: ${report.approvals.total}`);
  console.log(`Adverse Events: ${report.adverseEvents.total} (${report.adverseEvents.seriousEvents} serious)`);
  console.log(`510(k) Clearances: ${report.clearances.total}`);
  console.log(`Recalls: ${report.recalls.total} (${report.recalls.classICount} Class I, ${report.recalls.activeRecalls} active)`);
  console.log(`Risk Score: ${report.risk.score}/100 (${report.risk.level})`);
  console.log(`Phase 3 Trials: ${report.clinicalTrials.phase3Count}`);

  return report;
}

buildMedicalDeviceDueDiligence('insulin pump', 'Medtronic').catch(console.error);
```

### Expected Output

```
=== Medical Device Due Diligence: insulin pump ===

[1/6] Checking FDA approval status...
[2/6] Analyzing adverse event reports...
[3/6] Verifying 510(k) status...
[4/6] Checking recall history...
[5/6] Assessing risk profile...
[6/6] Checking clinical trial status...

=== DUE DILIGENCE SUMMARY ===
Device: insulin pump
Manufacturer: Medtronic
FDA Approvals: 12
Adverse Events: 1,247 (89 serious)
510(k) Clearances: 8
Recalls: 2 (0 Class I, 1 active)
Risk Score: 72/100 (MEDIUM)
Phase 3 Trials: 5
```

## MCP Tool Reference

### Healthcare Compliance MCP

**Endpoint:** `red-cars--healthcare-compliance-mcp.apify.actor`

| Tool | Price | Description | Key Parameters |
|------|-------|-------------|----------------|
| `search_fda_approvals` | $0.03 | FDA device approvals | `searchTerm`, `deviceState`, `dateFrom` |
| `search_maude_reports` | $0.05 | FDA adverse event reports | `manufacturer`, `deviceName`, `dateFrom` |
| `search_510k` | $0.03 | 510(k) premarket clearances | `searchTerm`, `productCode`, `dateFrom` |
| `search_clinical_trials` | $0.05 | ClinicalTrials.gov | `condition`, `phase`, `status` |
| `assess_medical_device_risk` | $0.15 | Risk assessment with scoring | `device_name`, `manufacturer`, `risk_factors` |
| `search_udi_database` | $0.03 | Unique Device Identification | `device_name`, `company_name` |
| `search_recalls` | $0.05 | FDA device recalls | `recalling_firm`, `classification`, `dateFrom` |

### Drug Intelligence MCP

**Endpoint:** `red-cars--drug-intelligence-mcp.apify.actor`

| Tool | Price | Description | Key Parameters |
|------|-------|-------------|----------------|
| `search_drug_labels` | $0.03 | FDA drug label database | `drug_name`, `indication` |
| `get_drug_adverse_events` | $0.08 | FDA adverse event reports | `drug_name`, `seriousness`, `date_from` |
| `search_drug_recalls` | $0.03 | FDA drug recalls | `drug_name` |
| `get_drug_interactions` | $0.05 | Drug-drug interactions | `drug1`, `drug2` |

### Academic Research MCP

**Endpoint:** `academic-research-mcp.apify.actor`

| Tool | Price | Description | Key Parameters |
|------|-------|-------------|----------------|
| `search_papers` | $0.02 | Search 600M+ papers | `query`, `max_results` |
| `find_grants` | $0.03 | NIH/NSF grants | `query` |

### Tech Scouting Report MCP

**Endpoint:** `tech-scouting-report-mcp.apify.actor`

| Tool | Price | Description | Key Parameters |
|------|-------|-------------|----------------|
| `tech_scout_report` | $0.10 | Full tech scouting report | `technology`, `field`, `region` |
| `tech_scout_trl_assessment` | $0.05 | TRL evaluation | `technology`, `field` |

## Cost Summary

| MCP | Typical Query | Est. Cost |
|-----|---------------|-----------|
| healthcare-compliance-mcp | FDA approval search | ~$0.03 |
| healthcare-compliance-mcp | MAUDE adverse events | ~$0.05 |
| healthcare-compliance-mcp | Risk assessment | ~$0.15 |
| healthcare-compliance-mcp | Clinical trials | ~$0.05 |
| drug-intelligence-mcp | Drug recall search | ~$0.03 |

Full medical device due diligence (6 MCP calls): ~$0.36 per report

## Next Steps

1. Clone the [healthcare-compliance-mcp](https://github.com/red-cars-io/healthcare-compliance-mcp) repo
2. Copy `.env.example` to `.env` and add your `APIFY_API_TOKEN`
3. Run `npm install`
4. Try the examples: `node examples/device-screening.js`

## Related Repositories

- [Drug Intelligence MCP](https://github.com/red-cars-io/drug-intelligence-mcp) - FDA drug labels, adverse events, drug interactions
- [Academic Research MCP](https://github.com/red-cars-io/academic-research-mcp) - 600M+ papers, citations, author profiles
- [University Research MCP](https://github.com/red-cars-io/university-research-mcp) - Institution reports, researcher profiles
- [Patent Search MCP](https://github.com/red-cars-io/patent-search-mcp) - Patent lookup by number, citation chains
- [Tech Scouting Report MCP](https://github.com/red-cars-io/tech-scouting-report-mcp) - Technology commercialization intelligence