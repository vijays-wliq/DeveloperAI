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

Use an appropriate field system if the project permits it.

If using ACF, do not recreate ACF functionality manually.

If no plugin is permitted, implement secure custom meta fields.

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

Use:

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

# 34. DO NOT USE ELEMENTOR UNLESS REQUESTED

Default implementation:

```text
Custom PHP WordPress Theme
```

Do not convert every HTML section into Elementor widgets unless explicitly requested.

The existing HTML should remain the source of truth for the frontend.

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

Partner detail should reproduce the existing Partner HTML template.

Use:

```text
single-partner.php
```

Dynamic fields must populate the existing HTML structure.

Do not create a completely new partner layout.

---

# 47. BLOG ARCHIVE

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
2. **Reusable CSS class only where native can't.** If a design detail or function cannot be
   achieved with Elementor, give that section/element an appropriate, reusable CSS class name
   (`Advanced → CSS Classes`). Name it by role, e.g. `card-hover-lift`, `logo-placeholder`,
   `section-curved-bg`. Never name it after a page or a colour.
3. **Global styles.** Manage reusable styles globally wherever possible (Global Colors, Global
   Typography, Site Settings, CSS variables) so the site stays consistent.
4. **All custom CSS lives in the theme's assets directory** (`assets/css/custom.css`, §135).
   - Never put it in a widget's `Advanced → Custom CSS`.
   - Never put it in Page Settings → Custom CSS.
   - Never put it in Site Settings → Custom CSS or in inline styles.
5. **Reusable, scalable, component-based.**
   - Build each repeated block once as a component (saved template / Global Widget / Loop Item, §104).
   - Style it once by its class, then reuse it.
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

---

# 101. RESPONSIVE TYPOGRAPHY

Inspect the original media queries and reproduce typography for:

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

If ACF is installed, use ACF fields with Elementor Dynamic Tags where appropriate.

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

# 114. CSS STRUCTURE — NO WIDGET CSS, REUSABLE CLASS-BASED CSS ONLY — MANDATORY

**Do not add Custom CSS in any widget.** The widget-level `Advanced → Custom CSS` field must stay
empty on every widget, container and template. The same goes for Page Settings → Custom CSS and
Site Settings → Custom CSS. No inline `style=""` either.

**Styling order (native first — see the ⭐ Main Requirement in §96):**

```text
Elementor Global Settings (Colors, Typography, Buttons, Forms, Layout)
    ↓
Global Variables
    ↓
Native Elementor Widget / Container Style Controls
    ↓
ONLY where native Elementor cannot match the original HTML:
reusable CSS class + CSS in assets/css/custom.css
```

**Section-level classes, wherever required.** When a section or element needs CSS that Elementor
cannot do, give it an appropriate, reusable class (`Advanced → CSS Classes`).
- Section classes look like `about-section` or `testimonials-section`.
- Shared components get a component class, e.g. `card-hover-lift` or `logo-placeholder`.
- Write the styling for that class **once** in `assets/css/custom.css` (§135) and reuse the class.
  Don't repeat the same padding, colours or font sizes widget by widget.

**CSS variables for common values.** Font sizes, colours, spacing, radius and other repeated values
are CSS variables, never repeated raw values. Use Elementor's own global variables where they exist
(`--e-global-color-<id>`, `--e-global-typography-<id>-font-size` …). Put any extra shared values in
one `:root` block at the top of `custom.css` (e.g. `--section-space`, `--container-gutter`).

**Global spacing.**
- Section top/bottom spacing is one variable (e.g. `--section-space`) applied through the section
  class, so it matches across every section.
- Container left/right spacing is set **once** globally, either through Site Settings → Layout or
  one rule in `custom.css`. Never re-enter it in each section.

**Keep the CSS clean, scalable and reusable:**
- One comment banner per section or component.
- Class names describe a role, not a page or a colour.
- No duplicate rules.
- Avoid `!important`.

---

# 115. SPACING RULE

For every padding/margin/gap ask:

> Is this part of the global design system or unique to this component?

If global, use a Global Spacing/Variable. If unique, use a component-specific value.

Do not create unnecessary one-off values.

---

# 116. TYPOGRAPHY RULE

For every text element ask:

> Is this a standard body, heading, label, or button style?

If yes, use Elementor Global Typography. Only use custom typography for genuinely unique designs.

---

# 117. COLOR RULE

For every color ask:

> Is this a site-wide design color?

If yes, use an Elementor Global Color. Do not manually repeat the same hex/RGB value across many widgets.

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

---

# 119. RESPONSIVE ELEMENTOR SETTINGS

Every important section must be checked at:

```text
Desktop
Tablet
Mobile
```

Use the original CSS media queries as the reference. Do not blindly rely on Elementor defaults.

---

# 120. ANIMATIONS

If the original uses GSAP, ScrollTrigger, AOS, IntersectionObserver, CSS animations, Three.js, Swiper or other libraries, inspect the implementation first.

Use Elementor Motion Effects only when they reproduce the original behavior accurately. If Elementor cannot reproduce it, preserve the required custom JavaScript.

---

# 121. GSAP / THREE.JS WITH ELEMENTOR

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
- [ ] Native Elementor used wherever possible. Reusable CSS classes only where it couldn't match.
- [ ] All custom CSS is in the theme's `assets/css/custom.css`. None is in widget, page or Site
      Settings Custom CSS.
- [ ] Repeated blocks are components (template / Global Widget / Loop Item), not page-by-page copies.
- [ ] Every page has `<meta name="DeveloperAI" content="Developer Skill">` in `<head>` (§141).

