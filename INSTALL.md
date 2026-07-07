# Installation & Usage Guide

Complete guide for installing and invoking the Control-Aware Risk Scoring skill in Claude Code.

## What You Need

1. **Claude Code** - CLI, desktop app, or web interface at claude.ai/code
2. **Tenable access** - MCP server configured or API credentials (required)
3. **Security control access** - API access to your firewalls, EDR, WAF, etc. (optional but recommended)

## What This Skill Does

The Control-Aware Risk Scoring Engine calculates **actual exploitability** by factoring in the security controls protecting each asset. Instead of treating all critical vulnerabilities the same, it accounts for:

- Firewall rules blocking exploitation
- EDR agents preventing execution
- WAFs protecting web applications
- Network segmentation limiting lateral movement
- IDS/IPS detecting and preventing attacks

**Result:** Focus on vulnerabilities that are actually exploitable in YOUR environment, not just theoretically risky.

## Installation

### Option 1: Install from Repository (Recommended)

If this skill is available in the claude-gsd repository:

```bash
# From Claude Code prompt
/install control-aware-risk-scoring
```

The skill will be automatically installed to `~/.claude/skills/control-aware-risk-scoring/`

### Option 2: Manual Installation

1. Clone or download this repository:
   ```bash
   git clone https://github.com/dpickenstenable/control-aware-risk-scoring.git
   ```

2. Copy the skill directory so that `~/.claude/skills/control-aware-risk-scoring/SKILL.md` exists:
   ```bash
   mkdir -p ~/.claude/skills/control-aware-risk-scoring
   cp -r control-aware-risk-scoring/control-aware-risk-scoring/* ~/.claude/skills/control-aware-risk-scoring/
   ```

3. Verify installation:
   ```bash
   ls ~/.claude/skills/control-aware-risk-scoring/SKILL.md
   ```

## Pre-Installation Setup

### Step 1: Configure Tenable Access (Required)

The skill needs access to your Tenable vulnerability data.

#### Option A: Using Tenable MCP Server (Easiest)

If you have Tenable MCP server configured in `~/.claude/config.json`, the skill will automatically use it.

Verify MCP connection:
```bash
# In Claude Code
Check if my Tenable MCP server is connected
```

#### Option B: Using Tenable API Credentials

Generate API keys:
1. Login to Tenable.io → Settings → My Account → API Keys
2. Click "Generate API Key"
3. Name: "Control-Aware Risk Scoring"
4. Permissions: Read Scans, Read Assets, Read Vulnerabilities
5. Copy Access Key and Secret Key

Set environment variables:
```bash
# Add to ~/.bashrc or ~/.zshrc
export TENABLE_ACCESS_KEY="your-access-key"
export TENABLE_SECRET_KEY="your-secret-key"
export TENABLE_URL="https://cloud.tenable.com"
```

Reload shell:
```bash
source ~/.bashrc  # or ~/.zshrc
```

### Step 2: Configure Security Control Access (Optional)

The more security controls you integrate, the more accurate your risk scoring. Start with what you have and add more over time.

#### Firewall Integration

**Palo Alto Networks (Panorama):**
```bash
export PALO_ALTO_API_KEY="your-api-key"
export PALO_ALTO_HOST="panorama.company.com"
```

**Fortinet FortiGate:**
```bash
export FORTINET_API_KEY="your-api-key"
export FORTINET_HOST="fortigate.company.com"
```

**Cisco ASA/FTD:**
```bash
export CISCO_ASA_USERNAME="api-user"
export CISCO_ASA_PASSWORD="api-password"
export CISCO_ASA_HOST="asa.company.com"
```

#### EDR Integration

**CrowdStrike Falcon:**
```bash
export CROWDSTRIKE_CLIENT_ID="your-client-id"
export CROWDSTRIKE_CLIENT_SECRET="your-client-secret"
export CROWDSTRIKE_BASE_URL="https://api.crowdstrike.com"
```

