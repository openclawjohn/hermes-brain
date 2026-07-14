# BeaNel (beanel.com) — Site Style Guide & Fix Plan

## Current Status
- **Type:** IP address lookup tool + blog
- **Theme:** Twenty Twenty-Five (dark mode variant)
- **CMS:** WordPress + Elementor
- **Hosting:** cp47-jhb.za-dns.com (SAME SERVER — currently down)

## Current Content
| Type | Count |
|------|-------|
| Blog posts | 16 |
| Pages | 8 |
| Categories | 6 |
| **Total indexable** | **30** |

## Issues Found

### 1. CSS Broken — Missing Semicolons
The `beanel-key-facts` CSS in the mu-plugin has `\n` instead of `;` between properties. The entire Key Facts box styling is broken.

**Fix:** Replace `\n` with `;` in the inline CSS, or rewrite the full block:
```css
.beanel-key-facts {
    background: linear-gradient(135deg, #e8f5e9, #e0f2f1);
    border: 1px solid #80cbc4;
    border-radius: 12px;
    padding: 20px 24px;
    margin: 20px 0;
    font-size: 15px;
    line-height: 1.7;
    color: #1a1a2e;
}
.beanel-key-facts h2 {
    margin-top: 0;
    font-size: 18px;
    color: #004d40;
    border-bottom: 2px solid #00897b;
    padding-bottom: 8px;
    margin-bottom: 14px;
}
.beanel-key-facts li {
    padding-left: 22px;
    margin-bottom: 8px;
    position: relative;
    color: #1a1a2e;
}
```

### 2. No Featured Images on Any Post
All 15 blog posts (excluding the /blog/ page itself) have 0 featured images.

### 3. Contact Page Redirect
- `/contact/` returns 301 → `/contact-us/` (should either fix the nav link or add a proper redirect)

### 4. FAQ Page
- `/faq/` returns 404, but `/ip-address-faq/` works (nav already links to the correct URL)

### 5. Text Contrast
- Dark background theme but some text is light blue on dark navy — needs fixing
- Blog listing has light grey text on dark cards — hard to read

## Content Plan (Target: 50+ Indexable URLs)

### Additional Blog Posts Needed (10-15 more):
1. What is My IP Address? Complete Guide
2. How to Hide Your IP Address in South Africa
3. IPv4 vs IPv6: Complete Comparison for SA Users
4. How to Change Your IP Address
5. What is a Proxy Server vs VPN?
6. Best VPNs for South Africa in 2026
7. How to Test if Your VPN is Working
8. Understanding IP Address Classes
9. Public vs Private IP Addresses Explained
10. What is a Dynamic IP Address?
11. How to Set Up a Static IP at Home
12. IP Address Conflict: Causes and Fixes
13. What is a Dedicated IP Address?
14. Difference Between IP and MAC Address

### Images Needed
- All posts need featured images (CC0 from Pexels/Unsplash)
- Topics: network diagrams, server racks, wifi icons, SA maps with IP markers

## Deploy Plan
Once server is back:
1. Fix CSS (missing semicolons in beanel-key-facts)
2. Create 15 new blog posts with featured images (PHP script via FTP)
3. Add featured images to existing 15 posts
4. Fix /contact/ redirect
5. Verify sitemap and all pages
