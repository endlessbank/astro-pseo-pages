# Astro pSEO Pages Generator

A Claude Code skill that generates programmatic SEO (pSEO) pages for Astro sites built with [astro-saas-site](https://github.com/endlessbank/astro-saas-site).

## What It Does

Creates an index page + multiple content pages from a keyword list. Perfect for:

- **Integration pages** — "[SaaS] Integration with Slack", "[SaaS] Integration with Zapier"
- **Audience pages** — "[SaaS] for Freelancers", "[SaaS] for Startups"
- **Comparison pages** — "[SaaS] vs Competitor A", "[SaaS] vs Competitor B"
- **Alternative pages** — "[SaaS] Alternative to Competitor"
- **Template pages** — "Invoice Templates", "Contract Templates"

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

**1. Topic Pattern** (SaaS name comes from site.config)
| Pattern |
|---------|
| "[SaaS_NAME] Integration with [platform]" |
| "[SaaS_NAME] for [audience]" |
| "[SaaS_NAME] vs [competitor]" |
| "[SaaS_NAME] Alternative to [competitor]" |
| "Free [tool-type] Generator" |
| "[industry] Solution with [SaaS_NAME]" |
| "[template-type] Templates" |

**2. URL Structure**
| Pattern | Example URLs |
|---------|--------------|
| `/integrations/[slug]` | `/integrations/slack`, `/integrations/zapier` |
| `/solutions/[slug]` | `/solutions/freelancers`, `/solutions/startups` |
| `/vs/[slug]` | `/vs/competitor-name` |
| `/alternatives/[slug]` | `/alternatives/competitor-name` |
| `/tools/[slug]` | `/tools/invoice-generator` |
| `/templates/[slug]` | `/templates/proposal`, `/templates/contract` |
| `/industries/[slug]` | `/industries/healthcare`, `/industries/real-estate` |

**3. Content Sections**

Describe each section's purpose (e.g., Hero, Problem, Solution, Features, How It Works, FAQ, CTA)

**4. Index Page Content**
| Headline | Intro |
|----------|-------|
| "Integrations" | "Connect [SaaS_NAME] with your favorite tools." |
| "Solutions for Every Industry" | "See how [SaaS_NAME] works for your business." |
| "Compare [SaaS_NAME]" | "See how we stack up against alternatives." |
| "Free Tools" | "No signup required. Start using now." |
| "Templates" | "Ready-to-use templates to get started fast." |

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
Slack
Zapier
Notion
Stripe
HubSpot
```

Claude generates unique content for each keyword, updates the index, and adds a footer link.

## Output Structure

```
src/
├── content/
│   ├── config.ts              # Updated with pseo collection
│   └── pseo/
│       ├── slack.md
│       ├── zapier.md
│       ├── notion.md
│       └── ...
└── pages/
    └── integrations/
        ├── index.astro        # Index page (auto-lists all content)
        └── [...slug].astro    # Content page template
```

## Example

**Input:**
- Topic: "[SaaS_NAME] Integration with [platform]"
- URL: `/integrations/[slug]`
- Keywords: Slack, Zapier, Notion, Stripe

**Output:**
- `/integrations` — Index page listing all integrations
- `/integrations/slack` — Slack integration content
- `/integrations/zapier` — Zapier integration content
- `/integrations/notion` — Notion integration content
- `/integrations/stripe` — Stripe integration content
- Footer updated with link to `/integrations`

## Requirements

- Astro site built with [astro-saas-site](https://github.com/endlessbank/astro-saas-site) skill
- Existing `BaseLayout` and `site.config.ts`

## Content Generation

Claude generates **unique content** for each keyword — not just variable swaps. Each page includes:

- Keyword-specific pain points and benefits
- Tailored solutions and features
- Custom FAQ answers
- Natural keyword usage for SEO

## License

MIT