**SentinelOne:**
```bash
export SENTINELONE_API_TOKEN="your-api-token"
export SENTINELONE_BASE_URL="https://your-tenant.sentinelone.net"
```

**Microsoft Defender for Endpoint:**
```bash
export DEFENDER_TENANT_ID="your-tenant-id"
export DEFENDER_CLIENT_ID="your-client-id"
export DEFENDER_CLIENT_SECRET="your-client-secret"
```

#### WAF Integration

**Cloudflare:**
```bash
export CLOUDFLARE_API_TOKEN="your-api-token"
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

**AWS WAF:**
```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_REGION="us-east-1"
```

**Azure Front Door WAF:**
```bash
export AZURE_TENANT_ID="your-tenant-id"
export AZURE_CLIENT_ID="your-client-id"
export AZURE_CLIENT_SECRET="your-client-secret"
export AZURE_SUBSCRIPTION_ID="your-subscription-id"
```

### Gradual Adoption Strategy

**You don't need all controls configured to get value!** The skill works with whatever you provide:

- **Minimum (Tenable only)**: Shows base risk scores - still useful for prioritization
- **Better (+ Firewall)**: Accounts for network-level protection
- **Great (+ EDR)**: Factors in endpoint protection
- **Excellent (+ WAF)**: Considers web application protection
- **Exceptional (All controls)**: Most accurate actual risk calculation

**Recommendation:** Start with Tenable + your primary firewall, then add more over time.

## How to Invoke the Skill

The skill runs **inside Claude Code** conversations. Invoke it with `/control-aware-risk-scoring` or describe what you want in natural language.

### Step 1: Start Claude Code

Open Claude Code in your preferred interface:
- **CLI**: Run `claude` in your terminal
- **Desktop**: Launch the Claude Code app
- **Web**: Visit claude.ai/code

### Step 2: Invoke the Skill

In the Claude Code conversation, invoke the skill:

```
/control-aware-risk-scoring
```

**Or be more specific:**

```
/control-aware-risk-scoring prioritize vulnerabilities 
by actual exploitability considering our security controls
```

> **Note**: You can invoke the skill directly with `/control-aware-risk-scoring`, or simply describe what you want to analyze in natural language and Claude Code will auto-invoke the skill for you.

### Using Opus Model for More Thorough Analysis

By default, skills run with the Sonnet model. For more comprehensive, thorough analysis, you can upgrade to the **Opus model with high effort**.

**When to use Opus:**
- Large environments (1000+ assets), multi-cloud infrastructures, quarterly risk reviews, demonstrating control effectiveness to auditors
- Need deeper analysis and more detailed recommendations
- Want exhaustive reports with step-by-step details
- Preparing for audits requiring comprehensive documentation
- First-time analysis of a new environment

**How to use Opus:**

In your Claude Code conversation, specify the model before invoking the skill:

```
Switch to Opus model

Then invoke /control-aware-risk-scoring with detailed control analysis
```

**Or use the direct command:**

```
/model opus

/control-aware-risk-scoring with detailed control analysis
```

**What changes with Opus + high effort:**
- ✅ More detailed analysis for each finding
- ✅ Deeper context about why issues matter
- ✅ More comprehensive recommendations
- ✅ Additional cross-references between related items
- ✅ More thorough validation steps
- ✅ Longer, more detailed reports (expect 2-3x length)

**Trade-offs:**
- ⏱️ Takes longer to complete (2-5x slower than Sonnet)
- 💰 Uses more tokens (higher cost)
- 📄 Generates longer reports (more to read)

**Recommendation:** Start with Sonnet for routine analysis, use Opus for quarterly deep-dives or audit preparation.

### What Happens When You Run It

1. **Connects to data sources** - Tenable + configured security controls
2. **Retrieves vulnerability data** - All assets and their vulnerabilities
3. **Fetches control data** - Firewall rules, EDR coverage, WAF protection, etc.
4. **Calculates defense factor** - For each asset based on layered controls
5. **Adjusts risk scores** - Base risk × defense factor = actual risk
6. **Generates prioritized list** - Sorted by actual exploitability
7. **Identifies control gaps** - Assets lacking adequate protection

## Complete Example Walkthrough

### Scenario: Monday morning vulnerability prioritization

**Step 1**: Start Claude Code
```bash
claude
```

**Step 2**: Invoke the skill with context
```
/control-aware-risk-scoring prioritize this week's patching.

