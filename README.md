# Worknoon WordPress Assessment

**Submitted by:** [Your Name]
**Role:** WordPress Developer (SEO + Systems Specialist)
**Submission Date:** [Date]

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
2. Upload the `/theme/worknoon-child` folder contents as a child theme via
   **Appearance → Themes → Add New → Upload Theme**.
3. Activate the child theme.

### Landing Page Setup

1. Go to **Pages → Add New**, name it "Home", and set it as the static front page
   under **Settings → Reading**.
2. Open the page in Elementor and build using the provided template or from scratch.
3. Sections to include:
   - Hero section with headline and CTA button
   - Services section (4-column grid)
   - Testimonials section
   - Contact Form (insert FluentForms shortcode)

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

- **Cloudflare + LiteSpeed Cache** provides two caching layers — edge and server — maximising response speed globally.
- **Yoast SEO** was chosen over Rank Math for its maturity and reliability in schema output, especially for Organization and Person entities.
- **JSON-LD schema** is injected via `wp_head` to keep it separate from page content, making it easier to maintain and update.

## Section F: Project Reflection

### Problem Overview

The goal was to build a functional, optimised WordPress landing page that demonstrates both development skill and SEO/entity strategy. Beyond just building a page, the challenge was to think holistically — ensuring the site is crawlable, indexable, schema-compliant, and architected for scale.

### Approach

I structured the project in two parallel tracks:

1. **Technical track:** WordPress setup → theme → Elementor page build → plugin stack → analytics integration.
2. **SEO/Entity track:** JSON-LD schema creation → Knowledge Panel strategy → indexing diagnosis → short answer documentation.

This parallel approach ensured that SEO considerations were baked in from the start rather than retrofitted.

### Key Decisions & Why

**Elementor over Gutenberg:**
Elementor was chosen for speed of visual development and the ability to produce a polished, responsive layout without writing custom CSS for every breakpoint.
For a client-facing deliverable built under time pressure, Elementor's drag-and-drop system is more pragmatic.

**JSON-LD over Microdata:**
JSON-LD is Google's recommended format for structured data. It keeps schema separate from HTML, making it easier to validate, update, and maintain. Microdata requires embedding attributes directly into HTML, which becomes messy with page builders like Elementor.

**LiteSpeed Cache + Autoptimize over WP Rocket:**
WP Rocket is premium (~$59/year). The free stack of LiteSpeed Cache + Autoptimize achieves comparable results for caching and asset minification, making it a better recommendation for clients with cost constraints.

**Yoast SEO for schema:**
While Rank Math offers more schema types in its free version, Yoast's Organization and Person schema implementation is more stable and widely tested, particularly for Knowledge Graph entity building.

### Tradeoffs Considered

| Decision               | Tradeoff                                                                    |
| ---------------------- | --------------------------------------------------------------------------- |
| Elementor Free         | No Theme Builder; limited design tokens vs Pro                              |
| LiteSpeed Cache (free) | Requires LiteSpeed server; less effective on Apache                         |
| Yoast SEO              | Less schema variety vs Rank Math free; heavier JS footprint                 |
| Static WordPress       | Simpler to deploy vs headless (Next.js), but less flexible for SPA-style UX |

### Challenges & Resolutions

**Challenge:** Ensuring schema `sameAs` links are consistent across all files without a live Wikidata or social profile to reference.
**Resolution:** Used placeholder URLs in a realistic format and documented the update process clearly so the client can swap them in without technical knowledge.

**Challenge:** Demonstrating indexing troubleshooting without a broken live site.
**Resolution:** Documented the diagnostic process as a systematic, step-by-step runbook that covers all real-world failure scenarios — applicable regardless of the specific site.

## Demo Video

[Link to Loom / YouTube / Google Drive video]
