# Cybersecurity Threat Analysis Workflow with GPT-4o

An automated n8n workflow that performs comprehensive cybersecurity threat analysis using AI-powered multi-agent architecture, CVSS scoring, and STRIDE methodology for risk-based alert routing.

## 🎯 Project Overview

This workflow automates end-to-end cybersecurity threat analysis using a multi-agent AI architecture, targeting Security Operations Centre (SOC) analysts, security engineers, and IT risk teams responsible for continuous threat monitoring and incident response. The core problem it solves is the slow, fragmented process of manually correlating threat intelligence, scoring vulnerabilities, and producing actionable reports—tasks that demand both speed and consistency under pressure.

## 📋 Problem Statement

Organizations face increasing pressure to:
- Rapidly analyze security logs and identify emerging threats
- Apply systematic threat modeling methodologies (STRIDE)
- Quantify risk using industry-standard CVSS scoring
- Generate appropriate reports for different stakeholders (SOC, executives, standard operations)
- Route alerts based on severity levels

Manual threat analysis is time-consuming, error-prone, and difficult to scale. This workflow addresses these challenges by automating the entire process from threat detection to report generation.

## ✨ Key Features

- **Multi-Agent AI Architecture**: Orchestrates specialized agents for threat intelligence and attack surface mapping
- **STRIDE Methodology**: Systematically identifies threats across six categories (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege)
- **CVSS Scoring**: Calculates industry-standard risk scores (0-10 scale) for threat scenarios
- **Risk-Based Routing**: Automatically routes reports based on severity (Critical, High, Medium)
- **Structured Output Parsing**: Generates consistent, machine-readable threat reports
- **Multi-Format Reporting**: Produces SOC alerts, executive summaries, and standard assessments
- **Email Notifications**: Delivers alerts to stakeholders automatically

## 🔄 How the Workflow Works

### Execution Flow

```
Manual Trigger
    ↓
Cybersecurity Orchestrator Agent (GPT-4o)
    ↓
├── Threat Intelligence Agent
│   ├── Fetch Security Logs Tool
│   └── Risk Score Calculator
│
└── Attack Surface Mapping Agent
    ├── STRIDE Analysis Tool
    └── CVSS Scoring Tool
    ↓
Structured Threat Report Parser
    ↓
Route by Risk Severity
    ↓
├── Critical → Format SOC Alert → Send Email
├── High → Format Executive Report → Send Email
└── Medium → Format Standard Report
```

### Workflow Execution Steps

1. **Trigger**: Manual trigger initiates the workflow with an analysis request
2. **Orchestration**: Cybersecurity Orchestrator Agent coordinates two specialist sub-agents
3. **Threat Intelligence**: Threat Intelligence Agent analyzes security logs, authentication traces, and anomaly detection outputs
4. **Attack Surface Mapping**: Attack Surface Mapping Agent applies STRIDE methodology and CVSS scoring
5. **Structured Parsing**: Output is parsed into a structured threat report format
6. **Risk Routing**: Reports are routed based on overall risk rating (Critical, High, Medium)
7. **Report Formatting**: Reports are formatted for appropriate audiences (SOC, Executive, Standard)
8. **Notification**: Email alerts are sent to configured recipients

## 🏗️ Architecture

### Agent Components

**Cybersecurity Orchestrator Agent**
- Coordinates threat intelligence analysis and attack surface mapping
- Synthesizes findings into comprehensive threat assessments
- Provides both SOC-level operational guidance and executive-level reporting

**Threat Intelligence Agent**
- Analyzes security logs for suspicious patterns and anomalies
- Identifies authentication trace irregularities
- Classifies emerging attack vectors by severity
- Provides actionable threat intelligence with indicators of compromise

**Attack Surface Mapping Agent**
- Constructs network topology graphs
- Applies STRIDE methodology for threat identification
- Models lateral movement scenarios
- Quantifies risk using CVSS-style scoring

### Tools

- **Fetch Security Logs Tool**: Retrieves security logs from SIEM or log sources
- **Risk Score Calculator**: Performs risk calculations for threat prioritization
- **STRIDE Analysis Tool**: Categorizes threats using STRIDE methodology
- **CVSS Scoring Tool**: Calculates CVSS-style risk scores (0-10 scale)

## 🛠️ Technologies and Integrations

- **n8n**: Workflow automation platform
- **OpenAI GPT-4o**: AI model for threat analysis and orchestration
- **LangChain**: AI agent framework integration
- **SMTP**: Email delivery for alerts
- **HTTP Requests**: Security log fetching from SIEM APIs

## 📦 Prerequisites

### Required Software

- n8n instance (self-hosted or n8n Cloud)
- n8n version with LangChain nodes support

### Required Services

- OpenAI API account with GPT-4o access
- SMTP email server for alert delivery
- SIEM or security log source with API access (optional - can use httpbin.org for testing)