Context:
- We have 437 assets with 1,247 vulnerabilities
- Most critical assets are in production environment
- We can patch 50 vulnerabilities this week
- Show me the top 50 by actual risk, not just severity
```

**Step 3**: Skill execution (automatic)
```
Connecting to security infrastructure...
✓ Tenable: 437 assets, 1,247 vulnerabilities
✓ Palo Alto Panorama: 47 firewall policies
✓ CrowdStrike: 342 agents (78% coverage)
✓ Cloudflare: 12 zones protected

Analyzing defense-in-depth for 437 assets...

Found:
- 156 vulnerabilities with high actual risk (weak/no controls)
- 891 vulnerabilities with low actual risk (well protected)
- 95 assets with control gaps requiring attention

Top 50 by actual risk (accounting for controls):

1. dev-web-server-42 - Apache Struts RCE
   Base Risk: 98 (VPR 9.8)
   Controls: Firewall only (5% protection)
   Actual Risk: 94.2 → URGENT - PATCH IMMEDIATELY

2. test-app-server-15 - SQL Injection
   Base Risk: 92 (VPR 9.2)
   Controls: None
   Actual Risk: 92.0 → URGENT - PATCH IMMEDIATELY

...

49. prod-payment-gateway - Apache Struts RCE
    Base Risk: 98 (VPR 9.8)
    Controls: Firewall + EDR + WAF + Segmentation (97.5% protection)
    Actual Risk: 2.5 → LOW - Patch in normal cycle

50. prod-db-primary - Oracle WebLogic RCE
    Base Risk: 95 (VPR 9.5)
    Controls: Firewall + EDR + Segmentation (95% protection)
    Actual Risk: 4.8 → LOW - Patch in normal cycle

Summary:
- 42 vulnerabilities require emergency patching (actual risk > 50)
- 8 can wait for scheduled maintenance (actual risk < 10)
- Traditional prioritization would have wasted effort on well-protected assets
```

**Step 4**: Ask follow-up questions
```
Show me detailed control analysis for dev-web-server-42
```

```
Show me all assets with control gaps (missing EDR or segmentation)
```

```
What's the estimated risk reduction if we deploy EDR to the 95 unprotected assets?
```

## Advanced Usage Examples

### Single Asset Deep Dive

```
Analyze security controls protecting prod-payment-gateway

Show me:
- All active controls (firewall, EDR, WAF, segmentation, IDS)
- Effectiveness of each control layer
- Combined defense-in-depth score
- Vulnerabilities present and their adjusted risk
```

### Control Gap Analysis

```
/control-aware-risk-scoring identify assets with weak or missing controls

Prioritize by:
1. Number of critical vulnerabilities
2. Asset importance (production > dev)
3. Current control coverage

Generate a remediation plan for deploying controls to high-risk assets.
```

### Comparison Report

```
Compare traditional vulnerability prioritization vs control-aware prioritization

Show me:
- Top 20 by VPR score
- Top 20 by actual risk (control-adjusted)
- How many are different between the two lists
- Estimated effort savings from using actual risk
```

### Investment Justification

```
Calculate ROI for deploying EDR to our 95 unprotected assets

Show:
- Current risk exposure (vulnerabilities × assets)
- Risk reduction if EDR deployed (70% effectiveness)
- Number of vulnerabilities that become low-priority
- Estimated patching effort savings
```

### Crown Jewel Protection Analysis

```
Analyze security controls protecting our crown jewel assets:
- prod-db-primary
- prod-db-secondary
- prod-payment-gateway
- prod-auth-server

