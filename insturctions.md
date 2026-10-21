# ANTIGRAVITY MASTER PROMPT — Tenacious Hub Full-Stack Redesign

> Paste everything below the line into Antigravity (Manager view → new agent task). Use **Planning mode** so the agent produces an Implementation Plan artifact for your approval before it writes code.

---

## 0. ROLE & WORKING AGREEMENT

You are a senior full-stack engineer, UX designer and technical SEO lead. You are rebuilding **tenacioushub.com** from scratch (front end + back end + admin CMS). The current site is a legacy PHP site (`.php` pages, query-string detail URLs such as `service-detail.php?action=detail&url=...`).

Working rules:
1. **Plan first.** Produce an Implementation Plan artifact (architecture, folder tree, data model, page list, phased task list) and wait for my approval before scaffolding.
2. Work **phase by phase** (Section 11). After each phase: run lint, type-check, tests and a build; open the app in the browser agent and verify visually; produce a Walkthrough artifact with screenshots; then stop for my review.
3. Never invent company facts (client names, statistics, addresses, prices, testimonials). Use clearly marked `TODO_CONFIRM` placeholders from Section 2 and list all of them in `/docs/OPEN_QUESTIONS.md`.
4. Prefer small, typed, tested, documented modules. Commit in small conventional commits (`feat:`, `fix:`, `chore:`).

---

## 1. PROJECT CONTEXT (from analysis of the live site)

**Company:** Tenacious Hub Private Limited — positioned as a digital technology and management-consulting firm. Offices/regions referenced: Noida (UP) in site metadata, Rudrapur (Uttarakhand) as registered/origin location, Agra (UP) marketing office. Contact on live site: +91 9105267000 (Call/WhatsApp), sales@tenacioushub.com, info@tenacioushub.in.

**Current site map (to be preserved in content, restructured in architecture):**
- Home, About Us, Our Team (founder page), Pricing Plan, Blog (+ blog detail), Contact (also used for Careers), Terms & Conditions, Privacy Policy, Refund Policy
- **Services & Solutions:** IT Solutions, Marketing & Branding Solution, Attendance Management, Lead Management, Outsourcing Services, Rural Economic Development, Finance Advisor, Investment & Insurance, Startup Advice
- **Digital Marketing:** Social Media Management, SEO, Ads Management, App Store Optimization, Content Writing, Lead Generation
- **Service detail pages (6 on home):** Responsive Website Design, Digital Marketing, E-Commerce & Portal Design, Logo Design, Android App Development, Bulk SMS / Voice Call (multi-language: Hindi, Tamil, Telugu, etc.)
- **Outsourcing/BPO offerings:** Back Office & Data Entry, Virtual Assistant & Operations, Finance & Accounting Advisory, IT Managed Helpdesk

**Current home-page structure:** hero ("Powering Smarter Technology & Operations", Get Started / Explore Services, "4.9/5 Google Reviews", "+148% organic traffic", "10+ years") → About → 6 solution cards → Why Choose Us (Niche Expertise, Flexibility, Reliability & Quality) → CTA banner → BPO section (4 cards) → "Revenue-Generating Machine" stats (86% / 98%) → 3 testimonials (Zed Security Services, PEJKSMS, Yoganta) → client logo strip (Google, Meta, Shopify, WordPress, Amazon) → 3 latest blog posts → footer with newsletter, social links, policies.

**Problems found that the redesign MUST fix:**
1. **Unfocused positioning.** The company sells web design, SEO, BPO, finance, insurance, rural development and attendance software at once, with no clear hierarchy. → Create a clear 3-pillar information architecture (Section 3).
2. **Legacy URLs & weak SEO structure.** `.php` pages, query-string detail pages, mixed `www` / non-`www` links. → Clean slugs, canonical host, 301 redirect map, sitemap, structured data.
3. **Inconsistent content:** meta says "AI SEO / BPO", hero says "Smarter Technology & Operations", services list is broader than home page shows; "Careers" points to Contact; footer Instagram link points to an unrelated handle (`shaadiadda`); two different email domains (.com / .in); "Instant SEO Score" is advertised but no tool is visible.
4. **Unverifiable trust claims** ("500+ global enterprises", "10+ years", "Trusted by Google/Meta/Amazon" logo strip). Logos of third-party platforms imply partnership. → Replace with verifiable proof (real case studies, real client logos with permission, "Technology we work with" labelled honestly).
5. **No lead-capture system.** Contact is a basic page. → Build a proper multi-step quote/lead flow with CRM-style admin.
6. **No case studies / portfolio / pricing clarity** beyond a package page.
7. **Content is not manageable** by non-developers. → Build an admin CMS.
8. **Accessibility, performance and security** are unknown/unaudited. → Set measurable budgets (Section 9).

