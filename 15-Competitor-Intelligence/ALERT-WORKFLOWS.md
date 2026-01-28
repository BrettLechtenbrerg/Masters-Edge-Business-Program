# Alert Workflows & GHL Integration
## Competitor Intelligence | The Master's Edge Business Program

---

## Overview

When ChangeDetection.io detects a change, it sends a webhook to Go High Level. This guide shows you how to receive that webhook and turn it into actionable workflows — notifications, tasks, logging, and team coordination.

---

## Architecture

```
┌──────────────────────┐
│  ChangeDetection.io  │
│                      │
│  Change Detected!    │
│  Competitor X        │
│  Pricing Page        │
└──────────┬───────────┘
           │
           │ Webhook POST
           │
           ▼
┌──────────────────────┐
│   GHL Inbound        │
│   Webhook            │
│                      │
│   Receives JSON      │
│   payload            │
└──────────┬───────────┘
           │
           │ Triggers
           │
           ▼
┌──────────────────────────────────────────────────────────────┐
│                    GHL WORKFLOW                               │
│                                                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │  Parse      │  │  Route by   │  │  Execute Actions    │  │
│  │  Webhook    │──▶│  Priority   │──▶│                     │  │
│  │  Data       │  │  & Type     │  │  • Send notification │  │
│  └─────────────┘  └─────────────┘  │  • Create task       │  │
│                                     │  • Log to database   │  │
│                                     │  • Tag contact       │  │
│                                     │  • Add to pipeline   │  │
│                                     └─────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

## Step 1: Create GHL Inbound Webhook

### In Go High Level:

1. Go to **Automation** → **Workflows**
2. Click **Create Workflow**
3. Name it: `Competitor Intelligence - Inbound`
4. For trigger, select **Inbound Webhook**
5. GHL generates a webhook URL like:
   ```
   https://services.leadconnectorhq.com/hooks/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```
6. **Copy this URL** — you'll add it to ChangeDetection.io

### Configure ChangeDetection.io:

1. Open your ChangeDetection.io dashboard
2. Click **Settings** (gear icon)
3. Scroll to **Notification URL list**
4. Paste your GHL webhook URL
5. Save

Now every change will POST to GHL.

---

## Step 2: Understanding the Webhook Payload

ChangeDetection.io sends this JSON when a change is detected:

```json
{
  "watch_url": "https://competitor.com/pricing",
  "watch_uuid": "abc123-def456-ghi789",
  "watch_title": "Acme Corp - Pricing Page",
  "watch_tag": "competitor-acme",
  "current_snapshot": "https://your-instance.com/snapshot/abc123",
  "diff_url": "https://your-instance.com/diff/abc123",
  "preview_url": "https://your-instance.com/preview/abc123",
  "screenshot_url": "https://your-instance.com/screenshot/abc123",
  "triggered": true,
  "trigger_text": "",
  "message": "Change detected"
}
```

### Key Fields:

| Field | Use In GHL |
|-------|-----------|
| `watch_title` | Identify which competitor/page |
| `watch_tag` | Route to correct workflow branch |
| `watch_url` | Link to the actual page |
| `diff_url` | Link to see what changed |
| `screenshot_url` | Visual of the change |

---

## Step 3: Build the Main Workflow

### Workflow: Competitor Intelligence - Inbound

