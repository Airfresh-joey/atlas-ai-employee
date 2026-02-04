# ATLAS - AI Sales Employee Plan
## Vision: Full-Stack AI Sales Employee

### What Joey Wants:
1. **Chat with ATLAS** - Conversational interface to guide and manage
2. **Company Knowledge** - Access to Air Fresh Marketing & Humming Agent AI docs
3. **ICP Understanding** - Who we target, what problems we solve
4. **Personalized Landing Pages** - Custom URLs per prospect with their problems + CTA
5. **Automation** - Research → Email → Track → Follow-up

---

## Architecture Plan

### Phase 1: Chat Interface + Knowledge Base (MVP)
**Goal:** Talk to ATLAS, upload docs, set targeting

**Tech Stack:**
- Frontend: HTML/JS (keep it simple, like current widget)
- Backend: Vercel Serverless Functions
- AI: Claude API (Sonnet 4)
- Storage: Vercel KV for simple key-value storage
- Docs: Text files + simple vector search (or just Claude with long context)

**Features:**
1. Chat panel next to the widget
2. Upload company docs (PDFs → text extraction)
3. Define ICPs conversationally
4. ATLAS remembers everything in context

### Phase 2: Dynamic Landing Pages
**Goal:** Generate personalized pages for each prospect

**Flow:**
1. ATLAS researches prospect (company, industry, pain points)
2. Generates custom landing page:
   - Hero: "[Company Name], we help [industry] with [their specific problem]"
   - Benefits: Tailored to their business
   - Case study: Similar company
   - CTA: Book meeting with custom calendar link
3. Deploys to: `atlas-sales-employee.vercel.app/p/[prospect-slug]`
4. Tracks: Views, time on page, clicks

**Tech:**
- Template engine (simple HTML templates)
- Dynamic routing on Vercel
- Analytics: Simple event tracking

### Phase 3: Full Automation
**Goal:** End-to-end sales automation

**Features:**
1. Prospect research (LinkedIn, company website, news)
2. Email composition + sending (via Gmail API)
3. Response detection + classification
4. Follow-up sequences
5. Meeting booking integration (Calendly/Cal.com)
6. CRM sync (HubSpot)

---

## MVP Implementation Plan (Next 2 Hours)

### Step 1: Enhanced Widget with Chat (30 min)
- Split-pane layout: Widget on left, chat on right
- Chat connects to Claude API
- ATLAS can explain what it's doing
- Upload button for company docs

### Step 2: Knowledge Base (30 min)
- Text area to paste company info
- Save to Vercel KV
- ATLAS reads from KB when researching prospects

### Step 3: Landing Page Generator (45 min)
- Simple template system
- Mock prospect data
- Generate sample personalized page
- Deploy to `/p/[slug]` route

### Step 4: Demo Ready (15 min)
- Polish UI
- Add sample prospects
- Show full flow:
  - Chat: "Target construction companies in Denver"
  - ATLAS: Researches Apex Construction
  - Generates: `/p/apex-construction` personalized page
  - Shows: Email preview with link

---

## Files to Create:

```
~/clawd/atlas-demo/
├── index.html (enhanced widget + chat)
├── api/
│   ├── chat.js (Claude API endpoint)
│   ├── generate-page.js (landing page generator)
│   └── save-kb.js (knowledge base saver)
├── p/
│   └── [slug].html (dynamic landing pages)
├── templates/
│   └── landing-page.html (template)
└── data/
    ├── company-kb.json (Air Fresh + Humming Agent info)
    └── prospects.json (prospect database)
```

---

## Success Metrics for Demo:

1. ✅ Chat with ATLAS about targeting
2. ✅ Upload company docs
3. ✅ ATLAS generates personalized landing page
4. ✅ Show custom URL with prospect's pain points
5. ✅ Live demo end-to-end flow

---

## Next Steps:
1. Build enhanced widget with chat
2. Set up Vercel API routes
3. Create landing page templates
4. Generate sample prospect pages
5. Deploy and demo

Let's build! 🚀