**Goals of the redesign:** (a) look and feel like a credible modern agency, (b) generate more qualified leads, (c) rank for service + location keywords, (d) let the team manage everything without a developer.

---

## 2. PLACEHOLDERS TO CONFIRM WITH THE COMPANY (`TODO_CONFIRM`)
Legal name/registered address, final primary phone/WhatsApp, single canonical email, GST/CIN display preference, office addresses + map pins, real client logos and permission, real case studies with metrics, real testimonials with names/roles, pricing figures, team bios + photos, social handles (fix Instagram), brand colours/logo vector files, Google Business Profile link, preferred analytics (GA4/Plausible), email provider, hosting target.

---

## 3. INFORMATION ARCHITECTURE

Reorganise services into **three pillars** (final naming adjustable):

| Pillar | Includes |
|---|---|
| **Digital Engineering** | Website design & development, E-commerce & portals, Android app development, Logo/brand identity, Attendance Management, Lead Management software |
| **Growth Marketing** | SEO (incl. AI/GEO search optimisation), Social Media Management, Ads Management, App Store Optimization, Content Writing, Lead Generation, Bulk SMS / Voice Call |
| **Business Operations & Advisory** | Outsourcing/BPO (back office, VA, finance & accounting, IT helpdesk), Finance Advisor, Investment & Insurance, Startup Advice, Rural Economic Development |

**Routes (clean, lowercase, hyphenated):**
```
/                              Home
/about                         About
/team                          Team (+ /team/[slug] for founder/leaders)
/services                      Services hub (3 pillars)
/services/[pillar]             Pillar landing
/services/[pillar]/[service]   Service detail (e.g. /services/growth-marketing/seo)
/industries/[industry]         Optional programmatic industry pages
/locations/[city]              Optional local-SEO pages (Noida, Rudrapur, Agra …)
/work                          Portfolio / case studies index
/work/[slug]                   Case study
/pricing                       Packages & pricing
/blog                          Blog index (+ category, tag, search, pagination)
/blog/[slug]                   Article
/careers                       Careers (separate from contact) + /careers/[slug]
/contact                       Contact + quote form
/get-a-quote                   Multi-step lead wizard
/free-seo-audit                Instant SEO score tool (lead magnet)
/resources                     Downloads (gated lead magnets)
/legal/terms | /legal/privacy | /legal/refund | /legal/cookies
/admin/**                      Protected CMS & CRM
/api/**                        Backend endpoints
/sitemap.xml  /robots.txt  /llms.txt
```

**301 redirect map (implement in config + test every one):**
`/index.php→/`, `/about-us.php→/about`, `/founder.php→/team`, `/package.php→/pricing`, `/blog.php→/blog`, `/blog-detail.php?action=detail&id=N→/blog/<slug>`, `/contact-us.php→/contact`, `/terms.php→/legal/terms`, `/privacy.php→/legal/privacy`, `/refund.php→/legal/refund`, `/service.php→/services/digital-engineering`, `/marketing.php→/services/growth-marketing`, `/seo.php→/services/growth-marketing/seo`, `/social-media.php→…/social-media-management`, `/ads.php→…/ads-management`, `/app-store.php→…/app-store-optimization`, `/content-writing.php→…/content-writing`, `/lead-generation.php→…/lead-generation`, `/outsourcing.php→/services/business-operations/outsourcing`, `/finance.php`, `/investment.php`, `/startup.php`, `/rural.php`, `/attandance-management.php`, `/lead-management.php` → mapped to their new service slugs, and `/service-detail.php?action=detail&url=<old-key>` → new service slug. Force a single canonical host (`https://www.tenacioushub.com`) and HTTPS.