For each asset, show:
- Current control layers
- Defense-in-depth score
- Critical vulnerabilities with adjusted risk
- Recommendations for additional controls
```

### Zero-Day Response

```
A new Apache Struts zero-day was just announced (VPR 9.9)

Show me:
1. All assets running Apache Struts
2. Current control protection for each
3. Actual risk ranking (accounting for controls)
4. Which need emergency patching vs can wait for controls to be updated
```

## Understanding the Output

### Control Analysis Dashboard

The skill shows protection for each asset:

```
┌─────────────────────────────────────────────────────────┐
│ Control Analysis: prod-payment-gateway                  │
├─────────────────────────────────────────────────────────┤
│ 🛡️ Firewall: Palo Alto (60% risk reduction)            │
│ 🔗 Segmentation: DMZ isolated (50% risk reduction)      │
│ 🦅 EDR: CrowdStrike Falcon (70% risk reduction)         │
│ 🌐 WAF: Cloudflare (60% risk reduction)                 │
│                                                         │
│ Combined Defense Factor: 0.0252 (97.5% protection)     │
│ Rating: ⭐⭐⭐⭐⭐ EXCEPTIONAL                           │
└─────────────────────────────────────────────────────────┘
```

### Vulnerability Prioritization

Shows vulnerabilities ranked by **actual risk**, not just severity:

```
Rank | Asset              | CVE          | Base | Controls | Actual | Priority
-----|--------------------|-----------------|------|----------|--------|----------
1    | dev-web-42         | CVE-2017-5638  | 98   | 5%       | 94.2   | URGENT
2    | test-app-15        | CVE-2021-44228 | 92   | 0%       | 92.0   | URGENT
...
49   | prod-payment-gw    | CVE-2017-5638  | 98   | 97.5%    | 2.5    | LOW
50   | prod-db-primary    | CVE-2020-2883  | 95   | 95%      | 4.8    | LOW
```

**Key insight:** Same vulnerability (CVE-2017-5638) ranks #1 and #49 based on controls!

### Control Gap Report

Identifies assets needing additional protection:

```
95 assets with control score < 40/100

Top 3 Gaps:
1. dev-web-server-42 (Score: 15/100) ⚠️⚠️⚠️
   Missing: EDR, WAF, Segmentation
   Action: Deploy EDR + isolate VLAN

2. test-app-server-15 (Score: 22/100) ⚠️⚠️
   Missing: EDR, Segmentation
   Action: Deploy EDR within 7 days

3. legacy-file-server (Score: 28/100) ⚠️
   Missing: EDR
   Action: Deploy EDR or decommission
```

## Troubleshooting

### "Skill not found" or skill does not auto-invoke

**Problem**: Skill isn't installed correctly

**Solution**:
```bash
# Verify installation
ls -la ~/.claude/skills/control-aware-risk-scoring/SKILL.md

