# ATLAS Multi-Agent Architecture

## Overview

ATLAS uses a specialized 3-agent system where each agent is an expert in their domain. They communicate through a shared message queue and coordinate work automatically.

---

## Agent Definitions

### 🔍 Research Agent

**Specialty:** Intelligence gathering and prospect qualification

**Skills:**
- Web scraping (company websites, LinkedIn, news)
- Data enrichment (Apollo.io, Clearbit, Hunter.io)
- Natural language analysis (extracting pain points from content)
- Lead scoring (A/B/C based on ICP fit)

**Inputs:**
- ICP definition (industry, size, location, pain points)
- Target company list (or search criteria)

**Outputs:**
- Prospect dossier (JSON):
  ```json
  {
    "company": {
      "name": "Apex Construction LLC",
      "industry": "Commercial Construction",
      "size": 150,
      "location": "Denver, CO",
      "revenue": "$45M",
      "website": "apexconstruction.com"
    },
    "decisionMaker": {
      "name": "Michael Rodriguez",
      "title": "COO",
      "linkedin": "linkedin.com/in/mrodriguez",
      "email": "mrodriguez@apexconstruction.com"
    },
    "painPoints": [
      "Outdated website (last updated 2019)",
      "No active social media presence",
      "Low visibility in Google searches",
      "Competing with national firms for contracts"
    ],
    "triggers": [
      "Recently hired VP of Marketing",
      "Announced 3 new projects in Denver metro"
    ],
    "score": "A",
    "confidence": 0.92
  }
  ```

**Tools:**
- Playwright (web scraping)
- Apollo API (data enrichment)
- Claude (text analysis)
- Custom scoring algorithm

---

### 💼 Sales Agent

**Specialty:** Relationship building and conversion

**Skills:**
- Email copywriting (personalized, conversion-focused)
- Conversation management (context awareness)
- Objection handling (trained on sales best practices)
- Meeting scheduling (natural language coordination)

**Inputs:**
- Prospect dossier (from Research Agent)
- Landing page URL (from Web Agent)
- Email templates (customizable)
- Conversation history

**Outputs:**
- Outreach email (HTML):
  ```html
  Subject: Apex Construction - Winning More Bids in Denver
  
  Hi Michael,
  
  I noticed Apex Construction is growing fast—congrats on the new 
  projects in the Denver metro area!
  
  I help commercial construction firms like yours increase visibility 
  and win 30% more contracts through targeted marketing.
  
  We recently helped Mile High Builders (similar size, same market) 
  go from invisible online to ranking #1 for "Denver commercial 
  construction." They won 3 major contracts totaling $8.2M in 6 months.
  
  I put together a quick overview of how we could do the same for Apex:
  → https://atlas.airfresh.com/p/apex-construction
  
  Worth a 15-minute conversation?
  
  Best,
  Joey Kercher
  Air Fresh Marketing
  ```

- Follow-up sequence (based on engagement)
- Meeting invite (when ready)

**Tools:**
- Claude (email generation)
- Gmail API (sending)
- Calendly API (scheduling)
- Custom engagement tracking

---

### 🌐 Web Agent

**Specialty:** Content generation and performance tracking

**Skills:**
- Landing page generation (HTML/CSS from templates)
- Content personalization (dynamic based on prospect data)
- A/B testing (headline, CTA, layout variations)
- Analytics (page views, time on site, click tracking)

**Inputs:**
- Prospect dossier (from Research Agent)
- Template selection (industry-specific)
- Brand assets (logos, colors, case studies)

**Outputs:**
- Landing page (deployed to Vercel):
  ```
  URL: https://atlas.airfresh.com/p/apex-construction
  
  Content:
  - Custom headline: "Apex Construction: Stop Losing Bids to Competitors"
  - Pain points: Outdated website, no social presence, low visibility
  - Case study: Mile High Builders (similar Denver firm)
  - Social proof: 30% more contracts, 3x traffic
  - CTA: Book 15-min strategy call
  ```

- Analytics event stream:
  ```json
  {
    "pageView": {
      "url": "/p/apex-construction",
      "visitor": "174.96.xxx.xxx",
      "timestamp": "2026-02-04T10:30:00Z",
      "referrer": "email"
    },
    "engagement": {
      "timeOnPage": 180,
      "scrollDepth": 85,
      "ctaClicks": 1
    }
  }
  ```

**Tools:**
- Template engine (Handlebars)
- Vercel API (deployment)
- Custom analytics (event tracking)
- Image generation (OG images, hero images)

---

## Communication Flow

### Message Queue Architecture

Agents communicate through a shared message queue (Redis/Vercel KV):

```
Research Agent → [Queue] → Sales Agent
Sales Agent → [Queue] → Web Agent
Web Agent → [Queue] → Sales Agent
```

**Example Message:**
```json
{
  "from": "research-agent",
  "to": "sales-agent",
  "type": "prospect-qualified",
  "data": {
    "prospectId": "apex-construction-001",
    "dossier": { ... },
    "priority": "high"
  },
  "timestamp": "2026-02-04T10:30:00Z"
}
```

---

## Workflow Example

### New Prospect Flow

1. **Research Agent** receives ICP:
   - Industry: Construction
   - Location: Colorado
   - Size: 100-500 employees

2. **Research Agent** finds prospects:
   - Searches LinkedIn Sales Navigator
   - Finds: Apex Construction LLC
   - Enriches data from Apollo.io