---

## 4. TECH STACK (propose, justify in the plan, then proceed unless I object)

- **Framework:** Next.js (App Router) + TypeScript (strict), React Server Components, ISR for marketing/blog pages
- **Styling/UI:** Tailwind CSS + shadcn/ui (Radix) + Framer Motion (subtle, respects `prefers-reduced-motion`); icons via lucide-react
- **Database:** PostgreSQL + Prisma ORM (migrations + seed)
- **Auth (admin):** Auth.js (credentials + optional Google), role-based access (Admin, Editor, Sales, Viewer), rate-limited login, optional TOTP 2FA
- **CMS:** custom admin built in-app (rich-text via Tiptap, media library with image optimisation, draft/schedule/publish, revisions)
- **Forms/validation:** React Hook Form + Zod (shared schemas client/server)
- **Email:** Resend or SMTP via Nodemailer (transactional + newsletter double opt-in)
- **Search:** Postgres full-text for blog/services
- **Storage:** S3-compatible (Cloudflare R2 / S3) for uploads
- **Analytics:** GA4 or Plausible behind a cookie-consent banner; Search Console + Bing verification
- **Testing:** Vitest (unit), Playwright (e2e + a11y via axe), Lighthouse CI
- **DevOps:** Docker + docker-compose for local; GitHub Actions CI (lint, typecheck, test, build, Lighthouse); deploy-ready for Vercel or a VPS (provide both guides); `.env.example`; Sentry for errors
- **Spam protection:** Cloudflare Turnstile + honeypot + server-side rate limiting

---

## 5. DESIGN SYSTEM & UX DIRECTION

**Brand feel:** confident, modern, trustworthy, India-based agency with global polish. Not template-looking. Clean, generous whitespace, strong typographic hierarchy, purposeful motion.

- Extract/recreate colours from the current logo (`/images/logo.png`) (`TODO_CONFIRM`); build a token set: primary, accent, neutral scale, success/warn/error, in **light + dark** modes, all WCAG AA contrast.
- Typography: a distinctive display font + highly legible body font (e.g. self-hosted via `next/font`), fluid type scale.
- 8-pt spacing grid, 12-col layout, container max-width 1280px.
- Components to build and document in a `/design-system` route: Button, Input/Select/Textarea, Card (service, case study, blog, testimonial, pricing), Badge, Tabs, Accordion (FAQ), Modal, Toast, Breadcrumb, Pagination, Stat counter, Logo marquee, Timeline/process steps, CTA band, Navbar (mega-menu for 3 pillars), Footer, Cookie banner, Sticky mobile CTA bar, WhatsApp floating button.
- **Mobile-first**; design at 360 / 768 / 1024 / 1440. Large tap targets, thumb-friendly CTAs.
- Micro-interactions: scroll reveals, hover states, animated counters; no layout shift.

---

## 6. PAGE-BY-PAGE REQUIREMENTS

**Home**
1. Hero: one clear value proposition + 2 CTAs (Get a Free Quote / Explore Services), concise trust line (real rating only if confirmed), hero visual (animated abstract/dashboard mock — no stock cliché).
2. "What we do" — the 3 pillars as large interactive cards.
3. Featured services grid (the 6 current solutions, re-written).
4. Process (Discover → Strategy → Build → Launch → Grow).
5. Proof: 3 featured case studies with real metrics, verified testimonials, honest client logos.
6. Outsourcing/BPO highlight with cost-saving framing (claim only if substantiated).
7. Industries served.
8. Free SEO audit lead-magnet banner.
9. Latest blog posts (3), FAQ accordion (with FAQ schema), final CTA, newsletter.

**Service detail (template, CMS-driven):** hero, problem/solution, deliverables list, process, tech/tools used, pricing teaser, related case study, FAQ, testimonial, CTA form. Each has unique title/meta/OG image, `Service` JSON-LD, breadcrumb schema.

