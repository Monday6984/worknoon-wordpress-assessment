# Worknoon WordPress Assessment

**Submitted by:** Monday Ofem
**Role:** WordPress Developer (SEO + Systems Specialist)
**Submission Date:** 3/05/2026

---

## Repository Structure

```
worknoon-wordpress-assessment/
├── organization-schema.json       # Section B: Organization Schema
├── person-schema.json             # Section B: Founder Person Schema
├── website-schema.json            # Section B: Website + Logo + sameAs Schema
├── knowledge-panel-strategy.md   # Section C: Knowledge Panel Strategy
├── seo-diagnosis.md               # Section D: SEO Indexing Troubleshooting
├── short-answers.md               # Section E: Short Answer Questions
├── theme/                         # Section A: Custom child theme files
│   ├── style.css
│   ├── functions.php
│   └── screenshot.png
└── README.md                      # This file
```

---

## Section A: Setup Instructions

### Prerequisites

- PHP 8.1+
- WordPress 6.4+
- MySQL 5.7+ or MariaDB 10.4+
- A live or staging server (e.g., Cloudways, Kinsta, or cPanel hosting)

### WordPress Installation

1. Download WordPress from [wordpress.org](https://wordpress.org/download/)
2. Upload to your server's `public_html` directory
3. Create a MySQL database and user in your hosting control panel
4. Run the WordPress installer at `https://yourdomain.com/wp-admin/install.php`

### Required Plugins

Install the following from **Plugins → Add New** in the WordPress admin:

| Plugin             | Purpose                       |
| ------------------ | ----------------------------- |
| Elementor          | Page builder for landing page |
| WPForms Lite       | Contact form                  |
| Yoast SEO          | Schema markup + sitemap       |
| LiteSpeed Cache    | Page speed optimization       |
| Smush              | Image compression             |
| Autoptimize        | CSS/JS minification           |
| Site Kit by Google | Google Analytics integration  |

### Theme Setup

1. Install and activate the **Hello Elementor** theme.
2. Upload the `/theme/` folder contents as a child theme via
   **Appearance → Themes → Add New → Upload Theme**.
3. Activate the child theme.

### Landing Page Setup

1. Go to **Pages → Add New**, name it "Home", and set it as the static front page
   under **Settings → Reading**.
2. Open the page in Elementor and build using the provided template or from scratch.
3. Sections to include:
   - Hero section with headline and CTA button
   - Services section (3-column grid)
   - Testimonials section
   - Contact Form (insert WPForms shortcode)

### Schema Markup Integration

Add the JSON-LD schema files to WordPress using one of these methods:

**Option A (Recommended):** Use Yoast SEO's schema settings (Organization, Person)
under **SEO → Search Appearance**.

**Option B (Manual):** Add the JSON-LD from `organization-schema.json`,
`person-schema.json`, and `website-schema.json` into the `<head>` via
`functions.php`:

```php
function worknoon_add_schema() {
    if ( is_front_page() ) {
        echo '<script type="application/ld+json">';
        echo file_get_contents( get_template_directory() . '/schema/organization-schema.json' );
        echo '</script>';
    }
}
add_action( 'wp_head', 'worknoon_add_schema' );
```

### Analytics Setup

1. Install **Site Kit by Google** plugin.
2. Connect your Google account and link Google Analytics 4 (GA4).
3. Verify the tracking tag is firing via browser DevTools → Network tab
   (filter for `gtag`).

---

## Tools & Technologies Used

| Tool / Technology     | Purpose                                  |
| --------------------- | ---------------------------------------- |
| WordPress 6.4         | Core CMS                                 |
| Elementor Free        | Visual page builder                      |
| WPForms Lite          | Contact form management                  |
| Yoast SEO             | On-page SEO, schema, sitemap             |
| LiteSpeed Cache       | Server-side caching                      |
| Smush                 | Lossless image compression               |
| Autoptimize           | Asset minification                       |
| Site Kit by Google    | GA4 analytics integration                |
| Cloudflare (free)     | CDN + DDoS protection                    |
| JSON-LD               | Structured data / schema format          |
| Google Search Console | Indexing monitoring + sitemap submission |
| Screaming Frog        | Technical SEO auditing                   |

---

## System Architecture Overview

```
User Browser
     │
     ▼
Cloudflare CDN (DNS + Edge Cache)
     │
     ▼
Hosting Server (Apache/LiteSpeed)
     │
     ├── LiteSpeed Cache (Serves cached HTML if available)
     │
     ├── WordPress Core (PHP)
     │    ├── Elementor (Page Rendering)
     │    ├── WPForms (Form Handling → Email)
     │    ├── Yoast SEO (Schema Output + Sitemap)
     │    └── Site Kit (GA4 Tag Injection)
     │
     └── MySQL Database (Posts, Pages, Settings, Form Submissions)

External Services:
     ├── Google Analytics 4 (Behaviour Tracking)
     ├── Google Search Console (Indexing + Crawl Monitoring)
     └── Google Knowledge Graph (Entity Recognition via Schema)
```

**Key design decisions:**

- **Cloudflare + LiteSpeed Cache** provides two caching layers — edge and server —
  maximising response speed globally.
- **Yoast SEO** was chosen over Rank Math for its maturity and reliability in
  schema output, especially for Organization and Person entities.
- **JSON-LD schema** is injected via `wp_head` to keep it separate from page
  content, making it easier to maintain and update.

---
