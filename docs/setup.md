# Setup Guide for Cybersecurity Threat Analysis Workflow

This guide provides detailed step-by-step instructions for configuring and deploying the cybersecurity threat analysis workflow in your n8n environment.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Initial Setup](#initial-setup)
3. [Credential Configuration](#credential-configuration)
4. [Node-Specific Configuration](#node-specific-configuration)
5. [Testing](#testing)
6. [Production Deployment](#production-deployment)
7. [Troubleshooting](#troubleshooting)

## Prerequisites

Before starting, ensure you have:

- ✅ n8n instance running (version 1.0+ with LangChain nodes)
- ✅ OpenAI API account with GPT-4o access
- ✅ SMTP email server (for alert delivery)
- ✅ Security log source (SIEM, log management system, or test endpoint)
- ✅ Basic understanding of n8n workflow editor

## Initial Setup

### Step 1: Import the Workflow

1. Download the workflow file from this repository:
   ```
   workflow/cybersecurity-threat-analysis-workflow.json
   ```

2. Open your n8n instance in a browser

3. Click the "Import from File" button (usually in the top-right corner)

4. Select the downloaded JSON file

5. The workflow will load with all nodes and connections

### Step 2: Verify Workflow Structure

After import, verify the following nodes are present:

**Core Nodes:**
- Start Threat Analysis (Manual Trigger)
- Cybersecurity Orchestrator Agent
- Orchestrator Chat Model
- Structured Threat Report Parser
- Route by Risk Severity

**Threat Intelligence Nodes:**
- Threat Intelligence Agent
- Threat Intelligence Chat Model
- Fetch Security Logs Tool
- Risk Score Calculator

**Attack Surface Mapping Nodes:**
- Attack Surface Mapping Agent
- Attack Surface Chat Model
- STRIDE Analysis Tool
- CVSS Scoring Tool

**Reporting Nodes:**
- Format SOC Alert
- Format Executive Report
- Format Standard Report
- Send an Email

## Credential Configuration

### OpenAI API Credentials

You need to configure OpenAI credentials for three Chat Model nodes:

1. **Orchestrator Chat Model**
2. **Threat Intelligence Chat Model**
3. **Attack Surface Chat Model**

#### Steps:

1. Click on the "Orchestrator Chat Model" node

2. In the "Credentials" section, click "Create New"

3. Select "OpenAI API" from the credential types

4. Enter your OpenAI API key:
   - Get your API key from: https://platform.openai.com/api-keys
   - Copy the key (starts with `sk-...`)
   - Paste it into the "API Key" field

5. Name the credential (e.g., "OpenAI - GPT-4o")

6. Click "Save"

7. Repeat for the other two Chat Model nodes, selecting the same credential

#### Model Configuration (Already Set):

- Model: `gpt-4o`
- Temperature: `0.3` (Orchestrator), `0.2` (Specialist agents)
- These settings balance creativity with consistency

### SMTP Email Credentials

Configure email delivery for the "Send an Email" node:

1. Click on the "Send an Email" node

2. In the "Credentials" section, click "Create New"

3. Select "SMTP" from the credential types

4. Configure your SMTP server:
   ```
   Host: smtp.your-provider.com
   Port: 587 (for TLS) or 465 (for SSL)
   User: your-email@example.com
   Password: your-email-password-or-app-specific-password
   ```

5. Test the connection (if available in your n8n version)

6. Click "Save"

7. Update the email addresses in the node parameters:
   - From: `your-email@example.com`
   - To: `recipient@example.com`

#### Common SMTP Providers:

**Gmail:**
- Host: `smtp.gmail.com`
- Port: `587`
- Note: Use App Password if 2FA is enabled

**Outlook/Office 365:**
- Host: `smtp.office365.com`
- Port: `587`

**SendGrid:**
- Host: `smtp.sendgrid.net`
- Port: `587`

## Node-Specific Configuration

### Fetch Security Logs Tool

This node retrieves security logs from your SIEM or log source.

#### For Testing (Default Configuration):

The workflow comes pre-configured with:
```
URL: https://httpbin.org/anything?event=failed_login&source_ip=185.220.101.1&severity=high&service=ssh
```

This is a public test endpoint that simulates log data. Use this for initial testing.

#### For Production:

1. Click on the "Fetch Security Logs Tool" node

2. Update the URL to your SIEM API endpoint:
   ```
   Example: https://your-siem.example.com/api/v1/logs
   ```

3. Add authentication if required:
   - In "Authentication" dropdown, select your method
   - Common methods: Header Auth, Generic Credential Type, OAuth2
   - Enter API key or token as needed

4. Configure request parameters:
   - Add query parameters in "Query Parameters" section
   - Add headers in "Headers" section if needed
   - Set "Method" to GET or POST as required by your API

5. Configure "Options" if needed:
   - Response format (JSON, XML, etc.)
   - Timeout settings
   - SSL verification

#### Example SIEM Integrations:

**Splunk:**
```
URL: https://splunk.example.com:8089/services/search/jobs/export
Authentication: Header Auth (Bearer token)
Headers: Authorization: Bearer YOUR_TOKEN
```

**Elasticsearch:**
```
URL: https://elasticsearch.example.com/_search
Method: POST
Body: {"query": {"match_all": {}}}
```

### Risk Threshold Configuration

Configure routing thresholds in the "Route by Risk Severity" node:

1. Click on "Route by Risk Severity" node

2. Review the three routing rules:
   - **Rule 1**: `overall_risk_rating` equals "Critical"
   - **Rule 2**: `overall_risk_rating` equals "High"
   - **Rule 3**: `overall_risk_rating` equals "Medium"

3. To modify thresholds:
   - Change the comparison values
   - Add or remove rules as needed
   - Adjust the operator (equals, contains, etc.)

4. Default routing:
   - Critical → Format SOC Alert → Send Email
   - High → Format Executive Report → Send Email
   - Medium → Format Standard Report

### STRIDE Analysis Tool

This tool categorizes threats using STRIDE methodology. It's pre-configured with JavaScript code and typically doesn't need modification.

If you want to customize the categorization logic:

1. Click on "STRIDE Analysis Tool" node

2. Review the JavaScript code in the "JS Code" field

3. Modify the keyword matching logic as needed:
   - Add new keywords for each STRIDE category
   - Adjust the matching logic
   - Add custom categorization rules

### CVSS Scoring Tool

This tool calculates CVSS-style risk scores. It's pre-configured with the CVSS v3.1 calculation formula.

To customize:

1. Click on "CVSS Scoring Tool" node

2. Review the JavaScript code in the "JS Code" field

3. Adjust scoring parameters if needed:
   - Attack vector weights
   - Impact calculations
   - Severity thresholds

## Testing

### Step 1: Test with Manual Trigger

1. Click on the "Start Threat Analysis" node

2. Click "Execute Workflow" button in the top-right

3. Watch the execution progress:
   - Check each node completes successfully
   - Review execution results
   - Look for error messages

### Step 2: Verify AI Agent Responses

After execution, check the outputs:

1. Click on "Cybersecurity Orchestrator Agent" node

2. View the output in the execution panel

3. Verify:
   - Executive summary is generated
   - Threat intelligence is included
   - Attack surface analysis is present
   - Risk rating is assigned (Critical, High, or Medium)

### Step 3: Verify Report Formatting

Check each report format node:

1. Click on "Format SOC Alert" node (if routed there)
2. Verify the output structure includes:
   - alert_type
   - timestamp
   - risk_rating
   - executive_summary
   - soc_actions
   - threat_vectors
   - attack_surface
   - immediate_actions_required

3. Repeat for "Format Executive Report" and "Format Standard Report"

### Step 4: Test Email Delivery

If email is configured:

1. Execute the workflow

2. Check your email inbox (and spam folder)

3. Verify the email contains:
   - Subject with risk rating
   - Risk rating value
   - CVSS score
   - Threat count
   - Executive summary
   - High priority actions
   - Timestamp

### Step 5: Test with Real Security Logs

After successful testing with the default endpoint:

1. Configure "Fetch Security Logs Tool" with your real SIEM endpoint

2. Execute the workflow again

3. Verify that:
   - Security logs are fetched successfully
   - AI agents analyze the real data
   - CVSS scores are calculated based on actual threats
   - Reports reflect your actual security posture

## Production Deployment

### Step 1: Change Trigger Type

For automated execution, replace the manual trigger:

**Option A: Webhook Trigger**
1. Delete "Start Threat Analysis" node
2. Add "Webhook" node
3. Configure webhook path (e.g., `/security-analysis`)
4. Set HTTP method (POST recommended)
5. Configure authentication if needed
6. Connect webhook to "Cybersecurity Orchestrator Agent"

**Option B: Schedule Trigger**
1. Delete "Start Threat Analysis" node
2. Add "Schedule Trigger" node
3. Set interval (e.g., daily at 9 AM)
4. Configure timezone
5. Connect to "Cybersecurity Orchestrator Agent"

**Option C: Event Trigger**
1. Delete "Start Threat Analysis" node
2. Add "Event Trigger" node
3. Configure event source (e.g., n8n events, external webhooks)
4. Set event filters
5. Connect to "Cybersecurity Orchestrator Agent"

### Step 2: Activate the Workflow

1. Click the "Active" toggle in the top-right corner
2. Confirm activation
3. The workflow will now run based on the trigger type

### Step 3: Monitor Execution

1. Go to "Executions" in n8n
2. Monitor workflow runs
3. Check for errors or failures
4. Review execution times
5. Monitor API usage (OpenAI costs)

### Step 4: Set Up Alerts (Optional)

Configure n8n to alert you on workflow failures:

1. Go to workflow settings
2. Configure error notifications
3. Set up email or Slack alerts for failures
4. Configure retry logic if needed

## Troubleshooting

### Issue: Workflow Won't Execute

**Symptoms:** Clicking execute does nothing or shows error

**Solutions:**
- Check n8n server logs
- Verify all credentials are configured
- Ensure nodes are properly connected
- Check for syntax errors in code nodes
- Restart n8n if needed

### Issue: OpenAI API Errors

**Symptoms:** "Authentication failed" or "Rate limit exceeded"

**Solutions:**
- Verify API key is valid
- Check OpenAI account has credits
- Monitor usage at https://platform.openai.com/usage
- Implement rate limiting if needed
- Consider using different models (GPT-4-turbo) for cost savings

### Issue: Email Not Sending

**Symptoms:** No email received after execution

**Solutions:**
- Verify SMTP credentials are correct
- Test SMTP connection separately
- Check email server logs
- Verify recipient email address
- Check spam folder
- Ensure firewall allows SMTP traffic

### Issue: Security Log Fetch Fails

**Symptoms:** "Fetch Security Logs Tool" returns error

**Solutions:**
- Test API endpoint with curl or Postman
- Verify API credentials
- Check network connectivity to SIEM
- Review API rate limits
- Check for CORS issues (if calling from browser)
- Verify SSL certificates

### Issue: Structured Output Parsing Fails

**Symptoms:** Parser fails to extract structured data

**Solutions:**
- Lower temperature settings (0.1-0.2)
- Review the output schema in parser node
- Check if AI response matches expected format
- Add more specific instructions in system prompts
- Consider using different output parser type

### Issue: Wrong Risk Routing

**Symptoms:** Reports routed to wrong format

**Solutions:**
- Verify field names in routing conditions
- Check that risk rating values match exactly
- Test with known values to verify routing
- Review switch node logic
- Add debug nodes to inspect values

### Issue: High API Costs

**Symptoms:** OpenAI usage costs are high

**Solutions:**
- Monitor usage regularly
- Reduce execution frequency
- Use smaller models for less critical tasks
- Implement caching for repeated analyses
- Consider self-hosted LLMs for sensitive data

## Additional Configuration

### Customizing System Prompts

To modify AI agent behavior:

1. Click on the agent node (e.g., "Threat Intelligence Agent")

2. In "Options", find "System Message"

3. Modify the system prompt to:
   - Change analysis focus
   - Add specific requirements
   - Adjust output format expectations
   - Include domain-specific knowledge

4. Save and test

### Adding Custom Tools

To add new analysis tools:

1. Add a new tool node (Code, HTTP Request, Calculator, etc.)

2. Configure the tool functionality

3. Connect it to the appropriate agent node
   - Use "ai_tool" connection type
   - Connect from tool to agent

4. Update the agent's tool description if needed

### Integrating Additional Outputs

To add more output formats:

1. Add new "Set" nodes for formatting

2. Connect them from "Route by Risk Severity" node
   - Add new routing rules
   - Connect each rule to new format node

3. Add additional output nodes (Slack, Jira, database, etc.)

4. Test the new outputs

## Security Best Practices

1. **Credential Management**
   - Use n8n's credential vault
   - Never hardcode credentials in workflows
   - Rotate credentials regularly
   - Use read-only API keys where possible

2. **Data Privacy**
   - Review data sent to OpenAI
   - Anonymize sensitive data before processing
   - Consider on-premise LLMs for highly sensitive data
   - Implement data retention policies

3. **Access Control**
   - Restrict workflow editor access
   - Use n8n authentication
   - Implement role-based access
   - Audit workflow changes

4. **Monitoring**
   - Set up execution monitoring
   - Alert on failures
   - Review logs regularly
   - Track API usage and costs

## Support

For additional help:

- Review n8n documentation: https://docs.n8n.io/
- Check OpenAI API docs: https://platform.openai.com/docs/
- Open an issue on GitHub
- Contact the workflow author

---

**Last Updated:** 2026-10-12
**Workflow Version:** 1.0
