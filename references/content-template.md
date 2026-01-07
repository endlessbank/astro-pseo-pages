# Content Page Template

Structure for individual pSEO content pages.

## Sample Page (Mode 1)

During Mode 1, create a sample content page at `src/content/pseo/example.md` so users can preview the template:

```markdown
---
title: "Example Page Title"
description: "This is a sample page to preview the content template. It will be replaced when you generate real pages."
keyword: "Example"
keywordSlug: "example"
order: 0
---

## Sample Section

This is placeholder content to demonstrate the template structure. When you run "Generate pSEO pages", this sample will be deleted and replaced with your real content pages.

[Continue with all sections defined by the user, filled with realistic placeholder content...]
```

The sample page should:
- Use realistic placeholder content that matches the user's section brief
- Show all sections the user defined
- Be clearly marked as a sample/example
- Use "example" as the slug so it's obvious

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
title: "Best CRM for Healthcare"
description: "Discover why healthcare professionals choose our CRM for patient management and compliance."
keyword: "Healthcare"
keywordSlug: "healthcare"
order: 1
---

## The Challenge for Healthcare Teams

[Unique content about healthcare-specific pain points...]

## How {{SAAS_NAME}} Helps Healthcare Teams

[Unique content about the solution for healthcare users...]

## Key Features for Healthcare

- HIPAA-compliant data storage
- Patient record management
- [More healthcare-specific features...]

## Frequently Asked Questions

### Is your CRM HIPAA compliant?
[Unique answer...]

### Can I import existing patient records?
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
