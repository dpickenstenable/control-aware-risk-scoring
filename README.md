# Control-Aware Risk Scoring Engine

**Transform vulnerability management from theoretical risk to actual risk**

The Control-Aware Risk Scoring Engine calculates **true exploitability** by factoring in the security controls protecting each asset. It integrates with firewalls, EDR platforms, WAFs, and network topology to determine what's actually at risk, not just what's theoretically vulnerable.

## 🚀 New User? Start Here

**[→ Installation & Usage Guide (INSTALL.md)](INSTALL.md)** - Complete walkthrough for installing and invoking this skill in Claude Code

Already installed? Continue with Quick Start below.

---

## 🎯 The Problem

Traditional vulnerability prioritization uses generic scores (VPR, CVSS) that don't account for YOUR defenses:

- **VPR 9.8** on a server behind firewall + EDR + WAF + network segmentation
- **VPR 9.8** on an unprotected dev server in a flat network

**Are these the same risk?** ❌ **NO!**

Traditional tools treat them equally. This skill calculates **actual risk**.

---

## 💡 How It Works

### Defense-in-Depth Formula

```
Actual_Risk = Base_Technical_Risk × Defense_Factor

Defense_Factor = Firewall × Segmentation × EDR × WAF × IDS

Example:
- Base Risk: 98 (VPR 9.8, critical)
- Firewall: 40% reduction → 0.6 remaining
- Segmentation: 50% reduction → 0.5 remaining
- EDR: 70% reduction → 0.3 remaining
- WAF: 60% reduction → 0.4 remaining
- IDS: 30% reduction → 0.7 remaining

Defense_Factor = 0.6 × 0.5 × 0.3 × 0.4 × 0.7 = 0.0252

Actual_Risk = 98 × 0.0252 = 2.47

Result: CRITICAL vuln becomes LOW priority (97.5% protected by controls)
```

---

## 🛡️ Supported Security Controls

### Firewalls
- ✅ Palo Alto Networks (Panorama API)
- ✅ Fortinet FortiGate (REST API)
- ✅ Cisco ASA/FTD (REST API)
- ✅ Check Point (Management API)
- ✅ pfSense (API)

### Endpoint Detection & Response (EDR)
- ✅ CrowdStrike Falcon (FalconPy SDK)
- ✅ SentinelOne (REST API)
- ✅ Microsoft Defender for Endpoint (Graph API)
- ✅ Carbon Black (REST API)
- ✅ Cisco AMP (AMP API)

### Web Application Firewalls (WAF)
- ✅ Cloudflare (REST API)
- ✅ AWS WAF (boto3)
- ✅ Azure Front Door WAF (Azure SDK)
- ✅ Imperva/Incapsula (API)
- ✅ Akamai Kona (OPEN API)

### Network Controls
- ✅ Network segmentation analysis (VLAN, routing tables)
- ✅ Cloud security groups (AWS VPC, Azure NSG, GCP Firewall)
- ✅ Lateral movement potential calculation
- ✅ Crown jewel reachability assessment

### Intrusion Detection/Prevention
- ✅ Snort (API)
- ✅ Suricata (REST API)
- ✅ Cisco Firepower (FMC API)
- ✅ Palo Alto Threat Prevention

---

## 🚀 Quick Start

### Installation

See [INSTALL.md](INSTALL.md) for detailed installation instructions.

### Usage

Open Claude Code and invoke the skill:

```
/control-aware-risk-scoring
```

**Or be more specific:**
```
/control-aware-risk-scoring prioritize vulnerabilities 
by actual exploitability considering our security controls
```

> **Note**: The skill is invoked with `/control-aware-risk-scoring`. You can also just describe what you want in natural language and Claude Code will auto-invoke the skill.

**What happens:**
```
Connecting to your security infrastructure...
✓ Tenable: 437 assets, 1,247 vulnerabilities
✓ Palo Alto: 47 firewall policies
✓ CrowdStrike: 342 agents (78% coverage)
✓ Cloudflare: 12 zones protected

What would you like to see?
1. Show me assets with weak controls
2. Prioritize vulnerabilities by actual risk
3. Analyze controls for prod-db-01
4. Compare traditional vs control-aware prioritization

You: "Prioritize vulnerabilities by actual risk"
```

---

## 💡 Example Queries

### Single Asset Analysis
```
"Analyze security controls for prod-db-01"
```

Shows all controls protecting that asset and combined effectiveness.

### Control Gap Analysis
```
"Show me assets with weak or missing controls"
```

Identifies under-protected assets requiring control deployment.

### Control-Adjusted Prioritization
```
"Prioritize vulnerabilities by actual risk"
```

