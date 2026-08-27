# PHASE 8.3 — PRIVACY, TRUST & RELEASE READINESS

**Status: IN PROGRESS** (live document — update at phase close; do not mark
future checks as passed in advance)
**Backend: external Supabase ref `iwiqprhoohkxvjyxojto` (canonical). Lovable Cloud DB not used.**
**Production email: PAUSED / not configured.**
**Production deployment: NOT permitted in this phase.**

## Objective

Make the public web app trustworthy and release-ready: publish a bilingual
Privacy Policy, integrate it honestly into the registration and footer
surfaces, and complete a release-readiness audit of auth, email, demo content
and the deploy gate.

## Scope

### 1. Bilingual Privacy Policy — implemented, under review

- Public routes: `/chinh-sach-bao-mat` (VI) and `/en/privacy` (EN), rendered
  through the existing PageShell / editorial system, one H1 per page.
- `privacy` added to `PAGE_PATHS` in `src/lib/i18n/locale.ts` so the VN | EN
  switch maps each page to its exact counterpart. Deliberately **not** added to
  `NAV_PAGES` — primary navigation stays focused.
- Copy lives in `src/lib/i18n/dictionary.ts` (VI/EN), with a `page_privacy`
  block definition in `src/lib/cms/block-schema.ts` so the content can be
  CMS-edited later with the dictionary as safe fallback.
- Copy restraint rule: no invented legal entity name, registration number,
  address, DPO, email, phone or legal representative; no invented retention
  period. Rights/contact guidance points to `/dong-hanh` · `/en/get-involved`
  and to replying on the same channel used for Journey communication.
- The Privacy copy is written to support applicable personal-data protection
  requirements in Vietnam without claiming certification or completed legal
  compliance review.

### 2. Integrations — implemented

- Registration form: the plain-text privacy note now carries an accessible
  inline localized link to the correct Privacy route. No marketing-consent
  checkbox was introduced.
- Footer: discreet localized legal/privacy link, visually secondary.
- Sitemap: both Privacy URLs included; `/auth` and `/admin` remain excluded;
  robots does not block the Privacy pages.

### 3. Tracking claims — verified before stating

`src`, `public` and the HTML entry were scanned for gtag / GTM / Plausible /
PostHog / Pixel / Hotjar / Clarity / Mixpanel: none present. The policy's
statement that the site stores only a language preference and the internal
login session — no advertising cookies, no behavioural profiling — is therefore
accurate as of this phase. If analytics or non-essential cookies are added
later, the policy must be updated before or when they ship.

### 4. Release-readiness audit — OPEN items

These are audit findings, not completed fixes.

| item | current state |
| --- | --- |
| Supabase Auth URL / Site URL + redirect domains | **OPEN** — still points at preview host; production domain must be configured before deploy. |
| Leaked Password Protection | **OPEN — currently DISABLED.** Pre-existing security-advisor finding, carried forward from earlier phases. Recorded here as a release-readiness item; it has **not** been enabled. |
| Transactional email / Resend readiness | **OPEN** — code path ready, returns `provider_not_configured` without `RESEND_API_KEY` / `EMAIL_FROM`. Production email intentionally paused; must not be described as live. |
| AI / demo Journey media release treatment | **OPEN** — demo/prototype imagery must be clearly labelled as demo or replaced with Owner-supplied real documentary media before public release. Not resolved. |
| Journey demo EN metadata pairings | **OPEN** — Owner-side content correction on the demo journey. |
| Official contact / legal information for the Privacy "Rights" section and footer | **OPEN** — Owner-dependent; nothing invented. |
| Final Deploy Gate | **NOT STARTED** — requires Owner approval; no production deploy in this phase. |

### 5. QA status so far

- TypeScript typecheck clean; production build succeeds.
- Privacy routes return 200 with correct `lang`, one H1 and localized
  title/description; language switch maps VI ↔ EN correctly.
- Responsive 1440 / 768 / 390 with no horizontal overflow; no console, page or
  network (≥400) errors on the checked routes.
- Archived QA journey absent from public journey lists and the sitemap.
- Registration-form privacy link uses `pageHref("privacy", locale)`; visual
  confirmation still pending because no journey is currently open for public
  registration.

## Phase close checklist (to be completed later)

- [ ] Owner confirms official contact/legal info; Privacy copy updated.
- [ ] Auth URL configuration switched to the production domain.
- [ ] Leaked Password Protection decision recorded (enabled, or accepted with
      justification).
- [ ] Demo media labelled or replaced.
- [ ] Email either configured or explicitly kept paused at release.
- [ ] Final Deploy Gate approved by Owner.
