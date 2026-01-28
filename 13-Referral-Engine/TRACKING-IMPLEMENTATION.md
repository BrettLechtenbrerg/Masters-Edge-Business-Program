# Tracking Implementation Guide
## Lightweight Referral Link Tracking

---

**Time Required:** 30-60 minutes
**Technical Level:** Basic - Copy/paste friendly
**Options:** Vercel (recommended), Cloudflare Workers, or Self-hosted

---

## Overview

The tracking layer is a simple script that:
1. Receives clicks on referral links (`yourtracker.com/ref/CODE`)
2. Stores the referral code in a cookie
3. Redirects to your main landing page

This is intentionally lightweight—no database, no complex logic. Just cookie setting and redirects.

---

## Option 1: Vercel Deployment (Recommended)

### Why Vercel?
- Free tier is generous (100k requests/month)
- Automatic HTTPS
- Edge network (fast everywhere)
- Easy deployment
- No server management

### Step 1: Create the Project

Create a new folder on your computer:
```
referral-tracker/
├── api/
│   └── ref/
│       └── [code].js
├── package.json
└── vercel.json
```

### Step 2: Create package.json

```json
{
  "name": "referral-tracker",
  "version": "1.0.0",
  "private": true
}
```

### Step 3: Create vercel.json

```json
{
  "rewrites": [
    {
      "source": "/ref/:code",
      "destination": "/api/ref/:code"
    }
  ]
}
```

### Step 4: Create the Tracking Script

Create `api/ref/[code].js`:

```javascript
export default function handler(req, res) {
  // Get the referral code from the URL
  const { code } = req.query;

  // Validate the code (basic security)
  if (!code || code.length > 20 || !/^[a-zA-Z0-9-_]+$/.test(code)) {
    return res.redirect(302, process.env.REDIRECT_URL || 'https://yoursite.com');
  }

  // Configuration
  const REDIRECT_URL = process.env.REDIRECT_URL || 'https://yoursite.com';
  const COOKIE_DAYS = parseInt(process.env.COOKIE_DAYS) || 30;
  const COOKIE_NAME = process.env.COOKIE_NAME || 'ref_code';

  // Calculate cookie expiration
  const expires = new Date();
  expires.setDate(expires.getDate() + COOKIE_DAYS);

  // Set the cookie
  res.setHeader('Set-Cookie', [
    `${COOKIE_NAME}=${code}; Path=/; Expires=${expires.toUTCString()}; SameSite=Lax; Secure`
  ]);

  // Optional: Log the click (you could send to GHL webhook here)
  console.log(`Referral click: ${code} at ${new Date().toISOString()}`);

  // Redirect to the main site
  // Optionally append the code as a URL parameter too
  const redirectWithParam = `${REDIRECT_URL}?ref=${code}`;

  res.redirect(302, redirectWithParam);
}
```

### Step 5: Deploy to Vercel

1. Install Vercel CLI (if not installed):
   ```bash
   npm install -g vercel
   ```

2. Navigate to your project folder:
   ```bash
   cd referral-tracker
   ```

3. Deploy:
   ```bash
   vercel
   ```

4. Follow the prompts:
   - Link to existing project? No
   - What's your project name? `referral-tracker` (or your choice)
   - Which directory? `./` (current directory)

5. Set environment variables in Vercel Dashboard:
   - Go to your project settings
   - Add these environment variables:
   ```
   REDIRECT_URL = https://yoursite.com
   COOKIE_DAYS = 30
   COOKIE_NAME = ref_code
   ```

6. Redeploy:
   ```bash
   vercel --prod
   ```

### Step 6: Test Your Tracker

Visit: `https://your-project.vercel.app/ref/TEST123`

You should:
- Be redirected to your main site
- Have a cookie set with value `TEST123`
- See `?ref=TEST123` in the URL

---

## Option 2: Cloudflare Workers

### Why Cloudflare Workers?
- 100k requests/day free
- Extremely fast (edge computing)
- No cold starts
- Great for high-volume

### Step 1: Create Worker