Ranks vulnerabilities by actual exploitability, not just severity.

### Comparison Analysis
```
"Compare traditional prioritization vs control-aware"
```

Shows how much priorities change when controls are factored in.

---

## 📊 Real-World Example

**Scenario:** Same vulnerability (Apache Struts RCE, VPR 9.8) on two assets

### Asset 1: dev-web-server-42

**Controls:**
- ✅ Firewall (basic rules only)
- ❌ No EDR
- ❌ No WAF
- ❌ Flat network (no segmentation)

**Analysis:**
```
Base Risk: 98
Control Protection: 5%
Adjusted Risk: 94.2
Priority: URGENT - PATCH IMMEDIATELY (4-hour SLA)
```

### Asset 2: prod-payment-gateway

**Controls:**
- ✅ Palo Alto firewall (47 restrictive rules)
- ✅ CrowdStrike EDR (healthy agent)
- ✅ Cloudflare WAF (OWASP rules enabled)
- ✅ Network segmentation (isolated DMZ)
- ✅ Snort IDS (up-to-date signatures)

**Analysis:**
```
Base Risk: 98
Control Protection: 97.5%
Adjusted Risk: 2.5
Priority: LOW - Patch in normal cycle (7-14 days OK)
```

**Traditional prioritization would treat these identically (both VPR 9.8).**

**Control-aware prioritization correctly identifies the real risk difference.**

---

## 🎨 Output Examples

### Control Analysis Dashboard

```
┌─────────────────────────────────────────────────────────┐
│ Control Analysis: prod-payment-gateway                  │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ 🛡️ Firewall Protection                                  │
│ ├─ Vendor: Palo Alto Networks                           │
│ ├─ Rules: 47 total, 12 restrictive                      │
│ ├─ Effectiveness: 60% risk reduction                    │
│ └─ Status: ✅ STRONG                                     │
│                                                         │
│ 🔗 Network Segmentation                                  │
│ ├─ VLAN: Payment-DMZ                                     │
│ ├─ Crown Jewel Access: Denied                           │
│ ├─ Reachable Assets: 8                                   │
│ ├─ Effectiveness: 50% risk reduction                    │
│ └─ Status: ✅ EXCELLENT                                  │
│                                                         │
│ 🦅 Endpoint Protection (EDR)                             │
│ ├─ Vendor: CrowdStrike Falcon                           │
│ ├─ Agent: v7.10.0 (healthy)                             │
│ ├─ Effectiveness: 70% risk reduction                    │
│ └─ Status: ✅ STRONG                                     │
│                                                         │
│ 🌐 Web Application Firewall                              │
│ ├─ Vendor: Cloudflare                                    │
│ ├─ Rules: 156 (OWASP managed)                            │
│ ├─ Effectiveness: 60% risk reduction                    │
│ └─ Status: ✅ STRONG                                     │
│                                                         │
│ ────────────────────────────────────────────────────────│
│ Combined Defense-in-Depth                               │
│ ├─ Defense Factor: 0.0252 (97.5% protection)            │
│ ├─ Control Layers: 4                                     │
│ └─ Rating: ⭐⭐⭐⭐⭐ EXCEPTIONAL                         │
└─────────────────────────────────────────────────────────┘
```

### Control Gap Report

```
┌─────────────────────────────────────────────────────────┐
│ Control Gap Analysis - PRIORITY REMEDIATION             │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ 95 assets with control score < 40/100                   │
│                                                         │
│ Top 3 Gaps:                                             │
│                                                         │
│ 1. dev-web-server-42 (Score: 15/100) ⚠️⚠️⚠️              │
│    ├─ Missing: EDR, WAF, Segmentation                   │
│    ├─ Present: Basic firewall only                      │
│    ├─ Vulnerabilities: 47 critical, 89 high             │
│    └─ Action: Deploy EDR + isolate VLAN                 │
│                                                         │
│ 2. test-app-server-15 (Score: 22/100) ⚠️⚠️               │
│    ├─ Missing: EDR, Segmentation                        │
│    ├─ Present: Firewall + IDS                           │
│    ├─ Vulnerabilities: 31 critical, 54 high             │
│    └─ Action: Deploy EDR within 7 days                  │
│                                                         │
│ Recommended Actions:                                    │
│ 1. Deploy EDR to 42 unprotected assets                  │
│ 2. Implement segmentation for 28 flat-network assets    │
│ 3. Add WAF to 15 internet-facing web servers            │
│                                                         │
│ Estimated Risk Reduction: 78% if all gaps closed        │
└─────────────────────────────────────────────────────────┘
```

---

## 🔧 Configuration

