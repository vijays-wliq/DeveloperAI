# FULL HTML → PRODUCTION-READY WORDPRESS THEME
## Expert PHP / WordPress Developer Project Instructions

> **ROLE:** You are a senior PHP + WordPress theme developer.
>
> **PRIMARY TASK:** Convert the complete existing HTML website in this folder into a production-ready, maintainable, secure, responsive WordPress theme.
>
> **CRITICAL:** Do NOT start coding before understanding the existing website. First inspect the complete HTML project, then design the WordPress architecture, then convert it.
>
> **ELEMENTOR PROJECTS:** Read the **⭐ MAIN REQUIREMENT in §96** first. The Elementor site must match the original HTML site visually and functionally. That rule wins over everything else in this file.
>
> **SITE ALREADY BUILT, ONLY AN UPDATE?** Use **§143 Quick Update Mode** — read only the matching section and go straight to the point.

> **HOW TO USE THIS FILE — PICK THE MODE FIRST**
>
> | Project type | Follow | Ignore |
> |---|---|---|
> | HTML → custom PHP theme (no page builder) | Use `CLAUDE-WORDPRESS-CONVERSION-EXPERT.md` instead — it has the importer, page builder, ACF and forms rules (§1–§72 here are its older base) | this file |
> | Existing WordPress theme to update/extend | §73–§95 (+ §1–§72 for new parts) | — |
> | HTML / PDF / Figma → **Elementor Pro** (usual WLIQ case) | §96–§151 (+ §23–§33, §39–§44 for PHP, SEO, a11y) | the PHP-template parts of §1–§72 (each has an **Elementor build** note pointing to the Elementor section) |
>
> §146–§149 (lessons, bug library, sign-off, build-right-first-time) apply to **every Elementor project**. The numbers in them (gaps 40/30/20, card padding 40/30/25/20, wave height 35–41 px, 2 hero slides, 4 s marquee…) are **examples from the Buell Recreation project** — on a new project always take the values from the new design. The *rule* (e.g. "set the gap at every breakpoint", "same padding on 4 sides") is what carries over.

---

# 1. PRIMARY OBJECTIVE

Convert the **entire existing static HTML website** into a proper custom WordPress theme.

The final result must:

- Preserve the existing frontend design.
- Preserve the existing HTML structure wherever practical.
- Preserve CSS.
- Preserve JavaScript.
- Preserve animations.
- Preserve responsive behavior.
- Preserve images and fonts.
- Preserve third-party libraries.
- Preserve existing functionality.
- Convert appropriate static content into dynamic WordPress content.
- Create a clean and maintainable WordPress theme.
- Follow WordPress/PHP best practices.
- Avoid PHP warnings, notices, fatal errors, broken queries, broken URLs, broken assets, and JavaScript errors.
- Never redesign the website unless explicitly requested.

The goal is:

```text
Existing HTML Website
        ↓
Analyze Complete Project
        ↓
Plan WordPress Architecture
        ↓
Custom WordPress Theme
        ↓
Same / Equivalent Frontend
        +
Proper WordPress CMS
```

---

# 2. VERY IMPORTANT: INSPECT FIRST, CODE SECOND

Before creating or modifying any PHP file:

## Step 1 — Inventory the complete project

Inspect:

```text
HTML files
CSS files
SCSS files if present
JavaScript files
Images
SVGs
Fonts
Videos
JSON files
XML files
Third-party libraries
Plugins/scripts
Forms
API integrations
Animations
Responsive styles
```

Do not inspect only `index.html`.

The whole project must be understood before conversion.

---

# 3. CREATE A SITE INVENTORY

Before coding, identify:

### Pages

Example:

```text
index.html
about.html
services.html
contact.html
partners.html
partner-detail.html
blog.html
blog-detail.html
```

### Common components

Identify:

```text
Header
Desktop Navigation
Mobile Navigation
Footer
Footer Navigation
Announcement Bar
CTA
Breadcrumb
Cookie banner
Modals
Forms
Sliders
Cards
```

### Dynamic-looking content

Identify content that should become WordPress-managed:

```text
Partners
Blog Posts
Menus
Logo
Footer content
Social links
CTA
Contact details
Page content
Images
Categories
Tags
```

---

# 4. HEADER MUST BE INSPECTED BEFORE BUILDING

## DO NOT create a generic WordPress header.

First inspect the original HTML header.

Analyze:

- Exact DOM structure
- Header classes
- Logo structure
- Desktop menu
- Mobile menu
- Dropdowns
- Mega menu
- CTA buttons
- Search
- Icons
- Sticky behavior
- Header states
- Scroll behavior
- Mobile breakpoint
- Header JavaScript
- Header CSS
- Header animations
- Header assets

Then convert that exact structure into:

```text
header.php
```

### Rule

Preserve:

```text
DOM structure
class names
IDs where necessary
CSS hooks
JS hooks
data attributes
ARIA attributes
responsive behavior
```

Only replace hard-coded content with WordPress dynamic functionality.

---

# 5. FOOTER MUST BE INSPECTED BEFORE BUILDING

## DO NOT create a generic WordPress footer.

First inspect the original HTML footer.

Analyze:

- Footer DOM
- Footer columns
- Footer navigation
- Logo
- Description
- Contact details
- Social links
- Newsletter
- CTA
- Legal links
- Copyright
- Footer scripts
- Footer CSS
- Responsive behavior

Then convert it into:

```text
footer.php
```

Preserve the original structure and styling.

---

# 6. HEADER DYNAMIC REQUIREMENTS

The header should support WordPress management where applicable.

Possible dynamic fields:

```text
Logo
Mobile Logo
Primary Menu
CTA Text
CTA URL
CTA visibility
Announcement bar
Announcement text
Announcement URL
Search
```

Do not create fields that are not required by the original site.

---

# 7. WORDPRESS MENUS

Register WordPress menu locations based on the actual HTML.

Typical:

```php
register_nav_menus([
    'primary' => __('Primary Menu', 'site-theme'),
    'footer'  => __('Footer Menu', 'site-theme'),
]);
```

If the mobile menu uses the same navigation, do not duplicate the menu unnecessarily.

Use:

```php
wp_nav_menu()
```

instead of hard-coded navigation links.

### Preserve

- Menu classes where possible
- Dropdown behavior
- Existing HTML hooks
- Mobile menu behavior
- Active states

If custom walker logic is required to reproduce the existing menu structure, create a proper custom Walker instead of hacking the output with JavaScript.

---

# 8. FOOTER DYNAMIC REQUIREMENTS

Use WordPress settings/custom fields only for content that needs admin control.

Potential settings:

```text
Footer Logo
Footer Description
Email
Phone
Address
Social Links
Copyright
Footer CTA
```

Footer menu should use a registered WordPress menu.

---

# 9. WORDPRESS THEME STRUCTURE

> **Elementor build:** the theme is the one in §135 (`assets/css/custom.css`, `assets/js/vendor.js`). Header, footer, pages, singles and archives are Elementor Theme Builder templates (§105, §107, §111, §112, §125) — not the PHP template files below.

Create a proper custom theme.

Recommended:

```text
theme-name/
│
├── style.css
├── functions.php
├── index.php
├── front-page.php
├── home.php
├── page.php
├── single.php
├── archive.php
├── category.php
├── tag.php
├── search.php
├── 404.php
├── header.php
├── footer.php
│
├── single-partner.php
├── archive-partner.php
│
├── template-parts/
│   ├── header/
│   ├── footer/
│   ├── content/
│   ├── blog/
│   └── partner/
│
├── inc/
│   ├── theme-setup.php
│   ├── enqueue.php
│   ├── post-types.php
│   ├── taxonomies.php
│   ├── theme-settings.php
│   ├── custom-fields.php
│   ├── template-functions.php
│   └── helpers.php
│
└── assets/
    ├── css/
    ├── js/
    ├── images/
    ├── fonts/
    └── vendor/
```

Do not put the entire project into one huge `functions.php`.

---

# 10. FUNCTIONS.PHP

> **Elementor build:** keep `functions.php` light the same way; it loads only theme setup, enqueue (§135), CPTs and helpers — no PHP page templates.

Keep `functions.php` lightweight.

Example:

```php
require_once get_template_directory() . '/inc/theme-setup.php';
require_once get_template_directory() . '/inc/enqueue.php';
require_once get_template_directory() . '/inc/post-types.php';
require_once get_template_directory() . '/inc/taxonomies.php';
require_once get_template_directory() . '/inc/theme-settings.php';
require_once get_template_directory() . '/inc/custom-fields.php';
require_once get_template_directory() . '/inc/template-functions.php';
require_once get_template_directory() . '/inc/helpers.php';
```

Use `require_once`.

---

# 11. PARTNERS — CUSTOM POST TYPE

> **Elementor build:** register the CPT the same way, but its single and archive are Elementor Theme Builder templates (§111) — no `archive-partner.php` / `single-partner.php`.

If the existing site has dedicated Partner pages, create:

```text
partner
```

as a Custom Post Type.

Expected admin:

```text
Partners
├── All Partners
└── Add New Partner
```

Expected URLs:

```text
/partners/
/partners/partner-name/
```

Register with proper:

```text
labels
public
has_archive
rewrite
supports
show_in_rest
```

Use the WordPress template hierarchy.

Required:

```text
archive-partner.php
single-partner.php
```

---

# 12. PARTNER FIELDS

> **Elementor build:** fields = ACF PRO, shown with Elementor Dynamic Tags (§111).

Inspect the original Partner HTML first.

Only then determine required fields.

Possible:

```text
Partner Name
Partner Logo
Hero Image
Short Description
Content
Website URL
Location
Partner Type
Gallery
CTA Text
CTA URL
Featured Partner
```

Do not blindly create all fields.

Custom fields are **always ACF PRO** (MANDATORY):

- Never install, activate or require **Secure Custom Fields (SCF)** next to ACF PRO.
- Never hand-make meta boxes or custom meta fields — do not recreate ACF functionality manually.
- Save every field group as **ACF local JSON** in the theme's `acf-json/` folder and sync it
  (ACF → Field Groups → Sync), so fields travel with the theme.

---

# 13. PARTNER TAXONOMIES

Only create taxonomies if the website actually needs them.

Possible:

```text
Partner Type
Partner Industry
Partner Location
```

Do not create unnecessary database structures.

---

# 14. BLOG

> **Elementor build:** use native Posts as below; the blog archive and single are Theme Builder templates (§112), not `home.php` / `single.php`.

Blog must remain separate from Partners.

Use the native WordPress Post system:

```text
post
```

Do NOT create a `blog` CPT unless there is a specific requirement.

Use:

```text
home.php
single.php
archive.php
category.php
tag.php
```

based on the existing design.

---

# 15. BLOG FEATURES

Preserve existing blog functionality:

```text
Title
Featured Image
Content
Author
Publish Date
Categories
Tags
Related Posts
Pagination
Search
```

Only implement features that exist or are required.

---

# 16. STANDARD WORDPRESS PAGES

Use native Pages for normal website content:

```text
About
Services
Contact
Privacy
Terms
Other static pages
```

Use:

```text
page.php
```

or specific templates only when necessary.

---

# 17. HOMEPAGE

> **Elementor build:** Home is a normal WordPress page built with Elementor and set as the static front page — not `front-page.php`. Dynamic parts use Loop Grid / Dynamic Tags (§104, §113).

**Only for a non-Elementor (custom PHP) theme** — use:

```text
front-page.php
```

The homepage must be converted from the actual HTML.

Do not redesign.

If homepage sections contain dynamic data, connect them to:

```text
Pages
Partners
Posts
Theme Settings
Custom Fields
```

as appropriate.

---

# 18. TEMPLATE HIERARCHY

> **Elementor build:** Theme Builder templates + display conditions (§125) replace this PHP template hierarchy.

Follow WordPress template hierarchy.

Preferred examples:

```text
front-page.php
page.php
page-{slug}.php

home.php

single.php
single-partner.php

archive.php
archive-partner.php

category.php
tag.php

search.php
404.php
```

Do not create duplicate templates without a reason.

---

# 19. ASSETS

All existing assets must be audited.

Move theme assets into:

```text
assets/
```

For example:

```text
assets/css/
assets/js/
assets/images/
assets/fonts/
assets/vendor/
```

Use WordPress functions to reference them.

Example:

```php
get_template_directory_uri()
```

Do not use fragile relative paths such as:

```text
../../images/
```

---

# 20. CSS

> **Elementor build:** CSS follows **§114** — cover everything with native Elementor controls first; only when a native control cannot do it, add a CSS class on that element with its rule in `assets/css/custom.css` (§135). Keep the HTML's *look* (spacing, type, breakpoints, animations), not its selectors and classes.

## CRITICAL

Do not rewrite the CSS unnecessarily.

Preserve:

- Existing selectors
- Existing classes
- Existing spacing
- Existing typography
- Existing breakpoints
- Existing animations
- Existing responsive behavior

Do not add global styles that can break existing components.

Do not use `!important` unless genuinely necessary.

Do not introduce a CSS framework simply because WordPress is being introduced.

---

# 21. JAVASCRIPT

> **Elementor build:** scripts load from the theme with `wp_enqueue_script()` (`assets/js/vendor.js`, §135). Editor-safe init: §121.

Audit all JavaScript before changing it.

Check:

```text
Navigation
Mobile menu
Sliders
Tabs
Accordions
Modals
Animations
GSAP
ScrollTrigger
Three.js
AJAX
Forms
API integrations
Lazy loading
Custom interactions
```

Preserve existing behavior.

Load scripts with:

```php
wp_enqueue_script()
```

Do not duplicate libraries.

Respect dependencies and execution order.

---

# 22. GSAP / THREE.JS / ANIMATIONS

> **Elementor build:** see **§121**.

If the existing website uses:

```text
GSAP
ScrollTrigger
Three.js
Swiper
AOS
or other libraries
```

preserve them.

Do not replace a working animation with a different implementation unless necessary.

After conversion verify:

- animation timing
- scroll triggers
- canvas rendering
- model loading
- texture loading
- responsive behavior
- mobile behavior
- console errors

---

# 23. PHP SECURITY

All output must be escaped.

Use:

```php
esc_html()
esc_attr()
esc_url()
wp_kses_post()
```

Sanitize incoming values.

Use nonces for custom admin forms.

Use capability checks.

Never trust:

```text
$_GET
$_POST
$_REQUEST
custom fields
user input
```

without validation/sanitization.

---

# 24. DATABASE / QUERY SAFETY

Use WordPress APIs and `WP_Query` where possible.

Example:

```php
$query = new WP_Query([
    'post_type'      => 'partner',
    'posts_per_page' => 12,
]);
```

Do not write raw SQL unless there is a real requirement.

If raw SQL is required:

- Use `$wpdb`
- Use `$wpdb->prepare()`
- Never concatenate user input into SQL
- Validate input
- Escape output

Avoid N+1 queries.

Do not run unnecessary database queries on every page.

---

# 25. WORDPRESS LOOP

Use proper loops.

Example:

```php
if (have_posts()) :
    while (have_posts()) :
        the_post();

        // Template output

    endwhile;
endif;
```

For custom queries:

```php
$query = new WP_Query($args);

if ($query->have_posts()) :
    while ($query->have_posts()) :
        $query->the_post();

        // Output

    endwhile;
endif;

wp_reset_postdata();
```

Never forget:

```php
wp_reset_postdata();
```

after custom loops.

---

# 26. IMAGE HANDLING

> **Elementor build:** images go into the Media Library once and are set in the widgets — see §131, §136, §140.

Use WordPress image functions.

Prefer:

```php
the_post_thumbnail()
wp_get_attachment_image()
get_the_post_thumbnail_url()
```

Do not hard-code uploaded image URLs.

Register image sizes if needed.

Do not unnecessarily load full-resolution images.

---

# 27. RESPONSIVE DESIGN

The WordPress version must match the original responsive behavior.

Test:

```text
Mobile
Tablet
Laptop
Desktop
Large Desktop
```

Verify:

```text
Header
Menu
Hero
Cards
Images
Partner pages
Blog pages
Footer
Forms
Animations
```

---

# 28. FORMS

Inspect all existing forms.

Determine whether they use:

```text
HTML
PHP
AJAX
API
CRM
Third-party service
WordPress plugin
```

Do not change the backend integration without understanding it.

Forms must be:

- Secure
- Validated
- Responsive
- Working
- Spam-protected where appropriate

> **Forms plugin — always ask at the start (MANDATORY), one question (§65):**
> *Which forms plugin should the site use? 1. Elementor Pro Form widget  2. Gravity Forms  3. Contact Form 7.*
> Never assume it. Use **only** the chosen one for every form on the site. Elementor Form widget:
> style with its native controls + globals, Email action to the admin email, Honeypot / reCAPTCHA,
> follow the Form-widget gotchas in §147.10. Gravity Forms / CF7: turn off the plugin's default look
> and make the form match the design. If the original form posts to a CRM / API, keep that
> integration through the chosen option.

---

# 29. SEO

Preserve existing SEO.

Check:

```text
Title
Meta Description
Canonical
H1
H2/H3 structure
Image ALT
Open Graph
Schema
Sitemap
Robots
URLs
Redirects
```

If an SEO plugin exists, integrate with it instead of duplicating its functionality.

---

# 30. URL MIGRATION

Create an old-to-new URL mapping.

Example:

```text
/about.html
→
/about/

/partners.html
→
/partners/

/partner-a.html
→
/partners/partner-a/

/blog.html
→
/blog/

/blog-post.html
→
/blog/blog-post/
```

Do not blindly change URLs.

Preserve existing production URLs whenever practical.

Where URLs must change, implement 301 redirects.

---

# 31. 404 AND REDIRECTS

> **Elementor build:** the 404 and Search pages are Theme Builder templates built per the design (§138). Redirects as below.