# If missing, reinstall
git clone https://github.com/dpickenstenable/control-aware-risk-scoring.git
mkdir -p ~/.claude/skills/control-aware-risk-scoring
cp -r control-aware-risk-scoring/control-aware-risk-scoring/* ~/.claude/skills/control-aware-risk-scoring/
```

### "Tenable API Authentication Failed"

**Problem**: Invalid Tenable credentials

**Solution**:
1. Verify API keys are correct
2. Check keys haven't expired
3. Ensure keys have Read permissions
4. Test MCP connection if using MCP server

### "Unable to connect to [Firewall/EDR/WAF]"

**Problem**: Security control API credentials invalid or missing

**Solution**:
1. Check environment variables are set correctly
2. Verify API credentials are valid
3. Test connectivity: `curl https://api.crowdstrike.com` (example)
4. The skill will still work without that control - it just won't factor it into risk calculations

### "No control data found for asset"

**Problem**: Asset exists in Tenable but not in control systems

**Solution**:
- This is expected for some assets (e.g., cloud resources, BYOD)
- Skill will show base risk score without control adjustment
- Consider deploying controls to unprotected assets

### Skill runs but shows all base risk scores

**Problem**: No control integrations configured

**Solution**:
1. Check environment variables: `env | grep -E 'PALO|CROWD|CLOUD|AZURE'`
2. Set at least one control integration (firewall recommended first)
3. Verify API credentials work by testing manually
4. Restart Claude Code after setting environment variables

## Integration Examples

### AWS Environment

```bash
# Tenable for vulnerabilities
export TENABLE_ACCESS_KEY="..."
export TENABLE_SECRET_KEY="..."

# AWS Security Groups for network segmentation
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_REGION="us-east-1"

# AWS WAF for web protection
# (uses same AWS credentials)

# CrowdStrike for EDR
export CROWDSTRIKE_CLIENT_ID="..."
export CROWDSTRIKE_CLIENT_SECRET="..."
```

### Azure Environment

```bash
# Tenable
export TENABLE_ACCESS_KEY="..."
export TENABLE_SECRET_KEY="..."

# Azure NSGs for segmentation
export AZURE_TENANT_ID="..."
export AZURE_CLIENT_ID="..."
export AZURE_CLIENT_SECRET="..."
export AZURE_SUBSCRIPTION_ID="..."

# Azure Front Door WAF
# (uses same Azure credentials)

# Microsoft Defender for Endpoint
export DEFENDER_TENANT_ID="..."
export DEFENDER_CLIENT_ID="..."
export DEFENDER_CLIENT_SECRET="..."
```

### On-Premises Data Center

```bash
# Tenable
export TENABLE_ACCESS_KEY="..."
export TENABLE_SECRET_KEY="..."

# Palo Alto Panorama
export PALO_ALTO_API_KEY="..."
export PALO_ALTO_HOST="panorama.company.local"

# CrowdStrike (cloud)
export CROWDSTRIKE_CLIENT_ID="..."
export CROWDSTRIKE_CLIENT_SECRET="..."

# Cloudflare WAF (for internet-facing apps)
export CLOUDFLARE_API_TOKEN="..."
```

## Security & Privacy

### Credential Handling

- ✅ Credentials read from environment variables or MCP config
- ✅ Used only during execution, never logged
- ✅ All API calls use HTTPS/TLS encryption
- ✅ Read-only access - skill never modifies control configurations

### Best Practices

- Use read-only API keys for all integrations
- Minimum required permissions only
- Rotate credentials every 90 days
- Store credentials in secure environment (not in code)
- Use MCP server for centralized credential management

### Data Privacy

- All processing happens locally in Claude Code
- No vulnerability data sent to external services (except querying APIs)
- Control configuration data stays local
- Reports saved to local filesystem only

## Regular Maintenance

### Daily
- Review actual risk prioritization
- Patch high actual-risk vulnerabilities first

### Weekly
- Run control gap analysis
- Identify newly unprotected assets

### Monthly
- Review control effectiveness metrics
- Update environment variables if credentials rotated
- Validate control integrations still working

### Quarterly
- Audit control coverage across all assets
- Generate ROI reports for control investments
- Review and update control effectiveness assumptions

## Getting Help

- **Full Documentation**: See `README.md`
- **Algorithm Details**: See README for defense-in-depth formula
- **Supported Controls**: See README for complete list
- **Tenable API Docs**: https://developer.tenable.com/
- **Vendor API Docs**: Check your firewall/EDR/WAF vendor documentation

## Tips for Best Results

✅ **DO**:
- Start with Tenable + one control integration, add more over time
- Use read-only API credentials
- Provide context about your environment when invoking the skill
- Review control gap reports regularly
- Use actual risk for prioritization, not just VPR/CVSS

❌ **DON'T**:
- Try to configure all controls at once - start small
- Skip Tenable integration (it's required)
- Use privileged API keys when read-only works
- Ignore control gap reports - they show where you're vulnerable
- Blindly follow traditional severity scores

---

**Ready to start?**

1. Set up Tenable access (required)
2. Configure at least one security control integration (recommended)
3. Open Claude Code and invoke: `/control-aware-risk-scoring`

The skill will show you what's **actually** at risk in your environment!
