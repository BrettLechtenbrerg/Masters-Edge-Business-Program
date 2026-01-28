# Monitoring Strategies
## Competitor Intelligence | The Master's Edge Business Program

---

## Strategic Framework: What to Monitor and Why

Not all competitor pages are equally valuable. This guide helps you prioritize what to watch, how often to check, and what insights you'll gain.

---

## Priority Matrix

```
                    HIGH IMPACT
                         │
         ┌───────────────┼───────────────┐
         │               │               │
         │   PRICING     │   SERVICES    │
         │   PAGES       │   PAGES       │
         │               │               │
         │  Check: 1-4h  │  Check: 6-12h │
         │               │               │
LOW  ────┼───────────────┼───────────────┼──── HIGH
CHANGE   │               │               │    CHANGE
FREQ     │   ABOUT /     │   BLOG /      │    FREQ
         │   TEAM        │   NEWS        │
         │               │               │
         │  Check: 24h   │  Check: 6-12h │
         │               │               │
         └───────────────┼───────────────┘
                         │
                    LOW IMPACT
```

**Focus on the top-right quadrant first:** High impact + changes frequently = highest value monitoring.

---

## Page-by-Page Monitoring Guide

### 1. Pricing Pages (HIGHEST PRIORITY)

**Why monitor:** Price changes directly affect your competitive position. A competitor dropping prices could steal customers; raising prices creates opportunity.

**What to watch for:**
- Price increases or decreases
- New pricing tiers added
- Tiers removed or consolidated
- Feature changes within tiers
- Billing changes (monthly → annual only)
- Limited-time offers or discounts
- "Contact us for pricing" (hiding prices = testing new structure)

**Check frequency:** Every 1-4 hours

**CSS selectors to try:**
```
.pricing, #pricing, .pricing-table, .pricing-grid
.plan, .tier, .package
[data-pricing], [data-plans]
```

**Alert urgency:** 🔴 HIGH — Review within 24 hours

**Example insight:**
> "Competitor X raised their Pro tier from $99 to $129/month but kept the same features. Our Pro at $89 is now 31% cheaper. Consider: (1) hold price and emphasize value, (2) raise to $99 and pocket margin, (3) add a feature to justify premium."

---

### 2. Services / Products Pages

**Why monitor:** New offerings, discontinued services, and feature changes reveal strategy and create competitive windows.

**What to watch for:**
- New services or products launched
- Services discontinued or "sunsetting"
- Feature additions or removals
- Service descriptions changed (repositioning)
- New categories or verticals
- Partnership announcements
- Beta or "coming soon" sections

**Check frequency:** Every 6-12 hours

**CSS selectors to try:**
```
.services, #services, .products, .offerings
.service-card, .product-card
.features, .feature-list
```

**Alert urgency:** 🟡 MEDIUM — Review within 48 hours

**Example insight:**
> "Competitor Y added 'AI-Powered Analytics' to their enterprise tier. This suggests they're targeting larger accounts. We should: (1) evaluate if we need similar capability, (2) emphasize our human-touch differentiator, (3) monitor if this attracts our target customers."

---

### 3. Homepage

**Why monitor:** Homepage changes often signal major strategic shifts — rebranding, new positioning, different target audience, or new campaigns.

**What to watch for:**
- Headline/tagline changes (positioning shift)
- Hero image or video changes
- Primary CTA changes
- Social proof additions (logos, testimonials)
- New sections added
- Navigation changes
- Messaging tone shifts

**Check frequency:** Every 12-24 hours

**CSS selectors to try:**
```
.hero, #hero, .hero-section
.headline, h1
.cta, .primary-cta
.social-proof, .logos, .trust-badges
```

**Alert urgency:** 🟡 MEDIUM — Review within 72 hours

**Example insight:**
> "Competitor Z changed their headline from 'Software for Small Teams' to 'Enterprise-Ready Platform.' They're moving upmarket. This creates opportunity to own the SMB positioning they're abandoning."

---

### 4. Careers / Jobs Pages

**Why monitor:** Hiring patterns reveal growth areas, struggles, and strategic priorities. Job descriptions expose technology stack, culture, and compensation.