Create:

```text
404.php
```

Review old URLs and add required redirects.

Do not create redirect loops.

Test redirects.

---

# 32. WORDPRESS SETTINGS

Use native WordPress functionality where possible.

Do not build a custom setting for something WordPress already manages.

Use custom Theme Settings only for site-specific fields such as:

```text
Header Logo
Header CTA
Footer Contact
Social Links
Footer Copyright
```

---

# 33. ADMIN UX

The WordPress backend should be understandable.

Recommended:

```text
Dashboard

Posts
Media
Pages
Partners
Comments

Appearance
    Menus / Navigation
    Theme Settings

Users
Tools
Settings
```

Partner editors should not need to edit PHP or HTML.

---

# 34. BUILDER IN THIS FILE = ELEMENTOR PRO

This file builds the site with **Elementor Pro** (§96). The HTML site stays the source of truth for the
design and behaviour (⭐ §96 Main Requirement); Elementor is how it is rebuilt.

- Where a section in §1–§72 says to make a PHP template (`front-page.php`, `archive-partner.php`,
  `single.php`, `template-parts/` …), follow its **Elementor build** note instead — Theme Builder
  templates, Loop Items and native widgets.
- Do not paste the HTML into one big HTML / Custom HTML widget. Rebuild each section with native
  widgets (§113).
- The user asks for **no page builder** (custom PHP theme)? Then this is the wrong file — use
  `CLAUDE-WORDPRESS-CONVERSION-EXPERT.md`.

---

# 35. DO NOT USE A GENERIC THEME

Do not install a generic theme and place the HTML inside it.

Build a proper custom theme from the supplied project.

---

# 36. NO REDESIGN RULE

This project is a conversion, not a redesign.

Do NOT:

- Change colors
- Change typography
- Change spacing
- Change layout
- Change animations
- Change component appearance
- Replace icons
- Replace images
- Replace navigation
- Change responsive breakpoints

unless explicitly required.

---

# 37. CODE QUALITY

Write production-quality PHP.

Requirements:

- Clean naming
- Small reusable functions
- Proper comments for complex logic
- No dead code
- No duplicated code
- No unnecessary globals
- No debug output
- No `var_dump()`
- No `print_r()`
- No test code in production
- No hard-coded environment-specific URLs
- No hard-coded database credentials

---

# 38. DEBUGGING

During development, WordPress debugging may be enabled.

Use:

```php
WP_DEBUG
WP_DEBUG_LOG
```

where appropriate.

Do not display PHP errors to end users in production.

Before completion:

- Fix PHP warnings
- Fix PHP notices
- Fix deprecated calls
- Fix fatal errors
- Fix JS console errors
- Fix broken links
- Fix missing assets

---

# 39. ACCESSIBILITY

Preserve and improve:

```text
Semantic HTML
ARIA attributes
Keyboard navigation
Focus states
Alt text
Form labels
Button labels
Link labels
Heading hierarchy
```

Do not remove useful accessibility attributes from the original HTML.

---

# 40. PERFORMANCE

Optimize without changing the design.

Check:

```text
CSS loading
JS loading
Image sizes
Lazy loading
Font loading
Third-party scripts
Duplicate libraries
Database queries
```

Do not load every library on every page if it is only required on a specific page.

Example:

```text
Three.js
→ load only on pages that require Three.js
```

---

# 41. THIRD-PARTY SCRIPTS

Inventory every external script.

Examples:

```text
Google Analytics
Tag Manager
Maps
Chat widgets
Tracking
CDNs
Libraries
APIs
```

Do not remove them without understanding their purpose.

Use WordPress enqueue/hooks appropriately.

---

# 42. NO HARDCODED DOMAIN

Do not use:

```text
https://example.com/
```

inside theme code for internal links.

Use:

```php
home_url()
site_url()
get_permalink()
get_template_directory_uri()
```

where appropriate.

---

# 43. INTERNAL LINKS

> **Elementor build:** internal links use Dynamic Tags → Internal URL (§132), and every clickable element is linked (§145).

Convert static internal HTML links where required.

Example:

```html
<a href="/about.html">
```

should become a WordPress permalink when the destination is a WordPress page.

Prefer:

```php
get_permalink($page_id)
```

or:

```php
get_permalink()
```

Do not hard-code URLs unnecessarily.

---

# 44. MENU LINK HANDLING

> **Elementor build:** menus = WordPress Menus in the Nav Menu widget (§106); see §145.

The WordPress menu should handle:

```text
Pages
Custom Links
Partner Archive
Blog
Categories
```

Do not hard-code the navigation in `header.php`.

---

# 45. PARTNER ARCHIVE

> **Elementor build:** Theme Builder **Archive** template + Loop Grid (§111) — not `archive-partner.php`.

Partner archive should reproduce the existing partner listing design.

Use:

```text
archive-partner.php
```

Preserve:

```text
Cards
Images
Typography
Spacing
Hover effects
Animations
Filters
Pagination
```

Only implement filters if the original site requires them.

---

# 46. PARTNER SINGLE

> **Elementor build:** Theme Builder **Single** template with Dynamic Tags (§111) — not `single-partner.php`.

Partner detail should reproduce the existing Partner HTML template.

Use:

```text
single-partner.php
```

Dynamic fields must populate the existing HTML structure.

Do not create a completely new partner layout.

---

# 47. BLOG ARCHIVE

> **Elementor build:** Theme Builder Archive template + Loop Grid (§112) — not `home.php` / `archive.php`.

Blog listing must reproduce the existing blog HTML.

Use:

```text
home.php
archive.php
category.php
```

as appropriate.

Use WordPress Posts.

---

# 48. BLOG SINGLE

> **Elementor build:** Theme Builder Single Post template (§112) — not `single.php`.

Blog detail must reproduce the existing blog detail HTML.

Use:

```text
single.php
```

or a specific post template if required.

Dynamic:

```text
Title
Featured Image
Author
Date
Category
Content
Related Posts
```

---

# 49. REUSABLE TEMPLATE PARTS

> **Elementor build:** repeated sections = saved templates / Global Widgets / Loop Items (§104, §124) — not `template-parts/` PHP.

If the HTML contains repeated sections, extract them.

Example:

```text
template-parts/
├── blog/
│   └── blog-card.php
├── partner/
│   └── partner-card.php
└── content/
    └── content-card.php
```

Do not create unnecessary abstraction.

The goal is reusable and readable code.

---

# 50. CSS CLASS PRESERVATION

> **Elementor build:** no classes by habit (§114); a class exists only for an approved `custom.css` rule or a JS hook — and check its usage before renaming it.

If JavaScript depends on:

```text
class names
IDs
data attributes
```

do not rename them without updating the JavaScript.

Before removing a class, search the whole project for its usage.

---

# 51. JAVASCRIPT DEPENDENCY CHECK

Before modifying JS:

Search for:

```text
querySelector
querySelectorAll
getElementById
jQuery selectors
classList
data attributes
IDs
```

Ensure PHP conversion does not break selectors.

---

# 52. AJAX

If the existing HTML uses AJAX:

- Identify endpoint.
- Identify request method.
- Identify payload.
- Identify response.
- Identify authentication.
- Identify nonce/security requirements.

Convert it to proper WordPress AJAX or REST API architecture if necessary.

Do not break the existing external API integration.

---

# 53. REST API

If custom REST endpoints are required:

- Register with `register_rest_route()`.
- Use namespaces.
- Validate parameters.
- Sanitize parameters.
- Add permission callbacks.
- Return proper `WP_REST_Response` / `WP_Error`.

Never expose private data through an unprotected endpoint.

---

# 54. CUSTOM ADMIN SETTINGS

If custom Theme Settings are needed:

Use the WordPress Settings API where appropriate.

Do not create insecure custom admin forms.

Use:

```text
register_setting()
add_settings_section()
add_settings_field()
```

Use capability checks and sanitization callbacks.

---

# 55. CHILD THEME

Do not create a child theme unless there is a specific reason.

The default target is a standalone custom theme.

---

# 56. PLUGIN VS THEME RESPONSIBILITY

Theme should handle:

```text
Presentation
Templates
Theme-specific settings
Frontend UI
```

Site functionality that must survive theme switching should ideally be a plugin.

However, for this conversion, do not unnecessarily create plugins unless the requirement warrants it.

For example:

```text
Partner CPT
```

can be implemented in a site plugin if long-term portability is important.

If implementing it in the theme, keep the code isolated in:

```text
inc/post-types.php
```

so it can later be moved easily.

---

# 57. DATABASE DESIGN

Do not create custom database tables unless necessary.

Prefer:

```text
Posts
Pages
Custom Post Types
Post Meta
Taxonomies
Options
```

Use custom tables only for genuinely relational/high-volume data where WordPress post/meta is inappropriate.

---

# 58. QUERY OPTIMIZATION

Avoid:

```text
query inside query
query inside loop
unnecessary get_posts()
repeated WP_Query
```

Use efficient queries.

For repeated cards, query once and loop.

Always reset post data after custom loops.

---

# 59. CACHING COMPATIBILITY

Do not write code that prevents normal WordPress caching.

Avoid unnecessary dynamic queries.

Do not depend on PHP session state unless absolutely necessary.

---

# 60. INTERNATIONALIZATION

Use translation-ready strings for theme UI text.

Example:

```php
__('Read More', 'site-theme');
```

Text domain:

```text
site-theme
```

Use one consistent text domain.

---

# 61. WORDPRESS CODING STANDARDS

Follow WordPress coding conventions:

- Proper indentation
- Meaningful function names
- Prefix custom functions/classes
- Avoid generic function names
- Avoid naming collisions
- Escape output
- Sanitize input

Example prefix:

```text
site_theme_
```

Avoid generic names like:

```php
function setup()
function helper()
function options()
```

---

# 62. FUNCTION / CLASS NAMING

Use project-specific prefixes.

Example:

```php
site_theme_setup()
site_theme_enqueue_assets()
site_theme_register_partner_cpt()
site_theme_register_menus()
```

Avoid collisions with plugins or WordPress core.

---

# 63. FILE NAMING

Use predictable names:

```text
single-partner.php
archive-partner.php
partner-card.php
blog-card.php
theme-settings.php
post-types.php
```

Do not create files with random names.

---

# 64. DEVELOPMENT ORDER

> **Elementor build:** the build order is **§130**.

Follow this order:

```text
1. Inspect complete HTML project
2. Create site inventory
3. Identify templates
4. Identify reusable components
5. Identify dynamic content
6. Identify Partner architecture
7. Identify Blog architecture
8. Inspect Header HTML
9. Inspect Footer HTML
10. Plan WordPress theme
11. Create theme structure
12. Create theme setup
13. Enqueue CSS/JS
14. Convert Header
15. Convert Footer
16. Register Menus
17. Convert Homepage
18. Convert standard Pages
19. Create Partner CPT
20. Create Partner fields
21. Create Partner archive
22. Create Partner single
23. Convert Blog
24. Convert forms
25. Integrate animations
26. Test responsive behavior
27. Test URLs
28. Test SEO
29. Test PHP
30. Test JavaScript
31. Performance review
32. Final QA
```

Do not skip the inspection stage.

---

# 65. BEFORE CODING: ARCHITECTURE REPORT

> **Elementor build:** report the Elementor architecture (§97 inspect, globals §98, templates §124/§125, components §104) — not PHP template files.

Before making major code changes, create an internal implementation plan containing:

```text
Page inventory
Template mapping
Header structure
Footer structure
Menu locations
Partner CPT structure
Partner fields
Blog structure
Theme settings
Asset mapping
JS dependencies
CSS dependencies
Potential risks
Migration/redirect requirements
```

Do not ask unnecessary questions if the answer can be determined by inspecting the project.

Only ask the user when a business requirement genuinely cannot be inferred from the supplied project.

**Ask questions one at a time (MANDATORY).** Never send a list of questions in one message — the
user needs time to think about each answer, and nothing gets missed.

1. Ask **one question per message**, numbered (*"Question 2 of 7"*), so the user knows how many are
   left.
2. Give the options, mark the default / recommended one, and add one line on what the answer
   changes (e.g. *"Hello Elementor + child theme — recommended"*).
3. **Wait for the answer** before asking the next question. If an answer raises a follow-up, ask that
   next, as its own question.
4. Skip a question only when the answer is already given (in the prompt, the project docs or
   `login.md`) — say in one line what you read instead of asking.
5. After the last answer, show **all answers as one short summary** and ask the user to confirm
   before starting. Write the confirmed answers into the project notes / architecture report.

---

# 66. DO NOT MAKE ASSUMPTIONS

If something is unclear:

First search the project.

For example:

```text
Search where a class is used.
Search where an image is referenced.
Search where a JS function is called.
Search which HTML pages use a component.
```

Do not guess.

---

# 67. HEADER / FOOTER SPECIAL RULE

This is mandatory:

```text
FIRST:
Inspect original Header HTML + CSS + JS + assets.

THEN:
Build header.php.

FIRST:
Inspect original Footer HTML + CSS + JS + assets.

THEN:
Build footer.php.
```

The WordPress version must be based on the actual supplied implementation.

---

# 68. CONVERSION RULE

Do not simply copy:

```text
index.html → index.php
```

A proper WordPress conversion requires:

```text
HTML
↓
WordPress Template
↓
Dynamic Content
↓
WordPress APIs
↓
Secure PHP
```

---

# 69. DO NOT BREAK EXISTING HTML

Before modifying any component:

1. Identify its CSS.
2. Identify its JS.
3. Identify its assets.
4. Identify pages using it.
5. Convert it.
6. Test it.

---

# 70. FINAL QA CHECKLIST

> **Elementor build:** the Final QA is **§128** — not this list.

Before declaring the project complete:

## Theme

- [ ] Theme activates without errors.
- [ ] No PHP fatal errors.
- [ ] No PHP warnings/notices.
- [ ] No deprecated errors caused by theme code.
- [ ] Theme files are organized.

## Header

- [ ] Header matches original HTML.
- [ ] Logo works.
- [ ] Desktop menu works.
- [ ] Mobile menu works.
- [ ] Dropdowns work.
- [ ] CTA works.
- [ ] Sticky behavior works.
- [ ] Header animations work.

## Footer

- [ ] Footer matches original HTML.
- [ ] Footer menu works.
- [ ] Social links work.
- [ ] Contact details work.
- [ ] Copyright works.
- [ ] Responsive footer works.

## Pages

- [ ] Homepage works.
- [ ] Every standard page works.
- [ ] Images work.
- [ ] Internal links work.
- [ ] Every button, card, logo, menu and footer link is set — no `#` / empty links (§145).

## Partners

- [ ] Partner CPT works.
- [ ] Partner admin works.
- [ ] Partner archive works.
- [ ] Partner single works.
- [ ] Partner fields work.
- [ ] Partner images work.
- [ ] Partner URLs work.

## Blog

- [ ] Blog archive works.
- [ ] Blog single works.
- [ ] Categories work.
- [ ] Tags work if required.
- [ ] Featured images work.
- [ ] Pagination works if required.

## Assets

- [ ] CSS loads.
- [ ] JS loads.
- [ ] Fonts load.
- [ ] Images load.
- [ ] SVGs load.
- [ ] Vendor libraries load.

## JavaScript

- [ ] No console errors.
- [ ] Menu JS works.
- [ ] Sliders work.
- [ ] Animations work.
- [ ] GSAP works if present.
- [ ] Three.js works if present.
- [ ] Forms work.
- [ ] AJAX works if present.

## Responsive

- [ ] Mobile tested.
- [ ] Tablet tested.
- [ ] Desktop tested.
- [ ] Safari tested.
- [ ] Chrome tested.
- [ ] Firefox tested.
- [ ] Edge tested.

## SEO

- [ ] Titles preserved.
- [ ] Heading hierarchy correct.
- [ ] Meta integration works.
- [ ] `<meta name="DeveloperAI" content="Developer Skill">` is in `<head>` on every page (§141).
- [ ] Canonicals work.
- [ ] Image ALT works.
- [ ] URLs are correct.
- [ ] Redirects work.

## Security

- [ ] Output escaped.
- [ ] Input sanitized.
- [ ] Admin forms protected.
- [ ] Nonces implemented where required.
- [ ] Capability checks implemented.
- [ ] No credentials in source.
- [ ] No debug output.

## Performance

- [ ] No unnecessary duplicate assets.
- [ ] Images optimized.
- [ ] Libraries loaded only where needed.
- [ ] Queries reviewed.
- [ ] No obvious N+1 queries.

---

# 71. FINAL DELIVERABLE

> **Elementor build:** the deliverable and Definition of Done are in **§129**.

The final result must be a complete WordPress theme that can be installed in:

```text
/wp-content/themes/
```

and activated from:

```text
WordPress Admin
→ Appearance
→ Themes
```

The theme must be production-ready.

---

# 72. FINAL PRINCIPLE

> **Elementor build:** the final goal is **§129**.

Always prioritize:

```text
EXISTING HTML
        ↓
UNDERSTAND
        ↓
PRESERVE
        ↓
CONVERT
        ↓
DYNAMIC WORDPRESS
        ↓
SECURE PHP
        ↓
TEST
        ↓
PRODUCTION
```

Never prioritize a faster conversion over preserving existing functionality.

Never redesign the site during conversion.

Never guess when the answer can be found by inspecting the supplied files.

Never leave known PHP errors, query issues, broken links, missing assets, or JavaScript console errors unresolved.

**The existing HTML project is the source of truth for the frontend. WordPress is the CMS layer that should be added without breaking the existing website.**


---

# 73. EXISTING WORDPRESS THEME — UPDATE / EXTEND MODE

## CRITICAL RULE

The project may already contain a WordPress theme.

If an existing WordPress theme is present, **DO NOT automatically create a new theme**.

