---
name: astro-pseo-pages
description: Generate programmatic SEO (pSEO) pages for Astro sites built with astro-saas-site skill. Creates index page + multiple content pages from keyword lists. Use when asked to "create pSEO pages", "add pSEO templates", "generate programmatic SEO pages", or "build landing pages from keywords".
---

# Astro pSEO Pages Generator

Generate programmatic SEO pages for Astro sites. Works with sites built using the `astro-saas-site` skill.

## Two-Mode Workflow

### Mode 1: Create pSEO Templates

Trigger: "Create pSEO templates" or "Set up pSEO pages"

**Question Flow** (ask one at a time):

1. **Topic Pattern**: "What's the pSEO topic pattern? Use [placeholder] for the variable part.
   Examples:
   - 'Bulk Invoices Creation from [platform] CSV'
   - 'Best CRM for [industry]'
   - '[service] in [city]'
   - 'How to Use [tool] for [use-case]'
   - '[framework] vs [competitor] Comparison'
   - 'Free [tool-type] for [audience]'"

2. **URL Structure**: "What's the URL structure?
   Examples:
   - `/platform/[keyword-slug]` → `/platform/stripe-csv-to-invoice`
   - `/industries/[keyword-slug]` → `/industries/real-estate-crm`
   - `/locations/[keyword-slug]` → `/locations/new-york`
   - `/tools/[keyword-slug]` → `/tools/free-invoice-generator`
   - `/compare/[keyword-slug]` → `/compare/react-vs-vue`
   - `/use-cases/[keyword-slug]` → `/use-cases/freelancers`"

3. **Content Sections**: "What sections should each content page have? Describe each section's purpose and content.
   Example:
   - **Hero**: Headline with [platform] name, subheadline explaining the benefit
   - **Problem**: Describe the pain point of manual invoice creation
   - **Solution**: How our tool solves it for [platform] users
   - **Steps**: 3-step process (Upload CSV, Map fields, Download invoices)
   - **Features**: Key features relevant to [platform]
   - **FAQ**: 3-4 questions specific to [platform]
   - **CTA**: Final call to action"

4. **Index Page Content**: "What's the index page headline and intro text?
   Examples:
   - Headline: 'Create Invoices from Any Platform' / Intro: 'Convert your CSV exports into professional invoices in seconds.'
   - Headline: 'The Best CRM for Every Industry' / Intro: 'Find the perfect CRM solution tailored to your industry needs.'
   - Headline: 'We Service All Major Cities' / Intro: 'Professional services available in your area. Find your city below.'
   - Headline: 'Free Tools for Everyone' / Intro: 'No signup required. Start using our free tools today.'
   - Headline: 'Framework Comparisons' / Intro: 'Detailed side-by-side comparisons to help you choose the right tech.'"

**Output**:
- `src/content/config.ts` — Updated with pSEO collection
- `src/content/pseo/` — Content collection folder
- `src/pages/[index]/index.astro` — Index page template
- `src/pages/[index]/_slug_.astro` — Content page template (rename to `[...slug].astro`)

After generating, tell user:
```
✅ pSEO templates created!

Preview:
  npm run dev
  Open http://localhost:4321/[index]

Review the templates. Let me know if you want changes:
- "Change the hero section to..."
- "Add a comparison table section"
- "Make the CTA more prominent"

When you're happy, say "Generate pSEO pages" and paste your keywords.
```

### Mode 2: Generate pSEO Pages

Trigger: "Generate pSEO pages" or user pastes keyword list after template approval

**Input**: Keyword list (comma-separated or one per line)
```
Stripe
PayPal
Wise
Mercury
QuickBooks
```

**Process**:

1. **Parse Keywords** — Extract from user input

2. **Derive Variables** — For each keyword, create:
   - `keyword`: Original (e.g., "Stripe")
   - `keyword_slug`: URL-safe (e.g., "stripe")
   - `full_slug`: Complete slug from pattern (e.g., "stripe-csv-to-invoice")
   - `full_title`: From topic pattern (e.g., "Bulk Invoices Creation from Stripe CSV")

3. **Generate Content** — For each keyword:
   - Create unique content based on template sections
   - Content should be specific to that keyword/platform
   - Save as `src/content/pseo/[full_slug].md`

4. **Verify Index** — Index page auto-pulls from content collection

**Output**:
```
✅ Generated 5 pSEO pages!

Created:
- src/content/pseo/stripe-csv-to-invoice.md
- src/content/pseo/paypal-csv-to-invoice.md
- src/content/pseo/wise-csv-to-invoice.md
- src/content/pseo/mercury-csv-to-invoice.md
- src/content/pseo/quickbooks-csv-to-invoice.md

Preview at http://localhost:4321/[index]
```

## Template Structure

### Index Page Template

See `references/index-template.md` for structure:
- Hero with headline + intro
- Grid of content page cards (auto-generated from collection)
- CTA section
- Uses site config for styling consistency

### Content Page Template

See `references/content-template.md` for structure:
- Frontmatter with title, description, keyword, slug
- Sections defined by user during Mode 1
- Consistent styling with main site
- Internal links to index and other content pages

## Content Collection Schema

```typescript
// Add to src/content/config.ts
const pseo = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    keyword: z.string(),
    keywordSlug: z.string(),
    order: z.number().default(0),
  }),
});

export const collections = { blog, faq, pseo };
```

## Key Notes

- Always wait for user approval before Mode 2
- Generate unique, valuable content for each keyword (not just variable swaps)
- Match styling with the existing Astro site
- Use Tailwind Typography for content sections
- Include internal links between pSEO pages and main site
