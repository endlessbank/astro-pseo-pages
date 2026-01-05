# Index Page Template

Structure for the pSEO index page.

## Template Structure

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';
import { getCollection } from 'astro:content';
import { siteConfig } from '../../config/site.config';

const pseoPages = (await getCollection('pseo')).sort(
  (a, b) => a.data.order - b.data.order
);
---

<BaseLayout title="{{INDEX_TITLE}}" description="{{INDEX_DESCRIPTION}}">
  <!-- Hero Section -->
  <section class="{{HERO_SECTION_CLASS}}">
    <div class="{{HERO_CONTAINER_CLASS}}">
      <h1 class="{{HERO_HEADING_CLASS}}">
        {{INDEX_HEADLINE}}
      </h1>
      <p class="{{HERO_SUBHEADING_CLASS}}">
        {{INDEX_INTRO}}
      </p>
    </div>
  </section>

  <!-- Pages Grid -->
  <section class="{{GRID_SECTION_CLASS}}">
    <div class="{{GRID_CONTAINER_CLASS}}">
      <div class="{{GRID_CLASS}}">
        {pseoPages.map((page) => (
          <a href={`/{{INDEX_SLUG}}/${page.data.keywordSlug}`} class="{{CARD_CLASS}}">
            <h2 class="{{CARD_TITLE_CLASS}}">{page.data.title}</h2>
            <p class="{{CARD_DESC_CLASS}}">{page.data.description}</p>
            <span class="{{CARD_LINK_CLASS}}">Learn more →</span>
          </a>
        ))}
      </div>
    </div>
  </section>

  <!-- CTA Section -->
  <section class="{{CTA_SECTION_CLASS}}">
    <div class="{{CTA_CONTAINER_CLASS}}">
      <h2 class="{{CTA_HEADING_CLASS}}">{{CTA_HEADLINE}}</h2>
      <p class="{{CTA_TEXT_CLASS}}">{{CTA_SUBTEXT}}</p>
      <a href="/#pricing" class="{{CTA_BUTTON_CLASS}}">
        Get Started
      </a>
    </div>
  </section>
</BaseLayout>
```

## Styling Guidelines

- Match existing site's BaseLayout and styling
- Use consistent color scheme from site.config
- Cards should be visually appealing and clickable
- Responsive grid (1 col mobile, 2-3 cols desktop)
- CTA should match other CTAs on the site

## Dynamic Content

The grid automatically populates from the `pseo` content collection. When new content pages are added, they appear automatically.

## SEO Considerations

- Unique title and meta description
- H1 contains primary keyword
- Each card links to content page with descriptive anchor text