First inspect the existing theme completely and determine whether the HTML website should be:

```text
Existing Theme
      +
Existing HTML Website
      ↓
Safe Theme Update / Extension
```

The existing theme becomes the starting point unless there is a clear technical reason to replace it.

---

# 74. DETECT EXISTING THEME

Before creating theme files, check for:

```text
style.css
functions.php
header.php
footer.php
index.php
page.php
single.php
archive.php
front-page.php
theme.json
screenshot.png
wp-content/themes/
```

Also inspect:

```text
inc/
template-parts/
assets/
woocommerce/
acf/
```

If an existing theme is found, identify:

```text
Theme Name
Theme Version
Theme URI
Author
Template hierarchy
Existing functions
Existing hooks
Existing menus
Existing widgets
Existing custom post types
Existing taxonomies
Existing custom fields
Existing theme settings
Existing CSS
Existing JS
Existing plugins/dependencies
```

---

# 75. EXISTING THEME MUST BE BACKED UP

Before modifying an existing theme:

1. Inspect it.
2. Create a backup/copy.
3. Record the current structure.
4. Record important existing functionality.
5. Identify files that will be modified.
6. Avoid destructive changes.

Never delete existing theme functionality simply because it is not immediately understood.

---

# 76. DO NOT OVERWRITE EXISTING FUNCTIONALITY

Before changing any existing PHP file:

Search where its functions/hooks/classes are used.

Check:

```text
functions.php
inc/
template-parts/
hooks
filters
actions
shortcodes
REST endpoints
AJAX
custom post types
taxonomies
widgets
menus
theme settings
```

Do not remove existing code unless it is proven obsolete and safe to remove.

---

# 77. EXISTING THEME + HTML CONVERSION STRATEGY

If the existing theme already provides:

```text
Header
Footer
Menus
Theme Settings
Page templates
Blog templates
```

reuse them where appropriate.

Do not create duplicate systems.

Example:

If the theme already has:

```text
site-header.php
```

do not automatically create a second unrelated:

```text
header-new.php
```

Instead, update the existing implementation safely.

---

# 78. EXISTING HEADER UPDATE

If an existing `header.php` exists:

1. Read the existing `header.php`.
2. Read related CSS.
3. Read related JS.
4. Identify existing WordPress hooks.
5. Identify existing menu locations.
6. Identify existing logo/settings.
7. Compare it with the supplied HTML Header.
8. Adapt the existing WordPress Header to match the supplied HTML.

Do NOT blindly replace the existing header.

Target:

```text
Existing WordPress Header
        +
Original HTML Header Design
        ↓
Updated Dynamic WordPress Header
```

Preserve working functionality.

---

# 79. EXISTING FOOTER UPDATE

If an existing `footer.php` exists:

1. Read the existing footer.
2. Read footer CSS.
3. Read footer JS.
4. Identify widgets/menu locations.
5. Identify existing theme settings.
6. Compare with original HTML Footer.
7. Update the existing implementation safely.

Do not blindly replace the footer.

---

# 80. EXISTING MENU SYSTEM

If the theme already has registered menus:

```php
register_nav_menus()
```

inspect and reuse them.

Do not register duplicate menu locations such as:

```text
primary
main
header-menu
main-menu
primary-menu
```

all for the same purpose.

Consolidate only when safe.

Preserve existing menu behavior and custom walkers.

---

# 81. EXISTING THEME SETTINGS

If the theme already has:

```text
Customizer
Theme Options
Settings API
ACF Options Page
theme.json
```

reuse the existing settings architecture where practical.

Do not create duplicate settings such as:

```text
Existing Logo Setting
New Logo Setting
Another Logo Setting
```

Use the existing source of truth.

---

# 82. EXISTING CUSTOM POST TYPES

Before registering a new Partner CPT, check whether the existing theme/plugin already has:

```text
partner
partners
client
clients
case-study
```

or another equivalent CPT.

If an existing CPT already represents Partners, reuse it rather than registering a duplicate post type.

Do not create:

```text
partner
partners
partner_item
partner_profile
```

all for the same content.

---

# 83. EXISTING BLOG SYSTEM

Check whether the existing theme already modifies:

```text
Posts
Categories
Tags
Blog archive
Single post
```

Reuse the native WordPress Post system unless the existing project has a valid custom architecture.

Do not create a duplicate Blog CPT.

---

# 84. EXISTING PLUGINS

Before adding functionality, inspect installed/required plugins.

Check whether functionality is already provided by:

```text
ACF
Yoast SEO
Rank Math
WooCommerce
Contact Form 7
Gravity Forms
Elementor
WPForms
custom plugins
```

Do not recreate functionality already provided by an installed plugin.

Do not remove plugin dependencies without checking their usage.

---

# 85. EXISTING ACF

If ACF is already being used:

- Inspect existing field groups.
- Reuse existing fields.
- Extend existing field groups when appropriate.
- Do not duplicate fields unnecessarily.
- Preserve field names if existing templates depend on them.

If ACF is not installed and the project does not permit adding it, use secure native WordPress meta/settings.

---

# 86. EXISTING THEME CHILD THEME

If the current setup uses a child theme:

```text
parent-theme/
child-theme/
```

determine whether the conversion should be implemented in the child theme.

Do not modify a third-party parent theme unnecessarily.

Preferred approach:

```text
Parent Theme
      ↓
Existing Child Theme
      ↓
Custom HTML → WordPress Changes
```

This protects parent-theme updates.

---

# 87. THIRD-PARTY THEME

If the existing theme is a commercial/third-party theme:

1. Identify it.
2. Do not modify vendor files unless unavoidable.
3. Prefer child-theme overrides.
4. Preserve vendor update compatibility.
5. Document any required overrides.

Never destroy third-party theme functionality without a clear reason.

---

# 88. EXISTING THEME FILE MODIFICATION RULE

Before modifying any existing file, classify it:

```text
SAFE TO MODIFY
NEEDS CARE
DO NOT MODIFY
```

Examples:

```text
Custom child theme files       → Safe with review
Custom theme files             → Safe with review
Third-party parent files       → Avoid
WordPress core files            → NEVER
Plugin core files               → NEVER
```

---

# 89. NO DUPLICATE ARCHITECTURE

The final project must have one clear source of truth.

Avoid duplicates such as:

```text
Two headers
Two footers
Two partner CPTs
Two blog systems
Two menu systems
Two logo settings
Two theme option systems
Two versions of the same JS library
Two copies of the same CSS
```

If duplicates already exist, inspect dependencies before consolidating.

---

# 90. SAFE UPDATE PRINCIPLE

When an existing theme is present:

```text
Inspect
   ↓
Understand
   ↓
Backup
   ↓
Compare with HTML
   ↓
Reuse existing architecture
   ↓
Update only what is required
   ↓
Test existing functionality
   ↓
Test new functionality
```

Never:

```text
Delete theme
↓
Start from zero
```

unless explicitly instructed or technically necessary.

---

# 91. EXISTING THEME REGRESSION TEST

After updating an existing theme, verify that all previous functionality still works.

Test:

```text
Existing pages
Existing posts
Existing menus
Existing forms
Existing widgets
Existing shortcodes
Existing CPTs
Existing taxonomies
Existing settings
Existing integrations
Existing scripts
Existing responsive behavior
```

The HTML conversion must not break unrelated existing functionality.

---

# 92. EXISTING THEME VERSION CONTROL

If the project contains Git:

First inspect:

```text
.git/
.gitignore
branch
status
recent changes
```

Do not overwrite or discard user changes.

Do not reset or force-delete existing work.

If Git is not available, create a safe backup before major modifications.

---

# 93. EXISTING THEME FINAL ARCHITECTURE

The final architecture should be:

```text
Existing WordPress Theme
          +
Existing HTML Website
          ↓
        Merge
          ↓
Updated Custom Theme
          ↓
Dynamic WordPress CMS
```

The existing theme's valid functionality must be retained unless explicitly replaced.

---

# 94. DECISION RULE

Use this decision tree:

```text
Is an existing WordPress theme present?
            │
          YES
            ↓
   Inspect existing theme
            │
            ↓
Can existing architecture support the HTML?
       │                 │
      YES                NO
       ↓                  ↓
Update/extend       Safely refactor only
existing theme      required architecture
       │                  │
       └────────┬─────────┘
                ↓
        Test existing features
                ↓
        Test converted pages
                ↓
          Final QA
```

If no existing WordPress theme is present:

```text
Create a new custom theme
```

---

# 95. FINAL INSTRUCTION FOR THE CODING AGENT

If an existing WordPress theme is supplied, your first responsibility is **not to create another theme**.

Your first responsibility is to:

1. Inspect the existing theme.
2. Understand its architecture.
3. Identify what can be reused.
4. Identify what must be updated.
5. Compare it with the complete HTML project.
6. Safely integrate the HTML design.
7. Preserve existing WordPress functionality.
8. Avoid duplicate systems.
9. Test the complete site.
10. Deliver a production-ready updated theme.

**Never destroy an existing working theme architecture just to make the conversion easier.**

---

# 96. PROJECT MODE — HTML TO ELEMENTOR PRO

The primary target is now **WordPress + Elementor Pro**. The supplied HTML/CSS/JS is the source of truth for the frontend. Do not redesign the website.

## ⭐ MAIN REQUIREMENT — MOST IMPORTANT RULE OF THIS PROJECT

**Goal: the final Elementor website must visually and functionally match the original HTML
website, without compromising its design, responsiveness or user experience.** Every other
section of this file serves this goal. If a rule seems to conflict with it, this one wins. Report
the conflict instead of dropping a design detail.

### Replicate everything from the original HTML, as closely as possible

- [ ] All page sections and layouts — every section, in the same order, nothing skipped or merged.
- [ ] Header and footer.
- [ ] Design, spacing, typography and colours.
- [ ] Hover effects and animations.
- [ ] Buttons, links and interactive elements (menus, tabs, accordions, sliders, popups, forms).
- [ ] Responsive behaviour across all devices — desktop, laptop, tablet, mobile.
- [ ] Any other custom design or functionality present in the original HTML.

"Close enough" is not done. Compare each page with the original HTML side by side at every
breakpoint before calling it finished.

### How to implement it in Elementor

1. **Native first.** Wherever possible, build every section with native Elementor widgets and
   features: containers, widgets, Global Settings, Motion Effects, Theme Builder, Loop Grid, Dynamic
   Tags.
2. **No class-based CSS. CSS only with approval.** If a design detail or function cannot be
   achieved with native Elementor, do not silently add CSS or a class. Report it, explain why the
   native controls are not enough, and get approval first (§114). A class goes on the element only
   for an approved rule.
3. **Global styles.** Manage reusable styles globally wherever possible (Global Colors, Global
   Typography, Site Settings, Elementor Variables) so the site stays consistent.
4. **Approved CSS lives only in the theme's assets directory** (`assets/css/custom.css`, §135).
   - Never put it in a widget's `Advanced → Custom CSS`.
   - Never put it in Page Settings → Custom CSS.
   - Never put it in Site Settings → Custom CSS or in inline styles.
5. **Reusable, scalable, component-based.**
   - Build each repeated block once as a component (saved template / Global Widget / Loop Item, §104).
   - Style it once with globals and its own style controls, then reuse it.
   - Never copy and restyle it page by page.

```text
Existing HTML
  ↓
Inspect complete HTML/CSS/JS
  ↓
Extract the existing design system
  ↓
Create Elementor Global Settings
  ↓
Build reusable Elementor components
  ↓
Build Header / Footer with Elementor Pro
  ↓
Build Pages / Blog / Partners
  ↓
Connect dynamic WordPress data
  ↓
Responsive + visual QA
```

Elementor Pro must be the primary page/template builder. Use custom PHP only for functionality that genuinely requires WordPress/PHP.

---

# 97. INSPECT BEFORE BUILDING

Before creating Elementor pages/templates, inspect:

- Every HTML page
- All CSS/SCSS
- All JavaScript
- Header and Footer
- Responsive media queries
- Fonts
- Colors
- Typography
- Spacing
- Container widths
- Border radius
- Shadows
- Images/SVGs/icons
- Animations
- GSAP / ScrollTrigger
- Three.js
- Forms and APIs
- Repeated components

Do not rebuild pages randomly before understanding the complete design system.

---

# 98. ELEMENTOR GLOBAL DESIGN SYSTEM — MANDATORY

Before building the majority of the pages, extract the design system from the existing HTML/CSS.

Use Elementor Site Settings / Global Settings for values reused throughout the website:

```text
Global Colors
Global Fonts / Typography
Global Button Styles
Global Form Styles where applicable
Global Container/Layout Settings
Global Variables where appropriate
```

Do not repeatedly enter the same value manually into individual widgets.

**Tokens ↔ Elementor globals stay in sync (MANDATORY).** The Elementor globals (colours, fonts,
spacing variables) and the `vendor.css` `:root` tokens (§135) hold **the same values with the same
names**. When a value changes, change it in both places at once. Keep a short **token → Elementor
global** table in the theme docs, so the next developer knows which is which. A value that is not a
token is a question for the user, not a new global.

## Required final state

The final Elementor website should have one clear design system:

```text
Global Colors
Global Typography
Global Buttons
Global Forms
Global Container Width
Global Spacing / Variables
Border Radius
Shadows
Reusable Components
```

Values must be derived from the original HTML/CSS.

---

# 99. GLOBAL COLORS

Inspect the original CSS and identify recurring colors. Create Elementor Global Colors based on the actual design.

Suggested logical names:

```text
Primary
Secondary
Accent
Heading
Text
Muted
Background
Surface
Border
White
Black
```

Do not invent new colors. If the same color appears repeatedly, use one Global Color instead of repeated hex/RGB values.

Bad:

```text
Widget A → #123456
Widget B → #123456
Widget C → #123456
```

Good:

```text
Global Color: Primary
        ↓
All relevant widgets
```

## Color rule

For every color ask:

> Is this a site-wide design color?

If yes, use an Elementor Global Color. Do not manually repeat the same hex/RGB value across many widgets.

---

# 100. GLOBAL TYPOGRAPHY

Inspect the original CSS and create Elementor Global Typography from the actual design.

Identify:

```text
Font family
Font weight
Font size
Line height
Letter spacing
Text transform
```

Create logical styles such as:

```text
Body
H1
H2
H3
H4
H5
H6
Button
```

Use Global Typography wherever the original design uses the same style. Do not manually repeat typography values on every widget.

## Typography rule

For every text element ask:

> Is this a standard body, heading, label, or button style?

If yes, use Elementor Global Typography. Only use custom typography for genuinely unique designs.

---

# 101. RESPONSIVE TYPOGRAPHY

Breakpoints are the fixed seven (§119). Inspect the original media queries for *what* changes, and
reproduce typography for:

```text
Desktop
Tablet
Mobile
```

If the original uses `clamp()`, fluid sizing, or specific breakpoints, reproduce the behavior accurately where Elementor supports it. Do not replace accurate original values with arbitrary Elementor defaults.

---

# 102. GLOBAL SPACING / VARIABLES

Extract recurring values from the original CSS:

```text
Section padding
Container padding
Grid gap
Column gap
Card spacing
Heading spacing
Button spacing
Margins
```

Create a consistent spacing system where practical, for example:

```text
Spacing XS
Spacing SM
Spacing MD
Spacing LG
Spacing XL
Spacing XXL
```

The actual values must come from the original project. If the HTML/CSS already contains CSS variables such as `--spacing-md`, preserve that design logic where appropriate.

## Spacing rule

For every padding/margin/gap ask:

> Is this part of the global design system or unique to this component?

If global, use a Global Spacing/Variable. If unique, use a component-specific value.

Do not create unnecessary one-off values.

---

# 103. NO RANDOM INLINE STYLING

Avoid repeatedly setting these values directly on individual widgets when they belong to the global design system:

```text
font-size
font-family
color
padding
margin
line-height
border-radius
```

Prefer:

```text
Elementor Global Settings
Global Typography
Global Colors
Global Variables
Reusable Components
```

---

# 104. REUSABLE ELEMENTOR COMPONENTS

Identify repeated HTML components and make them reusable where appropriate.

Examples:

```text
Section Heading
Primary Button
Secondary Button
Blog Card
Partner Card
CTA Banner
Testimonial
Feature Card
```

Use Elementor reusable templates, Global Widgets, Loop Items, or other appropriate Elementor Pro mechanisms. Do not duplicate identical structures across many pages unnecessarily.

**Create reusable components and templates wherever possible.** Any section, block, or widget group that appears more than once (or is likely to be reused later) must be saved as a reusable template / Global Widget / Loop Item, so that a single edit updates every instance. This maintains consistency and simplifies future updates.

---

# 105. HEADER — ELEMENTOR PRO THEME BUILDER

The Header must use:

```text
Elementor Pro → Theme Builder → Header
```

**FIRST inspect the original Header HTML, CSS, JS, assets, menu structure and responsive behavior. THEN build it in Elementor.**

Inspect:

- Exact structure
- Logo
- Desktop menu
- Mobile menu
- Dropdowns / mega menu
- CTA
- Search
- Icons
- Sticky behavior
- Scroll states
- Animations
- Breakpoints

Do not create a generic Elementor header.

---

# 106. HEADER MENU

Use the WordPress menu/navigation system with Elementor Pro. Do not hard-code navigation links into text widgets.

Preserve:

- Menu hierarchy
- Dropdowns
- Active state
- Hover state
- Mobile navigation
- Existing responsive behavior

If the existing menu requires custom PHP/Walker logic, inspect it before changing it.

---

# 107. FOOTER — ELEMENTOR PRO THEME BUILDER

The Footer must use:

```text
Elementor Pro → Theme Builder → Footer
```