**What to watch for:**
- Number of open positions (growth indicator)
- New roles added (what capabilities they're building)
- Roles removed (strategy shift or hiring freeze)
- Specific job descriptions (tech stack, methodologies)
- Salary ranges (compensation benchmarking)
- Remote/hybrid policies
- Department focus (Sales heavy = growth push, Engineering = product investment)

**Check frequency:** Every 12-24 hours

**CSS selectors to try:**
```
.careers, #careers, .jobs, .openings
.job-listing, .career-card
.open-positions
```

**Alert urgency:** 🟢 LOW — Review weekly

**Example insight:**
> "Competitor A posted 5 sales roles in the Midwest region last week. They're expanding into our territory. Time to: (1) strengthen relationships with existing customers, (2) increase outbound in that region, (3) consider a competitive campaign."

---

### 5. About / Team Pages

**Why monitor:** Leadership changes, team growth, and organizational shifts often precede strategic changes.

**What to watch for:**
- New executives (especially CEO, CTO, CMO, VP Sales)
- Departures (especially founders or key leaders)
- Team size changes
- New departments or teams
- Advisory board additions
- Investor announcements
- Company milestones

**Check frequency:** Every 24 hours

**CSS selectors to try:**
```
.team, #team, .leadership, .about-team
.team-member, .person-card
.advisors, .board
```

**Alert urgency:** 🟢 LOW — Review weekly

**Example insight:**
> "Competitor B added a 'VP of Enterprise Sales' and three new headshots in Sales. Combined with their pricing page change to hide enterprise pricing, they're clearly making a push upmarket. Time to double down on our mid-market focus."

---

### 6. Testimonials / Case Studies / Reviews

**Why monitor:** Social proof changes show who they're winning, what results they're delivering, and how they're positioning success.

**What to watch for:**
- New testimonials added
- Case studies published
- Logo additions (new notable customers)
- Industry verticals highlighted
- Metrics/results claimed
- Video testimonials (high investment = key customer)

**Check frequency:** Every 12-24 hours

**CSS selectors to try:**
```
.testimonials, #testimonials, .reviews
.case-study, .success-story
.customer-logos, .trust-logos
```

**Alert urgency:** 🟢 LOW — Review weekly

**Example insight:**
> "Competitor C added testimonials from three healthcare companies this month. They're making a vertical push. We should: (1) review our healthcare messaging, (2) reach out to our healthcare customers for testimonials, (3) consider healthcare-specific content."

---

### 7. Blog / News / Resources

**Why monitor:** Content strategy reveals thought leadership positioning, SEO focus, and marketing campaigns.

**What to watch for:**
- New blog posts (topics, frequency)
- Content themes and categories
- Lead magnets or downloadable resources
- Webinar announcements
- Partnership or integration announcements
- Company news and press releases

**Check frequency:** Every 6-12 hours (blogs change often)

**CSS selectors to try:**
```
.blog, #blog, .posts, .articles
.blog-feed, .post-list
.resources, .content-hub
.news, .press
```

**Alert urgency:** 🟢 LOW — Review weekly

**Example insight:**
> "Competitor D published 8 blog posts about 'AI in Customer Service' this month vs. 2 last month. They're making an SEO play for that keyword cluster. We should: (1) audit our AI content, (2) consider a competing content push, (3) evaluate if this reflects a product roadmap shift."

---

### 8. Landing Pages / Campaigns

**Why monitor:** Campaign landing pages reveal current marketing focus, offers, and messaging tests.

**What to watch for:**
- New landing page URLs appearing
- Offer changes (discounts, bonuses, guarantees)
- Headline and copy tests
- Form field changes (lead qualification)
- Urgency tactics (countdown timers, limited spots)
- Ad message alignment

**Check frequency:** Every 4-12 hours (campaigns change frequently)

**CSS selectors to try:**
```
.landing, .lp-content
.offer, .promo
.cta-section
form, .lead-form
```

**Alert urgency:** 🟡 MEDIUM — Review within 48 hours

**Example insight:**
> "Competitor E launched a landing page offering '3 Months Free' for annual plans. This is aggressive discounting suggesting slow sales or a push for cash flow. We could: (1) match with our own offer, (2) emphasize our no-contract flexibility, (3) wait and see if it's temporary."

---

### 9. Terms of Service / Privacy Policy

**Why monitor:** Legal document changes often precede product changes, data practices updates, or compliance requirements.

**What to watch for:**
- Significant policy rewrites
- New data collection disclosures
- Pricing/refund policy changes
- Service level commitments
- Liability limitations
- Geographic restrictions
- Integration/API terms

**Check frequency:** Every 24-48 hours (rarely changes, but important)

**CSS selectors to try:**
```
.legal, .terms, .privacy
article, .content-body
main
```

**Alert urgency:** 🟢 LOW — Review monthly

**Example insight:**
> "Competitor F updated their terms to include AI data usage provisions. They're likely preparing an AI feature launch. Time to accelerate our own AI roadmap communication."

---

## Building Your Competitor Watch List

### Step 1: Identify Your Competitors

**Direct Competitors** (same product, same market):
- Customers compare you side-by-side
- They show up in your deals
- You lose deals to them

**Indirect Competitors** (different product, same problem):
- Customers could use instead of you
- Alternative solutions to the same pain
- May not see themselves as competitors

**Adjacent Competitors** (same product, different market):
- Could expand into your market
- You could expand into theirs
- Future competitive threats

### Step 2: Prioritize Competitors

Rate each competitor on:

| Factor | Score 1-5 |
|--------|-----------|
| How often do you lose deals to them? | |
| How similar is their offering? | |
| How aggressive is their growth? | |
| How much do customers mention them? | |
| How likely are they to expand your way? | |
| **Total** | |

**Top 3-5 scores = Primary competitors** (monitor heavily)
**Next 3-5 = Secondary competitors** (monitor lightly)
**Rest = Watch list** (quarterly check-ins)

### Step 3: Map Pages to Monitor

For each primary competitor, create watches for:

| Priority | Pages | Check Frequency |
|----------|-------|-----------------|
| Must Have | Pricing, Services/Products | 1-6 hours |
| Should Have | Homepage, Careers | 12-24 hours |
| Nice to Have | Blog, Testimonials, About | 24 hours |

For secondary competitors:
- Pricing page only (weekly check)
- Homepage (weekly check)

---

## Industry & Market Monitoring

### Beyond Direct Competitors

**Industry News Sites:**
- TechCrunch, Industry Dive, trade publications
- Monitor for mentions of your market or keywords
- Alert on competitor press coverage

**Review Sites:**
- G2, Capterra, TrustRadius, Yelp, Google Reviews
- Monitor competitor review pages
- Track rating changes and review volume

**Regulatory Bodies:**
- Government sites relevant to your industry
- Compliance requirement changes
- Licensing updates

**Job Boards:**
- Indeed, LinkedIn, Glassdoor
- Search for competitor names
- Track hiring velocity across the market

### Sample Industry Watches

| Watch | URL Pattern | Why |
|-------|-------------|-----|
| TechCrunch - Your Category | `techcrunch.com/tag/your-category` | Industry news |
| G2 - Competitor Reviews | `g2.com/products/competitor/reviews` | Customer sentiment |
| LinkedIn Jobs - Competitor | `linkedin.com/company/competitor/jobs` | Hiring trends |
| Industry Association News | `industryassociation.org/news` | Regulatory/market changes |

---

## Monitoring Schedule Template

### Daily Review (5 minutes)
- Check new alerts from overnight
- Triage into "needs attention" vs "log and move on"
- Flag anything requiring team discussion

### Weekly Review (30 minutes)
- Review all changes from the past week
- Update competitive intelligence log
- Identify patterns or themes
- Brief team on significant changes
- Adjust watch priorities if needed

### Monthly Review (1 hour)
- Audit all watches for broken pages/selectors
- Remove irrelevant competitors
- Add new competitors identified
- Review false positive rate and tune filters
- Compile monthly competitive summary

### Quarterly Review (2 hours)
- Strategic competitive analysis
- Re-evaluate competitor rankings
- Identify market trends from monitoring data
- Brief leadership on competitive landscape
- Plan competitive responses if needed

---

## Red Flags to Watch For

### Immediate Attention (🔴)
- Major price drop (>20%)
- New funding announcement
- Key executive hire in your region
- Direct attack on your positioning
- Patent filing in your space

### Monitor Closely (🟡)
- Consistent hiring increases
- New feature launches
- Geographic expansion
- Partnership announcements
- Aggressive discounting campaigns

### Background Awareness (🟢)
- Minor copy changes
- Team growth
- New blog categories
- Testimonial additions
- Design refreshes

---

## Setting Up Your First 10 Watches

Start here — these provide 80% of competitive value with minimal setup:

| # | Competitor | Page | Priority |
|---|------------|------|----------|
| 1 | Primary Competitor A | Pricing | 🔴 |
| 2 | Primary Competitor A | Services | 🔴 |
| 3 | Primary Competitor A | Homepage | 🟡 |
| 4 | Primary Competitor B | Pricing | 🔴 |
| 5 | Primary Competitor B | Services | 🔴 |
| 6 | Primary Competitor B | Homepage | 🟡 |
| 7 | Primary Competitor C | Pricing | 🔴 |
| 8 | Primary Competitor C | Services | 🟡 |
| 9 | Industry News Site | Your Category | 🟢 |
| 10 | Review Site | Your Company | 🟡 |

Once these are running smoothly (1-2 weeks), expand to:
- Career pages for primary competitors
- Secondary competitor pricing pages
- Testimonial/case study pages

---

## Metrics to Track

### Monitoring Health
- **Watches active:** Total pages being monitored
- **Check frequency:** Average time between checks
- **False positive rate:** Alerts that weren't meaningful / Total alerts
- **Response time:** Hours from alert to review

### Competitive Activity
- **Changes per competitor/month:** Activity level indicator
- **Pricing changes/quarter:** Market pricing stability
- **New features launched:** Innovation velocity
- **Hiring velocity:** Growth indicator

### Intel Quality
- **Actionable insights/month:** Intel that drove a decision
- **Competitive wins influenced:** Deals won with intel advantage
- **Time saved:** vs. manual competitor checking

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