Go to [Cloudflare Dashboard](https://dash.cloudflare.com) → Workers & Pages → Create Worker

### Step 2: Add Worker Code

```javascript
export default {
  async fetch(request, env) {
    const url = new URL(request.url);

    // Check if this is a referral link
    if (url.pathname.startsWith('/ref/')) {
      const code = url.pathname.replace('/ref/', '');

      // Validate code
      if (!code || code.length > 20 || !/^[a-zA-Z0-9-_]+$/.test(code)) {
        return Response.redirect(env.REDIRECT_URL || 'https://yoursite.com', 302);
      }

      // Configuration
      const REDIRECT_URL = env.REDIRECT_URL || 'https://yoursite.com';
      const COOKIE_DAYS = parseInt(env.COOKIE_DAYS) || 30;
      const COOKIE_NAME = env.COOKIE_NAME || 'ref_code';

      // Calculate expiration
      const expires = new Date();
      expires.setDate(expires.getDate() + COOKIE_DAYS);

      // Create redirect response with cookie
      const redirectUrl = `${REDIRECT_URL}?ref=${code}`;

      return new Response(null, {
        status: 302,
        headers: {
          'Location': redirectUrl,
          'Set-Cookie': `${COOKIE_NAME}=${code}; Path=/; Expires=${expires.toUTCString()}; SameSite=Lax; Secure`
        }
      });
    }

    // Default: redirect to main site
    return Response.redirect(env.REDIRECT_URL || 'https://yoursite.com', 302);
  }
};
```

### Step 3: Set Environment Variables

In Worker Settings → Variables:
```
REDIRECT_URL = https://yoursite.com
COOKIE_DAYS = 30
COOKIE_NAME = ref_code
```

### Step 4: Add Custom Domain (Optional)

1. Go to Worker → Triggers → Custom Domains
2. Add: `track.yoursite.com` or `go.yoursite.com`

---

## Option 3: Self-Hosted (Node.js/Express)

### For existing servers or VPS

Create `server.js`:

```javascript
const express = require('express');
const app = express();

const REDIRECT_URL = process.env.REDIRECT_URL || 'https://yoursite.com';
const COOKIE_DAYS = parseInt(process.env.COOKIE_DAYS) || 30;
const COOKIE_NAME = process.env.COOKIE_NAME || 'ref_code';
const PORT = process.env.PORT || 3000;

app.get('/ref/:code', (req, res) => {
  const { code } = req.params;

  // Validate
  if (!code || code.length > 20 || !/^[a-zA-Z0-9-_]+$/.test(code)) {
    return res.redirect(REDIRECT_URL);
  }

  // Set cookie
  res.cookie(COOKIE_NAME, code, {
    maxAge: COOKIE_DAYS * 24 * 60 * 60 * 1000,
    httpOnly: false, // Needs to be readable by JavaScript
    secure: process.env.NODE_ENV === 'production',
    sameSite: 'lax'
  });

  // Redirect
  res.redirect(`${REDIRECT_URL}?ref=${code}`);
});

app.listen(PORT, () => {
  console.log(`Referral tracker running on port ${PORT}`);
});
```

Run with:
```bash
npm install express
node server.js
```

---

## Landing Page Integration

### Reading the Cookie with JavaScript

Add this to your landing page to read the referral code:

```html
<script>
// Get referral code from cookie or URL
function getReferralCode() {
  // First check URL parameter
  const urlParams = new URLSearchParams(window.location.search);
  const urlRef = urlParams.get('ref');
  if (urlRef) return urlRef;

  // Then check cookie
  const cookies = document.cookie.split(';');
  for (let cookie of cookies) {
    const [name, value] = cookie.trim().split('=');
    if (name === 'ref_code') {
      return value;
    }
  }
  return null;
}

// Populate hidden form fields
document.addEventListener('DOMContentLoaded', function() {
  const refCode = getReferralCode();

  if (refCode) {
    console.log('Referral detected:', refCode);

    // Find all hidden fields named 'referred_by' and populate them
    document.querySelectorAll('input[name="referred_by"]').forEach(field => {
      field.value = refCode;
    });

    // Also set for GHL forms (they use different naming sometimes)
    document.querySelectorAll('input[name="referredBy"]').forEach(field => {
      field.value = refCode;
    });

    // For GHL embedded forms, you might need to wait for them to load
    setTimeout(() => {
      document.querySelectorAll('input[name="referred_by"], input[name="referredBy"]').forEach(field => {
        field.value = refCode;
      });
    }, 2000);
  }
});
</script>
```

### GHL Form Hidden Field Setup

In your GHL form builder:

1. Add a **Hidden Field**
2. Name it: `referred_by` (or match your custom field key)
3. Leave default value empty
4. The JavaScript above will populate it

---

## Advanced: Click Tracking to GHL

### Send Click Data to GHL Webhook

Modify your tracking script to also notify GHL of clicks:

```javascript
// In your Vercel function, add before redirect:

async function notifyGHL(code, ip, userAgent) {
  const GHL_WEBHOOK = process.env.GHL_WEBHOOK_URL;

  if (!GHL_WEBHOOK) return;

  try {
    await fetch(GHL_WEBHOOK, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        event: 'referral_click',
        referral_code: code,
        timestamp: new Date().toISOString(),
        ip_address: ip,
        user_agent: userAgent
      })
    });
  } catch (error) {
    console.error('GHL webhook error:', error);
  }
}

// Call it in your handler:
await notifyGHL(code, req.headers['x-forwarded-for'], req.headers['user-agent']);
```

Then create a GHL workflow triggered by this webhook to:
- Log the click
- Update affiliate's click count (if you want to track clicks, not just conversions)

---

## Testing Your Setup

### Test Checklist

1. **Basic Redirect Test**
   - [ ] Visit `yourtracker.com/ref/TEST123`
   - [ ] Confirm redirect to your main site
   - [ ] Confirm `?ref=TEST123` in URL

2. **Cookie Test**
   - [ ] After redirect, open browser DevTools
   - [ ] Go to Application → Cookies
   - [ ] Confirm `ref_code` cookie exists with value `TEST123`

3. **Form Population Test**
   - [ ] With cookie set, go to a form page
   - [ ] Open DevTools, find the hidden field
   - [ ] Confirm it has the referral code value

4. **GHL Submission Test**
   - [ ] Submit a form with referral code
   - [ ] Check GHL contact created
   - [ ] Confirm `referred_by` field is populated
   - [ ] Confirm workflow triggered

5. **End-to-End Test**
   - [ ] Clear cookies
   - [ ] Click referral link
   - [ ] Fill out and submit form
   - [ ] Check affiliate's referral count incremented
   - [ ] Check notifications sent

---

## Referral Link Formats

### Your affiliates will share links like:

**Standard Format:**
```
https://track.yoursite.com/ref/JOHN123
```

**With Custom Domain:**
```
https://go.yoursite.com/ref/JOHN123
```

**Branded Format (if you configure it):**
```
https://yoursite.com/partner/JOHN123
```

### Give Affiliates Their Link

Template email/message:
```
Your unique referral link is:
https://track.yoursite.com/ref/{{contact.referral_code}}

Share this link on social media, email, or anywhere!
When someone clicks and becomes a customer, you earn {{contact.commission_rate}}%.
```

---

## Troubleshooting

### Cookie Not Being Set
- Check if your site uses HTTPS (required for Secure cookies)
- Verify SameSite settings
- Check browser privacy settings

### Form Field Not Populating
- Verify JavaScript is running (check console)
- Ensure field name matches exactly
- Add delay for dynamically loaded forms

### Redirect Not Working
- Check environment variables are set
- Verify URL format is correct
- Check Vercel/Cloudflare logs

### Code Not Passing to GHL
- Confirm hidden field is in the form
- Check JavaScript console for errors
- Verify GHL form field name matches

---

## Security Best Practices

1. **Validate Referral Codes**
   - Only allow alphanumeric + dash/underscore
   - Set max length (20 chars is plenty)
   - Reject suspicious patterns

2. **Rate Limiting** (Advanced)
   - Limit clicks per IP per hour
   - Prevents click fraud

3. **Cookie Security**
   - Use Secure flag (HTTPS only)
   - Use SameSite=Lax or Strict
   - Reasonable expiration (30-90 days)

4. **Input Sanitization**
   - Never trust user input
   - Escape/encode before using

---

## Cost Analysis

| Platform | Free Tier | Cost if Exceeded |
|----------|-----------|------------------|
| **Vercel** | 100k requests/month | $20/month for 1M |
| **Cloudflare Workers** | 100k requests/day | $5/month for 10M |
| **Self-hosted** | Depends on hosting | VPS ~$5-20/month |

For most affiliate programs, the free tiers are more than sufficient.

---

*Part of The Master's Edge Business Program*
*Tier 3: Referral Engine - Tracking Implementation*