**FIRST inspect the original Footer HTML, CSS, JS, assets and responsive behavior. THEN build it in Elementor.**

Preserve:

```text
Footer layout
Logo
Description
Navigation
Contact details
Social links
CTA
Legal links
Copyright
```

Do not create a generic footer.

---

# 108. HEADER / FOOTER SITE-WIDE SETTINGS

Site-wide values must have one source of truth.

Examples:

```text
Logo
Header CTA
Footer Logo
Footer Contact
Social Links
Copyright
```

Do not duplicate these values on every page.

---

# 109. EXISTING WORDPRESS THEME + ELEMENTOR PRO

If an existing WordPress theme is present:

1. Inspect it first.
2. Check whether Elementor Pro is installed.
3. Inspect existing Elementor templates and Theme Builder templates.
4. Inspect Global Colors and Global Fonts.
5. Inspect Custom CSS/Code.
6. Inspect CPTs, ACF and plugins.
7. Reuse the existing architecture where practical.
8. Do not automatically replace the existing theme.

If a third-party parent theme is present, avoid modifying vendor files unnecessarily and prefer a child-theme/customization approach where appropriate.

If a new theme (or child theme) has to be created, follow §135 — it must include `vendor.js` and `custom.css`.

---

# 110. EXISTING ELEMENTOR SITE

If Elementor is already used, inspect:

```text
Saved Templates
Theme Builder
Global Colors
Global Fonts
Site Settings
Global Widgets
Loop Templates
Custom CSS
Custom Code
```

Reuse working structures rather than rebuilding them unnecessarily.

---

# 111. PARTNERS — ELEMENTOR PRO + CPT

If Partner pages exist, use a dedicated Partner Custom Post Type and Elementor Pro Theme Builder templates:

```text
Partner CPT
   ↓
Elementor Pro Single Template
+
Elementor Pro Archive Template
```

Use dynamic Partner data. Do not hard-code individual Partner information into Elementor templates.

Partner fields are ACF PRO (local JSON, §12), shown with Elementor Dynamic Tags.

---

# 112. BLOG — ELEMENTOR PRO

Use native WordPress Posts for Blog. Do not create a duplicate Blog CPT unless explicitly required.

Use Elementor Pro Theme Builder for:

```text
Blog Archive
Single Blog Post
Category Archive if required
```

Use dynamic Elementor widgets/Loop Grid where they match the original design.

---

# 113. HTML → ELEMENTOR MAPPING

Do not put an entire page into one Elementor HTML widget.

Convert semantic HTML sections into native Elementor structures where practical.

Example:

```text
Original HTML
<section>
  container
    heading
    text
    button
</section>

Elementor
Container
 ├── Heading
 ├── Text Editor
 └── Button
```

Use the HTML widget only for small pieces that genuinely require raw HTML.

---

# 114. NO CUSTOM CSS / NO CLASS-BASED CSS — MANDATORY

**Do not add Custom CSS in any widget.** The widget-level `Advanced → Custom CSS` field must stay empty on every widget, container, and template.

**Do not write class-based CSS.** Do not create custom CSS classes (via `Advanced → CSS Classes` or otherwise) and then write CSS rules targeting them — not in widgets, page settings, Site Settings Custom CSS, or theme stylesheets.

All styling must be done through native Elementor controls, in this order:

```text
Elementor Global Settings (Colors, Typography, Buttons, Forms, Layout)
    ↓
Global Variables
    ↓
Elementor Widget / Container Style Controls
```

If a design detail genuinely cannot be reproduced with native Elementor controls, do not silently add CSS. Report it, explain why the controls are insufficient, and get approval before using any CSS. Approved CSS goes **only** in the theme's `assets/css/custom.css` (see §135) — never in a widget.

**Exception for approved CSS:** when a rule in custom.css has been approved, a class on the Elementor element may be added **only for that rule** (e.g. a curved-panel mask, a full-bleed carousel). Keep a list of these classes; never add "section-name" classes by habit (§147.1 rule 5). Before writing CSS, check whether a native control or a native widget can do it — on Buell the client rejected custom CSS for card hover, logo marquee and waves and asked for native hover, the Elementor carousel and native shape dividers instead.

Avoid `!important`.

---

# 115. SPACING RULE

→ Merged into **§102 Global Spacing / Variables** (Spacing rule). See §102.

---

# 116. TYPOGRAPHY RULE

→ Merged into **§100 Global Typography** (Typography rule). See §100.

---

# 117. COLOR RULE

→ Merged into **§99 Global Colors** (Color rule). See §99.

---

# 118. CONTAINER / LAYOUT SYSTEM

Inspect the original:

```text
Maximum width
Side padding
Columns
Grid
Gaps
Section widths
```

Configure Elementor global container/layout settings accordingly. Do not randomly choose a container width.

### Global container — set ONCE in Site Settings, never per container (MANDATORY)

Set the container system globally, in **Site Settings → Layout**, before building any page:

| Site Settings → Layout | Value comes from |
|---|---|
| **Content Width** — **every breakpoint** | the original CSS container `max-width` (e.g. `1200px`), then each breakpoint's stepped-down value |
| **Container Padding** — desktop / tablet / mobile | the original CSS container side padding — set for every breakpoint |
| **Gaps** (Widgets / Elements gap) | the global grid / column gap |
| **Breakpoints** | the seven fixed breakpoints (§119) |

**The container is responsive.** `--container` (and `--gutter`) change at the breakpoints, so the
global Content Width and Container Padding are set on **each device** in Site Settings → Layout
(switch the responsive icon: Widescreen, Desktop, Laptop, Tablet Extra, Tablet, Mobile Extra,
Mobile) with the container's stepped-down value at each of the seven breakpoints (§119). Example: 1200 → 1150 → 1105 → 1010 → 910 → 960 fixed → 100%.
Never set only the desktop value and leave the smaller screens on Elementor's default.

Then on every page and template:

- **Outer section container = Boxed** with the width **left empty / Default**, so it inherits the
  global Content Width. Never type a pixel width (`1180px`, `1240px`) or a side padding on each
  container.
- **Full-bleed sections** (background to the screen edge) = outer container **Full Width**, with
  one **inner Boxed container on Default** that holds the content — so the content still lines up
  with every other section.
- **Inner / nested containers = Full Width (100%)** of their parent. Their gap and padding use
  Global Spacing values (above), not one-off numbers.
- Section top / bottom padding = the global section-spacing value; only the left / right padding
  comes from the global Container Padding.
- Need a narrower block (text column, form)? Give that **inner** element a max-width from the design,
  keep the outer container on the global width.
- Changing the site width later = change **one** value in Site Settings → Layout, and every page
  follows. If a page still looks different, a container has a hard-coded width — remove it.
- Never set the container width in `custom.css` or widget Custom CSS to fight the global setting.

QA:
- [ ] Site Settings → Layout has Content Width + Container Padding set on every breakpoint, and Gaps, set from
      the original HTML/CSS.
- [ ] No container has its own pixel width or side padding; content edges line up on every section
      at desktop, tablet and mobile.

---

# 119. RESPONSIVE ELEMENTOR SETTINGS

Every important section must be checked at:

```text
Desktop
Tablet
Mobile
```

**Breakpoints — always these seven, never others (MANDATORY):**

```text
1599.98 · 1439.98 · 1365.98 · 1199.98 · 991.98 · 767.98 · 575.98
```

Set Elementor's custom breakpoints (Site Settings → Layout → Breakpoints) to the nearest of these
values, so Elementor's responsive controls switch at the same widths as `vendor.css`. Check the
breakpoint names and limits in the installed Elementor version. The original CSS media queries show
*what* changes on smaller screens, not *where* the breakpoint sits. Do not blindly rely on Elementor
defaults.

---

# 120. ANIMATIONS

→ Merged into **§121 Animations — GSAP / Three.js with Elementor**. See §121.

---

# 121. ANIMATIONS — GSAP / THREE.JS WITH ELEMENTOR

If the original uses GSAP, ScrollTrigger, AOS, IntersectionObserver, CSS animations, Three.js, Swiper or other libraries, inspect the implementation first.

**Sliders:** use Elementor's own default carousel widgets (Image Carousel, Loop Carousel, Media Carousel …), styled to match the design. Never add a new slider JS (no Slick, no Swiper of our own), even if the original HTML used one. Never Owl, Splide or hand-written slider JS.

Use Elementor Motion Effects only when they reproduce the original behavior accurately. If Elementor cannot reproduce it, preserve the required custom JavaScript.

If GSAP or Three.js is present:

- Preserve required selectors and data attributes.
- Avoid duplicate library versions.
- Load scripts only where required.
- Avoid duplicate initialization.
- Ensure Elementor Editor mode does not initialize frontend animations twice.
- Test desktop and mobile.
- Test resizing.

---

# 122. ELEMENTOR EDITOR COMPATIBILITY

Custom JavaScript must not break Elementor Editor.

Check for:

```text
Duplicate event listeners
Duplicate animations
Duplicate sliders
Duplicate Three.js canvases
```

If required, prevent frontend-only initialization while editing in Elementor.

---

# 123. ELEMENTOR PERFORMANCE

Do not create excessive nested containers.

Avoid unnecessary:

```text
Container → Container → Container → Container
```

Use the simplest Elementor structure that reproduces the design.

Do not load every custom library on every page if it is only needed on specific pages.

---

# 124. ELEMENTOR TEMPLATE NAMING

Use clear names:

```text
[Global] Header
[Global] Footer
[Page] Home
[Page] About
[Page] Contact
[Partner] Archive
[Partner] Single
[Blog] Archive
[Blog] Single
[Component] Blog Card
[Component] Partner Card
[Component] Section Heading
```

Use one consistent naming convention.

---

# 125. ELEMENTOR DISPLAY CONDITIONS

Use Theme Builder conditions correctly.

Typical:

```text
Header → Entire Site
Footer → Entire Site
Partner Single → Partner
Partner Archive → Partner Archive
Blog Single → Posts
Blog Archive → Posts Archive
```

Avoid overlapping templates that produce duplicate headers, footers, or content.

---

# 126. GLOBAL DESIGN SYSTEM — REQUIRED FINAL STATE

→ Merged into **§98 Elementor Global Design System** (Required final state). See §98.

---

# 127. NO DESIGN DRIFT

Compare the Elementor version against the original HTML page-by-page.

Check:

```text
Layout
Position
Width
Height
Typography
Color
Spacing
Images
Buttons
Borders
Radius
Shadows
Animations
Responsive behavior
```

Do not accept “close enough” when the original values can be reproduced accurately.

---

# 128. ELEMENTOR FINAL QA

## ⭐ Main Requirement (§96) — check this first

- [ ] Every page compared side by side with the original HTML at desktop, tablet and mobile.
- [ ] All sections and layouts present, in the same order. Header and footer match.
- [ ] Design, spacing, typography and colours match.
- [ ] Hover effects and animations match.
- [ ] Buttons, links and every interactive element work like the original.
- [ ] Any other custom design or functionality from the HTML is reproduced.
- [ ] Native Elementor used everywhere. No class-based CSS; any CSS was approved first (§114).
- [ ] Approved CSS is only in the theme's `assets/css/custom.css`. None is in widget, page or Site
      Settings Custom CSS.
- [ ] Repeated blocks are components (template / Global Widget / Loop Item), not page-by-page copies.
- [ ] Every page has `<meta name="DeveloperAI" content="Developer Skill">` in `<head>` (§141).

## Global Design

- [ ] Global colors created and actually used.
- [ ] Favicon made from the logo icon is set as the Site Icon and shows on every page (§144).
- [ ] Global typography created and actually used.
- [ ] Global spacing/variables established.
- [ ] Container width matches original.
- [ ] Container width + padding set once in Site Settings → Layout; no per-container widths (§118).
- [ ] Responsive typography matches.
- [ ] Repeated components are reusable.

## Header

- [ ] Original HTML Header inspected.
- [ ] Elementor Pro Header created.
- [ ] Logo is dynamic.
- [ ] Menu is dynamic.
- [ ] Desktop menu matches.
- [ ] Mobile menu matches.
- [ ] Dropdowns work.
- [ ] CTA works.
- [ ] Sticky behavior works.

## Footer

- [ ] Original HTML Footer inspected.
- [ ] Elementor Pro Footer created.
- [ ] Footer menus are dynamic.
- [ ] Social links work.
- [ ] Contact information works.
- [ ] Forms use the forms plugin chosen at the start (Elementor Form / Gravity Forms / CF7, §28) — only that one.
- [ ] Responsive Footer matches.

## Pages

- [ ] All required pages converted.
- [ ] No unnecessary giant HTML widgets.
- [ ] Existing design preserved.
- [ ] Dynamic content connected.

## Partners

- [ ] Partner CPT works.
- [ ] Partner fields work.
- [ ] Single Partner Elementor template works.
- [ ] Partner Archive works.
- [ ] Dynamic data displays correctly.

## Blog

- [ ] Native WordPress Posts used.
- [ ] Blog Archive works.
- [ ] Single Post works.
- [ ] Categories work where required.
- [ ] Dynamic post data works.

## Elementor

- [ ] No conflicting Theme Builder templates.
- [ ] No duplicate Header/Footer.
- [ ] No unnecessary nested containers.
- [ ] Global settings are actually used.
- [ ] Elementor Editor works.
- [ ] Frontend works.
- [ ] No widget contains Custom CSS (`Advanced → Custom CSS` empty everywhere).
- [ ] No class-based CSS was written; classes exist only for approved `custom.css` rules (§114).
- [ ] Font sizes, colours and spacing use Elementor globals / Variables, not repeated raw values.
- [ ] Section spacing is consistent; container left/right spacing is set once globally.
- [ ] Repeated sections/blocks saved as reusable templates / Global Widgets.
- [ ] If a new theme was created: `assets/js/vendor.js` and `assets/css/custom.css` exist and are enqueued correctly.

## Images, Links & Permalinks

- [ ] Meaningful SVG icons have alternative text; decorative ones are hidden from screen readers (§136).
- [ ] Every image has an SEO-friendly file name and proper alt text — none left blank or default (§136).
- [ ] Images, avatars and SVG icons renamed from what they actually show (`icon-phone.svg`, `priya-shah-avatar.webp`) — no Figma layer names left (§136).
- [ ] Dummy content uses the standard placeholders with correct ratio, size and styling at every breakpoint (§137).
- [ ] Logo placeholders: light gray background, logo centered both ways (§137).
- [ ] 404 page and Search Results page built and checked against the design (§138).
- [ ] HTML Sitemap page exists, lists everything automatically and is linked in the footer; XML sitemap works (§139).
- [ ] Every image/icon is in the Media Library **once** — no `-1` / `-2` copies, no duplicate uploads (§140).
- [ ] WooCommerce sites: shop design, CSS, JS and responsive pass the §142 QA list.
- [ ] All website images converted to and served as WebP.
- [ ] Images optimized (compressed, correctly sized).
- [ ] All internal links use Dynamic Tags → Internal URL → Type: Content.
- [ ] No hardcoded internal URLs; no broken internal links.
- [ ] Permalink structure set to **Post name**.

## JavaScript

- [ ] No console errors.
- [ ] GSAP works if present.
- [ ] Three.js works if present.
- [ ] Sliders work.
- [ ] Mobile interactions work.
- [ ] No duplicate initialization.

## Responsive

- [ ] Desktop matches.
- [ ] Tablet matches.
- [ ] Mobile matches.

## Pre-delivery sign-off — tick every box TWICE (summary of §146 + §147)

Do not tell the user "done" until all are ticked after the **second** pass.

### Setup & rules
- [ ] Worked on staging only; no credentials in any file/reply; unique theme name.
- [ ] CSS per §114 (no HTML widget / inline style for layout; classes only for approved rules).
- [ ] Zero static px for spacing (Elementor controls AND custom.css); section spacing editable via Elementor Variables.
- [ ] Global colours/fonts used; real client fonts uploaded; ™ ® 4 & % glyphs compared with design.
- [ ] Repeating content = CPT + Loop Grid/Carousel; every loop image has a placeholder fallback (grey + site logo).

### Design match (PDF zoomed, per section)
- [ ] Border-radius, shapes, curves, icon sizes, alignment, left/centre text as in PDF.
- [ ] Card/section padding equal on 4 sides (phone 20px); top and bottom section space equal.
- [ ] Gaps 40/30/20 everywhere; icons/logos smaller on tablet/mobile; no image crop on mobile.
- [ ] Buttons: text vertically centred, not full width on phones; optical centring in menu/tabs/inputs/page numbers.

### Header / menu / footer
- [ ] Top bar centred; device rules for location/phone/tagline; logo links home.
- [ ] Mega menu: starts at header bottom, not cut, hover image works, no logo placeholder; active states right.
- [ ] Mobile menu: custom burger, scroll lock, dividers, chevron toggle; 700–800 px OK.
- [ ] Footer: hovers, email not cut, ® centred, copyright centred + last on mobile, social icons smaller.

### Interactions
- [ ] Sliders: arrows per design, disabled = not-allowed, gaps right, hover not cut, zoom on product images.
- [ ] Logo tiles fully clickable → detail page; marquee speed; entrance animations play.
- [ ] Forms: validation, checkbox above button; accordions/tabs/pagination work.

### Content & pages
- [ ] Every post type tested with temporary data in every view; empty fields hide cleanly; few/many entries OK.
- [ ] All designed pages built (incl. detail templates and archives); slugs semantic (/faq/).
- [ ] Breadcrumbs dynamic and correct (no "About / About").
- [ ] Dummy content and links waiting on the client listed (links: §145).

