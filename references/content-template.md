# Content Page Template

Structure for individual pSEO content pages.

## Template Structure

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';
import { getCollection } from 'astro:content';
import { siteConfig } from '../../config/site.config';

export async function getStaticPaths() {
  const pages = await getCollection('pseo');
  return pages.map((page) => ({
    params: { slug: page.data.keywordSlug },
    props: { page },
  }));
}

const { page } = Astro.props;
const { Content } = await page.render();

// Get other pages for internal linking
const allPages = await getCollection('pseo');
const otherPages = allPages.filter(p => p.data.keywordSlug !== page.data.keywordSlug).slice(0, 3);
---

<BaseLayout title={page.data.title} description={page.data.description}>
  <article>
    <!-- Hero Section -->
    <section class="{{HERO_SECTION_CLASS}}">
      <div class="{{HERO_CONTAINER_CLASS}}">
        <h1 class="{{HERO_HEADING_CLASS}}">{page.data.title}</h1>
        <p class="{{HERO_SUBHEADING_CLASS}}">{page.data.description}</p>
        <a href="/#pricing" class="{{HERO_CTA_CLASS}}">Get Started</a>
      </div>
    </section>

    <!-- Main Content (from markdown) -->
    <section class="{{CONTENT_SECTION_CLASS}}">
      <div class="prose prose-lg max-w-none {{PROSE_EXTRA_CLASS}}">
        <Content />
      </div>
    </section>

    <!-- Related Pages -->
    {otherPages.length > 0 && (
      <section class="{{RELATED_SECTION_CLASS}}">
        <div class="{{RELATED_CONTAINER_CLASS}}">
          <h2 class="{{RELATED_HEADING_CLASS}}">Related</h2>
          <div class="{{RELATED_GRID_CLASS}}">
            {otherPages.map((related) => (
              <a href={`/{{INDEX_SLUG}}/${related.data.keywordSlug}`} class="{{RELATED_CARD_CLASS}}">
                {related.data.title}
              </a>
            ))}
          </div>
        </div>
      </section>
    )}

    <!-- Final CTA -->
    <section class="{{FINAL_CTA_SECTION_CLASS}}">
      <div class="{{FINAL_CTA_CONTAINER_CLASS}}">
        <h2 class="{{FINAL_CTA_HEADING_CLASS}}">Ready to get started?</h2>
        <a href="/#pricing" class="{{FINAL_CTA_BUTTON_CLASS}}">
          Try {siteConfig.name} Free
        </a>
      </div>
    </section>
  </article>
</BaseLayout>
```

## Markdown Content Structure

Each content page is a markdown file with frontmatter + content:

```markdown
---
title: "Bulk Invoices Creation from Stripe CSV"
description: "Convert your Stripe CSV exports into professional invoices automatically."
keyword: "Stripe"
keywordSlug: "stripe-csv-to-invoice"
order: 1
---

## The Problem with Manual Invoice Creation

[Unique content about Stripe-specific pain points...]

## How {{SAAS_NAME}} Solves This

[Unique content about the solution for Stripe users...]

## Step-by-Step Process

1. **Export your Stripe CSV** — Download your transactions...
2. **Upload to {{SAAS_NAME}}** — Drag and drop...
3. **Download your invoices** — Get professional PDFs...

## Key Features for Stripe Users

- Automatic field mapping for Stripe CSV format
- [More Stripe-specific features...]

## Frequently Asked Questions

### What Stripe CSV formats are supported?
[Unique answer...]

### Can I customize the invoice template?
[Unique answer...]
```

## Content Generation Guidelines

When generating content for each keyword:

1. **Make it unique** — Don't just swap variables. Write content specific to that platform/keyword.

2. **Be specific** — Mention platform-specific features, pain points, and solutions.

3. **Add value** — Each page should genuinely help someone searching for that keyword.

4. **Natural keyword usage** — Include the keyword naturally, don't stuff.

5. **Internal links** — Link to:
   - Main site pages (pricing, features)
   - Other pSEO pages
   - Relevant blog posts or FAQs

## SEO Best Practices

- H1 = Primary keyword (title from frontmatter)
- Meta description unique per page
- Proper heading hierarchy (H2, H3)
- 500-1500 words of unique content
- Schema markup if applicable