```
TRIGGER: Inbound Webhook
    │
    ▼
┌───────────────────────────────────────┐
│  STEP 1: Parse & Store Data           │
│                                       │
│  Custom Values:                       │
│  {{webhook.watch_title}} → intel_source
│  {{webhook.watch_url}} → intel_url    │
│  {{webhook.watch_tag}} → intel_tag    │
│  {{webhook.diff_url}} → intel_diff    │
│  {{webhook.screenshot_url}} → screenshot
└───────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────┐
│  STEP 2: Determine Priority           │
│                                       │
│  IF/ELSE Branch:                      │
│                                       │
│  IF watch_tag CONTAINS "pricing"      │
│     → Priority = HIGH                 │
│     → Go to High Priority Branch      │
│                                       │
│  ELSE IF watch_tag CONTAINS "careers" │
│     → Priority = LOW                  │
│     → Go to Low Priority Branch       │
│                                       │
│  ELSE                                 │
│     → Priority = MEDIUM               │
│     → Go to Medium Priority Branch    │
└───────────────────────────────────────┘
    │
    ├─── HIGH PRIORITY ──────────────────┐
    │                                    │
    │    ┌─────────────────────────┐     │
    │    │ Send SMS to Owner       │     │
    │    │ "🚨 Competitor Alert:   │     │
    │    │  {{intel_source}}       │     │
    │    │  View: {{intel_diff}}"  │     │
    │    └─────────────────────────┘     │
    │              │                     │
    │              ▼                     │
    │    ┌─────────────────────────┐     │
    │    │ Send Slack Message      │     │
    │    │ #competitor-intel       │     │
    │    └─────────────────────────┘     │
    │              │                     │
    │              ▼                     │
    │    ┌─────────────────────────┐     │
    │    │ Create Task             │     │
    │    │ Due: Today              │     │
    │    │ Assigned: Owner         │     │
    │    │ Title: "Review:         │     │
    │    │  {{intel_source}}"      │     │
    │    └─────────────────────────┘     │
    │              │                     │
    │              └─────────────────────┼──┐
    │                                    │  │
    ├─── MEDIUM PRIORITY ────────────────┤  │
    │                                    │  │
    │    ┌─────────────────────────┐     │  │
    │    │ Send Email Digest       │     │  │
    │    │ (or add to daily batch) │     │  │
    │    └─────────────────────────┘     │  │
    │              │                     │  │
    │              ▼                     │  │
    │    ┌─────────────────────────┐     │  │
    │    │ Send Slack Message      │     │  │
    │    │ #competitor-intel       │     │  │
    │    └─────────────────────────┘     │  │
    │              │                     │  │
    │              └─────────────────────┼──┤
    │                                    │  │
    └─── LOW PRIORITY ───────────────────┘  │
                                            │
         ┌─────────────────────────┐        │
         │ Log Only                │        │
         │ (Weekly review batch)   │        │
         └─────────────────────────┘        │
                   │                        │
                   └────────────────────────┤
                                            │
                                            ▼
                             ┌─────────────────────────┐
                             │ ALL ALERTS:             │
                             │                         │
                             │ Log to Intel Database   │
                             │ (Custom Object or       │
                             │  Google Sheet)          │
                             └─────────────────────────┘
```

---

## Step 4: Notification Templates

### SMS Alert (High Priority)

```
🚨 COMPETITOR ALERT

{{intel_source}} just changed.

View changes: {{intel_diff}}

Priority: HIGH - Review today
```

### Email Alert

**Subject:** `[Competitor Intel] {{intel_source}} - Change Detected`

**Body:**
```html
<h2>Competitor Change Detected</h2>

<p><strong>Source:</strong> {{intel_source}}</p>
<p><strong>Page:</strong> <a href="{{intel_url}}">{{intel_url}}</a></p>
<p><strong>Detected:</strong> {{current_date}} at {{current_time}}</p>

<h3>Quick Actions</h3>
<ul>
  <li><a href="{{intel_diff}}">View What Changed</a></li>
  <li><a href="{{intel_url}}">View Live Page</a></li>
</ul>

<p>This alert was generated by your Competitor Intelligence System.</p>
```

### Slack Message

```
🔔 *Competitor Intel Alert*

*Source:* {{intel_source}}
*Priority:* {{priority}}
*Time:* {{current_date}} {{current_time}}

<{{intel_diff}}|View Changes> | <{{intel_url}}|View Page>
```

---

## Step 5: Create Intel Logging System

### Option A: GHL Custom Object (Recommended)

Create a custom object called `Competitor Intel`:

| Field | Type | Purpose |
|-------|------|---------|
| `source` | Text | Which competitor/page |
| `url` | URL | The monitored URL |
| `change_date` | DateTime | When detected |
| `priority` | Dropdown | High / Medium / Low |
| `category` | Dropdown | Pricing / Services / Careers / Homepage / Other |
| `diff_link` | URL | Link to the diff view |
| `screenshot` | URL | Link to screenshot |
| `summary` | Long Text | What changed (manual or AI-generated) |
| `action_taken` | Dropdown | None / Monitored / Responded / Matched |
| `notes` | Long Text | Analysis notes |