### Technical QA
- [ ] 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320 crawled: no console errors, failed requests, broken images, missing alt, overflow, clipping.
- [ ] axe: one `<main>`, no duplicate landmarks, unique `<nav>` labels, icon-only names, contrast measured and reported.
- [ ] Unique title + description ≤160 on every page incl. archives/search/404; noindex/canonical noted for launch.
- [ ] Elementor cache cleared; theme version bumped; site returns 200 after upload.
- [ ] `PROJECT-SUMMARY.md` and `DAILY-LOG.md` updated; final report by QA bug type (fixed / clean / needs decision).

## Final round — one last full check of the whole site (MANDATORY)

*Every QA list above has passed? Now do **one more complete round over the whole site** before
saying "done". It catches what the per-section lists miss.*

1. **Make the checklist file** `FINAL-QA.md` in the project root: one row per page / template
   (every page, single / archive templates, 404, Search, Sitemap, WooCommerce pages) × the checks below. Tick a box only after actually checking it, and note how
   (screenshot compare, crawl, test submit, test order).
2. **Design match — 100% to the Figma / PDF:** every page and every section side by side with its
   Figma frame / PDF page, zoomed in: section order, layout, spacing, sizes, fonts, weights, line
   heights, colours, radius, shadows, icons, images (sharp, right crop), hover / active states and
   animations. Measure the numbers, don't judge by eye — any difference over ~2 px gets fixed.
3. **Responsive:** every page at 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 (§118, §119), plus 320 px: no horizontal scroll, no overlap, no
   clipped or squeezed text, menus / dropdowns / mini-cart stay inside the screen, tap targets at
   least 44 px, images not wrongly cropped.
4. **Functionality:** header menu, dropdowns, mobile menu, sticky header, search, sliders, tabs,
   accordions, modals, video, pagination, filters, animations; forms (validation, submit, the email
   arrives); WooCommerce (if any): add to cart, mini-cart, cart, coupon, checkout test order, emails, My Account. Everything works by keyboard. No console errors, PHP warnings or failed requests.
5. **Content:** exact text from the design (HTML / Figma / PDF) — no lorem ipsum, no made-up copy;
   spelling, ™ / ®, phone, email and address correct. Placeholders only where they are listed for
   the client.
6. **Linking:** every clickable element opens the right page (§145); no `#`, empty, `.html`,
   staging or localhost links; no 404; external links open in a new tab.
7. **Images & icons:** all from the design export, sharp, WebP, alt text set, Media Library once (§140).
8. **SEO & `<head>`:** unique title and meta description per page, one `<h1>`, DeveloperAI meta tag
   and favicon on every page (§141, §144).
9. **Accessibility:** landmarks, heading order, alt text, form labels, visible focus, colour contrast
   measured.
10. **Performance:** pages load fast; no unused heavy assets; images correctly sized.
11. **Browsers:** Chrome, Safari, Firefox and Edge, plus one real phone (iOS Safari) where possible.
12. **Admin / editability:** every text, image, button and link is editable in WordPress / the
    Elementor editor with no code; dynamic content (posts, CPTs, products, menus, settings) comes
    from the CMS — nothing hard-coded.

**Rules**
- Any fail → fix it → re-check that item **and** every page that shares that part.
- Never tick a box that was not actually checked. Anything that could not be checked is written
  down as "not checked + why".
- Only when every row in `FINAL-QA.md` is ticked, report: what was checked, what the final round
  fixed, and what is still waiting on the client.

---

# 129. DEFINITION OF DONE AND FINAL GOAL

**Final goal:** deliver a complete, responsive, fully functional WordPress website built with
**Elementor Pro** that faithfully reproduces the original HTML website (⭐ §96), following WordPress
best practices.

The conversion is complete only when:

```text
Original HTML Design
        ≈
Elementor Pro Frontend
```

while:

```text
Static Content
    ↓
WordPress / Elementor Dynamic Content
```

and:

```text
Repeated Design Values
    ↓
Elementor Global Settings / Variables
```

and:

```text
Repeated Components
    ↓
Reusable Elementor Templates / Widgets
```

and:

```text
Internal Links       → Elementor Dynamic Tags (Internal URL → Content)
Images               → Optimized WebP
Permalink Structure  → Post name
Styling              → Native Elementor controls only (no Custom CSS, no class-based CSS; approved CSS only in custom.css)
<head> meta          → <meta name="DeveloperAI" content="Developer Skill"> on every page (§141)
Favicon              → WordPress Site Icon made from the logo icon (§144)
WooCommerce (if any) → Theme Builder + native Woo widgets, CSS in custom.css, woocommerce.js (§142)
Images & SVGs        → accessible, SEO-friendly names and alt text (§136), Media Library once (§140)
Placeholders         → consistent placeholders (§137); 404 + Search pages built (§138)
Links                → every clickable element linked, no `#` (§145)
Content              → manageable by the client in WordPress / Elementor
```

Normal content changes should be possible through WordPress/Elementor without manually editing the original HTML files.

---

# 130. ELEMENTOR BUILD ORDER (DEVELOPMENT PRINCIPLE)

Always follow:

```text
INSPECT
   ↓
UNDERSTAND
   ↓
EXTRACT DESIGN SYSTEM
   ↓
CREATE GLOBAL COLORS
   ↓
CREATE GLOBAL TYPOGRAPHY
   ↓
CREATE GLOBAL SPACING / VARIABLES
   ↓
BUILD REUSABLE COMPONENTS
   ↓
BUILD HEADER / FOOTER
   ↓
BUILD PAGES
   ↓
BUILD PARTNER CPT TEMPLATES
   ↓
BUILD BLOG TEMPLATES
   ↓
CONNECT DYNAMIC DATA
   ↓
CONVERT IMAGES TO WEBP / SET INTERNAL LINKS / SET PERMALINKS
   ↓
MATCH ORIGINAL DESIGN
   ↓
TEST
   ↓
FINAL QA
```

**Do not build the entire Elementor website first and try to standardize the design later. The global design system must be established before large-scale page construction.**

---

# 131. IMAGE OPTIMIZATION — WEBP

Convert and optimize **all** website images to WebP format.

```text
Original images (JPG / PNG / GIF)
    ↓
Convert to WebP
    ↓
Compress / resize to the dimensions actually used
    ↓
Check the Media Library — already there? Reuse it (§140)
    ↓
Upload to the WordPress Media Library ONLY if it is not there yet
    ↓
Use in Elementor widgets from the Media Library
```

Rules:

- Do not hardcode image URLs; always select images from the Media Library.
- Keep sensible quality — no visible degradation versus the original.
- Preserve transparency where the original uses it.
- Set meaningful `alt` text for every image.
- SVG icons/logos may remain SVG where appropriate (WebP applies to raster images).

---

# 132. INTERNAL LINKING — ELEMENTOR DYNAMIC TAGS

All internal page links must be configured correctly and must not be hardcoded.

For every internal link in Elementor (buttons, headings, images, icons, icon lists, etc.) use:

```text
Link → Dynamic Tags → Internal URL → Type → Content
    ↓
Select the appropriate Page or Post
```

Rules:

- Do not type or paste internal URLs manually (no `https://domain.com/about/`, no `/about.html`).
- Original `.html` links must map to the matching WordPress page/post.
- This keeps links working when slugs or domains change.
- Navigation menus use WordPress Menus (see Header/Footer sections), not hardcoded links.
- Verify every internal link after the build — no 404s.
- Every clickable element must be linked (link map, no `#` / empty links) — see §145.

---

# 133. PERMALINK STRUCTURE

Set the WordPress permalink structure to **Post name**:

```text
Settings → Permalinks → Post name   (/%postname%/)
```

Set this before building internal links, then flush permalinks (save the Permalinks screen once) after registering any Custom Post Types.

---

# 134. FINAL GOAL

→ Merged into **§129 Definition of Done and Final Goal**. See §129.

---

# 135. NEW THEME — VENDOR.JS AND CUSTOM.CSS

If the project needs a new theme (or child theme), create it, and include these three asset files:

```text
theme-name/
└── assets/
    ├── js/
    │   └── vendor.js      ← third-party libraries (GSAP, ScrollTrigger, Three.js, etc.) — no slider library (§121)
    └── css/
        ├── vendor.css     ← :root tokens ONLY (from the original CSS) — no components
        └── custom.css     ← the ONLY place for approved custom CSS
```

### vendor.js

- Holds the third-party JavaScript libraries the original HTML site depends on — except slider libraries: sliders are Elementor's own carousel widgets (§121).
- Do not load the same library twice (check whether Elementor or a plugin already loads it).
- Enqueue with `wp_enqueue_script()` in the footer, with correct dependencies and version numbers.
- Load only on the pages that need it where practical.
- It may also hold small, approved front-end fixes that Elementor cannot do (e.g. adding a missing `aria-label`, a menu-chevron toggle). Fix things in the template/PHP first; use JS only when the markup comes from Elementor/Pro and cannot be changed. Comment each fix with what it fixes.

### vendor.css (tokens only)

- Holds **only the `:root` tokens** taken from the original site's CSS (colours, fonts, spacing,
  transition …) — the same names and values as the Elementor globals (§98–§102) — so `custom.css`
  can use `var(--…)`.
- **No global components** (no `.container`, buttons, headings, forms) — those are set in Elementor
  Site Settings and the Global Design System (§98–§102, §118).
- Enqueue it before `custom.css`, after Elementor's styles.

### custom.css

- The single home for any CSS that §114 approved. Never put CSS in widget `Advanced → Custom CSS`.
- Do not use it to replace things Elementor Global Settings or widget controls can handle.
- Comment each rule with the section/component it belongs to.
- Enqueue with `wp_enqueue_style()` after Elementor's styles, so its rules apply in the right order.
- Avoid `!important`.

Register them through `inc/enqueue.php` (or `functions.php` for a minimal child theme) using `get_stylesheet_directory_uri()` / `get_template_directory_uri()`. Never hardcode paths or domains.

---

# 136. SVG & IMAGE ACCESSIBILITY — MANDATORY

*Noted during real builds — this point was missed before. Check it on every page.*

### SVG icons

- **Meaningful SVG** (it carries information or is the only content of a link/button): give it
  alternative text.
  - Inline SVG: `role="img"` + `aria-label="…"` (or a `<title>` inside it).
  - Icon-only link/button: put the `aria-label` on the link/button and `aria-hidden="true"` on the SVG.
  - SVG uploaded through the Media Library: fill in its **Alt Text** there.
- **Decorative SVG** (it sits next to text that already says the same thing): `aria-hidden="true"`
  and no alt text, so screen readers skip it.
- Never leave an icon-only control with no accessible name.

### Images

- **SEO-friendly file names, set before upload.** WordPress keeps the uploaded file name in the URL.
  - Use lowercase, hyphen-separated, descriptive names: `team-meeting-office.webp`,
    `acme-logo.webp`, `blog-hero-seo-guide.webp`.
  - Never use names like `IMG_1234.webp`, `Group-12.webp`, `image (1).webp` or `Screenshot-…`.
- **Alt text on every image**, filled in the Media Library so every widget that uses it inherits it.
  - Describe what the image shows or does. For a logo, use the brand name.
  - Don't start with "image of".
  - A purely decorative image gets an empty alt (`alt=""`). It is never left missing.
- Fill in the image **Title** in the Media Library as well.
- Do not leave image names or accessibility attributes incomplete. A build with default file names
  or blank alt text is not done.

### Rename every image, avatar and SVG by what it actually shows

Wherever possible, **rename the file and write its alt text from the image itself**:
- Open or view each image, avatar and SVG before naming it.
- Never copy the Figma layer name (`Ellipse 3`, `Group 1000004521`, `Frame 12`, `Vector`), the
  export name or `image-1`.
- Never guess from where the file sits in the design. Describe what is actually in it.

**Steps for every file:**
1. Look at the file. What does it show, and what is its job on the page?
2. Rename it with the pattern below: lowercase, hyphens, no spaces, 2–5 meaningful words.
3. Upload it once (§140). Set **Title** and **Alt Text** in the Media Library to match.

| File type | File name pattern | Example file name | Alt text |
|---|---|---|---|
| Content / section image | `<what-it-shows>.webp` | `doctor-consulting-patient.webp` | `Doctor consulting a patient at a clinic desk` |
| Hero / banner image | `<page>-hero-<subject>.webp` | `about-hero-team-workshop.webp` | `Team working together in a design workshop` |
| Blog / post image | `<post-topic>.webp` | `seo-audit-checklist.webp` | describe the image, not just the post title |
| Avatar — real person | `<first>-<last>-avatar.webp` | `priya-shah-avatar.webp` | `Priya Shah, Marketing Director at Acme` |
| Avatar — dummy / unknown | `<context>-avatar-<n>.webp` | `testimonial-avatar-01.webp` | `Testimonial author photo` |
| Team photo | `<first>-<last>-<role>.webp` | `rahul-mehta-ceo.webp` | `Rahul Mehta, CEO` |
| Logo (client / partner) | `<brand>-logo.<svg/webp>` | `acme-logo.svg` | `Acme` (the brand name only) |
| Site logo | `<site>-logo.svg` | `brightpath-logo.svg` | `<Site name> home` when it links home |
| SVG icon | `icon-<what-it-shows>.svg` | `icon-phone.svg`, `icon-arrow-right.svg`, `icon-shield-check.svg` | the icon's meaning (`Phone`, `Next`), or empty if decorative (see SVG rules above) |
| Social icon | `icon-<network>.svg` | `icon-linkedin.svg` | `LinkedIn` (on the link: `Follow us on LinkedIn`) |
| Background / decorative shape | `bg-<what-it-is>.webp/svg` | `bg-wave-pattern.svg` | empty alt (`alt=""`) |
| Placeholder | `placeholder-<ratio>.webp` | `placeholder-16x9.webp` | `<Intended content> placeholder` (§137) |

**Alt text rules:**
- Describe what is in the image **and** why it is there.
- Keep it under ~125 characters. Don't start with "image of" or "photo of".
- Don't stuff keywords.
- Same image in two places = same alt (it comes from the Media Library).
- If a different context needs a different alt, override it in that widget only.
- Avatars: use the person's name and role when known. Never invent a name for a dummy avatar.

**If renaming is not possible** (the file is already uploaded and in use):
- Do not upload a second copy just to change its name (§140).
- Correct its **Title** and **Alt Text** in the Media Library.
- List the file in the handover report as "file name not SEO-friendly".

---

# 137. DUMMY CONTENT & PLACEHOLDER IMAGES

*Noted during real builds — use one consistent placeholder system across the whole site.*

Wherever the real content or images are not supplied yet, use dummy content **with a proper
placeholder image**. Never leave an empty space, a broken image or a random stock photo.

### Image placeholders

- Match the **aspect ratio, dimensions and styling** of the design's real image slot (same radius,
  same object-fit, same size at every breakpoint).
- Create one placeholder per ratio the design uses (e.g. `placeholder-16x9.webp`,
  `placeholder-4x3.webp`, `placeholder-1x1.webp`). Upload them once to the Media Library and reuse
  them everywhere. Don't make a new one per section.
- Lock the ratio with native controls (Image widget height + Object Fit: Cover, set per
  breakpoint). Save the placeholder setup as one reusable component. CSS only after §114 approval.
- Give placeholders alt text that names the intended content (e.g. `Team photo placeholder`) so
  they are easy to find and replace.

### Logo placeholders

- Light gray / neutral background (a Global Color, e.g. `Placeholder BG`).
- Logo centered **both vertically and horizontally** inside a fixed-ratio box (flex,
  `align-items: center; justify-content: center`).
- Every logo box is the same size, and the logo keeps its proportions (`object-fit: contain`,
  max width/height).
- One saved component / Loop Item for every logo grid and logo slider on the site (no class-based CSS, §114).

### Responsive and consistent

- Check placeholders on **Desktop, Tablet and Mobile**. The ratio and centering must hold, nothing
  stretches, and nothing overflows.
- Use the **same placeholder treatment on every page**. One look for image placeholders and one
  for logo placeholders.

---

# 138. ADDITIONAL PAGES — 404 AND SEARCH

*Noted during real builds — these two pages were missed before. They are part of every build.*

### 404 Page

- Build it in **Theme Builder** as a 404 page template, matching the design: heading, message,
  button back to the homepage, and the search field if the design has one.
- Use the global header and footer, global typography, colours and buttons. Link the home button
  with Dynamic Tags (§132).
- Test it by opening a URL that does not exist.

### Search Page

- Build the **Search Results** page in **Theme Builder** as an Archive template with the display
  condition **Search Results**, matching the design.
- Include the search term heading, the results loop (title, excerpt/meta, image if designed),
  pagination, and a clear **"no results"** state.
- Make sure the header/site search form submits to it.
- Test it with a term that returns results and with one that returns nothing.

If the design has no 404 or Search layout, build a simple one from the global design system, and
say so in the handover report.

---

# 139. SITEMAP PAGE

*Noted during real builds — the sitemap page was missed before. It is part of every build.*

### HTML Sitemap page (for visitors)

- Create a normal WordPress Page called **Sitemap** (slug `sitemap`), built with Elementor and the
  global header, footer, typography and colours.
- Use the Elementor Pro **Sitemap** widget so the list is **dynamic**:
  - Pages
  - Posts
  - Partners (CPT)
  - Categories, if the design uses them
- Never type the links by hand. A new page or post must show up on its own.
- Leave out pages that should not be listed: Thank You, 404, private/draft pages and utility pages.
- Link it from the footer (usually next to Privacy / Terms) through the WordPress footer menu.
- Match the design if it has a sitemap layout. If it doesn't, keep it simple, using a section title
  and grouped lists from the global design system.

### XML Sitemap (for search engines)

- If an SEO plugin is active (Yoast, Rank Math), use **its** XML sitemap and do not add another one.
- If not, WordPress core's `/wp-sitemap.xml` is enough. Do not install a separate sitemap plugin.
- Check that Pages, Posts and the Partner CPT are included, and that Thank You / utility pages are
  excluded.
