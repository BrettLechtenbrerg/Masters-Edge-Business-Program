# The Master's Edge: Open Source Tool Research

**Research Date:** January 25, 2025
**Purpose:** Identify existing open source foundations for each proposed tool

---

## Executive Summary

| Tool | Open Source Exists? | Build Effort | Recommendation |
|------|:------------------:|:------------:|----------------|
| 1. SOP Factory | ⚠️ Partial | Medium | Build with AI + existing templates |
| 2. Delegation Engine | ❌ None | High | Build from scratch (unique value) |
| 3. Meeting Eliminator | ✅ Several | Low | Curate + customize |
| 4. Hiring Oracle | ✅ Several | Low-Med | Fork + customize for culture fit |
| 5. Difficult Conversations | ⚠️ Limited | Medium | Build with LLM roleplay |
| 6. Culture Pulse | ⚠️ Partial | Medium | Build on question frameworks |
| 7. Competitor Intelligence | ✅ Several | Low | Fork + simplify |
| 8. Customer Journey Architect | ⚠️ Limited | Medium | Build custom |
| 9. Pricing Optimizer | ❌ None | High | Build simplified prompt-based |
| 10. CEO Dashboard | ✅ Excellent | Low | Configure Metabase/Superset |
| 11. Voice of Customer | ✅ Several | Low | Customize sentiment tools |
| 12. Referral Engine | ✅ Excellent | Low | Deploy RefRef or Weferral |

---

## Detailed Research by Tool

---

### 1. 📋 SOP Factory
*"Document once, delegate forever"*

#### What Exists:

**AWS Strands Agent SOPs** (Most Relevant - Nov 2025)
- Standardized markdown format for defining AI agent workflows
- Used by Amazon teams across thousands of use cases
- Open sourced for community benefit
- 🔗 [AWS Blog: Strands Agent SOPs](https://aws.amazon.com/blogs/opensource/introducing-strands-agent-sops-natural-language-workflows-for-ai-agents/)

**TechSpokes/sops**
- Schema-based SOP standard with validation
- Plans for CLI, VSCode support, flowchart renderer
- Still in early planning phase
- 🔗 [github.com/TechSpokes/sops](https://github.com/TechSpokes/sops)

**Open Source Documentation Tools**
- Tools like Notion alternatives, wiki software exist
- None specifically for operational procedure creation
- 🔗 [Sowflow: Open Source Documentation Tools](https://www.sowflow.io/blog-post/10-open-source-documentation-tools-to-streamline-your-processes)

#### Gap Analysis:
- NO tool that interviews owners conversationally to extract procedures
- NO tool that generates GHL automations from SOPs
- NO tool combining documentation + training + accountability

#### Recommendation: **BUILD - High Value Opportunity**
Use Claude Code + AWS SOP format as foundation. Create conversational interview system.

---

### 2. 🎯 Delegation Engine
*"What should I stop doing?"*

#### What Exists:

**Claude Task Master**
- AI-powered task management for development workflows
- Works with Cursor, Windsurf, Claude Code
- 🔗 [github.com/eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master)

**Plexo**
- Open source project management with AI suggestions
- Autonomous task creation
- 🔗 [github.com/minskylab/plexo-core](https://github.com/minskylab/plexo-core)

**Leantime**
- ADHD-friendly project management
- Task breakdown features
- 🔗 [leantime.io](https://leantime.io/)

**AI Delegability Research**
- Academic framework for understanding AI task delegation
- 🔗 [delegability.github.io](https://delegability.github.io/)

#### Gap Analysis:
- NO tool that audits owner's time by $ value
- NO tool that calculates "hourly worth" vs task value
- NO tool that generates delegation plans with job descriptions
- NO tool specifically for owner/founder delegation coaching

#### Recommendation: **BUILD - Unique Value Proposition**
This is a HIGHLY UNIQUE concept. Nothing exists. This could be the flagship tool.

---

### 3. 🗓️ Meeting Eliminator
*"Reclaim your calendar"*

#### What Exists:

**Open Source Platforms**
- **Jitsi Meet** - Self-hosted video conferencing
- **Rocket Chat** - Team chat with video integration
- **Mattermost** - Open source Slack alternative
- 🔗 [Enterprisers Project: Open Source Meeting Tools](https://enterprisersproject.com/article/2020/4/open-source-meeting-tools-3-things-know)

**Async Communication Tools (Commercial)**
- **Range** - Async check-ins (40-60% meeting reduction reported)
- **Geekbot** - Slack bot for async standups
- **Yac** - Async voice/video messages
- 🔗 [Range: Asynchronous Tools](https://www.range.co/blog/asynchronous-tools)
- 🔗 [Quely: Async Meeting Tools](https://www.quely.io/blog/5-async-tools-to-use-to-replace-status-meetings)

#### Gap Analysis:
- Tools exist for REPLACING meetings
- NO tool that AUDITS meetings for ROI
- NO tool that generates "meeting decision trees"
- Missing: GHL automation templates for async workflows

#### Recommendation: **CURATE + BUILD PROMPTS**
Create meeting audit prompt + async alternative templates. Don't build software.

---

### 4. 👥 Hiring Oracle
*"Hire character, train skill"*

#### What Exists:

**FoloUp** ⭐ (Best Match)
- AI generates interview questions from job descriptions
- Voice interviews that adapt to responses
- Dashboard for candidate tracking
- 🔗 [github.com/FoloUp/FoloUp](https://github.com/FoloUp/FoloUp)

**GPTInterviewer**
- Analyzes resume + job description
- Generates tailored questions
- Chat or voice interaction
- 🔗 [github.com/jiatastic/GPTInterviewer](https://github.com/jiatastic/GPTInterviewer)

**Pytai**
- Real-time interview platform
- Dynamic question generation
- Gemini AI evaluation
- 🔗 [github.com/getFrontend/app-ai-interviews](https://github.com/getFrontend/app-ai-interviews)

**AI-mock-Interview**
- Questions adjusted by experience level
- Detailed feedback and grading
- 🔗 [github.com/modamaan/Ai-mock-Interview](https://github.com/modamaan/Ai-mock-Interview)

#### Gap Analysis:
- Existing tools focus on CANDIDATE practice
- NO tool generates culture-fit questions from Brand Book
- NO tool creates 30/60/90 day onboarding plans
- Missing: Integration with values/mission from brand work

#### Recommendation: **FORK + CUSTOMIZE**
Fork FoloUp or GPTInterviewer. Add Brand Book integration for culture questions.

---

### 5. 💬 Difficult Conversations Coach
*"Say the hard thing, the right way"*

#### What Exists:

**Commercial Platforms (No Open Source)**
- **VirtualSpeech** - 40+ roleplay scenarios including difficult conversations
- **Yoodli** - Enterprise roleplay for crucial conversations
- **LinkedIn Learning** - New AI roleplay for workplace scenarios
- **SkillGym** - AI roleplay for leadership conversations
- 🔗 [VirtualSpeech: AI Roleplays](https://virtualspeech.com/ai-practice)
- 🔗 [Yoodli](https://yoodli.ai/)
- 🔗 [UC Link: AI Role Play](https://link.ucop.edu/2025/04/22/practice-tough-conversations-with-ai-powered-role-play/)

**Open Source LLM Base**
- Humanish-Roleplay-Llama-3.1-8B for general roleplay
- Would need significant customization
- 🔗 [DEV: Open Source LLMs for Companionship](https://dev.to/aiandml/5-best-open-source-llms-for-ai-companionship-40j1)

#### Gap Analysis:
- Commercial solutions dominate this space
- NO open source specifically for manager training
- NO tool with psychologically-safe feedback frameworks
- Opportunity: Claude Code-based roleplay system

#### Recommendation: **BUILD WITH PROMPTS**
Create comprehensive prompt system with scenarios. Use Claude Code for roleplay. No complex software needed.

---

### 6. 📊 Culture Pulse
*"What's really going on?"*

#### What Exists:

**Joyous Employee Survey Questions** ⭐
- 25 questions for employee experience
- Covers culture, fairness, inclusion, well-being
- Can be used for pulse surveys
- CC0-1.0 license (fully open)
- 🔗 [github.com/joyouslabs/employee-survey-questions](https://github.com/joyouslabs/employee-survey-questions)

**Survey Platforms (Open Source)**
- **LimeSurvey** - Full-featured survey platform
- Can be adapted for employee engagement

**Commercial Reference**
- CultureMonkey, Officevibe, Vantage Pulse
- 🔗 [CultureMonkey: Pulse Survey Tools](https://www.culturemonkey.io/employee-engagement/pulse-survey-tools/)

#### Gap Analysis:
- Question frameworks exist (open source)
- Survey platforms exist (open source)
- NO AI analysis layer for sentiment/patterns
- NO early warning detection system
- NO intervention recommendation engine

#### Recommendation: **BUILD ON FOUNDATIONS**
Use Joyous questions + LimeSurvey + AI analysis layer. Create warning system + interventions.

---

### 7. 🔍 Competitor Intelligence System
*"Know the battlefield"*

#### What Exists:

**Competitor Analyst (0xmetaschool)** ⭐
- Analyzes competitor data with AI
- Identifies strengths/weaknesses
- Tracks new launches and campaigns
- Next.js + AI
- 🔗 [github.com/0xmetaschool/competitor-analyst](https://github.com/0xmetaschool/competitor-analyst)

**Comperator (Procycons)** ⭐
- Crawls competitor websites
- Extracts and classifies content
- Generates summaries + comparative analysis
- Streamlit interface
- MIT License
- 🔗 [github.com/Procycons/Comperator](https://github.com/Procycons/Comperator)

**Bright Data Competitive Intelligence**
- Multi-agent workflow (Researcher, Analyst, Writer)
- Real-time streaming
- FastAPI + React + TypeScript
- 🔗 [github.com/brightdata/competitive-intelligence](https://github.com/brightdata/competitive-intelligence)

**Free OSINT Tools**
- 40+ open source intelligence tools
- 🔗 [github.com/SourcingDenis/free-online-competitive-intelligence](https://github.com/SourcingDenis/free-online-competitive-intelligence)

#### Gap Analysis:
- Good tools exist for TECHNICAL competitor analysis
- May be too complex for average business owner
- Could simplify and focus on small business needs

#### Recommendation: **FORK + SIMPLIFY**
Fork Comperator or Competitor Analyst. Simplify UI for small business owners.

---

### 8. 🗺️ Customer Journey Architect
*"Map every touchpoint"*

#### What Exists:

**CJX (Customer Journey Master)** ⭐
- Django-based web application
- Collects journey data from multiple sources
- Process Mining algorithms for visualization
- Trace clustering for journey grouping
- 🔗 [github.com/cnam0203/CJX](https://github.com/cnam0203/CJX)

**Customer-Journey-Analytics**
- E-commerce focused
- Combines qualitative + quantitative insights
- 🔗 [github.com/alfredsasko/Customer-Journey-Analytics](https://github.com/alfredsasko/Customer-Journey-Analytics)

**Laudspeaker**
- Open source alternative to Braze/Customer.io
- Product onboarding flows
- Event-triggered communications
- 🔗 Listed as open source Braze alternative

**Appsmith**
- Build custom journey mapping tools
- Pre-built widgets
- 🔗 [Appsmith: Customer Journey Mapping](https://www.appsmith.com/use-case/customer-journey-mapping-software)

#### Gap Analysis:
- Existing tools are DATA-HEAVY (need technical setup)
- NO simple conversational journey mapping
- NO GHL automation generation from touchpoints
- Missing: Simple visual mapping + automation export

#### Recommendation: **BUILD SIMPLIFIED VERSION**
Create prompt-based journey mapping that generates GHL automation blueprints.

---

### 9. 💰 Pricing Profit Maximizer
*"Stop leaving money on the table"*

#### What Exists:

**Commercial Solutions Only**
- Open Pricer, Intelligence Node, Pricefx, Quicklizard, Revionics
- All enterprise-grade, expensive ($7,000-$25,000+/year)
- 🔗 [Buynomics: AI Pricing Tool Comparison](https://www.buynomics.com/articles/ai-pricing-tool-comparison)
- 🔗 [Sniffie: AI Price Optimization](https://www.sniffie.io/software-solutions/ai-based-price-optimization/)

**Quicklizard (Interesting Note)**
- Open platform where any strategy can be implemented in Python
- But still commercial, not open source

#### Gap Analysis:
- NO open source pricing optimization tools
- All solutions are enterprise SaaS
- Massive opportunity for simplified version
- Small businesses have no accessible options

#### Recommendation: **BUILD PROMPT-BASED SYSTEM**
Create AI-driven pricing analysis prompts. No need for complex software. Focus on:
- Value-based pricing conversations
- Price increase scripts
- Competitive pricing analysis prompts

---

### 10. 📈 Weekly CEO Dashboard
*"One page, total clarity"*

#### What Exists:

**Metabase** ⭐⭐⭐ (Best for Non-Technical)
- Visual query builder (no SQL needed)
- AI-powered X-ray for automatic analysis
- Metabot AI assistant for insights
- Natural language queries
- Setup in under 5 minutes
- Mobile-friendly
- 🔗 [github.com/metabase/metabase](https://github.com/metabase/metabase)

**Apache Superset** ⭐⭐
- Most feature-rich open source BI
- 40+ visualization types
- Enterprise-ready
- More technical than Metabase
- 🔗 [github.com/apache/superset](https://github.com/apache/superset)

**Grafana** ⭐⭐
- Leading open source visualization
- 100+ data source integrations
- Real-time monitoring
- More DevOps focused
- 🔗 [github.com/grafana/grafana](https://github.com/grafana/grafana)

**Evidence**
- Code-first BI (SQL + Markdown)
- Great for developers
- MIT License
- 🔗 [github.com/evidence-dev/evidence](https://github.com/evidence-dev/evidence)

#### Gap Analysis:
- Excellent tools exist
- Setup/configuration can be challenging
- NO pre-built "CEO Dashboard" templates
- Missing: Business owner-friendly onboarding

#### Recommendation: **CONFIGURE + TEMPLATE**
Use Metabase. Create pre-built CEO dashboard templates. Provide setup guide.

---

### 11. 🗣️ Voice of Customer Synthesizer
*"Hear what they're really saying"*

#### What Exists:

**Customer Feedback Analyzer with Generative AI** ⭐
- Sentiment analysis, theme identification
- Improvement suggestions via Gemini API
- Streamlit app with CSV upload
- 🔗 [github.com/Devgan79/Customer-feedback-sentiment-analysis](https://github.com/Devgan79/Customer-feedback-sentiment-analysis-with-CSV-files-Generative-Ai)

**AI Customer Feedback Summarizer**
- Python-based, extracts insights
- Advanced version: sentiment + visualization
- OpenAI GPT integration
- 🔗 [github.com/thekartikeyamishra/AI-Customer-Feedback-Summarizer](https://github.com/thekartikeyamishra/AI-Customer-Feedback-Summarizer)

**Feature-Based Sentiment Analysis** ⭐
- Identifies WHICH features people discuss
- Positive/negative sentiment per feature
- Perfect for Product/Marketing teams
- 🔗 [github.com/fblascogarma/feature_based_sentiment_analysis](https://github.com/fblascogarma/feature_based_sentiment_analysis)

**Libraries**
- **spaCy** (30K stars) - 60+ languages
- **TextBlob** (9K stars) - Simple sentiment
- **NLP.js** (6K stars) - JavaScript option
- 🔗 [AIMultiple: Open Source Sentiment Tools](https://research.aimultiple.com/open-source-sentiment-analysis/)

#### Gap Analysis:
- Good technical tools exist
- Need business-friendly wrapper
- NO pre-built review aggregation
- Missing: Actionable recommendations

#### Recommendation: **CUSTOMIZE EXISTING**
Fork Customer Feedback Analyzer. Add business-friendly output + recommendations.

---

### 12. 🔗 Referral Engine Builder
*"Turn customers into recruiters"*

#### What Exists:

**RefRef** ⭐⭐⭐ (Best Overall)
- Open source referral + affiliate platform
- Launch in minutes
- Personalized links, attribution tracking
- Automated rewards
- Supports B2B, B2C, ecommerce, fintech
- 🔗 [github.com/refrefhq/refref](https://github.com/refrefhq/refref)
- 🔗 [refref.ai](https://refref.ai/)

**Weferral** ⭐⭐
- Full referral management + affiliate tracking
- Recurring/lifetime commissions
- Affiliate dashboard
- Email automation
- 🔗 [github.com/WeferralHq/weferral](https://github.com/WeferralHq/weferral)

**Usher Referrals**
- Web3 integration option
- Partner/affiliate/ambassador programs
- 🔗 [github.com/usherlabs/usher-referrals](https://github.com/usherlabs/usher-referrals)

**Marvec Referrals**
- Simple referral platform
- Email-based invitations
- 🔗 [github.com/marvec/referrals](https://github.com/marvec/referrals)

#### Gap Analysis:
- Excellent open source options exist!
- May need simplified setup guides
- Could add testimonial/case study features
- Missing: Referral ASK scripts + training

#### Recommendation: **DEPLOY + ENHANCE**
Deploy RefRef or Weferral. Add prompt templates for referral asks + testimonials.

---

## Summary: Build vs Buy vs Customize

### 🔨 BUILD FROM SCRATCH (Highest Value)
1. **Delegation Engine** - Nothing exists, unique opportunity
2. **SOP Factory** - Partial exists, need conversational AI layer

### 🔧 BUILD WITH PROMPTS (Medium Effort)
3. **Difficult Conversations Coach** - Use Claude Code for roleplay
4. **Pricing Optimizer** - Prompt-based pricing conversations
5. **Customer Journey Architect** - Simplified visual mapping
6. **Culture Pulse** - AI analysis layer on existing questions

### 🔀 FORK & CUSTOMIZE (Lower Effort)
7. **Hiring Oracle** - Fork FoloUp, add culture fit
8. **Competitor Intelligence** - Simplify Comperator
9. **Voice of Customer** - Wrap existing sentiment tools

### ✅ DEPLOY & TEMPLATE (Lowest Effort)
10. **CEO Dashboard** - Configure Metabase with templates
11. **Referral Engine** - Deploy RefRef with guides
12. **Meeting Eliminator** - Curate tools + create prompts

---

## Recommended Build Order

Based on uniqueness + impact + effort:

| Priority | Tool | Why |
|:--------:|------|-----|
| 1 | **Delegation Engine** | Unique. No competition. Huge value. |
| 2 | **SOP Factory** | Foundational. Enables delegation. |
| 3 | **Difficult Conversations** | Prompt-based. High impact. |
| 4 | **Hiring Oracle** | Fork existing. Integrates with Brand Book. |
| 5 | **CEO Dashboard** | Deploy Metabase. Create templates. |
| 6 | **Referral Engine** | Deploy RefRef. Add scripts. |

---

*Research compiled by Claude Code - January 25, 2025*