**Pricing:** tabbed by pillar, comparison table, "most popular" tag, monthly/one-time toggle, custom-quote CTA. Prices from CMS (`TODO_CONFIRM`).

**Work/Case studies:** filter by service/industry; case study template (challenge, approach, results with metrics, tech, gallery, quote).

**Blog:** categories/tags, search, reading time, TOC, author box, related posts, share buttons, Article JSON-LD, RSS feed. **Migrate all existing posts** (IDs 1–39+) with images, dates and slugs preserved.

**About / Team:** story (founded 2014, incorporated 2021 — `TODO_CONFIRM` wording), mission, values, offices, leadership profiles (Vaibhav Sheel Pandey as founder — `TODO_CONFIRM`), certifications.

**Careers:** open roles list from CMS, role detail, application form with resume upload (PDF, 5 MB cap, virus-scan hook).

**Contact:** form, phone/WhatsApp/email click-to-action, office map(s), business hours, response-time promise.

**Get-a-Quote wizard (3–4 steps):** service interest (multi-select) → project details & budget range → timeline → contact info + consent. Progress bar, autosave to session, success page with next steps and Calendly-style booking link (`TODO_CONFIRM`).

**Free SEO Audit tool:** user enters URL + email → server fetches page → returns on-page checks (title, meta, H1, alt tags, speed hints, HTTPS, mobile tag) as a scored report; stores lead; emails report. SSRF-safe (block private IP ranges), rate-limited, queue-based.

**Legal:** Terms, Privacy (India DPDP Act-aware), Refund, Cookie policy; versioned in CMS.

**404 / 500 / maintenance** pages designed and helpful.

---

## 7. BACKEND, DATABASE & ADMIN

**Core entities (Prisma models):** User, Role, Session, Service, ServicePillar, Industry, Location, CaseStudy, Testimonial, Client, TeamMember, BlogPost, BlogCategory, Tag, Author, Page (generic CMS), Faq, PricingPlan, PricingFeature, JobPost, JobApplication, Lead, LeadNote, LeadStatusHistory, ContactMessage, NewsletterSubscriber, SeoAuditRequest, MediaAsset, Redirect, SiteSettings, AuditLog.

**API (REST route handlers or tRPC; Zod-validated, typed):**
- Public: `POST /api/leads`, `/api/contact`, `/api/newsletter/subscribe|confirm|unsubscribe`, `/api/applications`, `/api/seo-audit`, `GET /api/search`
- Admin: full CRUD for every entity, media upload (signed URLs), bulk actions, CSV export of leads, redirect manager, settings

**Admin dashboard (`/admin`):**
- Overview: leads this week, conversion by source, top pages, latest messages
- **Lead CRM:** pipeline board (New → Contacted → Qualified → Proposal → Won/Lost), assignment, notes, reminders, email templates, UTM/source tracking, export
- Content managers: services, case studies, blog (Tiptap editor, SEO fields, scheduled publish, preview links), pages, FAQs, pricing, testimonials, team, careers
- Media library, redirect manager, SEO defaults, site settings (contact info, socials, scripts), user/role management, audit log
- Email notifications: instant alert to sales on new lead; auto-reply to the submitter

**Security requirements:** OWASP Top-10 review, CSRF protection, strict CSP and security headers, parameterised queries only, input sanitisation for rich text (DOMPurify/allow-list), password hashing (argon2), login throttling, secure cookies, secrets only via env, file-type/size validation, dependency audit in CI, daily DB backup instructions, GDPR/DPDP consent logging and data-deletion endpoint.

---

## 8. SEO, AEO/GEO & CONTENT

