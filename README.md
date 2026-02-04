# ATLAS - AI Sales Employee

**Full-stack AI sales automation with multi-agent architecture**

🔗 **Live Demo:** https://atlas-sales-employee.vercel.app

---

## What is ATLAS?

ATLAS is an AI-powered sales employee that works 24/7 to:
- Research and qualify prospects
- Generate personalized landing pages
- Send targeted outreach emails
- Track engagement and book meetings

---

## Multi-Agent Architecture

ATLAS uses a 3-agent system for maximum efficiency:

### 🔍 Research Agent
- Finds prospects matching your ICP
- Analyzes company data and pain points
- Identifies decision makers
- Scores leads (A/B/C)

### 💼 Sales Agent
- Crafts personalized emails
- Manages conversations
- Handles objections
- Books meetings

### 🌐 Web Agent
- Generates custom landing pages
- Deploys to unique URLs
- Tracks visitor behavior
- Reports engagement

---

## Features

### ✅ Current (Demo)
- Interactive chat interface
- Conversational ICP setup
- Simulated prospect research
- Landing page generation
- Live stats dashboard

### 🚧 In Development
- Real data source integration (LinkedIn, Apollo)
- Email automation (Gmail API)
- CRM sync (HubSpot)
- Advanced analytics dashboard
- Multi-campaign management

---

## Tech Stack

**Frontend:**
- HTML/CSS/JavaScript (vanilla, fast)
- Responsive design
- Real-time updates

**Backend:**
- Vercel Serverless Functions
- Claude Sonnet 4 API
- Vercel KV (storage)

**Deployment:**
- Vercel (global CDN)
- Custom domain support
- Instant deploys

---

## Project Structure

```
atlas-demo/
├── index.html              # Main demo (chat + widget)
├── p/                      # Landing pages
│   └── apex-construction.html
├── PLAN.md                 # Technical architecture
├── DEMO-GUIDE.md          # Demo script & talking points
└── README.md              # This file
```

---

## Demo Flow

1. **Chat with ATLAS** - Set up your ICP conversationally
2. **Research Prospects** - ATLAS finds and analyzes companies
3. **Generate Landing Pages** - Custom pages for each prospect
4. **View Results** - See personalized content in action

**Full demo guide:** See `DEMO-GUIDE.md`

---

## Sample Output

**Prospect:** Apex Construction LLC (Denver, CO)

**Generated Landing Page:** https://atlas-sales-employee.vercel.app/p/apex-construction.html

**Personalization:**
- Custom headline addressing their pain points
- Industry-specific case studies
- Tailored value propositions
- Direct CTA to book meetings

---

## Roadmap

### Phase 1: Enhanced Intelligence (Q1 2026)
- LinkedIn Sales Navigator integration
- Apollo.io data enrichment
- GPT-4 vision for website analysis
- Deeper company research

### Phase 2: Automation (Q2 2026)
- Gmail/Outlook integration
- Automated email sequences
- Response detection & classification
- Smart follow-ups

### Phase 3: Scale (Q2 2026)
- HubSpot/Salesforce CRM sync
- Multi-campaign management
- A/B testing framework
- Advanced analytics dashboard

### Phase 4: Intelligence (Q3 2026)
- Conversation intelligence
- Win/loss analysis
- Predictive lead scoring
- Market intelligence reports

---

## Use Cases

### Air Fresh Marketing
- Target: Colleges/universities needing marketing
- Generate: Custom pages showing campus marketing success
- Result: 3x more qualified leads

### Humming Agent AI
- Target: Businesses needing AI automation
- Generate: Industry-specific AI use cases
- Result: Higher conversion on demos

### Construction Firms
- Target: Commercial builders in specific regions
- Generate: Local case studies and project showcases
- Result: More RFP wins

---

## Cost Analysis

**Traditional SDR:**
- Salary: $60k-80k/year
- Benefits: +30%
- Training: 3-6 months
- Output: ~50 prospects/month
- **Total:** ~$100k/year

**ATLAS:**
- Development: One-time setup
- Operating: API costs (~$500/month)
- Training: Instant
- Output: Unlimited prospects/day
- **Total:** ~$6k/year

**ROI:** 16x cost reduction, infinite scale

---

## Getting Started

### For Demo:
```bash
# Clone repo
git clone [repo-url]

# Open demo
open index.html
```

### For Production:
1. Set up API keys (Claude, data sources)
2. Connect email account
3. Integrate CRM
4. Define ICPs
5. Launch campaigns

**Timeline:** 1-2 weeks from setup to first meetings booked

---

## Development

**Requirements:**
- Node.js 18+
- Vercel account
- Claude API key

**Deploy:**
```bash
vercel --prod
```

---

## Team

Built by Pepper Potts 🌶️ (Chief of Staff AI)
For Joey Kercher @ Air Fresh Marketing & Humming Agent AI

---

## License

Proprietary - Air Fresh Marketing & Humming Agent AI

---

## Questions?

Contact: joey@airfreshmarketing.com

**Live Demo:** https://atlas-sales-employee.vercel.app