### Environment Variables

```bash
# Tenable (required)
export TENABLE_ACCESS_KEY="your-access-key"
export TENABLE_SECRET_KEY="your-secret-key"

# Palo Alto (optional)
export PALO_ALTO_API_KEY="your-api-key"
export PALO_ALTO_HOST="panorama.company.com"

# CrowdStrike (optional)
export CROWDSTRIKE_CLIENT_ID="your-client-id"
export CROWDSTRIKE_CLIENT_SECRET="your-client-secret"

# Cloudflare (optional)
export CLOUDFLARE_API_TOKEN="your-api-token"

# AWS (optional)
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"

# Microsoft Defender (optional)
export DEFENDER_TENANT_ID="your-tenant-id"
export DEFENDER_CLIENT_ID="your-client-id"
export DEFENDER_CLIENT_SECRET="your-client-secret"
```

### Gradual Adoption

The skill works with partial control data. Start with what you have:

**Minimum:** Just Tenable (shows base risk only)
**Better:** Tenable + Firewall
**Great:** Tenable + Firewall + EDR
**Exceptional:** Tenable + Firewall + EDR + WAF + Segmentation + IDS

Each additional control integration improves risk accuracy.

---

## 💰 Business Value

### Quantifiable Benefits

**1. Stop Wasting Effort on Low-Actual-Risk Vulns**
- Traditional: Patch 1,000 critical vulns regardless of protection
- Control-Aware: Patch 200 truly-at-risk vulns, monitor 800 well-protected ones
- Result: 80% reduction in wasted patching effort

**2. Focus on Real Gaps**
- Identify assets lacking protection (control gaps)
- Prioritize control deployment where it matters most
- Justify EDR/WAF investments with quantified risk reduction

**3. Justify Resource Allocation**
- "Deploying EDR to these 50 assets will reduce risk by $8M"
- "Adding WAF to web servers eliminates 78% of critical web vulns"
- Board understands risk reduction, not just vulnerability counts

**4. Optimize Patch Cycles**
- Well-protected assets can wait for scheduled maintenance
- Under-protected assets need emergency patching
- Balance operational disruption with actual risk

---

## 🧮 Algorithm Details

### Control Reliability Factors

Controls aren't perfect. The skill accounts for failure rates:

| Control | Reliability | Why |
|---------|-------------|-----|
| Firewall | 98% | 2% misconfiguration/rule error rate |
| EDR | 95% | 5% miss rate for advanced threats |
| WAF | 90% | 10% bypass rate for novel attacks |
| IDS | 85% | 15% false negative rate |
| Segmentation | 99% | 1% human error in routing |

### Special Conditions

**Zero-Day Vulnerabilities:**
- Recently disclosed (< 7 days)
- Signature-based controls less effective
- Behavioral EDR still provides protection

**Insider Threats:**
- Privilege escalation vulns
- Authenticated users bypass perimeter controls
- Reduces control effectiveness

**Supply Chain Vulns:**
- Third-party software (SolarWinds, Log4j)
- Can't directly patch
- Controls become more critical

---

## 📈 Use Cases

### Security Operations
- Daily triage: "What's actually urgent today?"
- Sprint planning: "Which vulns need fixing this sprint?"
- Resource allocation: "Do we need more EDR licenses?"

### Management Reporting
- Executive briefings: "Here's our true risk posture"
- Budget requests: "Deploying X will reduce risk by Y"
- Audit preparation: "Show control effectiveness"

### Compliance
- Demonstrate defense-in-depth to auditors
- Show compensating controls for exceptions
- Prove risk-based approach

---

## 🔒 Security & Privacy

- **Read-Only**: Skill never modifies control configurations
- **No Credentials Stored**: Uses environment variables or MCP
- **API-Only Access**: No need for privileged network access
- **Local Processing**: All calculations run locally

---

## 📜 License

MIT License - See LICENSE file

---

## 🎯 Roadmap

**Version 1.0** (Current)
- ✅ Firewall rule analysis
- ✅ EDR coverage detection
- ✅ WAF protection identification
- ✅ Network segmentation scoring
- ✅ Defense-in-depth calculation
- ✅ Control gap analysis

**Version 1.1** (Planned)
- 🔄 ML model for control effectiveness
- 🔄 Automated control deployment recommendations
- 🔄 Integration with SOAR platforms
- 🔄 Real-time monitoring dashboard

**Version 2.0** (Future)
- 🔮 Attack path simulation with controls
- 🔮 Control failure simulation
- 🔮 Multi-tenant support
- 🔮 API for external tools

---

**Know your actual risk. Patch what matters. Justify control investments.** 🛡️
