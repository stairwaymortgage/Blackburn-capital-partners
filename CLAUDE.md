# Blackburn Capital Partners — Project Context

## Project Identity

**Blackburn Capital Partners (BCP)** is a Fort Lauderdale, Florida-based hotel wholesaler sourcing $50M+ Sun Belt coastal hotel acquisitions for an institutional investor network. The website is a marketing site with three audience funnels: hotel sellers, accredited investors, and real estate professionals.

**Founder:** Jim Blackburn — Scotsman Guide Top 1% mortgage producer (7 consecutive years), Licensed Florida Mortgage Broker.

**Spouse/Partner:** Olga Blackburn — Licensed real estate agent at The Keyes Company.

**Revenue model on hotel deals:** $1M finder fee + 1% mortgage commission + 5% GP equity.

## Tech Stack

- **Framework:** Astro 5 (static output)
- **Hosting:** Vercel
- **Source control:** GitHub (auto-deploy on push to `main`)
- **Domain:** blackburn.capital
- **No JavaScript framework** — vanilla JS only where absolutely required (funnel state management on `/start`)
- **No external dependencies** beyond Astro itself

## Site Structure

- `/` — homepage (one-pager, all nav links are `#anchors` to same page)
- `/sellers` — seller landing page (reached via direct marketing only)
- `/investors` — investor landing page (reached via direct marketing only)
- `/start` — multi-step pill-button funnel (4 steps + contact + thanks)

**Hook URLs in plan:** sellmyhotel.com → /sellers, privatehoteldeals.com → /investors. These pages should NOT link to each other in nav — no audience leakage.

## Component Architecture

Single source of truth for shared elements:

- `src/layouts/BaseLayout.astro` — HTML wrapper, meta tags, fonts, OG/Twitter cards
- `src/components/Header.astro` — navy strip with brand + phone + CTA (variant: 'full' | 'minimal')
- `src/components/Footer.astro` — "Our Industry Presence" block (configurable quickLinks prop)
- `src/components/CredentialsStrip.astro` — Scotsman Guide / Licensed / Capital Markets credentials
- `src/styles/global.css` — design tokens, shared button styles, animations

**Rule:** Editing a shared component must propagate to all pages. Never duplicate header/footer markup inline in page files.

## Brand System

### Colors (exact hex, do not approximate)

- **Navy** `#1E2A3A` — primary BCP brand
- **Crimson** `#8B1A1A` — accent, CTAs, italic headlines
- **Off-white** `#F8F7F4` — page background
- **Gold** `#C9A961` — phone numbers, section labels, brand accents
- **Charcoal** `#2C2C2C` — body text
- **White** `#FFFFFF` — cards, pills, form inputs

### Fonts

- **Playfair Display** — headlines (serif, weights 400/700, italic for accents)
- **Raleway** — body text (sans-serif, weights 400/500/600)

### Typography Rules

- Headlines use Playfair with crimson italic for emphasis: "Which Best *Describes You?*"
- Body always Raleway
- Uppercase section labels: small (0.75rem), letter-spaced, crimson or gold
- No light-weight body text — minimum 400, prefer 500 for primary content

## Hard Rules (Never Violate)

### Compliance & Privacy

- **Phone:** Always `(954) 824-1894` — this is the BCP business line. NEVER use Jim's personal cell.
- **Email:** NEVER expose `jim@blackburn.capital` or any real email in `mailto:` links or form placeholders. Use "Email Us" links that scroll to on-page contact forms. Form placeholder must be `you@example.com`.
- **No testimonials, no fake quotes, no fake client names.** If a testimonial section is added later, it requires real attribution.

### Brand Separation

- **Navy is BCP's primary color.** BBC (Blackburn Business Capital) uses navy + orange — do not mix palettes.
- **Olga is licensed at Keyes** — never claim Blackburn Realty Group is a standalone entity. Always show "A Division of The Keyes Company" attribution.
- **Stairway Mortgage** is a Nexa Mortgage DBA — always show "A Division of Nexa Mortgage" attribution.
- **No "LLC" suffixes** on Keyes or Nexa anywhere — they don't brand themselves that way.

### Footer Architecture

The footer must contain an "Our Industry Presence" block with three brands and parent attributions:

1. **Blackburn Business Capital** — Commercial Capital Brokerage
2. **Blackburn Realty Group** — A Division of The Keyes Company
3. **Stairway Mortgage** — A Division of Nexa Mortgage

The block is titled "Our Industry Presence" — NOT "Our Companies" (avoids implying ownership of Keyes/Nexa).

Full footer (not minimal) appears on all four pages. Reasoning: this is a $50M-transaction business — trust beats conversion friction at this dollar amount.

## Design Constraints

### Dark Mode Resistance (Critical)

iOS Safari and Chrome aggressively force dark mode. Defenses required on every page:

```html
<meta name="color-scheme" content="light only">
```

```css
html {
  color-scheme: light only !important;
  background: #F8F7F4 !important;
}

body {
  color: #2C2C2C !important;
  background: #F8F7F4 !important;
}

@media (prefers-color-scheme: dark) {
  html, body {
    background: #F8F7F4 !important;
    color: #2C2C2C !important;
  }
}
```