### Required Knowledge

- Basic understanding of n8n workflow configuration
- Familiarity with cybersecurity concepts (STRIDE, CVSS)
- Credential management in n8n

## 🚀 Installation and Import Instructions

### Step 1: Download the Workflow

Clone or download this repository:
```bash
git clone https://github.com/harishwani18/cybersecurity-threat-analysis-workflow.git
cd cybersecurity-threat-analysis-workflow
```

### Step 2: Import into n8n

1. Open your n8n instance
2. Click "Import from File" or drag-and-drop the workflow file
3. Select `workflow/cybersecurity-threat-analysis-workflow.json`
4. The workflow will be imported with all nodes and connections preserved

### Step 3: Verify Import

Check that all nodes are present:
- Start Threat Analysis (Manual Trigger)
- Cybersecurity Orchestrator Agent
- Orchestrator Chat Model
- Structured Threat Report Parser
- Threat Intelligence Agent
- Threat Intelligence Chat Model
- Fetch Security Logs Tool
- Risk Score Calculator
- Attack Surface Mapping Agent
- Attack Surface Chat Model
- STRIDE Analysis Tool
- CVSS Scoring Tool
- Route by Risk Severity
- Format SOC Alert
- Format Executive Report
- Format Standard Report
- Send an Email

## ⚙️ Configuration and Credentials Setup

### OpenAI API Configuration

1. Navigate to each Chat Model node:
   - Orchestrator Chat Model
   - Threat Intelligence Chat Model
   - Attack Surface Chat Model

2. Configure credentials:
   - Click on "Credentials" → "Create New"
   - Select "OpenAI API"
   - Enter your OpenAI API key
   - Save and apply to all Chat Model nodes

3. Model settings (already configured):
   - Model: gpt-4o
   - Temperature: 0.2-0.3 (for consistent outputs)

### SMTP Email Configuration

1. Navigate to "Send an Email" node
2. Click on "Credentials" → "Create New"
3. Select "SMTP"
4. Configure your SMTP server:
   - Host: your SMTP server address
   - Port: typically 587 (TLS) or 465 (SSL)
   - Username: your email address
   - Password: your email password or app-specific password
5. Update email addresses in node parameters:
   - From: your sender email
   - To: recipient email address

### Security Log Source Configuration

**For Testing (Default)**:
- The workflow uses `httpbin.org/anything` as a placeholder
- This simulates security log data for testing purposes

**For Production**:
1. Navigate to "Fetch Security Logs Tool" node
2. Update the URL to your SIEM or log source API endpoint
3. Add authentication if required (API key, OAuth, etc.)
4. Configure request headers and parameters as needed

### Risk Threshold Configuration

The workflow routes reports based on these thresholds (configurable in "Route by Risk Severity" node):
- **Critical**: CVSS ≥ 9.0 → SOC Alert
- **High**: CVSS ≥ 7.0 → Executive Report
- **Medium**: CVSS ≥ 4.0 → Standard Report

To adjust thresholds:
1. Open "Route by Risk Severity" node
2. Modify the conditions in the rules
3. Update risk rating comparisons as needed

## 🎮 How to Execute and Activate the Workflow

### Manual Execution

1. Open the workflow in n8n editor
2. Click "Execute Workflow" button
3. The workflow will run and generate a threat analysis report
4. Check the execution results for formatted outputs

### Active Mode (Optional)

1. Change the trigger from "Manual Trigger" to:
   - Webhook trigger (for API-based activation)
   - Schedule trigger (for periodic analysis)
   - Event trigger (for log-based activation)
2. Configure trigger parameters as needed
3. Activate the workflow

### Testing the Workflow

1. Start with the manual trigger
2. Use the default httpbin.org endpoint for initial testing
3. Verify that:
   - AI agents generate responses
   - CVSS scores are calculated
   - STRIDE analysis is performed
   - Reports are formatted correctly
   - Email is sent (if configured)
4. Once verified, connect to your real security log source

## 📊 Example Use Cases

### Use Case 1: Automated Vulnerability Triage
- Trigger workflow when new CVE is disclosed
- Automatically analyze impact using CVSS
- Route critical vulnerabilities to SOC team immediately
- Generate executive summary for CISO review

### Use Case 2: Periodic Security Posture Assessment
- Schedule workflow to run weekly
- Analyze security logs from the past week
- Identify emerging attack patterns
- Generate comprehensive threat assessment report

### Use Case 3: Incident Response Support
- Trigger workflow during security incident
- Analyze related logs and authentication traces
- Model lateral movement scenarios
- Provide prioritized action items for responders

### Use Case 4: Compliance Reporting
- Run workflow before compliance audits
- Generate structured threat assessments
- Document risk quantification with CVSS scores
- Produce executive-ready posture reports