**Workflow Action:** Create Record in `Competitor Intel` object with webhook data.

### Option B: Google Sheets (Simpler)

Create a Google Sheet with columns:
- Date
- Source
- URL
- Priority
- Diff Link
- Summary
- Action
- Notes

**Workflow Action:** Use GHL's Google Sheets integration to append row.

### Option C: GHL Notes (Simplest)

If you track competitors as contacts:

**Workflow Action:** Add note to competitor contact with change details.

---

## Step 6: Team Coordination Workflows

### Daily Digest (Batch Alerts)

Instead of real-time alerts for everything, batch lower-priority items:

**Workflow: Competitor Intel - Daily Digest**

```
TRIGGER: Schedule - Every day at 8:00 AM
    │
    ▼
┌───────────────────────────────────────┐
│ Query: Get all intel records from     │
│ yesterday with priority != HIGH       │
└───────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────┐
│ IF count > 0:                         │
│                                       │
│ Send Email:                           │
│ Subject: "Daily Competitor Digest -   │
│           {{count}} changes detected" │
│                                       │
│ Body: List of all changes with links  │
└───────────────────────────────────────┘
```

### Weekly Review Reminder

```
TRIGGER: Schedule - Every Monday at 9:00 AM
    │
    ▼
┌───────────────────────────────────────┐
│ Create Task:                          │
│                                       │
│ Title: "Weekly Competitor Review"     │
│ Due: Today                            │
│ Assigned: Owner                       │
│ Description: "Review this week's      │
│ competitor intel and update strategy  │
│ if needed."                           │
└───────────────────────────────────────┘
```

### Team Assignment Workflow

For larger teams, route intel to specialists:

```
TRIGGER: Inbound Webhook
    │
    ▼
┌───────────────────────────────────────┐
│ IF watch_tag CONTAINS "pricing"       │
│   → Assign to: Sales Lead             │
│                                       │
│ IF watch_tag CONTAINS "careers"       │
│   → Assign to: HR / Recruiting        │
│                                       │
│ IF watch_tag CONTAINS "marketing"     │
│   → Assign to: Marketing Manager      │
│                                       │
│ ELSE                                  │
│   → Assign to: Operations             │
└───────────────────────────────────────┘
```

---

## Step 7: Slack Integration (Detailed)

### Set Up Slack Webhook

1. Go to **Slack** → **Your Workspace** → **Settings & Administration** → **Manage Apps**
2. Search for **Incoming Webhooks**
3. Click **Add to Slack**
4. Choose a channel (e.g., `#competitor-intel`)
5. Copy the webhook URL:
   ```
   https://hooks.slack.com/services/TXXXXXX/BXXXXXX/XXXXXXXXXXXXXXXX
   ```

### GHL Webhook Action to Slack

In your GHL workflow, add a **Webhook** action:

**URL:** Your Slack webhook URL

**Method:** POST

**Headers:**
```
Content-Type: application/json
```

**Body:**
```json
{
  "text": "🔔 *Competitor Intel Alert*",
  "blocks": [
    {
      "type": "header",
      "text": {
        "type": "plain_text",
        "text": "🔔 Competitor Change Detected"
      }
    },
    {
      "type": "section",
      "fields": [
        {
          "type": "mrkdwn",
          "text": "*Source:*\n{{intel_source}}"
        },
        {
          "type": "mrkdwn",
          "text": "*Priority:*\n{{priority}}"
        }
      ]
    },
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "*Detected:* {{current_date}} at {{current_time}}"
      }
    },
    {
      "type": "actions",
      "elements": [
        {
          "type": "button",
          "text": {
            "type": "plain_text",
            "text": "View Changes"
          },
          "url": "{{intel_diff}}"
        },
        {
          "type": "button",
          "text": {
            "type": "plain_text",
            "text": "View Page"
          },
          "url": "{{intel_url}}"
        }
      ]
    }
  ]
}
```

---

## Step 8: Advanced Routing

### Route by Competitor