## Global Design

- [ ] Global colors created and actually used.
- [ ] Favicon made from the logo icon is set as the Site Icon and shows on every page (§144).
- [ ] Global typography created and actually used.
- [ ] Global spacing/variables established.
- [ ] Container width matches original.
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
- [ ] Every section has a section-level class; shared styling lives in `custom.css`, not inline.
- [ ] Font sizes, colours and spacing use CSS variables / Elementor globals, not repeated raw values.
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

---

# 129. DEFINITION OF DONE

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
Styling              → Elementor globals + section classes + reusable CSS in custom.css (no widget Custom CSS, no inline styles)
<head> meta          → <meta name="DeveloperAI" content="Developer Skill"> on every page (§141)
Favicon              → WordPress Site Icon made from the logo icon (§144)
WooCommerce (if any) → Theme Builder + native Woo widgets, CSS in custom.css, woocommerce.js (§142)
```

Normal content changes should be possible through WordPress/Elementor without manually editing the original HTML files.

---

# 130. FINAL ELEMENTOR DEVELOPMENT PRINCIPLE

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

---

# 133. PERMALINK STRUCTURE

Set the WordPress permalink structure to **Post name**:

```text
Settings → Permalinks → Post name   (/%postname%/)
```

Set this before building internal links, then flush permalinks (save the Permalinks screen once) after registering any Custom Post Types.

---

# 134. FINAL GOAL

Deliver a complete, responsive, fully functional WordPress website built with **Elementor Pro** that faithfully reproduces the original HTML website, while following WordPress best practices and ensuring:

```text
Proper Global Styling
Reusable Components / Templates
Correct Internal Linking (Dynamic Tags)
Optimized WebP Images
Post name Permalinks
Manageable Content
No Custom CSS in widgets / Reusable, variable-driven CSS in custom.css
Accessible SVGs and images, SEO-friendly image names
Consistent placeholders, working 404 and Search pages
```

---

# 135. NEW THEME — VENDOR.JS AND CUSTOM.CSS

If the project needs a new theme (or child theme), create it, and include these two asset files:

```text
theme-name/
└── assets/
    ├── js/
    │   └── vendor.js      ← third-party libraries (GSAP, ScrollTrigger, Swiper, Three.js, etc.)
    └── css/
        └── custom.css     ← the ONLY place for custom CSS (section classes, variables, components)
```

### vendor.js

- Holds the third-party JavaScript libraries the original HTML site depends on.
- Do not load the same library twice (check whether Elementor or a plugin already loads it).
- Enqueue with `wp_enqueue_script()` in the footer, with correct dependencies and version numbers.
- Load only on the pages that need it where practical.

### custom.css

- The single home for all custom CSS (§114): the `:root` variables block, global container spacing,
  section-level classes and reusable component classes. Never put CSS in widget `Advanced → Custom CSS`.
- Do not use it to replace things Elementor Global Settings or widget controls can handle.
- Comment each rule with the section/component it belongs to.
- Enqueue with `wp_enqueue_style()` after Elementor's styles, so its rules apply in the right order.
- Avoid `!important`.

Register both through `inc/enqueue.php` (or `functions.php` for a minimal child theme) using `get_stylesheet_directory_uri()` / `get_template_directory_uri()`. Never hardcode paths or domains.

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
- Lock the ratio with a reusable class in `custom.css` (e.g. `.img-placeholder` with
  `aspect-ratio` and `object-fit: cover`). Don't set heights widget by widget.
- Give placeholders alt text that names the intended content (e.g. `Team photo placeholder`) so
  they are easy to find and replace.

### Logo placeholders

- Light gray / neutral background (a CSS variable, e.g. `--placeholder-bg`).
- Logo centered **both vertically and horizontally** inside a fixed-ratio box (flex,
  `align-items: center; justify-content: center`).
- Every logo box is the same size, and the logo keeps its proportions (`object-fit: contain`,
  max width/height).
- One reusable class (e.g. `.logo-placeholder`) for every logo grid and logo slider on the site.

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

Check the Media Library once at the end of the build: every image and icon appears exactly once.

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

**Ask at the start** (with the other start-of-project questions):
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
  controls → only where those can't match the HTML, a reusable class + CSS in `custom.css` (§114).
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

## 7. Theme-level CSS — design + responsive

Write all WooCommerce styling in `assets/css/custom.css`, under one `/* ===== WooCommerce ===== */` banner, after the native Elementor styling has been used (§114).

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
- [ ] Simple and variable products add to cart; variation price / image / stock update.
- [ ] Quantity +/- works and enables "Update cart"; mini-cart count updates without reload.
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
| Animations / GSAP / JS | §120, §121, §135 |
| Forms | §28 |
| SEO / meta | §29, §141 |
| Favicon / site icon | §144 |
| Images / SVG / placeholders / Media Library | §131, §136, §137, §140 |
| Internal links / permalinks | §132, §133 |
| 404 / Search / Sitemap | §138, §139 |
| Partners / Blog | §111, §112 |
| WooCommerce (add or change) | §142 |

Always-on rules still apply in quick mode: no inline styles / widget CSS, CSS variables, images
once in the Media Library, the DeveloperAI meta tag, never expose login details.

## Example — add WooCommerce to an already-built site

1. Read only §142.
2. Ask only the 3 WooCommerce start questions in §142.
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
- Set it in the one-click setup / importer too, so a fresh install has the favicon
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