## 🔧 Error Handling and Troubleshooting

### Common Issues

**Issue: OpenAI API Authentication Failed**
- Solution: Verify your API key is valid and has sufficient credits
- Check that credentials are applied to all Chat Model nodes

**Issue: SMTP Email Not Sending**
- Solution: Verify SMTP server settings and credentials
- Check firewall rules for SMTP port access
- Ensure email addresses are correctly formatted

**Issue: Security Log Fetch Fails**
- Solution: Verify API endpoint is accessible
- Check authentication credentials
- Test API endpoint separately (e.g., with curl or Postman)

**Issue: Structured Output Parsing Fails**
- Solution: Check that the output schema matches the AI response
- Adjust temperature settings for more consistent outputs
- Review the JSON schema in the Structured Threat Report Parser node

**Issue: Risk Routing Not Working**
- Solution: Verify the field names in routing conditions match the actual output
- Check that risk rating values match expected strings (Critical, High, Medium)
- Test with known risk rating values

### Debugging Tips

1. Enable execution history in n8n to review step-by-step outputs
2. Use the "Execute Node" feature to test individual nodes
3. Check n8n execution logs for error messages
4. Verify API credentials using external tools first
5. Test with simplified prompts to isolate issues

## ⚠️ Known Limitations

1. **AI Model Costs**: Using GPT-4o incurs API costs; monitor usage
2. **API Rate Limits**: OpenAI has rate limits that may affect high-frequency executions
3. **Email Dependencies**: SMTP configuration required for email notifications
4. **Log Source Integration**: Custom integration may be needed for specific SIEM platforms
5. **Structured Output**: AI models may occasionally generate outputs that don't perfectly match the schema
6. **Single-threaded Execution**: Workflow processes one analysis request at a time

## 🔒 Security Considerations

### Credential Management

- Never commit actual API keys or credentials to version control
- Use n8n's credential management system
- Rotate credentials regularly
- Use least-privilege access for API keys

### Data Privacy

- The workflow sends security log data to OpenAI API
- Review OpenAI's data retention and privacy policies
- Consider using self-hosted LLMs for sensitive environments
- Anonymize or redact sensitive data before processing if needed

### Access Control

- Restrict access to the n8n workflow editor
- Implement proper authentication for n8n instance
- Use role-based access control for n8n users
- Audit workflow execution logs regularly

### Email Security

- Use TLS/SSL for SMTP connections
- Consider encrypting email content for highly sensitive reports
- Implement email filtering to prevent spoofing
- Regularly review email recipients and update as needed

## 🚀 Future Improvements

Potential enhancements for future versions:

1. **Additional Integrations**
   - Slack/Teams notifications
   - Jira/ServiceNow ticket creation
   - Threat intelligence feed integration (MITRE ATT&CK, VirusTotal)

2. **Enhanced Analysis**
   - Integration with more SIEM platforms (Splunk, QRadar, SentinelOne)
   - Support for custom threat models beyond STRIDE
   - Historical trend analysis and anomaly detection

3. **Reporting Improvements**
   - PDF report generation
   - Dashboard integration (Grafana, Kibana)
   - Customizable report templates

4. **Performance Optimizations**
   - Batch processing for multiple threats
   - Caching of common analysis results
   - Parallel execution of independent analysis tasks

5. **Additional Triggers**
   - Webhook-based integration with SIEM alerts
   - Scheduled periodic assessments
   - Event-driven triggers from security tools

## 🤝 Contribution Guidelines

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes (ensure they don't include credentials)
4. Test the workflow thoroughly
5. Commit your changes with descriptive messages
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Contribution Standards

- Maintain the existing code style and structure
- Update documentation for any new features
- Test with the default configuration before submitting
- Ensure no sensitive data is included in commits
- Follow semantic versioning for workflow updates

## 📄 License

This project is provided as-is for educational and professional use. Please ensure you have appropriate licenses for:
- n8n (check n8n licensing terms)
- OpenAI API usage
- Any third-party integrations you add

## 👤 Author and Contact

**Author**: Harish Wani
**GitHub**: [@harishwani18](https://github.com/harishwani18)

For questions, issues, or collaboration opportunities:
- Open an issue on GitHub
- Contact via email (see profile)

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Documentation](https://platform.openai.com/docs/)
- [STRIDE Methodology](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-threat-stride)
- [CVSS Scoring](https://www.first.org/cvss/)
- [LangChain Documentation](https://python.langchain.com/)

## 🙏 Acknowledgments

- n8n team for the excellent workflow automation platform
- OpenAI for GPT-4o API
- LangChain team for the agent framework
- Cybersecurity community for STRIDE and CVSS standards

---

**Note**: This workflow is a template that requires configuration with your specific credentials and systems. Always test thoroughly in a non-production environment before deploying to production.