- Per-page unique `<title>`, meta description, canonical, OG/Twitter images (dynamic OG generation), `hreflang` ready.
- JSON-LD: `Organization`, `LocalBusiness` (per office), `WebSite` + SearchAction, `Service`, `Article`, `FAQPage`, `BreadcrumbList`, `Review` only if verifiable.
- Auto-generated `sitemap.xml`, `robots.txt`, `llms.txt`, RSS.
- Keyword map in `/docs/SEO_PLAN.md` targeting: web design / SEO / digital marketing / BPO / app development + city modifiers (Noida, Rudrapur, Agra, Delhi NCR, Uttarakhand).
- Content rewrite: keep meaning of current copy but rewrite in a consistent brand voice (clear, benefit-led, no hype, no unverifiable numbers). Provide first-draft copy for every page in `/content/*.md`, flagged for human review.
- Image strategy: AVIF/WebP, responsive `sizes`, descriptive alt text, lazy-loading below the fold.

---

## 9. NON-FUNCTIONAL TARGETS (CI-enforced)

- Lighthouse (mobile): Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95, SEO ≥ 95
- Core Web Vitals: LCP < 2.5 s, INP < 200 ms, CLS < 0.1
- WCAG 2.2 AA: keyboard navigation, focus states, skip link, ARIA where needed, axe-clean in Playwright
- JS budget: < 170 KB gzipped on the home route
- Test coverage: ≥ 80% on lib/validation/API; e2e for lead form, quote wizard, newsletter, admin login, blog publish, redirects
- Browser support: last 2 versions of Chrome, Edge, Safari, Firefox; iOS Safari and Android Chrome

---

## 10. REPOSITORY STRUCTURE (expected)

```
/src
  /app            (routes, layouts, route handlers)
  /components     (ui, sections, forms, admin)
  /lib            (db, auth, email, seo, validation, utils)
  /server         (services, repositories, jobs)
  /styles         (tokens, globals)
/prisma           (schema.prisma, migrations, seed.ts)
/content          (migrated blog posts, draft copy)
/public           (fonts, icons, og, images)
/tests            (unit, e2e)
/docs             (ARCHITECTURE, OPEN_QUESTIONS, SEO_PLAN, DEPLOYMENT, ADMIN_GUIDE)
.github/workflows (ci.yml, lighthouse.yml)
docker-compose.yml  Dockerfile  .env.example  README.md
```

---

## 11. PHASED DELIVERY PLAN (stop for my approval after each phase)

**Phase 0 – Discovery & Plan:** crawl the live site, inventory every URL/content/image, produce Implementation Plan, sitemap, ERD, redirect map, risk list.
**Phase 1 – Foundation:** repo, tooling, CI, Docker, env, Prisma schema + seed, design tokens, base layout, navbar/footer, cookie banner.
**Phase 2 – Design System & Core Pages:** component library, Home, About, Services hub + detail template, Contact.
**Phase 3 – Content Pages:** Pricing, Work/case studies, Team, Careers, Legal, 404/500.
**Phase 4 – Blog & Migration:** blog engine, migrate existing posts, redirects, RSS, search.
**Phase 5 – Lead Engine:** quote wizard, contact API, newsletter, spam protection, email notifications, SEO audit tool.
**Phase 6 – Admin CMS & CRM:** auth/roles, all managers, lead pipeline, media library, audit log.
**Phase 7 – SEO, Analytics & Performance:** structured data, sitemaps, OG images, analytics + consent, performance tuning.
**Phase 8 – QA, Security & Launch:** full test suite, accessibility audit, pen-test checklist, redirect verification, staging → production runbook, post-launch monitoring, handover docs and admin training guide.

---

## 12. DEFINITION OF DONE
- Every legacy URL 301-redirects to a correct new page (automated test).
- Non-technical staff can create/edit/publish a blog post, service, case study and job from `/admin` without code.
- Every form stores a lead, notifies sales, and auto-replies to the user.
- All Section 9 targets pass in CI.
- No fabricated claims remain; all `TODO_CONFIRM` items are listed in `/docs/OPEN_QUESTIONS.md`.
- README lets a new developer run the project locally in under 10 minutes.

---

## 13. FIRST ACTION
Start **Phase 0** now: crawl `https://www.tenacioushub.com`, build the full URL and content inventory, then present the Implementation Plan artifact (architecture decisions, ERD, sitemap, redirect map, phase timeline, open questions). Do **not** write application code until I approve the plan.
