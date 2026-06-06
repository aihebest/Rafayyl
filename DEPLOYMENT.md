# Rafayyl Engineering — Deployment Guide
## Production-Ready Website Deployment

---

## 📁 File Structure

```
rafayyl-engineering/
├── index.html          # Main website (186KB, all sections)
├── admin.html          # Admin dashboard (52KB, password protected)
├── sitemap.xml         # SEO sitemap
├── robots.txt          # Search engine directives
├── og-image.jpg        # Open Graph image (1200×630px) — CREATE THIS
├── logo.png            # Company logo PNG — CREATE THIS
└── DEPLOYMENT.md       # This file
```

---

## 🚀 Recommended Tech Stack (Static Hosting)

| Tier | Provider | Cost | CDN | SSL |
|------|----------|------|-----|-----|
| **Free** | Netlify / Vercel | Free | ✅ | ✅ Auto |
| **Budget** | Cloudflare Pages | Free | ✅ Fastest | ✅ Auto |
| **Professional** | AWS S3 + CloudFront | ~$5/mo | ✅ Global | ✅ ACM |
| **Enterprise** | Azure Static Web Apps | ~$9/mo | ✅ | ✅ Auto |

**Recommendation:** Cloudflare Pages (free tier) — fastest CDN globally, zero config.

---

## ⚙️ Environment Variables / Configuration

Update these values in `index.html` before deployment:

```html
<!-- Line ~50: Replace GA4 Measurement ID -->
gtag('config', 'G-XXXXXXXXXX');  →  gtag('config', 'G-YOUR_ID');

<!-- WhatsApp number (search for wa.me) -->
href="https://wa.me/2348000000000"  →  href="https://wa.me/234XXXXXXXXXX"

<!-- Contact email (FormSubmit) -->
action="https://formsubmit.co/contact@rafayyl.com"  (already configured)

<!-- Google Maps (update to exact address) -->
Replace the Maps iframe src with your exact office coordinates
```

---

## 📧 Form Handling Setup

The contact form uses **FormSubmit.co** (free, no backend required).

**Activation steps:**
1. Deploy the site
2. Submit the contact form once
3. FormSubmit sends a confirmation email to `contact@rafayyl.com`
4. Click the confirmation link → forms start delivering