Use explicit hex codes with `!important` on backgrounds, text colors, and form inputs. CSS variables alone are not enough — iOS reinterprets them in dark contexts.

### Mobile-First Rules

- Hero sections stack vertically below 1024px
- Photos constrained to 320px max-width on mobile
- Form fields full-width single column on mobile
- Nav collapses to logo + primary CTA only below 1024px
- Geography grids: 3-col desktop → 2-col tablet → 1-col mobile
- Two-door layouts stack to single column on mobile
- Stat numbers minimum 3.4rem desktop, 3.6rem mobile (legibility)

### Performance Targets

- First Contentful Paint < 1.0s
- Largest Contentful Paint < 1.5s
- Total Blocking Time near zero
- PageSpeed Mobile score 95-100

These are not aspirations — they are requirements. A slow site reads as a small operation at this transaction size.

## Copy Voice

- **"We" not "I"** — this is a team operation (Jim + Olga + network)
- **Direct, professional, no fluff** — investors and hotel owners are sophisticated
- **No stale pleasantries** in emails ("hope you're well") or time-bound references ("this quarter")
- **Real numbers only** — no rounded promises, no projected returns presented as guaranteed
- **No "we are not a broker" or similar disclaimers** — Olga IS licensed; the framing is "third option" not "no commission"

### Approved Phrasings

- "The Team That Finds the Deals" (not "The Guy")
- "Most hotel owners think their only option is a public listing through CoStar or Hotel Brokers International. We give you a third option."
- "We do not run a six-percent listing service"
- "Privately. Confidentially." (NOT "Quietly. Confidentially." — the ornamental Q reads poorly)

## Seller Page: Three-Column Offer Table

The seller page features a structured offer comparison. Do not modify structure without approval.

| | Cash Close | Standard Close (Most Popular) | Diligence Close |
|---|---|---|---|
| Timeline | 30 days | 60 days | 90 days |
| Pricing | Lower price | Market price | Top of market |
| Financing | All cash | Conventional | Conventional |
| Diligence | Limited | Standard | Extended |
| Contingencies | None | Standard | Extended |
| Best For | Urgent sales | Most sellers | Premium assets |

## Funnel Page (`/start`) Structure

**4 steps + contact + thanks.** State managed in vanilla JavaScript.

### Step 1 — Audience (3 pills)
- I own a hotel and I'm thinking about selling
- I have capital to invest or lend
- I'm a real estate professional

### Step 2 — Branches by audience
- Hotel owner → State (FL/GA/SC/NC/AL/MS/Other)
- Capital → Equity / Debt / Both / Not sure
- Professional → Hotel Operator / Broker / Institutional Lender / GP-Sponsor / Other

### Step 3 — Narrows
- Hotel owner → Room count (Under 50 / 50-100 / 100-200 / 200-400 / Over 400)
- Capital → Check size ($100K-250K / $250K-500K / $500K-1M / $1M-5M / $5M+)
- Professional → Intent (Bring a deal / Co-invest / Provide capital / Just connect)

### Step 4 — Timeline
Ready now / 30-90 days / 3-6 months / 6-12 months / Just exploring (varies by branch)

### Contact + Thanks
- Contact: Name, phone, email (3 large tappable fields)
- Thanks: Personalized message by audience, headshot in circle frame, calendar block placeholder

**Funnel background is off-white** (#F8F7F4), NOT dark navy. Pills are crisp white cards. Headlines navy with crimson italic accents. Calendar block on thanks screen is navy as visual anchor.

## What NOT to Do

- **Don't add Lorem Ipsum** anywhere — use real copy or leave blank
- **Don't add stock photo testimonials** — no fake faces
- **Don't add "Trusted by 500+ companies"** logo strips — we don't have them
- **Don't use orange anywhere** — that's BBC's accent, not BCP's
- **Don't link homepage → /sellers or /investors in nav** — those pages are reached via direct marketing only
- **Don't use `mailto:` links** anywhere — bots scrape them
- **Don't add testimonials, awards, or press logos** unless explicitly provided with real attribution
- **Don't introduce new fonts** — Playfair Display + Raleway only
- **Don't add animations beyond subtle fades** — institutional aesthetic, not flashy
- **Don't add chat widgets, popups, or exit-intent modals** — $50M buyers and sellers hate them

## Image Assets

- `public/images/jim-blackburn.png` — Jim's solo headshot (used in hero on all four pages)
- `public/images/jim-olga-construction.jpg` — Jim + Olga at construction site (homepage about section only, caption: "Jim and Olga Blackburn · Fort Lauderdale, Florida.")

## Deployment

- **GitHub repo:** push to `main` branch
- **Vercel:** auto-detects Astro, no config needed
- **Custom domain:** blackburn.capital configured in Vercel dashboard
- **Build command:** `npm run build` (default Astro)
- **Output directory:** `dist` (default Astro)

## Future Roadmap (Reference Only)

- Form backend: Formspree, Basin, or GoHighLevel webhook
- Calendar embed on /start thanks screen: Calendly, Cal.com, or GoHighLevel
- Build seller list: 200-300 hotels via Reonomy/PropStream/county records
- Build buyer list: 10-12 conversion operators with NDAs
- Scotsman Guide hotel lender outreach (~50 contacts)
- Florida real estate broker license for Jim (preserves commission-claim standing)
