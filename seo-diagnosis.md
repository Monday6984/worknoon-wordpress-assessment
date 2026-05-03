# SEO Diagnosis: Website Not Indexing After Sitemap Submission

## Scenario
A new Worknoon website has been launched and the sitemap has been submitted to
Google Search Console, but pages are still not appearing in Google's index.

---

## Diagnostic Framework

### Step 1 — Crawlability Tests

Before Google can index a page, it must first be able to crawl it.

**Tests to run:**
- Use the **URL Inspection Tool** in Google Search Console (GSC) on key pages.
  Check the "Coverage" status — look for "URL is not on Google" or crawl errors.
- Run `site:worknoon.com` in Google Search — if zero results, no pages are indexed.
- Use **Screaming Frog** or **Sitebulb** to crawl the site locally and identify:
  - Broken internal links (4xx errors)
  - Redirect chains (301 → 301 → 200 slows crawlers)
  - Orphan pages (not linked from anywhere)
- Test crawl access using `curl -A "Googlebot" https://worknoon.com/` to simulate
  Googlebot and confirm the server responds with `200 OK`.

**Common blockers:**
- Hosting server returning 5xx errors
- Cloudflare or firewall blocking Googlebot IP ranges
- Password protection enabled on staging (very common)

---

### Step 2 — Canonical Checks

Canonical issues cause Google to index the wrong version or ignore pages entirely.

**Tests to run:**
- Inspect the `<link rel="canonical">` tag on each key page using browser DevTools
  or the GSC URL Inspection Tool.
- Ensure canonicals are **self-referencing** on unique pages (e.g.,
  `https://worknoon.com/services/` should point to itself).
- Check for **conflicting canonicals** — e.g., a page that canonicalizes to a
  different URL than what's in the sitemap.
- Check for `www` vs non-`www` inconsistencies. All pages should resolve to one
  version, and that version should be canonical.

**What to fix:**
- If using Yoast SEO or Rank Math, verify the canonical setting per page.
- Ensure the preferred domain is set in GSC under Settings → Preferred Domain.

---

### Step 3 — Robots.txt & No-Index Audit

This is the most common reason a new site fails to index.

**Robots.txt check:**
- Visit `https://worknoon.com/robots.txt` and inspect the rules.
- Look for `Disallow: /` which blocks all crawling.
- A common WordPress staging mistake leaves this rule active after going live.
- Use GSC's **robots.txt Tester** to verify Googlebot access per URL.

**No-index audit:**
- In WordPress: Go to **Settings → Reading** and check if
  **"Discourage search engines from indexing this site"** is checked. Uncheck it.
- Inspect individual pages for `<meta name="robots" content="noindex">` tags.
- In Yoast/Rank Math: Check that pages aren't set to "noindex" at the page level
  or category level.
- Check HTTP response headers: `curl -I https://worknoon.com/` and look for
  `X-Robots-Tag: noindex`.

---

### Step 4 — Sitemap Structure Issues

A malformed or incomplete sitemap confuses crawlers.

**Tests to run:**
- Visit `https://worknoon.com/sitemap.xml` — it should load without errors.
- Validate the sitemap using [XML Sitemaps Validator](https://www.xml-sitemaps.com/validate-xml-sitemap.html).
- Check that the sitemap:
  - Lists all important URLs (homepage, services, blog, about, contact)
  - Does **not** include noindexed pages, paginated archives, or tag/category pages
    (unless intentional)
  - Uses absolute URLs (not relative)
  - Has correct `<lastmod>` dates
- In GSC → Sitemaps: confirm the sitemap was submitted, shows "Success", and
  reflects the expected number of discovered URLs.

**What to fix:**
- If using Yoast SEO, go to SEO → General → Features and ensure XML Sitemaps is ON.
- Regenerate the sitemap and resubmit in GSC.
- Exclude low-value pages (tags, author archives) from the sitemap.

---

### Step 5 — Page Speed & Indexing Blockers

Google's crawler has a crawl budget. Slow pages get crawled less frequently.

**Tests to run:**
- Run a **Core Web Vitals** test via Google PageSpeed Insights
  (`https://pagespeed.web.dev/`).
- Check for render-blocking JavaScript or CSS in the PageSpeed report.
- Use GSC → Core Web Vitals report to identify poor URLs.
- Check if critical content is **JavaScript-rendered** — Googlebot can struggle with
  JS-heavy pages. Use the "Live Test" in GSC URL Inspection to see how Google
  renders the page.

**Common blockers:**
- Page takes >5s to load → lower crawl priority assigned by Google
- Key content inside JS that isn't pre-rendered (SSR/SSG not configured)
- Images not compressed, causing slow TTFB
- Too many redirect hops before reaching the final URL

**What to fix:**
- Install **LiteSpeed Cache** or **WP Rocket** for caching.
- Use **Smush** or **ShortPixel** for image compression.
- Enable lazy loading for images below the fold.
- Minimize plugins — each adds HTTP requests.

---

### Step 6 — Google Search Console Debugging Steps

GSC is the primary tool for diagnosing indexing issues.

**Step-by-step GSC workflow:**

1. **Verify ownership** — Ensure the site is properly verified in GSC (HTML tag,
   DNS record, or Google Analytics method).

2. **Check Coverage Report** (Index → Coverage):
   - **Error:** Pages with crawl errors — fix these first.
   - **Valid with warnings:** Submitted but has issues (e.g., canonical mismatch).
   - **Excluded:** Pages Google chose not to index — investigate the reason shown.
   - **Crawled but not indexed:** Google crawled but decided not to index —
     usually a content quality or duplicate content issue.

3. **Use URL Inspection Tool:**
   - Paste a key URL and click "Test Live URL".
   - Check: Is it crawlable? Is it indexable? What does rendered HTML look like?
   - Click "Request Indexing" for priority pages after fixing issues.

4. **Check Manual Actions** (Security & Manual Actions → Manual Actions):
   - A manual penalty blocks indexing entirely. Resolve any penalties listed.

5. **Check Security Issues** (Security & Manual Actions → Security Issues):
   - Hacked sites are deindexed by Google. Fix any flagged issues immediately.

6. **Monitor Crawl Stats** (Settings → Crawl Stats):
   - View how often Googlebot visits the site.
   - If crawl rate is very low (< 5 pages/day for a new site), there may be a
     crawlability or response time issue.

---

## Quick Diagnostic Checklist

- [ ] `robots.txt` does not block Googlebot
- [ ] WordPress "Discourage search engines" setting is OFF
- [ ] No pages have `noindex` meta tags unintentionally
- [ ] Canonical tags are correct and consistent
- [ ] Sitemap is valid, submitted, and shows "Success" in GSC
- [ ] Site loads in under 3 seconds (PageSpeed score > 70)
- [ ] No manual actions or security issues in GSC
- [ ] Server returns `200 OK` for all key pages
- [ ] Site is not behind a login/password wall
- [ ] GSC ownership is verified