- Open the XML sitemap URL after the build and confirm it loads with the correct URLs (Post name
  permalinks, §133).

---

# 140. MEDIA LIBRARY — UPLOAD EVERY IMAGE / ICON ONLY ONCE

*Noted during real builds — the same images and icons were uploaded to the Media Library twice.
That must not happen.*

**Rule: one file = one Media Library item.** The same image or icon used in five sections is
uploaded once and reused five times.

**Decide each file's home — theme or Media Library, never both:**
- **Fixed UI files** that admins never change stay in the theme's `assets/images/` and are **not**
  uploaded to the Media Library: UI icons, arrows, chevrons, decorative shapes, the SVG sprite.
- **Content images** that admins can change go into the Media Library: page images, post/CPT images,
  ACF image fields, partner logos, avatars.

### Before every upload

1. **Search the Media Library first** by file name (e.g. `acme-logo`) or title.
2. **Already there → reuse it.** Pick the existing item in the widget. Do not upload it again.
3. **Not there → upload it once**, with the SEO-friendly name, alt text and title from §136.

### When uploading by script / WP-CLI / REST API

- Before the run, list existing attachments (`wp post list --post_type=attachment --fields=ID,post_name,guid`)
  and keep a **file name → attachment ID map**.
- Upload only the files that are missing from that map. Reuse the stored ID for everything else.
- Re-running a build step or rebuilding a section must **not** upload the files again.
- When importing an Elementor template or kit that contains images, check whether those images are
  already in the library before importing them.

### Signs of a duplicate, and the clean-up

- The same name with a WordPress suffix: `acme-logo-1.webp`, `acme-logo-2.webp`, `hero-image-scaled-1.webp`.
- Two items with the same title or the same thumbnail.
- If duplicates already exist:
  1. Find every widget/page that uses the copy.
  2. Re-point it to the original item.
  3. Only then delete the copy.

  Never delete a media item that is still in use, because that leaves a broken image.

Check the Media Library once at the end of the build: every content image appears exactly once, and no fixed UI icon was uploaded.

---

# 141. DEVELOPERAI META TAG IN `<head>` — EVERY PAGE (MANDATORY)

*Default on every build.* Every front-end page of the site — pages, posts, archives, CPT pages, 404,
Search, Sitemap — outputs this hidden tag in `<head>`:

```html
<meta name="DeveloperAI" content="Developer Skill">
```

- Copy it exactly: `name="DeveloperAI"`, `content="Developer Skill"`. It is a hidden meta tag; it is
  never shown on the page.
- Add it once, through the `wp_head` hook, in the theme's `inc/template-functions.php` (new theme, §135) or the child theme's `functions.php` (existing theme):

```php
function {prefix}_developer_meta() {
    echo '<meta name="DeveloperAI" content="Developer Skill">' . "\n";
}
add_action( 'wp_head', '{prefix}_developer_meta', 1 );
```

- Do not hard-code it in each template, and do not add it through an SEO plugin, a page-level field
  or a Custom HTML widget. The hook covers every template, including Elementor Canvas / Full Width.
- It must appear only once per page (check the page source).

## QA

- [ ] View source on Home, an inner page, a post, 404 and Search: `<meta name="DeveloperAI" content="Developer Skill">` is in
      `<head>`, exactly once.

---

# 142. WOOCOMMERCE — SHOP PAGES, THEME-LEVEL CSS / JS + RESPONSIVE

*Use this section whenever the site sells products: the HTML has shop pages, or the user asks for
WooCommerce. All the other rules in this file still apply (§114 CSS, §141 DeveloperAI meta tag, §136 images, §140 Media Library once, §128 QA).*

**Site already built?** Add WooCommerce through Quick Update Mode (§143): follow the steps below and
skip any step that already exists.

**Ask at the start** (with the other start-of-project questions — one question per message, §65):
1. Is WooCommerce part of this build?
2. Cart and Checkout: **Block** (WooCommerce default) or **Classic** (shortcode)?
3. Which payment gateways and shipping methods?

## 1. Inspect first

- WooCommerce version and every WooCommerce add-on plugin (payment, shipping, filters, wishlist).
  Do not rebuild what a plugin already does (§84).
- List every e-commerce screen in the HTML: Shop, Category, Single Product, Cart, Checkout,
  My Account (login, orders, addresses), Thank You, mini-cart, empty cart, notices.
- Product types used: simple, variable, grouped, external, digital.

## 2. Theme files

```text
theme-name/
├── woocommerce/               ← template overrides, ONLY when hooks + Elementor can't do it
├── inc/woocommerce.php        ← theme support, hooks, image sizes, cart fragments
└── assets/
    ├── css/custom.css         ← WooCommerce block inside it (the only custom CSS file, §135)
    └── js/woocommerce.js      ← shop interactions (step 8)
```

Never edit WooCommerce plugin files.

## 3. Theme support (`inc/woocommerce.php`)

```php
function {prefix}_woocommerce_setup() {
    add_theme_support( 'woocommerce', array(
        'thumbnail_image_width' => 600,   // match the HTML product card
        'single_image_width'    => 900,   // match the HTML product page
    ) );
    add_theme_support( 'wc-product-gallery-zoom' );
    add_theme_support( 'wc-product-gallery-lightbox' );
    add_theme_support( 'wc-product-gallery-slider' );
}
add_action( 'after_setup_theme', '{prefix}_woocommerce_setup' );
```

Take the image widths and ratios from the HTML design, not from these sample numbers.

## 4. Hooks first, template overrides last

- Change layout with WooCommerce hooks and filters (`remove_action` / `add_action`). Override a
  template in `woocommerce/` only when no hook can do it.
- Keep a list of every overridden template (file + WooCommerce version) in the theme docs.
- After every WooCommerce update: WooCommerce → Status → **no outdated templates**.

## 5. Build with Elementor Pro (native first, §96)

- **Theme Builder:** a **Single Product** template and a **Product Archive** template, with display
  conditions (§125). Product cards are one **Loop Item** template, reused on the shop, category,
  related and home sections (§104).
- **Native WooCommerce widgets first:** Products, Product Title, Product Price, Product Images,
  Add to Cart, Product Meta, Product Data Tabs, Upsells / Related, Breadcrumb, Menu Cart, Cart,
  Checkout, My Account, Purchase Summary.
- **Site Settings → WooCommerce:** set the Cart, Checkout, My Account, Terms and Purchase Summary
  pages there.
- **Styling order:** Elementor Global Colors / Typography / Buttons / Forms → the widgets' own style
  controls → only where those can't match the HTML, report it and get approval, then CSS in `custom.css` (§114).
  Never widget Custom CSS.
- Header mini-cart = the **Menu Cart** widget, inside the Theme Builder header (§105).

## 6. Products, data and settings

- Price, sale price, variations, stock, SKU, gallery and reviews come from WooCommerce's native
  fields. Nothing hard-coded.
- Extra product fields = ACF PRO, saved as local JSON and synced.
- Product categories / tags / attributes = WooCommerce's own taxonomies. No duplicate taxonomy.
- **HPOS compatible:** read orders with `wc_get_order()` / `$order->get_meta()`, never
  `get_post_meta()` on orders or direct order SQL.
- Importing products: WooCommerce CRUD (`new WC_Product_Simple()`, `->save()`), never raw DB writes.
- WooCommerce → Settings → Advanced: Shop, Cart, Checkout, My Account and Terms pages set.
- Payment gateways in **sandbox / test mode on staging**. Keys never go in code, docs or chat.
- WooCommerce → Emails: logo, colours and footer text match the brand.

## 6a. Products are ALWAYS dynamic — never static

*Wherever a product appears on the site, it comes from WooCommerce. Never hard-code a product.*

- **Every place a product shows** — home page featured / new / best-seller sections, product
  sliders and carousels, "related" / "you may also like", category tiles, mega-menu product cards,
  banners with a price, landing pages — pulls the product from WooCommerce. Not only the Shop page.
- **Never static:** no product built from Heading + Image + Text + Button widgets, no price typed
  into a Text Editor, no hand-made product card copied section to section. A static card goes stale
  the moment the price or stock changes and its button doesn't add to cart.
- **How to pull them (Elementor):** the **Products** widget, or **Loop Grid / Loop Carousel** with
  source = **Products** and the product **Loop Item** template (the same card as the shop, step 5).
  Sliders = Loop Carousel, not a static Image Carousel / Slides widget.
- **Admin chooses, the query renders:** set the widget's Query — category, tag, featured, on sale,
  best sellers, newest, or manual selection — plus how many to show and the order. Inside the Loop
  Item use dynamic tags (Product Title, Product Price, Featured Image, Product Rating, Add to Cart,
  Sale / Stock badge) — no typed values.
- **Figma / HTML shows product cards?** Create those products in WooCommerce (importer, CRUD —
  step 6) and point the widget at them. The design's cards are the *look* of the card, not its data.
- Empty state: set the widget's "Nothing found" message or hide the section — never fall back to
  static placeholder products.

## 7. Theme-level CSS — design + responsive

Write all WooCommerce styling in `assets/css/custom.css`, under one `/* ===== WooCommerce ===== */` banner, after the native Elementor styling has been used, and only for CSS that was approved first (§114).

**Rules**
- Use the same `:root` variables (colours, fonts, font sizes, spacing, radius) as the rest of the
  site. No new raw hex / px values.
- Buttons use the site's global button style (normal, hover, `:focus-visible`, disabled, loading,
  "added"). WooCommerce buttons must look like every other button on the site.
- Inputs, selects, checkboxes and radios use the site's form style — Checkout, My Account, coupon,
  variation selects and quantity included.
- If the design has its own look, remove WooCommerce's default look instead of fighting it:
  `add_filter( 'woocommerce_enqueue_styles', … )` to drop `woocommerce-general` (keep
  `woocommerce-layout` / `woocommerce-smallscreen` only if they help). Avoid `!important`.
- Block Cart / Checkout use `wc-block-*` classes. Style those when Blocks are chosen.

**Style every one of these, as per the HTML**

```text
Product card     image ratio, title, price, sale price (<del>/<ins>), sale / out-of-stock badge,
                 rating, add-to-cart button, hover
Shop toolbar     result count, sorting select, filters, pagination, breadcrumbs
Single product   gallery + thumbnails, title, price, short description, variations, quantity,
                 add-to-cart, meta, tabs, reviews, related / upsells
Notices          success, error, info (cart, checkout, account)
Cart             table, quantity, remove, coupon, cart totals, empty cart, cross-sells
Checkout         billing / shipping fields, order review, payment methods, place order, errors
My Account       login / register, navigation, dashboard, orders, addresses, account details
Thank You        order received + order details
Mini-cart        header icon + count, dropdown / drawer, empty state
```

**Mini-cart dropdown — must look right (lesson from a real build)**

*Seen on a build: the header cart dropdown was too narrow and ran off the screen, product names broke
letter by letter in heading size, prices were huge, "View cart" wrapped and Checkout / subtotal were
cut. Build it like this:*

- **Panel:** fixed width as per design (about `360–400px` on desktop), opens under the cart icon,
  aligned to the icon's **right** edge, and **never goes past the viewport** (`max-width:
  calc(100vw - 2 × gutter)`). On mobile: full-width dropdown or a side drawer, as per design.
- **Height:** the items list has a `max-height` and scrolls inside; subtotal and buttons stay visible
  at the bottom.
- **Item row:** thumbnail fixed (about `64–80px`, same ratio as the product card) + details column
  that takes the rest (`min-width: 0`). The details column must never be squeezed to a few px.
- **Product name:** body / small text style (not H1–H6 global), normal word wrap — never broken letter
  by letter — max 2 lines (`line-clamp`), linked to the product.
- **Variation meta, quantity × price:** small text; sale price shows `<del>` / `<ins>` correctly.
  Remove (×) icon with an `aria-label`.
- **Subtotal:** one row — label left, amount right.
- **Buttons:** **View cart** and **Checkout** both visible, site button style (smaller size), text
  never wraps; side by side on desktop, full width stacked on small screens if needed.
- **Empty cart:** short message + "Shop now" button, not a blank box.
- **Behaviour:** opens on click (and on hover on desktop only if the design does), closes on outside
  click and `Esc`, sits above the header (`z-index`) and is not clipped by a parent
  `overflow: hidden`. Count badge and content update without reload (step 8).
- **Test it** with 1, 5 and 10+ items, a long product name, a variable product and a sale product, at
  every breakpoint and on a non-shop page.
- **Elementor:** use the **Menu Cart** widget (Cart Type = Mini Cart / Side Cart as per design) and set
  its own style controls first — container width, product title / price typography (Global body
  text), divider, subtotal, View cart / Checkout button typography and padding. Only what those
  can't match goes in `custom.css` (§114).

**Responsive** — use the site's own breakpoints, then check each screen at every breakpoint:
- Product grid: desktop columns as per design → tablet 2 → mobile 1 or 2 (as per design).
- Single product: gallery and summary side by side on desktop → stacked on tablet/mobile.
- Cart table → stacked rows/cards on mobile (WooCommerce adds `data-title` labels for this).
- Checkout: two columns → one column on tablet/mobile; order review below the fields.
- My Account navigation → stacked or horizontal scroll on mobile.
- Shop filters → off-canvas drawer on mobile (only if the design has filters).
- Tap targets at least 44px; input font size at least 16px on mobile (stops iOS zoom).
- No horizontal scroll at 320px on any WooCommerce page.

## 8. Theme-level JS — `assets/js/woocommerce.js`

- Enqueue with `wp_enqueue_script()` in the footer, dependency `jquery` (WooCommerce's own scripts
  use it). Load it only where it is needed.
- **Never** dequeue WooCommerce's core scripts: `wc-add-to-cart`, `wc-add-to-cart-variation`,
  `wc-cart`, `wc-checkout`, `wc-cart-fragments` (where the mini-cart uses it).
- Hook into WooCommerce's own events instead of rewriting its logic: `added_to_cart`,
  `removed_from_cart`, `updated_cart_totals`, `updated_checkout`, `found_variation`, `reset_data`,
  `wc_fragments_refreshed`.

**Functionality to cover (as per the HTML)**
- **Quantity +/- buttons** around the quantity input. Trigger `change` on the input, so the
  "Update cart" button enables. Respect min / max / step.
- **AJAX add to cart** on shop pages (WooCommerce → Settings → Products) and on the single product
  page if the design needs it. Show loading and "added" states.
- **Mini-cart count + content** refresh without page reload, through the
  `woocommerce_add_to_cart_fragments` filter in `inc/woocommerce.php`.
- **Mini-cart drawer** (if in the design): open/close, `aria-expanded`, focus trap, closes on
  `Esc` and on overlay click.
- **Variations:** price, image and stock change on `found_variation`. Swatches only if the design has
  them.
- **Product gallery:** use WooCommerce's built-in gallery (flexslider + photoswipe). Do not add a
  second slider library for it.
- **Mobile filters drawer** and sorting, if in the design.
- After any AJAX update (cart, checkout, filters), re-run the site's animations / layout code (for
  GSAP: `ScrollTrigger.refresh()`).
- Everything works by keyboard and screen reader.

## 9. Performance, caching and SEO

- Cart, Checkout and My Account are **excluded from page cache**.
- WooCommerce CSS/JS load only where needed; cart fragments only where the mini-cart needs them.
- Product images WebP and correctly sized; Media Library once per image.
- Product schema from WooCommerce or the SEO plugin — one source, no duplicate schema.
- Permalinks: product base and category base set (Settings → Permalinks).
- Every WooCommerce page also has `<meta name="DeveloperAI" content="Developer Skill">` in `<head>`.

## QA

- [ ] Shop, Category, Single Product, Cart, Checkout, My Account and Thank You match the HTML at
      desktop, tablet and mobile.
- [ ] WooCommerce buttons, inputs and notices use the site's global styles; no raw values, no
      `!important` fights.
- [ ] No horizontal scroll at 320px; cart and checkout are usable on mobile.
- [ ] Every product on the site (home, sliders, related, menus, landing pages) is pulled from WooCommerce
      — no static product name, price, image or button anywhere (step 6a).
- [ ] Simple and variable products add to cart; variation price / image / stock update.
- [ ] Quantity +/- works and enables "Update cart"; mini-cart count updates without reload.
- [ ] Header mini-cart dropdown: right width, inside the screen, names readable (no letter-by-letter
      break), View cart + Checkout + subtotal visible, empty state, works on mobile.
- [ ] Coupon, shipping and tax totals are correct.
- [ ] A test order completes with sandbox payment; customer and admin emails arrive and are branded.
- [ ] My Account: login, register, orders, addresses, account details and password reset work.
- [ ] WooCommerce → Status: no outdated templates, no errors; HPOS compatible.
- [ ] Cart, Checkout and My Account are excluded from cache.
- [ ] No JavaScript console errors on any WooCommerce page.

---

# 143. QUICK UPDATE MODE — SITE ALREADY BUILT, GO STRAIGHT TO THE POINT

*When the site has already been built from this file and the user asks for one change ("add
WooCommerce", "fix the cart on mobile", "change the footer", "add a section"), do **not** run the
full build again. Go straight to that point.*

## Which mode?

```text
No theme / site yet                          → FULL BUILD (this whole file, from §1)
Site already built + one change is asked for → QUICK UPDATE (this section)
```

Signs the site is already built: the theme exists in `wp-content/themes/`, pages exist in WP Admin,
the project has a `CLAUDE.md` / theme docs from the earlier build.

## Quick update — steps

1. **Understand the ask.** Restate the one change in one line. Do not widen the scope.
2. **Read only the matching sections** of this file (table below). Skip the rest — no full site
   inventory, no full architecture report, no re-reading every page.
3. **Do not ask again** what the earlier build already decided (Elementor globals, header/footer templates, custom.css structure). Read it from the
   theme / project docs. Ask only if this change needs a **new** decision (e.g. WooCommerce Block or
   Classic, if WooCommerce was never set up).
4. **Inspect only what the change touches:** the files / templates / widgets for that feature and
   the pages that use them.
5. **Back up only those files** before editing (git commit, or a dated copy) — the light version of
   §75. Use the site login the user provided if WP Admin is needed; never expose it.
6. **Make the change** following that section's rules. Reuse the existing classes, variables,
   components and file structure — no parallel structure, no duplicate CSS/JS.
7. **Check only what it affects:** that section's own QA list + the pages that use the changed part,
   at desktop, tablet and mobile. Confirm nothing else broke there.
8. **Report briefly:** what changed, which files, what was checked, anything still open.

## Topic → section index

| Change asked for | Read only |
|---|---|
| Any visual change | §96 ⭐ Main Requirement first, then the row below |
| Reusing existing templates / globals | §110 (only the parts the change touches) |
| Global colours / typography / spacing | §98–§102 |
| Header / menu | §105, §106, §108 |
| Footer | §107, §108 |
| CSS / design (custom.css) | §114, §135 |
| Reusable components / templates | §104, §124, §125 |
| Responsive | §119 |
| Animations / GSAP / JS | §121, §135 |
| Forms | §28 |
| SEO / meta | §29, §141 |
| Favicon / site icon | §144 |
| Images / SVG / placeholders / Media Library | §131, §136, §137, §140, §149.2–§149.3 |
| Internal links / permalinks | §132, §133, §145 |
| Button / card / menu links not set | §145 |
| 404 / Search / Sitemap | §138, §139 |
| Partners / Blog | §111, §112 |
| WooCommerce (add or change) | §142 |
| Known bugs / lessons / final sign-off | §146–§149 |
| One-click setup / importer | §150 |
| Verify against the design | §151 |

Always-on rules still apply in quick mode: no inline styles / widget CSS, CSS variables, images
once in the Media Library, the DeveloperAI meta tag, never expose login details.

## Example — add WooCommerce to an already-built site

1. Read only §142.
2. Ask only the 3 WooCommerce start questions in §142 — one at a time (§65).
3. Go through §142 step by step and **skip any step that already exists** (e.g. theme support
   already added, pages already set).
4. Reuse the site's existing variables, buttons, forms, header and breakpoints for the shop CSS/JS.
5. Run the §142 QA list and check the header (mini-cart) on a few non-shop pages.

## QA

- [ ] Only the asked change was made; nothing outside its scope changed.
- [ ] That section's own QA list passes, and the ⭐ Main Requirement (§96) still holds.
- [ ] Pages using the changed part look and work the same as before, apart from the change.

---

# 144. FAVICON — MADE FROM THE LOGO ICON (MANDATORY)

*Default on every build.* Every site gets a favicon (browser-tab icon) made from the **logo icon** —
the symbol / mark part of the logo, not the full logo with its wordmark text. A blank tab, the
WordPress "W" or a theme's default icon is a bug.

## Source — in this order

1. A favicon / app-icon frame in the design, if there is one.
2. Otherwise the **icon (mark) part of the logo**, from the logo SVG in the HTML site (or the Figma file, if it is available).
3. If the logo is text only (no mark), use the logo's first letter in the logo font and brand colour,
   centred on a square — and tell the user, so they can supply a different icon.

Never draw a new icon, never use a stock or generic icon, never stretch the full wordmark into a square.

## Make the icon

- **Square, 512 × 512 px PNG** (`site-icon.png`), the mark centred with about **10% padding**, so
  nothing touches the edge.
- Build it from the **SVG** of the logo, so it is sharp. Never upscale a small PNG.
- Background: transparent. If the mark is white / very light, put it on the brand background colour
  (the Elementor global brand colour) so it is visible on a light browser tab.
- Check it at **16 × 16 and 32 × 32**. If the mark is too detailed to read that small, use the
  simplest recognisable part of it.

## Add it to WordPress

- Use WordPress's own **Site Icon** (Appearance → Customize → Site Identity → Site Icon, or Elementor → Site Settings → Site Identity → Site Favicon — both set the same
  `site_icon` option, so use one, not both).
  WordPress creates the 32, 180 (Apple touch), 192 and 270 px sizes and prints all the
  `<link rel="icon">` / `apple-touch-icon` tags itself through `wp_site_icon()` in `wp_head`.
- Upload `site-icon.png` to the Media Library **once** (§140).
- Set it in the one-click setup / importer (§150) too, so a fresh install has the favicon
  (the setup step that builds the site):

```php
update_option( 'site_icon', $site_icon_attachment_id );
```

- Do **not** hard-code `<link rel="icon">` tags in `header.php`, a plugin, an HTML widget or an SEO
  plugin field while Site Icon is set — they would print twice.

## QA

- [ ] The browser tab shows the logo icon on Home, an inner page, a post, 404 and Search.
- [ ] View source: the icon tags come from WordPress Site Icon, with no duplicate `rel="icon"` tags.
- [ ] The icon is sharp and readable at 16 px, on a light and a dark browser tab.
- [ ] `apple-touch-icon` (180 × 180) loads.

---

# 145. LINKING — EVERY CLICKABLE ELEMENT IS LINKED (MANDATORY)

*Every button, link, card, logo, menu item and icon that looks clickable must go to its real
destination. Nothing is left as `#`, empty, or unlinked.*

