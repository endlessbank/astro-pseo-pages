# Astro pSEO Pages Generator

A Claude Code skill that generates programmatic SEO (pSEO) pages for Astro sites built with [astro-saas-site](https://github.com/endlessbank/astro-saas-site).

## What It Does

Creates an index page + multiple content pages from a keyword list. Perfect for:

- **Platform-specific pages** — "Invoice from Stripe CSV", "Invoice from PayPal CSV"
- **Location-based pages** — "Service in New York", "Service in Los Angeles"
- **Use-case pages** — "Tool for Freelancers", "Tool for Agencies"

## Installation

### Step 1: Create the skills folder

```bash
mkdir -p .claude/skills
```

### Step 2: Clone this skill

```bash
cd .claude/skills
git clone https://github.com/endlessbank/astro-pseo-pages.git
```

### Step 3: Verify structure

```
your-project/
└── .claude/
    └── skills/
        └── astro-pseo-pages/
            ├── SKILL.md
            └── references/
```

## Usage

### Step 1: Create Templates

Open Claude Code and say:

- "Create pSEO templates"
- "Set up pSEO pages"

Claude will ask 4 questions:

| Question | Example |
|----------|---------|
| Topic pattern | "Bulk Invoices Creation from [platform] CSV" |
| URL structure | `/platform/[keyword-slug]` |
| Content sections | Hero, Problem, Solution, Steps, Features, FAQ, CTA |
| Index page content | Headline + intro text |

### Step 2: Review & Iterate

Preview your templates:

```bash
npm run dev
```

Ask Claude to make changes until you're happy:

- "Change the hero section to have a bigger headline"
- "Add a comparison table section"
- "Make the CTA more prominent"
- "Add customer logos to the index page"

### Step 3: Generate Pages

When templates are ready, say:

- "Generate pSEO pages"

Then paste your keywords:

```
Stripe
PayPal
Wise
Mercury
QuickBooks
```

Claude generates unique content for each keyword and updates the index.

## Output Structure

```
src/
├── content/
│   ├── config.ts              # Updated with pseo collection
│   └── pseo/
│       ├── stripe-csv-to-invoice.md
│       ├── paypal-csv-to-invoice.md
│       ├── wise-csv-to-invoice.md
│       └── ...
└── pages/
    └── platform/
        ├── index.astro        # Index page (auto-lists all content)
        └── [...slug].astro    # Content page template
```

## Example

**Input:**
- Topic: "Bulk Invoices Creation from [platform] CSV"
- URL: `/platform/[keyword-slug]`
- Keywords: Stripe, PayPal, Wise

**Output:**
- `/platform` — Index page listing all platforms
- `/platform/stripe-csv-to-invoice` — Stripe-specific content
- `/platform/paypal-csv-to-invoice` — PayPal-specific content
- `/platform/wise-csv-to-invoice` — Wise-specific content

## Requirements

- Astro site built with [astro-saas-site](https://github.com/endlessbank/astro-saas-site) skill
- Existing `BaseLayout` and `site.config.ts`

## Content Generation

Claude generates **unique content** for each keyword — not just variable swaps. Each page includes:

- Platform-specific pain points
- Tailored solutions and features
- Custom FAQ answers
- Natural keyword usage for SEO

## License

MIT