**Alternative:** Replace FormSubmit with [Formspree.io](https://formspree.io) or [EmailJS](https://emailjs.com)

---

## 🔐 Admin Dashboard

- **URL:** `https://www.rafayyl.com/admin.html`
- **Default credentials:** `admin` / `rafayyl2024`
- **CHANGE PASSWORD** immediately after first login (Settings → Change Password)
- Data is stored in browser `localStorage` — no server needed
- For production, consider upgrading to a backend (see Phase 2 below)

---

## 🌐 Custom Domain Setup (Netlify Example)

```bash
# 1. Install Netlify CLI
npm install -g netlify-cli

# 2. Deploy
cd rafayyl-engineering/
netlify deploy --prod --dir=.

# 3. Add custom domain in Netlify dashboard
# Domain: rafayyl.com
# Add CNAME: www → your-site.netlify.app
# Add A record: @ → 75.2.60.5 (Netlify IP)
```

---

## 📊 Google Analytics 4 Setup

1. Go to [analytics.google.com](https://analytics.google.com)
2. Create account → Property → "Rafayyl Engineering Website"
3. Choose "Web" → enter `rafayyl.com`
4. Copy Measurement ID (format: `G-XXXXXXXXXX`)
5. Replace `G-XXXXXXXXXX` in `index.html`

---

## 🗺️ Google Maps Integration

Replace the iframe src in the contact section:
1. Go to [Google Maps](https://maps.google.com)
2. Search your exact office address
3. Click Share → Embed a map → Copy iframe code
4. Replace the existing iframe src in `index.html`

---

## 🔍 Google Search Console

1. Go to [search.google.com/search-console](https://search.google.com/search-console)
2. Add property → URL prefix → `https://www.rafayyl.com`
3. Verify ownership (HTML file method or DNS)
4. Submit sitemap: `https://www.rafayyl.com/sitemap.xml`

---

## ⚡ Performance Optimisation Checklist

- [ ] Create & add `og-image.jpg` (1200×630px) — required for social sharing
- [ ] Create & add `logo.png` for JSON-LD schema
- [ ] Compress images with [Squoosh](https://squoosh.app) (WebP format)
- [ ] Enable Brotli compression on your host (Cloudflare does this automatically)
- [ ] Set cache headers: `max-age=31536000` for static assets
- [ ] Enable HTTP/2 on your server
- [ ] Run [PageSpeed Insights](https://pagespeed.web.dev/) and fix any issues

---

## 🛡️ Security Headers (Netlify `_headers` file)

Create a `_headers` file in the root:

```
/*
  X-Frame-Options: DENY
  X-XSS-Protection: 1; mode=block
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: geolocation=(), microphone=(), camera=()
  Content-Security-Policy: default-src 'self' https: 'unsafe-inline' 'unsafe-eval'; img-src * data:
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

/admin.html
  X-Robots-Tag: noindex, nofollow
```

---

## 📱 WhatsApp Business Setup

1. Create a [WhatsApp Business](https://business.whatsapp.com/) account
2. Use your official business number
3. Update the WhatsApp button href: `https://wa.me/234XXXXXXXXXX?text=YOUR_MESSAGE`
4. Set up WhatsApp Business profile (logo, description, hours)

---

## 🔄 Phase 2 — Backend Upgrade (Future)

When ready to scale, upgrade to:

**Tech Stack:**
- **Framework:** Next.js 14 (React) or Nuxt 3 (Vue)
- **Database:** PostgreSQL (via Supabase — free tier)
- **CMS:** Strapi or Payload CMS (self-hosted)
- **Email:** Resend.com or SendGrid
- **Hosting:** Vercel (frontend) + Railway (backend)
- **Auth:** NextAuth.js

**Database Schema:**
```sql
-- Projects
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  title TEXT NOT NULL,
  category TEXT,
  description TEXT,
  client TEXT,
  location TEXT,
  year TEXT,
  budget TEXT,
  status TEXT DEFAULT 'Active',
  featured BOOLEAN DEFAULT FALSE,
  tags TEXT[],
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Inquiries
CREATE TABLE inquiries (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  phone TEXT,
  service TEXT,
  budget TEXT,
  message TEXT,
  status TEXT DEFAULT 'New',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Career Applications
CREATE TABLE applications (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  phone TEXT,
  position TEXT,
  cover_note TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## 🔁 CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/deploy.yml
name: Deploy to Netlify
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Netlify
        uses: netlify/actions/cli@master
        with:
          args: deploy --prod --dir=.
        env:
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
```

---

## 📋 Pre-Launch Checklist

- [ ] Update phone number (+234 800 000 0000 → real number)
- [ ] Update email (contact@rafayyl.com → real email)
- [ ] Update WhatsApp number in floating button
- [ ] Replace GA4 placeholder ID with real Measurement ID
- [ ] Update Google Maps to exact office address
- [ ] Update LinkedIn/Twitter/Facebook/Instagram URLs to real profiles
- [ ] Update canonical URL meta tag to production URL
- [ ] Add og-image.jpg (1200×630px)
- [ ] Change admin panel password
- [ ] Test all forms (contact, career, newsletter)
- [ ] Test on mobile (iOS Safari + Android Chrome)
- [ ] Submit sitemap to Google Search Console
- [ ] Set up Google Analytics 4
- [ ] Enable cookie consent for GDPR/NDPR compliance
- [ ] Test WhatsApp button on mobile

---

## 📞 Support

Rafayyl Engineering IT Team: contact@rafayyl.com

