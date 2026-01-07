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

1. **Topic Pattern**: "What's the pSEO topic pattern? Use `[variable]` for the keyword. The SaaS name comes from site.config.
   Examples:
   - '[SaaS_NAME] Integration with [platform]'
   - '[SaaS_NAME] for [audience]'
   - '[SaaS_NAME] vs [competitor]'
   - '[SaaS_NAME] Alternative to [competitor]'
   - 'Free [tool-type] Generator'
   - '[industry] Solution with [SaaS_NAME]'
   - '[template-type] Templates'"

2. **URL Structure**: "What's the URL structure?
   Examples:
   - `/integrations/[slug]` → `/integrations/slack`, `/integrations/zapier`
   - `/solutions/[slug]` → `/solutions/freelancers`, `/solutions/startups`
   - `/vs/[slug]` → `/vs/competitor-name`
   - `/alternatives/[slug]` → `/alternatives/competitor-name`
   - `/tools/[slug]` → `/tools/invoice-generator`
   - `/templates/[slug]` → `/templates/proposal`, `/templates/contract`
   - `/industries/[slug]` → `/industries/healthcare`, `/industries/real-estate`"

3. **Content Sections**: "What sections should each content page have? Describe each section's purpose and content.
   Example:
   - **Hero**: Headline with [variable] name, subheadline explaining the benefit
   - **Problem**: Describe the pain point this page addresses
   - **Solution**: How [SaaS_NAME] solves it for this [variable]
   - **Features**: Key features relevant to [variable]
   - **How It Works**: Step-by-step process
   - **FAQ**: 3-4 questions specific to [variable]
   - **CTA**: Final call to action"

4. **Index Page Content**: "What's the index page headline and intro text?
   Examples:
   - Headline: 'Integrations' / Intro: 'Connect [SaaS_NAME] with your favorite tools.'
   - Headline: 'Solutions for Every Industry' / Intro: 'See how [SaaS_NAME] works for your business.'
   - Headline: 'Compare [SaaS_NAME]' / Intro: 'See how we stack up against alternatives.'
   - Headline: 'Free Tools' / Intro: 'No signup required. Start using now.'
   - Headline: 'Templates' / Intro: 'Ready-to-use templates to get started fast.'"

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
Slack
Notion
Zapier
Stripe
HubSpot
```

**Process**:

1. **Parse Keywords** — Extract from user input

2. **Derive Variables** — For each keyword, create:
   - `keyword`: Original (e.g., "Slack")
   - `keyword_slug`: URL-safe (e.g., "slack")
   - `full_title`: From topic pattern (e.g., "Acme Integration with Slack")

3. **Generate Content** — For each keyword:
   - Create unique content based on template sections
   - Content should be specific to that keyword/platform
   - Save as `src/content/pseo/[full_slug].md`

4. **Update Footer** — Add link to pSEO index page in site footer:
   - Open `src/config/site.config.ts`
   - Add new link to `footer.links.product` array (e.g., `{ label: "{{INDEX_LABEL}}", href: "/{{INDEX_SLUG}}" }`)

5. **Verify Index** — Index page auto-pulls from content collection

**Output**:
```
✅ Generated 5 pSEO pages!

Created:
- src/content/pseo/[slug-1].md
- src/content/pseo/[slug-2].md
- src/content/pseo/[slug-3].md
- src/content/pseo/[slug-4].md
- src/content/pseo/[slug-5].md

Updated:
- src/config/site.config.ts (added footer link)

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
