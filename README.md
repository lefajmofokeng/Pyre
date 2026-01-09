# Pyre — Portfolio Gallery Component

## Overview
Pyre is a minimalist portfolio gallery component showcasing selected work in a clean 2×2 grid. Designed for designers and creatives, it features large typography, image cards with overlays, and a refined aesthetic.

## Live Demo

[View Pyre](https://lefajmofokeng.github.io/Pyre)

---

## Technology
- **HTML5**: Semantic structure with article elements
- **CSS3**: Grid layout, aspect-ratio, custom fonts
- **Custom Fonts**: Creato Display (local woff2 + CDN fallback)
- **No JavaScript**: Pure CSS implementation
- **Pexels API**: High-quality portfolio images

---

## Integration

### Basic Setup
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link href="https://fonts.cdnfonts.com/css/creato-display" rel="stylesheet">
    <link rel="stylesheet" href="pyre.css">
</head>
<body>
    <!-- Pyre component -->
</body>
</html>
```

### React Component
```jsx
import './pyre.css';

function PyrePortfolio({ works = [], heading = "Selected Work", description }) {
  return (
    <section className="sw-wrapper">
      <div className="sw-inner-container">
        <header className="sw-top-bar">
          <h1 className="sw-heading-hero">{heading}</h1>
          <div className="sw-header-info">
            <p className="sw-text-desc">{description}</p>
            <div className="sw-badge-count">{works.length}</div>
          </div>
        </header>
        
        <div className="sw-gallery-grid">
          {works.map((work, index) => (
            <article key={index} className="sw-card">
              <img src={work.image} alt={work.alt} className="sw-card-img" />
              <div className="sw-card-overlay">
                <span className="sw-lbl">{work.category}</span>
                <span className="sw-lbl">{work.type}</span>
              </div>
            </article>
          ))}
        </div>
      </div>
    </section>
  );
}
```

### WordPress Integration
```php
function pyre_portfolio_shortcode($atts) {
    $works = get_posts([
        'post_type' => 'portfolio',
        'posts_per_page' => 4,
        'meta_key' => '_featured',
        'meta_value' => 'yes'
    ]);
    
    ob_start();
    ?>
    <section class="sw-wrapper">
        <div class="sw-inner-container">
            <header class="sw-top-bar">
                <h1 class="sw-heading-hero">Selected<br>Work</h1>
                <div class="sw-header-info">
                    <p class="sw-text-desc">
                        2025© — A selection of featured portfolio works.
                    </p>
                    <div class="sw-badge-count"><?php echo count($works); ?></div>
                </div>
            </header>
            
            <div class="sw-gallery-grid">
                <?php foreach ($works as $work): ?>
                <article class="sw-card">
                    <?php echo get_the_post_thumbnail($work->ID, 'large', ['class' => 'sw-card-img']); ?>
                    <div class="sw-card-overlay">
                        <span class="sw-lbl"><?php echo get_the_category($work->ID)[0]->name; ?></span>
                        <span class="sw-lbl">Portfolio</span>
                    </div>
                </article>
                <?php endforeach; ?>
            </div>
        </div>
    </section>
    <?php
    return ob_get_clean();
}
add_shortcode('pyre_portfolio', 'pyre_portfolio_shortcode');
```

---

## Customization

### Font Configuration
```css
/* Local font loading (recommended for performance) */
@font-face {
    font-family: 'Creato Display';
    src: url('/fonts/CreatoDisplay-Regular.woff2') format('woff2');
    font-weight: 400;
    font-display: swap;
}

@font-face {
    font-family: 'Creato Display';
    src: url('/fonts/CreatoDisplay-Bold.woff2') format('woff2');
    font-weight: 700;
    font-display: swap;
}

/* CDN fallback */
.sw-wrapper {
    font-family: 'Creato Display', -apple-system, BlinkMacSystemFont, sans-serif;
}
```

### Layout Variations
```css
/* 3-column grid */
.sw-gallery-grid.three-col {
    grid-template-columns: repeat(3, 1fr);
}

/* Masonry layout */
.sw-gallery-grid.masonry {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    grid-auto-rows: 250px;
}

/* Full-bleed card */
.sw-card.full-bleed {
    aspect-ratio: 16 / 9;
    grid-column: 1 / -1;
}
```

### Enhanced Card Effects
```css
/* Gradient overlay */
.sw-card::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 50%;
    background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.sw-card:hover::after {
    opacity: 1;
}

/* Label animation */
.sw-card:hover .sw-lbl {
    transform: translateY(-5px);
    color: #ffffff;
}

/* Dark mode support */
@media (prefers-color-scheme: dark) {
    .sw-wrapper {
        background-color: #111111;
        color: #f4f4f4;
    }
    
    .sw-card {
        background-color: #222222;
    }
    
    .sw-lbl {
        color: #f4f4f4;
    }
}
```

### Theming System
```css
:root {
    --pyre-bg: #F4F4F4;
    --pyre-text: #111111;
    --pyre-accent: #111111;
    --pyre-white: #FFFFFF;
    --pyre-card-radius: 15px;
    --pyre-grid-gap: 12px;
    --pyre-font: 'Creato Display', sans-serif;
}

.sw-wrapper {
    background-color: var(--pyre-bg);
    color: var(--pyre-text);
    font-family: var(--pyre-font);
}

.sw-card {
    border-radius: var(--pyre-card-radius);
}

.sw-gallery-grid {
    gap: var(--pyre-grid-gap);
}
```

---

## Responsive Design

### Breakpoints
- **Desktop (1024px+)**: 140px hero type, 2-column grid
- **Tablet (768px-1024px)**: 100px hero type, 2-column grid
- **Mobile (<768px)**: 70px hero type, single column, stacked header

### Fluid Typography
```css
.sw-heading-hero {
    font-size: clamp(70px, 12vw, 140px);
    line-height: 0.9;
    letter-spacing: clamp(-1px, -0.1vw, -2px);
}

.sw-text-desc {
    font-size: clamp(14px, 1.5vw, 16px);
}
```

### Touch Optimization
```css
@media (hover: none) and (pointer: coarse) {
    .sw-card {
        min-height: 250px; /* Larger touch target */
    }
    
    .sw-card:hover {
        transform: none; /* Disable hover effects on touch */
    }
    
    .sw-card-overlay {
        padding: 20px; /* Larger touch area */
    }
}
```

---

## Performance

### Optimizations
- **Aspect-ratio CSS**: Maintains image proportions without JavaScript
- **Local font loading**: woff2 format with font-display: swap
- **Object-fit cover**: Efficient image cropping
- **Minimal animations**: Only essential hover effects
- **Responsive images**: Serve appropriate image sizes

### Loading Strategy
```html
<!-- Preload critical fonts -->
<link rel="preload" href="/fonts/CreatoDisplay-Regular.woff2" as="font" type="font/woff2" crossorigin>

<!-- Lazy load non-critical images -->
<img 
    src="placeholder.jpg" 
    data-src="portfolio-image.jpg" 
    class="sw-card-img lazy"
    alt="Portfolio work"
    loading="lazy"
>
```

### Core Web Vitals
- **LCP**: Optimize hero image and font loading
- **CLS**: Stable grid with aspect-ratio
- **INP**: Minimal interactive elements

---

## Accessibility

### WCAG Compliance
- **Semantic HTML**: article, header, h1 elements
- **Color contrast**: 4.5:1 minimum ratio
- **Keyboard navigation**: Focusable cards
- **Screen readers**: Alt text for all images
- **Reduced motion**: Respects user preferences

### Enhanced Accessibility
```css
/* Focus styles */
.sw-card:focus {
    outline: 3px solid #0056b3;
    outline-offset: 2px;
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
    .sw-card {
        transition: none;
    }
    
    .sw-card:hover {
        transform: none;
    }
}

/* High contrast mode */
@media (prefers-contrast: high) {
    .sw-wrapper {
        background-color: #ffffff;
        color: #000000;
    }
    
    .sw-card {
        border: 2px solid #000000;
    }
    
    .sw-badge-count {
        border: 2px solid #000000;
    }
}
```

---

## Browser Support

| Browser | Version | Key Features |
|---------|---------|--------------|
| Chrome  | 88+     | aspect-ratio, gap, object-fit |
| Firefox | 89+     | aspect-ratio, gap |
| Safari  | 14+     | aspect-ratio, gap |
| Edge    | 88+     | aspect-ratio, gap |

### Fallbacks
```css
/* aspect-ratio fallback */
.sw-card {
    padding-bottom: 75%; /* 4:3 aspect ratio */
    height: 0;
}

@supports (aspect-ratio: 4 / 3) {
    .sw-card {
        padding-bottom: 0;
        height: auto;
        aspect-ratio: 4 / 3;
    }
}

/* Grid gap fallback */
.sw-gallery-grid {
    margin: -6px; /* Half of gap */
}

.sw-card {
    margin: 6px;
}

@supports (gap: 12px) {
    .sw-gallery-grid {
        margin: 0;
    }
    
    .sw-card {
        margin: 0;
    }
}
```

---

## SEO & Metadata

### Best Practices
- Semantic HTML structure
- Descriptive alt text for images
- Proper heading hierarchy
- Mobile-responsive design
- Fast loading performance

### Structured Data
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "CollectionPage",
  "name": "Selected Work Portfolio",
  "description": "A selection of digital design works",
  "creator": {
    "@type": "Person",
    "name": "Your Name"
  },
  "image": [
    "https://example.com/portfolio-image-1.jpg",
    "https://example.com/portfolio-image-2.jpg"
  ]
}
</script>
```

---

## Maintenance

### Version Information
- **Current**: v1.0
- **License**: MIT
- **Dependencies**: None
- **Last Updated**: [Current Date]

### Update Checklist
- [ ] Test with latest browser versions
- [ ] Validate accessibility compliance
- [ ] Check performance metrics
- [ ] Update image assets as needed
- [ ] Review responsive behavior

---

## Quick Start

1. **Clone or download** the component files
2. **Add fonts** to `/fonts/` directory or use CDN
3. **Update image paths** in HTML
4. **Customize colors and typography** in CSS variables
5. **Deploy** to your hosting service

### File Structure
```
project/
├── index.html
├── style.css
├── fonts/
│   ├── CreatoDisplay-Regular.woff2
│   └── CreatoDisplay-Bold.woff2
└── README.md
```

---

## Troubleshooting

### Common Issues
1. **Fonts not loading**: Check font paths and CORS headers
2. **Images not cropping**: Ensure object-fit: cover is supported
3. **Grid gaps in IE11**: Use margin-based fallback
4. **Hover effects on mobile**: Use @media (hover: hover)

### Quick Fixes
```css
/* Font fallback */
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

.fonts-loaded body {
    font-family: 'Creato Display', sans-serif;
}

/* Image loading states */
.sw-card-img {
    background-color: #E5E5E5;
}

.sw-card-img.loaded {
    background-color: transparent;
}
```

---

**Pyre** — Clean portfolio gallery for creative professionals.
