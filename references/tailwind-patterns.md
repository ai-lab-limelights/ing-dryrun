# Tailwind CSS Technical Patterns — Design Agent Reference

Reusable Tailwind patterns for building landing pages. These are TECHNICAL building blocks only — no aesthetic direction. The design agent derives all visual choices from the competitor proposition.

## HTML Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><!-- competitor name --></title>
  
  <!-- Google Fonts — chosen per competitor -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DISPLAY+FONT&family=BODY+FONT&display=swap" rel="stylesheet">
  
  <!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
  tailwind.config = {
    theme: {
      extend: {
        colors: {
          // All colors derived from proposition
        },
        fontFamily: {
          'display': ['"Display Font"', 'sans-serif'],
          'body': ['"Body Font"', 'sans-serif'],
        }
      }
    }
  }
  </script>
  
  <style>
    /* Custom animations that Tailwind can't do natively */
  </style>
</head>
<body class="font-body">
  <!-- content -->
  
  <script>
    /* Scroll animations, interactions */
  </script>
</body>
</html>
```

## Scroll Animation Setup

```html
<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });

document.querySelectorAll('[data-animate]').forEach(el => observer.observe(el));
</script>

<style>
[data-animate] {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
[data-animate].animate-visible {
  opacity: 1;
  transform: translateY(0);
}
/* Stagger: add transition-delay via Tailwind's delay utilities or inline style */
</style>
```

Usage in HTML:
```html
<div data-animate class="...">Content fades in on scroll</div>
<div data-animate style="transition-delay: 0.1s" class="...">Staggered</div>
<div data-animate style="transition-delay: 0.2s" class="...">Staggered</div>
```

## Background Effect Techniques

These are technical recipes. Whether to use them depends on the brand.

### Gradient Mesh Background
```html
<style>
.gradient-mesh {
  position: absolute;
  inset: 0;
  overflow: hidden;
  z-index: 0;
}
.gradient-mesh::before {
  content: '';
  position: absolute;
  top: -50%; left: -50%;
  width: 200%; height: 200%;
  background: 
    radial-gradient(ellipse at 20% 50%, var(--glow-1, rgba(0,0,0,0.1)) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 20%, var(--glow-2, rgba(0,0,0,0.1)) 0%, transparent 40%);
  animation: mesh-drift 20s ease-in-out infinite alternate;
}
@keyframes mesh-drift {
  0% { transform: translate(0, 0) rotate(0deg); }
  100% { transform: translate(-3%, -2%) rotate(2deg); }
}
</style>
```

### Dot Grid Pattern
```html
<style>
.dot-grid {
  background-image: radial-gradient(circle, currentColor 1px, transparent 1px);
  background-size: 24px 24px;
  opacity: 0.06;
}
</style>
```

### Noise/Grain Overlay
```html
<style>
.grain-overlay::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  opacity: 0.03;
  z-index: 9999;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
</style>
```

### Floating Blobs
```html
<style>
.blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  animation: blob-float 15s ease-in-out infinite;
}
@keyframes blob-float {
  0%, 100% { transform: translate(0, 0) scale(1); }
  33% { transform: translate(20px, -15px) scale(1.05); }
  66% { transform: translate(-15px, 10px) scale(0.95); }
}
</style>
```

## Tailwind Component Patterns

### Fixed Navigation
```html
<nav class="fixed top-0 inset-x-0 z-50 backdrop-blur-xl border-b border-white/5">
  <div class="max-w-7xl mx-auto px-6 h-16 flex items-center justify-between">
    <a href="#" class="font-display text-lg font-bold">Logo</a>
    <div class="hidden md:flex items-center gap-8">
      <a href="#" class="text-sm hover:opacity-70 transition-opacity">Link</a>
      <!-- CTA button -->
    </div>
  </div>
</nav>
```

### Hero with Spacious Layout
```html
<section class="relative min-h-screen flex items-center pt-16">
  <div class="relative z-10 max-w-7xl mx-auto px-6 py-24">
    <h1 class="font-display text-5xl md:text-7xl font-bold leading-tight max-w-4xl">
      <!-- Headline -->
    </h1>
    <p class="mt-6 text-lg md:text-xl max-w-2xl opacity-70">
      <!-- Subhead -->
    </p>
    <div class="mt-10 flex flex-wrap gap-4">
      <!-- CTA buttons -->
    </div>
  </div>
</section>
```

### Feature Cards Grid
```html
<div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
  <div class="group p-8 rounded-2xl border transition-all duration-300 hover:-translate-y-1 hover:shadow-xl">
    <!-- icon/visual -->
    <h3 class="font-display text-xl font-semibold mt-4">Feature</h3>
    <p class="mt-2 opacity-70 leading-relaxed">Description</p>
  </div>
</div>
```

### Metric/Stats Row
```html
<div class="grid grid-cols-2 md:grid-cols-4 gap-8">
  <div class="text-center" data-animate>
    <div class="font-display text-4xl md:text-5xl font-bold">€2.3B</div>
    <div class="mt-2 text-sm uppercase tracking-wider opacity-50">Processed</div>
  </div>
</div>
```

### Testimonial Card
```html
<div class="p-8 rounded-2xl border">
  <p class="text-lg leading-relaxed italic">"Quote here."</p>
  <div class="mt-6 flex items-center gap-4">
    <div class="w-12 h-12 rounded-full flex items-center justify-center font-bold text-sm">
      AB
    </div>
    <div>
      <div class="font-semibold">Name</div>
      <div class="text-sm opacity-60">Title, Company</div>
    </div>
  </div>
</div>
```

### CTA Section
```html
<section class="py-24 text-center">
  <div class="max-w-3xl mx-auto px-6">
    <h2 class="font-display text-3xl md:text-5xl font-bold">
      <!-- Closing headline -->
    </h2>
    <p class="mt-4 text-lg opacity-70"><!-- Subtext --></p>
    <div class="mt-8">
      <!-- Primary CTA button -->
    </div>
  </div>
</section>
```

## Icon Libraries (use based on brand fit)

Load via CDN when needed — only if the design calls for icons:

**Lucide** (clean, minimal line icons)
```html
<script src="https://unpkg.com/lucide@latest"></script>
<script>lucide.createIcons();</script>
<i data-lucide="arrow-right"></i>
```

**Heroicons** (Tailwind ecosystem, two styles)
Use inline SVGs from https://heroicons.com — copy SVG markup directly.

**Phosphor** (versatile, 6 weights)
```html
<script src="https://unpkg.com/@phosphor-icons/web"></script>
<i class="ph ph-arrow-right"></i>
```

**No icons** — sometimes pure typography and spacing is stronger. Don't add icons by default.

## Responsive Breakpoints (Tailwind defaults)

- `sm:` — 640px+
- `md:` — 768px+
- `lg:` — 1024px+
- `xl:` — 1280px+

Mobile-first: write base styles for mobile, add prefixes for larger screens.

## Performance Notes

- Tailwind CDN is fine for single-page prototypes
- Keep custom `<style>` blocks minimal — use Tailwind utilities where possible
- Use `loading="lazy"` on any media elements
- Intersection Observer for scroll animations is lightweight and performant
- Avoid animating properties that trigger layout (width, height, margin) — stick to `transform` and `opacity`