3. **Research Agent** analyzes:
   - Scrapes website (outdated, 2019)
   - Checks social media (inactive)
   - Finds decision maker (Michael Rodriguez)
   - Scores: A (92% fit)

4. **Research Agent** sends to queue:
   ```json
   {
     "to": "sales-agent",
     "type": "prospect-qualified",
     "data": { "dossier": "..." }
   }
   ```

5. **Sales Agent** receives dossier:
   - Requests landing page from Web Agent
   - Waits for URL

6. **Web Agent** generates page:
   - Uses construction template
   - Personalizes for Apex
   - Deploys to `/p/apex-construction`
   - Returns URL to Sales Agent

7. **Sales Agent** sends email:
   - Personalized for Michael
   - Includes landing page URL
   - Tracks send via Gmail API

8. **Web Agent** tracks engagement:
   - Michael visits page (3 minutes)
   - Scrolls 85% down
   - Clicks CTA once

9. **Web Agent** alerts Sales Agent:
   ```json
   {
     "to": "sales-agent",
     "type": "high-engagement",
     "data": {
       "prospectId": "apex-construction-001",
       "timeOnPage": 180,
       "ctaClicks": 1
     }
   }
   ```

10. **Sales Agent** sends follow-up:
    - Within 1 hour (high engagement = hot lead)
    - References specific page sections
    - Proposes meeting times

11. **Meeting booked** ✅

---

## Scaling Considerations

### Horizontal Scaling

Each agent can be duplicated:

```
Research Agent 1 → Prospects 1-100
Research Agent 2 → Prospects 101-200
Research Agent 3 → Prospects 201-300

Sales Agent 1 → Conversations 1-50
Sales Agent 2 → Conversations 51-100

Web Agent (shared) → All landing pages
```

**Why Web Agent is shared:**
- Page generation is fast (<2 seconds)
- One agent can handle hundreds of pages/hour
- Centralized analytics easier to manage

---

### Load Balancing

**Priority Queue:**
```
High Priority: Hot leads (high engagement)
Medium Priority: New prospects (first outreach)
Low Priority: Cold leads (no engagement)
```

Agents always process high priority first.

---

## Technology Stack

### Research Agent
- **Runtime:** Node.js
- **Browser:** Playwright (headless Chrome)
- **APIs:** Apollo, Clearbit, Hunter
- **AI:** Claude Sonnet 4
- **Storage:** Vercel KV

### Sales Agent
- **Runtime:** Node.js
- **Email:** Gmail API / SendGrid
- **Calendar:** Calendly API / Google Calendar
- **AI:** Claude Sonnet 4
- **Templates:** Handlebars
- **Storage:** Vercel KV

### Web Agent
- **Runtime:** Node.js
- **Deployment:** Vercel API
- **Templates:** Handlebars
- **Analytics:** Custom (event stream)
- **AI:** Claude Sonnet 4 (content generation)
- **Storage:** Vercel KV

---

## Data Models

### Prospect
```typescript
interface Prospect {
  id: string;
  company: Company;
  decisionMaker: Contact;
  painPoints: string[];
  triggers: string[];
  score: 'A' | 'B' | 'C';
  confidence: number;
  status: 'new' | 'contacted' | 'engaged' | 'meeting' | 'closed';
  createdAt: Date;
  updatedAt: Date;
}
```

### Campaign
```typescript
interface Campaign {
  id: string;
  name: string;
  icp: ICP;
  template: EmailTemplate;
  landingPageTemplate: string;
  prospects: Prospect[];
  stats: {
    prospectsCounted: number;
    emailsSent: number;
    responses: number;
    meetings: number;
  };
  status: 'active' | 'paused' | 'completed';
}
```

### Message (Queue)
```typescript
interface Message {
  id: string;
  from: 'research-agent' | 'sales-agent' | 'web-agent';
  to: 'research-agent' | 'sales-agent' | 'web-agent';
  type: string;
  data: any;
  priority: 'high' | 'medium' | 'low';
  timestamp: Date;
  processed: boolean;
}
```

---

## Monitoring & Observability

### Agent Health Checks
```typescript
interface AgentHealth {
  agentId: string;
  status: 'online' | 'offline' | 'degraded';
  lastHeartbeat: Date;
  tasksProcessed: number;
  errorRate: number;
  averageTaskTime: number;
}
```

### Dashboard Metrics
- Prospects qualified (per hour)
- Emails sent (per hour)
- Response rate (%)
- Meeting booking rate (%)
- Landing page conversion (%)
- Agent utilization (%)

---

## Future Enhancements

### Phase 2: Advanced Intelligence
- GPT-4 Vision for website analysis
- Sentiment analysis on responses
- Competitor tracking
- Market intelligence reports

### Phase 3: Autonomy
- Self-optimizing email templates
- Dynamic ICP refinement
- Automated A/B testing
- Predictive lead scoring

### Phase 4: Multi-Channel
- LinkedIn messaging
- Twitter DMs
- SMS outreach
- Phone call scheduling

---

## Security & Compliance

### Data Protection
- All prospect data encrypted at rest
- PII handling compliant with GDPR/CCPA
- Email authentication (SPF, DKIM, DMARC)
- Rate limiting on all APIs

### Access Control
- Agent-to-agent authentication
- API key rotation
- Audit logs for all actions
- User permissions (view/edit/admin)

---

**Built by:** Pepper Potts 🌶️  
**For:** Air Fresh Marketing & Humming Agent AI  
**Date:** February 4, 2026
