# ContactLens.pk — Cloudflare Pages Deploy Bundle

## What this is

This directory is the complete `dist/` output for ContactLens.pk static site rebuild.
133 pages + 720+ images + SEO infrastructure files, ready for Cloudflare Pages.

## Deploy via Direct Upload (fastest, no Git needed)

1. Sign in to Cloudflare → Workers & Pages → Create application → Pages → **Upload assets**
2. Project name: `contactlens-pk` (or anything you like)
3. **Zip the *contents* of this folder** (not the folder itself — so `index.html` is at the zip root)
4. Drag-and-drop the zip onto the upload area
5. Cloudflare extracts and deploys in ~2 minutes
6. You get a `https://contactlens-pk.pages.dev` preview URL immediately

## Deploy via Git (recommended for ongoing updates)

1. Push the entire `contactlens-static/` source (not just `dist/`) to a GitHub or GitLab repo
2. Cloudflare → Workers & Pages → Create application → Pages → **Connect to Git**
3. Select your repo
4. Build settings:
   - Framework preset: **None**
   - Build command: `python3 generators/02_process_images.py && python3 generators/03_build_site.py`
   - Build output directory: `dist`
   - Root directory: `/`
5. Add environment variable if needed: `PYTHON_VERSION=3.12`
6. Save and deploy
7. Cloudflare rebuilds on every git push

## Custom domain setup (contactlens.pk)

1. In Cloudflare Pages project → Custom domains → Set up a custom domain
2. Enter `contactlens.pk` and `www.contactlens.pk`
3. Cloudflare auto-creates the DNS records if `contactlens.pk` is already on Cloudflare DNS
4. If using an external DNS provider, add the CNAME records Cloudflare shows you
5. SSL certificate auto-provisions in 5–15 minutes

## What is in this bundle

*Updated with branded SVG placeholders for missing products and hero banner OG images.*


```
dist/
├── index.html                          # Homepage (42 KB, targets "weak eyesight lens")
├── 404.html                            # Friendly error page
├── _redirects                          # 21 redirects (cart/, checkout/, etc → /)
├── _headers                            # Security + caching headers
├── robots.txt                          # Allow all, points to sitemap
├── sitemap.xml                         # 134 URLs
├── static/
│   ├── style.css                       # 12 KB stylesheet
│   ├── logo.png                        # CONTACTLENS.PK wordmark
│   └── favicon.png
├── images/products/
│   └── {93 product slugs}/             # main + size variants per product
└── {133 page directories}/
    └── index.html
```

### Pages by type

- 1 homepage
- 96 product pages (including 3 with SVG placeholders pending real photos)
- 16 category pages (`/colored-lenses/`, `/bella-contact-lenses/`, etc.)
- 12 color hub pages (new: `/hazel-lenses/`, `/grey-lenses/`, `/blue-lenses/`, etc.)
- 4 brand sub-hub pages (new: `/dahab-lenses/`, `/bella-elite-collection/`, etc.)
- 2 feature hub pages (new: `/eyesight-lenses/`, `/deals/`)
- 6 static pages (about-us, contact-us, privacy-policy, refund_returns, shop, blog)
- 1 error page (404.html)

### Schemas emitted

- Product, AggregateOffer (on product pages)
- BreadcrumbList (everywhere except homepage)
- FAQPage (on hub/category pages with Q&A)
- CollectionPage + ItemList (on category and hub pages)
- OnlineStore + WebSite (homepage)

No fake reviews. No AggregateRating with fabricated counts. Schema is clean.

## Post-deploy checklist

1. Verify `https://contactlens-pk.pages.dev/` loads with the logo, hero, and 8 best sellers
2. Test 3–5 product pages, confirm WhatsApp CTA opens with the right pre-filled message
3. Test 1 category page (`/bella-contact-lenses/`)
4. Test 1 color hub (`/hazel-lenses/`)
5. Test the eyesight page (`/eyesight-lenses/`) — the prize keyword page
6. Test redirects:
   - `/cart/` → should 301 to `/`
   - `/dahab-contact-lenses/` → should 301 to `/dahab-lenses/`
   - `/product/freshlook-blue/` → should 301 to `/freshlook-blue/`
7. Run the live URL through https://search.google.com/test/rich-results to validate Product schema
8. Submit `https://contactlens.pk/sitemap.xml` to Google Search Console once DNS cuts over
9. In GSC, request indexing of the homepage and `/eyesight-lenses/` to fast-track recovery

## Known gaps (see status report)

- `/extended-wear-lenses/` has 0 products (Acuvue Oasys should be tagged here)
- `/black-lenses/`, `/red-lenses/`, `/pink-lenses/` color hubs render with placeholder content + WhatsApp CTA
- `/blog/` is a placeholder linking to guide hubs — no actual blog posts yet
- `/dahab-lenses/` has only 3 products (the prize keyword page) — 18 priority Dahab shades pending sourcing
- 3 newer products on live site not in the build (Air Optix Grey, Air Optix Sterling Grey, Chaniel M°6) — DB dump pre-dates them
- Empty SKU on all products → schema uses `mpn: CLP-{id}` fallback

## Support

The full source bundle (templates, generators, content JSON) is in
`contactlens-static-source.zip` — make any content/template/schema changes
there, run `python3 generators/03_build_site.py`, and redeploy.