```
IF watch_tag = "competitor-acme"
   → Add to Acme Corp monitoring pipeline
   → Tag as "acme-update"

IF watch_tag = "competitor-beta"
   → Add to Beta LLC monitoring pipeline
   → Tag as "beta-update"
```

### Route by Change Type

Use keywords in watch titles to route:

```
IF watch_title CONTAINS "Pricing"
   → Priority = HIGH
   → Category = "Pricing"
   → Notify Sales team

IF watch_title CONTAINS "Careers"
   → Priority = LOW
   → Category = "Hiring"
   → Notify HR (weekly batch)

IF watch_title CONTAINS "Homepage"
   → Priority = MEDIUM
   → Category = "Positioning"
   → Notify Marketing
```

### Route by Urgency Keywords

Check the diff content for urgent terms:

```
IF trigger_text CONTAINS "price drop" OR "discount" OR "sale"
   → Priority = HIGH
   → Send immediate SMS
   → Create urgent task

IF trigger_text CONTAINS "new feature" OR "announcing"
   → Priority = MEDIUM
   → Send Slack notification
```

---

## Workflow Templates (Copy & Paste)

### Template 1: Basic Alert Workflow

```
Name: Competitor Intel - Basic
Trigger: Inbound Webhook

Actions:
1. Send Email
   To: {{owner_email}}
   Subject: [Competitor] {{webhook.watch_title}} changed
   Body: View changes: {{webhook.diff_url}}

2. Create Task
   Title: Review competitor change - {{webhook.watch_title}}
   Due: Tomorrow
   Assigned: Owner
```

### Template 2: Priority-Based Workflow

```
Name: Competitor Intel - Priority Routing
Trigger: Inbound Webhook

Actions:
1. IF/ELSE: {{webhook.watch_tag}} contains "pricing"

   YES Branch:
   - Send SMS to Owner
   - Send Slack to #alerts
   - Create Task (Due: Today)

   NO Branch:
   - Send Email (batch in daily digest)
   - Log to Google Sheet
```

### Template 3: Full Logging Workflow

```
Name: Competitor Intel - Full Logging
Trigger: Inbound Webhook

Actions:
1. Create Record in "Competitor Intel" custom object
   - source: {{webhook.watch_title}}
   - url: {{webhook.watch_url}}
   - change_date: {{current_datetime}}
   - diff_link: {{webhook.diff_url}}
   - priority: (set based on tag logic)

2. Send Slack notification
   - Channel: #competitor-intel
   - Include quick action buttons

3. IF priority = HIGH:
   - Send SMS to owner
   - Create task due today
```

---

## Testing Your Setup

### 1. Test the Webhook
Use a tool like [RequestBin](https://requestbin.com) or [Webhook.site](https://webhook.site) to verify ChangeDetection.io is sending data correctly.

### 2. Trigger a Manual Change
1. Set up a watch on a page you control
2. Make a small change to that page
3. Click "Recheck" in ChangeDetection.io
4. Verify GHL receives the webhook
5. Verify workflow executes correctly

### 3. Verify Each Notification Channel
- [ ] Email arrives
- [ ] SMS arrives (if configured)
- [ ] Slack message posts
- [ ] Task is created
- [ ] Log entry is added

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Webhook not received in GHL | Check webhook URL is correct, GHL workflow is active |
| Wrong data in notifications | Verify field mappings (`{{webhook.field_name}}`) |
| Too many alerts | Add filters in ChangeDetection.io or GHL workflow |
| Slack not posting | Verify Slack webhook URL, check payload format |
| Tasks not creating | Check GHL user permissions, task assignment settings |
| Duplicate alerts | Add deduplication logic or check interval settings |

---

## Best Practices

1. **Start simple** — Basic email alerts first, add complexity later
2. **Test thoroughly** — Before going live, trigger test changes
3. **Don't over-notify** — Alert fatigue kills engagement
4. **Use batching** — Daily/weekly digests for low-priority items
5. **Log everything** — Even if you don't alert, keep a record
6. **Review regularly** — Are your alerts actionable? Adjust if not.
7. **Assign ownership** — Someone should own the intel review process

---

*Part of The Master's Edge Business Program*
*"Automate the grind. Elevate the human."*