## 1. Make a link map first

Before building pages, list every clickable element from the HTML site (and the Figma prototype, if any):
logo, header menu + dropdowns, header CTA, hero buttons, section buttons / "Learn more" / "Read more",
service / blog / team / product / category cards, breadcrumbs, pagination, footer menus, footer CTA,
social icons, phone, email, address, legal links (Privacy, Terms, Cookies), 404 / Search page links.

For each one write: **element → page / post / product / external URL**. Keep this as
`link-map.md` in the theme docs.

- Destination is in the design / HTML → use it (`about.html` → the WordPress **About** page).
- Destination page doesn't exist yet → create that page (or ask), don't leave the link empty.
- Destination is unclear (no target in the design) → **ask the user** with the list of unclear
  links. Never guess and never silently leave `#`.

## How to set each link (Elementor)

- Internal page / post → **Link → Dynamic Tags → Internal URL → Content** (§132). Never a typed URL.
- Product / product category → the product's own link from the **Loop Item** / WooCommerce widgets
  (Product Title, Product Image, Add to Cart) or Internal URL → the product / category.
- Archive / category / tag → Dynamic Tags → Archive URL / Internal URL → Taxonomy.
- Menus → WordPress Menus in the Nav Menu widget (§106) — every menu item points to a real page.
- Whole card clickable → set the link on the Loop Item / container (Elementor container link), not
  only on the small "Read more" text.
- Phone → `tel:`, email → `mailto:`, address → Google Maps link — taken from Site Settings / ACF
  options, not typed again per widget.
- Social icons → the real profile URLs from the design / client, opened in a new tab with
  `rel="noopener"`.

## Never

- `href="#"`, `href=""`, `javascript:void(0)` or a button with no link — except real toggles (mobile
  menu, accordion, tab, modal open), which are `<button>` elements, not links.
- A hard-coded domain (`https://staging.site/...`, `localhost`) or a `.html` link.
- A link to the wrong page because it was copied from another section.

## Final link check (before handover)

- Search the theme / templates / exported content for `href="#"`, `href=""` and `.html` — fix every hit.
- Click (or crawl) every link on every page: header, footer, each section, cards, buttons, at
  desktop and mobile menu. No 404, no link to `#`, no staging / localhost URL.
- Report any link still waiting on the client (e.g. social profile not given) in the handover note.

## QA

- [ ] `link-map.md` exists and every clickable element on the site is in it.
- [ ] No `href="#"`, empty href or `.html` link anywhere (except real toggle buttons).
- [ ] Logo → Home; every menu item, CTA, button, card and footer link opens the correct page.
- [ ] Product and category cards open the right product / category (WooCommerce sites).
- [ ] Phone, email, map and social links work; external links open in a new tab with `rel="noopener"`.
- [ ] No staging / localhost / hard-coded domain URLs; no 404 from any internal link.

---

# 146. LESSONS LEARNED — ISSUES QA ALREADY FOUND (DO NOT REPEAT)

These came from the Buell Recreation build. On any new Elementor project, **check each one yourself and fix it before anyone else sees the site.** If you hit a new kind of issue, fix it and add it here.

## 146.1 Landmarks & structure
- Every page needs exactly **one `<main>`** (role="main" on the page content container). Check pages, single, archive, 404 and search templates.
- The Theme Builder Header/Footer *location* already renders `<header>` / `<footer>`. The top container inside the template must be `html_tag = div`, never `header` / `footer`, otherwise the page gets duplicate `banner` / `contentinfo` landmarks.
- Loop Grid / Carousel containers must not carry `role="list"` unless every child is `role="listitem"`. Remove the stray role. Same for `.elementor-posts-container` on search/archive.
- One `<h1>` per page, no skipped heading levels (a card `<h3>` after the `<h1>` with no `<h2>` is a bug — fix the tag, keep the visual size through the style controls).

## 146.2 Accessibility names
- Every icon-only link/button gets an `aria-label`: social icons (name from the network), slider arrows, off-canvas open/close, card arrows (use the card heading).
- When a page has more than one `<nav>`, each needs a **unique** `aria-label` (Main menu, Footer – Markets, Footer – Company…).
- **Colour contrast is a QA bug.** Test every text/background pair with axe: white on a light brand blue/green usually fails (below 4.5:1, large text 3:1). Do NOT silently change the brand colour — list the failing pairs (ratio + where) in the handover so the client picks a darker token. Never hide the fix in custom CSS.

## 146.3 Text & symbols
- Superscripts (®, ™) inside headings, banners and breadcrumbs must render as a real `<sup>`. Breadcrumb/label code that uses `esc_html()` prints `<sup>` as raw text — use `wp_kses( $label, [ 'sup' => [] ] )`.
- A ® styled as a knocked-out disc can show a blank circle if the glyph colour equals the disc colour. Set the glyph ink from the surrounding text luminance and look at it on both dark and light backgrounds.

## 146.4 Breadcrumbs & menus
- Breadcrumb must not repeat the current page ("About / About"): skip a menu ancestor whose object id equals the current page.
- An item in several categories shows only the first in the breadcrumb; decide on purpose (primary category) and note it.
- **Mega-menu hover image:** never fall back to a logo/placeholder. Fallback chain = featured image → gallery → product image → category banner → nothing. When there is no image, restore the default panel image.
- New CPT entries do **not** appear in a static WP menu. Say so, or build the menu column from a query.

## 146.5 Page hierarchy
- A sub-page of a section (e.g. Designers under Markets) must be a real child page (parent id + menu_order), use the same template/builder script as its siblings, and appear in the section's card list.
- The parent landing page's card list shows **children only** — never the parent itself.
- Give the parent its own landing content (banner + cards + contact panel) rather than leaving it empty.

## 146.6 Dynamic content (CPT / ACF / Loop)
- **Verify data flow for every post type:** create a temporary entry, fill every field, open every view where it should appear (single, archive, home carousel, related carousel, category page), then delete the temporary entry. Report what is manual (menus, relations) vs automatic.
- Check empty states: an empty ACF field must hide its block, not show an empty box or "Lorem".
- A "related / explore other…" carousel must query the **right source**. Example: brand page "Explore Other Product Categories" must loop **Product Categories** (taxonomy terms), not the brand gallery.
- Never run `WP_Query` / `get_children` / `get_pages` inside an `elementor/query/*` hook — infinite recursion → HTTP 500. Alter the passed query object (`$query->set(...)`), or use `$wpdb` / post meta.
- Don't post `acf` props to core REST endpoints that don't register them; post core fields only.

## 146.7 SEO
- Every page: unique `<title>` and meta description (≤160 characters, count them). Include the archive, search and 404 templates.
- Never copy one page's SEO text to another (Markets once carried Designers' copy). When a page is split or renamed, rewrite its entry.
- Staging is `noindex` → no canonical tag. Add "turn on search-engine visibility + check canonicals" to the launch checklist.

## 146.8 Links & content
- Links: §145. **List every link still waiting on the client and every placeholder/lorem block in the handover** so QA does not log them as bugs.
- Read the copy: wrong excerpts pasted from another brand/page and test data (e.g. a "Visit Website" pointing to a search engine) are content bugs. Fix or list.

## 146.9 Cache & tooling
- Hosts like WP Engine cache query-string URLs. After a rebuild clear the Elementor cache (`DELETE /elementor/v1/cache`) and use a harmless cache-buster (not `?s=` — that triggers WordPress search).
- `curl` may get 403 from host bot protection; verify links in a real browser run.

## 146.10 SELF-QA — MANDATORY, TWO PASSES
Before telling the user "done", run QA yourself **twice** (pass 2 after fixing pass 1):
1. Crawl every page, CPT single/archive, taxonomy page, search and 404 at **1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320**.
2. Per page record: console errors, failed requests, broken images, missing alt, horizontal overflow, clipped text, heading order, landmarks, axe violations (Playwright + axe-core), title/description, `#` links (§145) / placeholder content, form validation.
3. Take screenshots and **look** at the header, mega menu, banner, cards and footer on desktop and mobile; measure spacing instead of guessing.
4. Fix everything that is code. Anything that needs the client (colour, copy, URLs) goes in a short "needs decision" list.
5. Final report grouped by QA bug type (Accessibility, SEO, Content, Links, Responsive, Functional) — what was fixed, what is clean, what is open.

## 146.11 DOCUMENTATION HABIT
- Keep `PROJECT-SUMMARY.md` (what exists, how to rebuild, content model, open items) and `DAILY-LOG.md` (newest first, what changed today) updated every working day so teammates can pick the project up cold.


---

# 147. BUG LIBRARY — EVERYTHING QA / THE CLIENT FOUND ON THE BUELL PROJECT (50+ ITEMS)

**How to use this section:** it is a pre-flight checklist. Before you show ANY page, go through every line below and test it yourself at **1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320**. If an item is not applicable, skip it. If you meet a new kind of bug, fix it and add one line here. Sections 146 and 147 together are the "do not repeat" list.

Legend: **[R]** = responsive-specific.

**Numbers below are Buell values (examples).** On a new project keep the rule, take the number from the new design. Example: "gaps 40/30/20" means *decide one gap scale from the design and use it on every slider and grid at every breakpoint*.

