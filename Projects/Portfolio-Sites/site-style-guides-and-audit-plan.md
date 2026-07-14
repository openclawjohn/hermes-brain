# Portfolio Site Style Guides & Audit Plan

## Sites Inventory

| Site | Type | Server | Status | Content (posts/pages/cats) |
|------|------|--------|--------|---------------------------|
| whippetqr.com | QR code generator | cp47 | DOWN | 35/18/3 ✅ |
| beanel.com | IP lookup tool | cp47 | DOWN | 16/8/6 |
| howzitza.co.za | SA culture/games | cp47 | DOWN | unknown |
| sumza.co.za | SA calculators | cp47 | DOWN | unknown |
| zadocs.co.za | Legal templates | cp47 | DOWN | unknown |
| saymyname.co.za | Name generator | cp47 | DOWN | unknown |
| 5minutes.co.za | Silly games | cp47 | DOWN | unknown |

---

## 1. WhippetQR (whippetqr.com)

### Style
- Light theme, blue accent (#1a237e headings)
- Key Facts box: light blue gradient #f0f4ff → #e8ecf8
- QR code generator as main tool
- Dark footer

### Completed ✅
- 35 blog posts with featured images
- WordPress native sitemap (Rank Math was crashing)
- Privacy policy slug fixed
- Duplicate meta tag fixed
- Key Facts CSS contrast fixed (color: #1a1a2e)
- Blog page + nav link (pending server restart)

### Remaining
- [ ] Run blog page creation PHP script (uploaded: wp-blog.php, wp-blog2.php, wp-blog3.php)
- [ ] Add "Blog" to navigation menu
- [ ] Submit sitemap to Google Search Console
- [ ] Re-enable LiteSpeed Cache when user is ready

---

## 2. BeaNel (beanel.com)

### Style
- Dark theme (navy/charcoal)
- Green accent (#004d40 headings, #00897b borders)
- Key Facts box: light green gradient #e8f5e9 → #e0f2f1
- IP lookup tool as main functionality
- Blog with 16 privacy/security posts

### Issues
- Key Facts CSS has missing semicolons (broken entirely)
- No featured images on any of 15 blog posts
- /contact/ 301 redirects to /contact-us/
- Light text on dark background contrast issues
- Blog listing has no thumbnails

### Plan
- [ ] Fix Key Facts CSS in mu-plugin
- [ ] Add featured images to all 15 existing posts
- [ ] Create 15 new blog posts (IP/networking topics)
- [ ] Fix contact redirect or nav link
- [ ] Create blog thumbnail images

---

## 3. HowzitZA (howzitza.co.za)

### Style
- SA culture/games focus
- User says: **specific needs for particular images**
- Needs full audit when server is back

### Plan
- [ ] Full audit (content volume, sitemap, meta, pages)
- [ ] Identify specific image requirements from user
- [ ] Create content plan
- [ ] Add blog posts with SA-specific images

---

## 4. SumZA (sumza.co.za)

### Style
- SA calculators
- Needs full audit

### Plan
- [ ] Full audit
- [ ] Create content plan
- [ ] Add blog posts

---

## 5. ZADocs (zadocs.co.za)

### Style
- Legal templates
- SA legal documents
- Needs full audit (may already have good content via zadocs workflow)

### Plan
- [ ] Full audit
- [ ] Check existing zadocs workflow skills
- [ ] Add content if needed

---

## 6. SayMyName (saymyname.co.za)

### Style
- Name pronunciation generator
- SA names focus
- Needs full audit

### Plan
- [ ] Full audit
- [ ] Create content plan
- [ ] Add blog posts

---

## 7. 5minutes (5minutes.co.za)

### Style
- Silly/fun games
- Light-hearted content
- Has 13 blog posts, 8 pages (from memory)
- Needs deep audit

### Plan
- [ ] Full audit
- [ ] Content volume check
- [ ] Add more posts if needed

---

## AdSense Checklist (Every Site)

- [ ] 30+ blog posts (1,500+ words each)
- [ ] All posts have featured images
- [ ] All posts have in-content images
- [ ] Essential pages: About, Contact, Privacy, Terms (all HTTP 200)
- [ ] Clean sitemap working
- [ ] No duplicate meta tags
- [ ] No broken slugs (-2, -3 suffixes)
- [ ] Proper categories with content
- [ ] Proper tags
- [ ] Blog page accessible from navigation
- [ ] Text contrast meets WCAG standards
- [ ] Categories have content (not empty)

---

## Monitoring

Cron job active: `cp47-server-monitor` — checks every 5 minutes