## 147.1 Working rules the client set (apply from day one)
1. Work **directly on the staging site** through the REST API / Application Password. Never build locally. Never put credentials, tokens or private URLs in any file, reply, commit or chat.
2. Create a **new, uniquely named theme/child theme** for the project (never reuse another project's theme name).
3. Match the design **pixel-for-pixel from the PDF/Figma** — zoom in per section and measure (border-radius, shapes, spacing, icon size, text alignment). "Close enough" is a bug. Photos and hero text can be client-changed later, so build them as editable.
4. CSS and styling: §114 — no HTML widget / raw markup / inline styles for layout.
5. **No classes or IDs in Elementor unless truly required** (e.g. a class that custom.css needs). Do not add "section-name" classes by habit.
6. **No static px for spacing** — margins, paddings, gaps use CSS variables (custom unit `var(--…)`), also inside `custom.css`. Section spacing must be editable in the admin (Elementor Variables, e.g. `section-space`, `section-top-space`, `section-bottom-space`, each with laptop/tablet/mobile values).
7. Links: §145. **Never link to the live/old site.** Dummy content is OK only where real data is missing; list it in the handover.
8. Build repeating content (projects, testimonials, team, brands, markets, categories, promotions, FAQ, areas) as **CPT + Loop Grid / Loop Carousel**, not copied static cards, so new entries show up automatically.
9. Give the client one final **list of: pages built, checks done, what is missing, what needs their input**.

## 147.2 Global header / top bar
- Top bar items (phone, location, tagline) must be **vertically centred** — measure it; demo/custom fonts often sit low in the line box (see 147.6).
- Top-bar content per device: keep the tagline on all devices; on small screens collapse phone/location to **icon-only** (text hidden) or move them into the menu. Never leave a half-shown mix. [R]
- Right-side top-bar icons, search icon and logo shape must match the design; the whole logo click area works (links: §145).
- Shape under the logo / curved header edge must match the PDF exactly at every width. [R]
- Header sticky behaviour and z-index must not hide content or cut dropdowns.
- Header "Products" mega panel starts **at the bottom edge of the header** (not overlapping it), is a floating card with the design's radius, and is **never cut off** (check desktop 1366–1920 with the panel open).

## 147.3 Menus (desktop)
- Submenu items carry the icon from the design; submenu panel layout (columns, photos) matches the design.
- Hover on a top item = the design's hover style (pill/underline). **Active state:** when a sub-page is current, its top-level item is also active. Home must not look active because of an `#anchor` or query-string menu link — test on the bare URL, not just with `?shot=`.
- Mega-menu image changes on hover of each product/brand link (fallback chain: 146.4).

## 147.4 Mobile / off-canvas menu [R]
- Hamburger uses a **custom uploaded SVG icon** (not the default Elementor icon) in the design's shape; check its size at 375/576/768.
- When the menu opens: **page scroll is locked**, only the menu scrolls.
- Menu rows: proper `li` borders/dividers and padding, clear tap targets, design look. Sub-menu chevron is a pure open/close toggle (does not navigate).
- Everything keeps working at 700–800 px where tablets/landscape phones show the menu: banner arrows must not be touched by the menu or overlap content.
- Social icons in the menu/footer are smaller on mobile.

## 147.5 Footer
- Copyright + privacy links: on small screens **centred and ordered last**; on a full-width bar the legal menu and copyright are centred.
- Phone and email links have **hover states**; the email never gets cut off (test around 1208 px — use `minmax(max-content, …)` column).
- ® / ™ symbols sit centred in their box; compare with the PDF zoomed in.
- Social icons smaller on mobile; footer wave/curve matches the design.
- Last section before the footer tucks under the wave by exactly the design's wave height (35–41 px); bottom padding matches the PDF. A section whose bottom space looks different from its top is a bug — view it, don't trust the numbers.

## 147.6 Typography & fonts
- Use the **client's real fonts** uploaded as Elementor Custom Fonts / local WOFF2 (remove placeholder fonts such as Sour Gummy when told). Keep exact weights; declare `@font-face` per real weight (a 100–900 range face can make 700 fall back to Arial bold).
- Demo/trial font files have watermarked glyphs (e.g. digit 4, `& - ' % ! / ™ ®`). Use `unicode-range` so only real glyphs come from the font and symbols come from a fallback or a small subset font. Compare ™ and ® with the design — client reported them looking different.
- Optical centring: trial fonts ride low in the line box → text looks low in buttons, header menu, tabs, page numbers, inputs. Fix with em-based offsets, then **measure** cap-height vs the box (not ink rows near rounded corners). Re-measure after the fix.
- Form fields need a font with correct ascent/descent metrics or Chrome clips placeholder descenders.
- Line-height: check multi-line card titles and big headings (a line-height bug was reported on link-card titles); h2 demo-font lift only when no intro follows.
- Text that is left-aligned in the PDF must stay **left** at all widths (do not centre it on mobile unless the PDF does).
- Title → content gap is **30 px (token)** on all pages and `section-title-wrap` bottom margin 30; use the token.
- Text/logos in the hero or a brand wordmark that gets cut by the wave must scale and keep the cut as designed; supply it as SVG and make it scale down on mobile. [R]

## 147.7 Spacing, sizing and responsive rules [R]
- Container side padding is global (gutter variable) — never added per section. Same padding on **all four sides** of cards and sections on phones (**20 px, never 15**), as a variable.
- Section top and bottom space must look equal when viewed; check every band, panel and CTA.
- Card padding variable (`--card-space`) = equal on 4 sides: 40 → 30 (≤1365) → 25 (≤991) → 20 (≤767).
- **Slider/loop gaps:** 40 desktop → 30 → 20, same on every page. Set the carousel gap (`image_spacing_custom`) at **every breakpoint explicitly** — an inherited value showed 16 px in the editor and a `var()` gap became 0, so carousel gaps stay in px.
- Icons and logos must be **smaller on tablet/mobile** (client repeatedly found them too big). Badge/icon glyph is centred with flex, not padding; check every icon badge.
- Brand logos in loop cards: width 100% + a max-width that steps down per breakpoint.
- Responsive typography steps per the design; at phone width a big number/heading must not be tiny (404 text) or huge.
- Buttons never stretch to 100 % width on phones; button text is vertically centred.
- Photos do **not crop** on mobile unless the PDF crops (About "Our Story" photo bug): use auto height ≤991. Inclusive/background photos keep behaving as background at all widths.
- A section with icon + title + text centred on mobile (e.g. "Why Communities") centres **all three**; its icons may be larger only for that section.
- Fixed breakpoints: do not leave a value for 991 and 575 inconsistent (client set 767 = 36 px, 991 and below = 40 px). Ask which value applies at each breakpoint or copy the design.
- Forms: checkbox row is placed **above** the Send button on all widths; phone field with country prefix looks like the design.
- Border radius at tablet/mobile must not look broken (curved panels use a 9-slice mask from the PDF path, with the same corner/bulge size); check every rounded card at 375.
- Marquee/logo band speed: ~4 s per logo (a slow marquee was a bug); paused on hover, reduced for `prefers-reduced-motion`.

## 147.8 Sliders, carousels & loops
- Arrows are the design's arrow (circle/square, chevron, hover and pressed colour). **Disabled arrow = `cursor: not-allowed`** + disabled colour.
- Carousel maths: slide width = (widget width − offset − gaps)/n; peeking cards use fractional `slides_to_show` at every breakpoint. Always set `slides_to_show` for desktop, laptop, tablet, mobile explicitly (Elementor's default tablet 2 showed two hero slides).
- Home hero = exactly the number of slides the client approved (2), banner slider on other pages uses dummy data if there is none.
- Do not let a slider "break" by trying to fix one slide: a fix for one slide must not rearrange the rest — test the whole row.
- Cards in the slider must not get cut on hover (hover transform/scale needs overflow space). Add zoom-on-hover for **product/gallery images** inside an overflow-hidden wrapper, using the global `--transition` variable.
- Loop Grid pagination (page numbers + prev/next) is styled like the slider arrows and is centred; search results pagination too.
- Every loop image (featured or ACF) has an Elementor dynamic-tag **fallback** = the §137 placeholder (Buell: neutral grey, correct ratio, **site logo centred**), same on every page and breakpoint.
- Brand/partner logo tiles are **clickable on the whole bordered box**, and go to the detail page. Logos in marquees link to the brand detail page too.
- Nested Carousel: the `offset` (peeking next card) only works with **infinite loop on**; the NEXT arrow's horizontal offset sign is the opposite of the PREV arrow's — check both arrows' positions at every breakpoint.
- A carousel that must be wider than its container (bleeding to the screen edge) needs `max-width: none` on the widget, otherwise a width over 100 % is ignored.
- Side arrows on wide screens (1366–1920) are placed with `calc()`/`clamp()` on `100vw` so they never sit on top of the cards or off-screen. Check at 1366, 1440, 1600 and 1920.
- After changing a slider, test the **whole row on every breakpoint** (slides shown, gap, peek, arrows, dots, swipe on mobile, autoplay pause) — not only the slide you changed.

## 147.9 Content, data & dynamic pages
- Every post type gets dummy data when empty so no section looks empty (area tiles, slider 2, team, projects). Mark dummy data clearly (e.g. "Sample") and list it.
- Every detail template must be built when the design exists (e.g. Areas We Serve detail page from the PDF). A button that leads nowhere is a bug.
- Dynamic pages must not break the design when data is **more or less** than the design shows (long titles, 1 item, 12 items). Test with few and with many entries.
- If the admin adds content (e.g. brand detail fields) and it does not show in the view, the template is missing a dynamic field — check each ACF field against the template.
- Slugs must be semantic (the FAQ page must be `/faq/`).
- Breadcrumbs are **dynamic** (page parent chain / menu / post type), not static text; correct on every page type. Static crumb code left in builders will crash rebuilds — remove it.
- Search and 404 pages: §138 — plus a search empty state + pagination, and the large "404" text scales on mobile. [R]
- Animations: `fade-up` and similar entrance animations must actually play on the sections the design has them (client reported a section not animating). Test scroll animations in a real browser.

## 147.10 Elementor technical gotchas (save hours)
- Container desktop width is printed inside `@media(min-width:576px)` at the end of the post CSS, so tablet-extra widths never apply → restate widths ≤1199 explicitly for every container (inheritance) via the generator/`custom_css`.
- Grid/flex containers: set columns at **every** breakpoint (Elementor defaults tablet 2 / mobile 1); containers wrap by default — set `nowrap` for rows.
- Elementor per-element CSS beats `body .elementor .x` rules; use `.elementor-element.x` or set the control in the generator.
- An undefined CSS var inside `calc()` silently breaks a whole width/card row — verify every var exists on `:root`.
- Zero spacing must be `0px` when a var-converted control is involved (a unitless 0 invalidates the whole margin). Form-widget column/row gap must stay px.
- Overlay default opacity 0.5 halves the scrim — set the overlay opacity explicitly.
- Linked image `<a>` is inline-block → fixed-height cover photos shrink; make it `display:block` for image-link wrappers.
- Form field custom classes go on the input, not the group; select has no `.elementor-field` class.
- Icon widget size key is `size`; get real control names from the widget config (`get_widgets_config`) and validate generated JSON before saving.
- Mega-menu / off-canvas: Pro off-canvas widget for mobile, Pro mega-menu widget for desktop panels; mobile nav submenus forced inline in custom CSS (approved CSS, §114).
- Custom `?area=`-style query vars can collide with a CPT query var; strip/handle them in a `request` filter on the archive.
- Theme upload: after every upload check the site returns 200; bump the theme version to flush rewrites/caches. No PHP linter may exist — review PHP by eye.
- Run a **pixel-diff A/B** when you bulk-convert (e.g. px → var tokens) to prove nothing moved.
- **Check you are on the right site before any write.** A shared dev install once had another client's theme active — confirm site name/theme via REST first (kit, Theme Builder and theme activation are site-wide).
- REST quirks on managed hosts: if `/wp-json/` redirects, use `/?rest_route=/…`; theme install/upload through REST can fail on WP Engine (`copy_failed_pclzip`) → upload the theme zip in wp-admin; SVG/WOFF uploads need a mime filter (admins only).
- ACF fields appear in REST only when the field group has "Show in REST" on; Yoast fields need registering for REST before a script can write them.
- Google Maps widget needs ~5 s to render before a screenshot; lazy content needs a scroll before QA screenshots.
- Git Bash on Windows rewrites arguments like `/about/` into file paths — set `MSYS_NO_PATHCONV=1` when passing URL paths to scripts.

## 147.11 QA protocol (client expectation: "zero bugs")
1. Compare **every page against the PDF/Figma, zoomed in**, not only overflow/console checks. Technical-only QA already failed once (the client found 10 visual bugs after a "clean" technical report).
2. Measure: padding on all four sides, icon centring in badges, image crops, gaps per breakpoint, text overflow, vertical centring in buttons, equal top/bottom section space.
3. Click-test: menus (desktop + mobile), accordions, tabs, sliders and arrows, forms (validation), logo links, brand links, pagination, hover states.
4. Devices: 1920 / 1600 / 1440 / 1366 / 1200 / 992 / 768 / 576 / 375 + 320 — plus 700–800 for the menu/banner arrow case.
5. Run the checks twice (after fixes) and write the result per QA bug type.
6. Before saying "done": updated `PROJECT-SUMMARY.md` + `DAILY-LOG.md`, listed dummy content, links waiting on the client, client decisions, and the theme version.


---

# 148. PRE-DELIVERY SIGN-OFF

→ Merged into **§128 Elementor Final QA** (Pre-delivery sign-off — tick every box twice). See §128.

---

# 149. BUILD IT RIGHT THE FIRST TIME — ICONS, IMAGES, MENU, DESIGN

§146–148 find bugs **after** building. This section prevents the most frequent ones **while** building. Follow it in this order on every new project.

## 149.1 Design numbers come from the source, not from eyes
1. If a PDF/Figma exists, extract exact values first: text, positions, box sizes, colours, radii and vector paths (e.g. PyMuPDF `get_text("dict")`, `get_drawings()`, `get_images()`; Figma variables/context). Save them (`pdftext` / `pdfboxes` style JSON).
2. Build from those numbers (padding, gaps, radius, icon size, font size per breakpoint).
3. After building, compare **live DOM boxes/text vs the extracted PDF boxes** with a script, then zoom-compare screenshots. Fix every difference above ~2 px.
4. Curved edges, waves and bulged panels: take the path from the PDF and build a reusable mask (9-slice: fixed corner size + bulge), do not approximate with a plain border-radius.

## 149.2 Icons — no icon issues
- Take **every icon from the design/HTML itself** (extract SVG from the PDF/HTML). Never substitute Font Awesome/Elementor default icons unless the design uses them.
- Enable SVG upload (theme, admins only), upload each SVG once (§140), store the ids in a map (e.g. `svgmedia.json`) and reuse via the native Icon widget.
- Icons must use the global colour (currentColor / Elementor global), keep cut-out parts (detail paths) in the secondary colour, and keep their own fills only when the icon is an illustration (flag, logo).
- Size by variable per breakpoint (smaller on tablet/mobile); centre in the badge with flex — verify centre point by measurement.
- Hamburger, search, arrows, social: all custom SVG from the design (accessible names: 146.2).
- Check every icon: correct glyph, colour, size, centring, hover colour, at 1920 and 375.

## 149.3 Images — no image issues
- WebP, names, alt text and Media Library: §131, §136, §140. Resize to ≤2× the largest display size; SVG stays SVG (logos, icons, wordmarks, watermark).
- Use PDF-extracted crops/positions: hero, split-intro and card photos get `aspect-ratio` + `object-fit: cover` + focal position as in the design; photos that must not crop on mobile use auto height ≤991.
- Wide/background photos keep behaving as background at every width; overlays have explicit opacity.
- Logo tiles: contain, centred, whitespace-trimmed so logos look the same optical size (dynamic-image fallback: 147.8).
- Check: no stretched, blurry, cropped-wrong, missing or oversized (heavy) image at any breakpoint; `loading="lazy"` below the fold, priority on the LCP image.

## 149.4 Menu — no menu issues
1. Menus are real WordPress menus (§106); header, footer and mobile reuse them.
2. Desktop: Pro Mega Menu widget for panels, native submenu for simple dropdowns. Build the panel from the design: columns, icons, photos, floating card, radius, start at the header bottom.
3. States: hover, focus-visible, active (also when a child page is current), open. Test on the **bare URL**, no query string.
4. Hover image and CPT columns: 146.4. Mobile: 147.4 (Pro Off-Canvas, works from 1199 px down).
5. Keyboard: Tab order, Esc closes, `aria-expanded`.

## 149.5 Design system first, pages second
Global Colours → Global Fonts (real client fonts) → Elementor Variables for spacing (section, card, gap, gutter, per breakpoint) → saved components (heading, button, card, loop items, CTA) → Header/Footer → pages → CPT templates. Building pages before the variables exist is what creates static-px and inconsistent-gap bugs.

## 149.6 Honest limit
These sections lower the bug count a lot, but they do not guarantee zero. Every new project has its own design details, so the two-pass self-QA (§146.10, §128 pre-delivery sign-off) and the zoomed PDF compare stay mandatory. Any new kind of bug → fix it, then add one line to §146/§147.

---

# 150. ONE-CLICK SETUP — TOOLS → {PROJECT} SETUP (MANDATORY)

Every Elementor build ships a **one-click importer**, so a fresh WordPress install becomes the full
site with one click — and a re-run never creates duplicates.

**Where it lives:** in the theme, `inc/importer/` (PHP) + `inc/importer/data/` (JSON seed + exported
Elementor templates). Admin screen: **Tools → {Project} Setup**, admins only, nonce-checked, with an
"Import" button and an "Overwrite" checkbox. Each step runs as a small AJAX batch with a progress log,
so it never times out.

**Seed data — extracted from the original HTML, never retyped, never invented.** Page text, menus,
CPT entries and images are parsed from the HTML site into the JSON seed. Missing content uses a
clearly marked placeholder (§137) and is listed in the handover.

**Import order:**

```text
1. Media            → every content image once, tagged with _{prefix}_source = file name (§140)
2. Templates        → Theme Builder templates (header, footer, singles, archives, 404, search),
                      section templates and Loop Items — exported as Elementor JSON
                      (Templates → Saved Templates → Export); imported once, matched by title
3. Pages            → Home first (set as front page right away), Blog early (posts page);
                      sections in the original page order
4. Posts / CPTs     → entries + featured images + ACF PRO fields (§12, local JSON field keys)
5. Menus            → built from the seed, assigned to the theme locations (§106)
6. Site settings    → Elementor globals (§98–§102), breakpoints (§119), display conditions,
                      Site Icon (§144), WooCommerce pages (§142), forms (§28)
7. Finalize         → permalinks = Post name (§133), flush rewrite rules,
                      Elementor → Tools → Regenerate CSS & Data
```

**Rules:**
- **Safe to run again:** find existing items by slug + post type (all statuses), keep them by default;
  only "Overwrite" updates them. Never create duplicate pages, templates or media.
- Use images by Media Library attachment ID — never the exported template's own image URLs.
- Use only documented Elementor import methods for the installed version (template import, Kit
  import or WP-CLI). Never guess meta keys or formats — check Elementor's official docs or ask.
- `is_wp_error()` on every insert; skip and report a failure instead of stopping. Never delete
  content that is not in the seed. No API keys or passwords in the seed files.

**QA:**
- [ ] On a fresh install: activate the theme + Elementor Pro + ACF PRO, run Tools → {Project} Setup →
      every page, template, menu, CPT entry and image is there and matches the design.
- [ ] Run it a second time → nothing is duplicated (pages, templates, media, menu items).
- [ ] Front page, posts page, menus, display conditions, Site Icon and permalinks are set.

---

# 151. VERIFY AGAINST THE DESIGN — BEFORE CALLING ANYTHING DONE

Every page and every section is checked against the design before it is called done.

1. Build **one real section** first as a proving run — ideally the header or hero, since it
   exercises type, colour, buttons, icons and responsive behaviour at once. Fix the drift before
   building the rest.
2. Screenshot every page headless at desktop / tablet / mobile and compare it side by side with the
   matching design (Figma frame / PDF page / original HTML page).
   > Headless Chrome enforces a ~500px minimum window, so a 375px screenshot comes out **cropped,
   > not narrow**. To check true mobile, load the page in a 375px-wide `<iframe>` inside a wrapper
   > page and screenshot that instead.
3. Check, per section: spacing, font sizes, line heights, colours, alignment, radius, shadows,
   image crops, icon sizes, hover / focus states and animations.
4. Zoom in on every image in the screenshot. If any image looks soft or blurry, export it again at a
   higher size.
5. Nothing from the design is missing on the site — every section, image and text block is there.
6. Open each page in the Elementor editor and confirm an editor can change text, images, links and
   section order without code.
7. Fix the drift, then report: what was checked, what was fixed, what still needs the user's input
   (usually real copy, real images and real URLs). Then run the §128 final QA.
