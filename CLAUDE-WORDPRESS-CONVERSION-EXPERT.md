# FULL HTML → PRODUCTION-READY WORDPRESS THEME
## Expert PHP / WordPress Developer Project Instructions

> **ROLE:** You are a senior PHP + WordPress theme developer.
>
> **PRIMARY TASK:** Convert the complete existing HTML website in this folder into a production-ready, maintainable, secure, responsive WordPress theme.
>
> **CRITICAL:** Do NOT start coding before understanding the existing website. First inspect the complete HTML project, then design the WordPress architecture, then convert it.
>
> **SITE ALREADY BUILT, ONLY AN UPDATE?** Use **§114 Quick Update Mode** — read only the matching section and go straight to the point.

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
├── search.php                  ← Search Results, as per design (§106)
├── searchform.php              ← the site search form, submits to search.php (§106)
├── 404.php                     ← 404 page, as per design (§106)
├── header.php
├── footer.php
│
├── single-partner.php
├── archive-partner.php
│
├── page-templates/
│   └── page-sitemap.php        ← HTML Sitemap page, dynamic (§107)
│
├── template-parts/
│   ├── header/
│   ├── footer/
│   ├── content/
│   ├── blog/
│   └── partner/
│
├── acf-json/                   ← ACF field groups as JSON, synced (§111)
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

Build the fields with **ACF PRO** (§110 — never SCF, never hand-made meta boxes), saved as local
JSON and synced (§111). Do not recreate ACF functionality manually.

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

Homepage sections are built with the page builder chosen in §108 (Flexible Content and/or ACF
Blocks). `front-page.php` loops the sections; it does not hardcode them.

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

`page-{slug}.php` only when a page needs a different wrapper. Page content always comes from the
page builder chosen in §108 — never hardcoded in a template.

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

For section classes, CSS variables, global spacing and no inline styles, see **§103**.

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

> **See also:** image accessibility, SEO-friendly names and alt text → §104; upload every image once → §105.

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

Build every form with the forms plugin the user chose at the start — Gravity Forms or Contact
Form 7 (§109).

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

Build `404.php` (and `search.php`) **as per the design**, not as a default theme page. See §106.

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

Do not convert every HTML section into Elementor widgets unless explicitly requested. Editable
sections come from the page builder chosen in §108 (ACF Flexible Content / Gutenberg ACF Blocks /
Both). If the user asks for Elementor, use `CLAUDE-ELEMENTOR-WORDPRESS-CONVERSION-EXPERT.md` instead.

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

> **See also §116:** every clickable element must be linked (link map, no `#` / empty links).

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

> **See also §116** (menus and every other link set, final link check).

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

> **See also §108:** each page-builder section has **one shared render file**, used by Flexible Content and/or the ACF Block — the template parts below are for cards and other small repeated pieces.

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

> With ACF PRO (§110), site-wide settings (phone, email, address, social links, footer text) go on an **ACF Options Page** (fields as local JSON, §111). Use the Settings API below only for a setting ACF can't hold.

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
0. Ask: ACF PRO Flexible Content / Gutenberg (ACF Blocks) / Both (§108)
   Ask: Gravity Forms or Contact Form 7 (§109)
   Ask: WooCommerce in this build? Cart/Checkout Block or Classic? (§113)
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
17. Build reusable sections + page builder (§108), then convert Homepage
18. Convert standard Pages (same sections, §108)
19. Create Partner CPT
20. Create Partner fields
21. Create Partner archive
22. Create Partner single
23. Convert Blog
24. Convert forms with the chosen plugin (§109)
25. Integrate animations
26. Test responsive behavior
27. Test URLs
28. Test SEO
29. Test PHP
30. Test JavaScript
31. Performance review
32. Build one-click setup importer (sections 96–102)
33. Final QA
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

Exception: the page-builder choice (Flexible Content / Gutenberg / Both, §108) and the forms plugin
(Gravity Forms / Contact Form 7, §109) are **always** asked first and recorded in this report.
If the site sells products, the WooCommerce questions (§113) are asked and recorded here too.

**Ask questions one at a time (MANDATORY).** Never send a list of questions in one message — the
user needs time to think about each answer, and nothing gets missed.

1. Ask **one question per message**, numbered (*"Question 2 of 7"*), so the user knows how many are
   left.
2. Give the options, mark the default / recommended one, and add one line on what the answer
   changes (e.g. *"Flexible Content — classic editor screen"*).
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
- [ ] Every button, card, logo, menu and footer link is set — no `#` / empty links (§116).

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
- [ ] `<meta name="DeveloperAI" content="Developer Skill">` is in `<head>` on every page (§112).
- [ ] Favicon made from the logo icon is set as the WordPress Site Icon and shows on every page (§115).
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

## Build lessons (§103–§107)

- [ ] Sections have appropriate section-level classes; no unnecessary inline styles (§103).
- [ ] Font sizes, colours, spacing and other repeated values use CSS variables (§103).
- [ ] Section spacing is consistent; container left/right spacing is set once globally (§103).
- [ ] Meaningful SVG icons have alternative text; decorative ones are `aria-hidden="true"` (§104).
- [ ] Every image has an SEO-friendly file name and proper alt text — none blank or default (§104).
- [ ] Images, avatars and SVGs are named after what they show — no Figma layer names (§104).
- [ ] Every image/icon is in the Media Library **once** — no `-1` / `-2` copies (§105).
- [ ] `404.php` and `search.php` match the design; search "no results" state works (§106).
- [ ] HTML Sitemap page exists, lists content automatically and is linked in the footer (§107).
- [ ] XML sitemap loads with correct URLs (§107).
- [ ] Page builder (§108) passes its own QA list: any section, any page, any order.
- [ ] Forms use the chosen plugin (Gravity Forms / Contact Form 7) and pass the §109 QA list.
- [ ] Only ACF PRO is used, SCF is not installed (§110); all field groups are in `acf-json/` and synced (§111).
- [ ] WooCommerce sites: shop design, CSS, JS and responsive pass the §113 QA list.

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
3. **Responsive:** every page at every project breakpoint, plus 320 px: no horizontal scroll, no overlap, no
   clipped or squeezed text, menus / dropdowns / mini-cart stay inside the screen, tap targets at
   least 44 px, images not wrongly cropped.
4. **Functionality:** header menu, dropdowns, mobile menu, sticky header, search, sliders, tabs,
   accordions, modals, video, pagination, filters, animations; forms (validation, submit, the email
   arrives); WooCommerce (if any): add to cart, mini-cart, cart, coupon, checkout test order, emails, My Account. Everything works by keyboard. No console errors, PHP warnings or failed requests.
5. **Content:** exact text from the design (HTML / Figma / PDF) — no lorem ipsum, no made-up copy;
   spelling, ™ / ®, phone, email and address correct. Placeholders only where they are listed for
   the client.
6. **Linking:** every clickable element opens the right page (§116); no `#`, empty, `.html`,
   staging or localhost links; no 404; external links open in a new tab.
7. **Images & icons:** all from the design export, sharp, WebP, alt text set, Media Library once (§105).
8. **SEO & `<head>`:** unique title and meta description per page, one `<h1>`, DeveloperAI meta tag
   and favicon on every page (§112, §115).
9. **Accessibility:** landmarks, heading order, alt text, form labels, visible focus, colour contrast
   measured.

- **Admin / editability:** every text, image, button and link is editable in WordPress / the page builder (§108); dynamic
  content (posts, CPTs, products, menus, settings) comes from the CMS — nothing hard-coded.
10. **Performance:** pages load fast; no unused heavy assets; images correctly sized.
11. **Browsers:** Chrome, Safari, Firefox and Edge, plus one real phone (iOS Safari) where possible.

**Rules**
- Any fail → fix it → re-check that item **and** every page that shares that part.
- Never tick a box that was not actually checked. Anything that could not be checked is written
  down as "not checked + why".
- Only when every row in `FINAL-QA.md` is ticked, report: what was checked, what the final round
  fixed, and what is still waiting on the client.

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

# 96. ONE-CLICK SETUP — TOOLS → {PROJECT} SETUP (MANDATORY)

Every ACF theme must ship with a **one-click content importer**, so a fresh WordPress install becomes the complete website with one button and no manual page building or data entry.

```text
Install theme  →  ACF PRO active (never SCF, §110)  →  Sync ACF field groups (§111)  →  Tools → {Project} Setup  →  Import
        ↓
Pages + templates, front page, posts page, posts, CPT entries,
Media Library images, ACF field values, SEO, menus — all created
```

Reference implementation: the Agency Core theme (`ag-theme/inc/importer.php` + `inc/seed/*.json`). Follow the same pattern.

---

# 97. IMPORTER FILE STRUCTURE

```text
theme-name/
├── inc/
│   ├── importer.php          ← admin screen, handler, WP-CLI command, import functions
│   ├── custom-fields.php     ← ACF JSON save/load paths + field helpers (§111)
│   └── seed/
│       ├── pages.json        ← pages: slug, title, template, status, SEO, optional content
│       ├── posts.json        ← blog posts: slug, title, date, excerpt, content, image, alt, SEO
│       ├── {cpt}.json        ← one file per CPT (partners.json, webinars.json …) incl. ACF field values
│       └── menus.json        ← menus: name, location, nested items
└── assets/images/            ← every image the seed references (imported into the Media Library)
```

- Load `inc/importer.php` from `functions.php` like every other `inc/` file.
- Use the theme prefix for every function, option, nonce, transient and meta key (`{prefix}_import_*`).

---

# 98. SEED DATA — EXTRACTED FROM THE HTML, NEVER RETYPED

- Build the `inc/seed/*.json` files by **parsing the finished HTML** (and its source data, if any). Never retype or rewrite content.
- Content must be identical to the HTML: titles, copy, headings, links, dates, image file names and alt text.
- Each page entry: `slug`, `title`, `template` (path in `page-templates/` or empty for default), `status`, `seo_title`, `seo_description`, optional `content`, plus `builder` and `sections` (§108).
- Each post / CPT entry: `slug`, `title`, `date`, `excerpt`, `content`, `image`, `image_alt`, SEO, and **every ACF field value keyed by the field `name`** (repeaters as arrays of rows).
- Menus: `name`, `location`, `items` as `[ title, target, extra, children ]`, where target is `page:slug`, `{cpt}:slug`, `archive:{cpt}`, `blog`, or a URL.
- Images referenced by file name only; the files live in `assets/images/`. File names and alt text
  follow §104, and each image appears in the seed's Media Library list once (§105).
- `pages.json` includes the **Sitemap** page (slug `sitemap`, template `page-templates/page-sitemap.php`, §107).
- Never put credentials, private URLs, form API keys or personal data in seed files.

---

# 99. ADMIN SCREEN — TOOLS → {PROJECT} SETUP

Register with `add_management_page()` under **Tools**, capability `manage_options`.

The screen must show:

1. A short description of what is imported.
2. A **status table**: for each content type, `count in seed / count on site`.
3. An **"Update existing content to match the seed"** checkbox (off by default), with a warning that edits made on the site to those items will be lost.
4. One **Import {Project} content** button.
5. A **live progress area** while the import runs: progress bar + log line per step (e.g. `Pages 4/12 — Home created, set as front page`).
6. After the run, a **report**: per content type `X created, Y updated, Z existing kept`, plus front page / posts page / menus status.
7. A short "after importing" note listing the manual steps that remain (site title, form shortcodes, etc.).

**Step-by-step processing — never one long request (MANDATORY)**

*Noted during a real build: a single-request import timed out ("first pass is still creating pages")
and the front page was left unset. One click must still do everything, but the work is split into
small steps so no request comes near the PHP / server timeout.*

- The button starts a JS runner. It calls `admin-ajax.php` (custom action) **once per step** and
  only sends the next request after the previous one has returned.
- Each request does **one small batch** and returns quickly: e.g. 1–5 pages, 1–5 posts / CPT
  entries, 1–3 images, one menu. Never import a whole content type, or all images, in one request.
- Step queue, in the §100 order: `pages` → `posts` → `{cpt}` (one queue per CPT) → `menus` →
  `finalize` (settings check, `flush_rewrite_rules()`, final report).
- The server returns JSON: `{ step, offset, done, total, log[], next }`. The runner updates the
  progress bar and log, then requests `next`.
- Progress and the running report live in a per-user option / transient (`{prefix}_import_state`),
  so a failed or closed tab can **resume** from the last finished step instead of starting over.
- A failed request is retried (max 3) and then shown in the log with a **Resume** button. One
  failed item never stops the rest of the import.
- Image processing (`wp_generate_attachment_metadata`) is the slowest part — keep image batches the
  smallest, or generate an item's images in its own step.

Handler rules (every AJAX step):

- `current_user_can( 'manage_options' )` + `check_ajax_referer()` nonce on **every** request.
- Sanitize `step` against a whitelist and `offset` with `absint()`.
- Return with `wp_send_json_success()` / `wp_send_json_error()`; never echo HTML.
- Escape every output on the screen (`esc_html`, `esc_url`, `esc_attr`).

Also register a WP-CLI command: `wp {prefix} import [--overwrite]`. It runs the **same step
functions** in a loop (CLI has no web timeout), printing one line per step.

---

# 100. IMPORT ORDER

```text
1. Pages           → create (Home first), set _wp_page_template, SEO;
                     set front page the moment Home exists, posts page the moment Blog exists
2. Posts           → create, category, date, featured image, SEO
3. CPT entries     → create, featured image, ACF fields (incl. repeaters + images), SEO
4. Menus           → build from menus.json, assign to theme locations
5. Finalize        → re-check front page / posts page / menu locations, flush_rewrite_rules()
```

Each line is split into small AJAX steps (§99). Pages must exist before menus, because menu items link to them by slug.

**Front page — set it as soon as the Home page exists**

- Put the Home page **first** in `pages.json` (and the Blog / posts page early), so they are created in the first batch.
- Right after Home is created (or found), set `show_on_front = page` + `page_on_front` in the **same step**. Do the same for `page_for_posts` when the Blog page is created.
- Never wait until all pages exist. If the import stops halfway, the front page must already be set.
- The `finalize` step only re-checks it (fills it if still empty) — it is a safety net, not the place where it is first set.

---

# 101. IMPORTER RULES

**Safe to run again (idempotent)**

- Find existing items by **slug + post type**, checking all statuses (`publish`, `private`, `draft`, `pending`, `future`) — `'any'` skips private items and creates duplicates.
- Default mode: existing items are **kept**, never overwritten. Only missing items are created.
- Overwrite mode (checkbox / `--overwrite`): existing items are updated to match the seed; menus are emptied and rebuilt; front page / posts page are reset.
- Front page, posts page, menu locations and other settings are only filled **where empty**, unless overwrite mode is on.

**Images**

- Copy each file from `assets/images/` into the Media Library **once** (`wp_upload_bits` + `wp_insert_attachment` + `wp_generate_attachment_metadata`).
- Tag the attachment with a source meta key (`_{prefix}_source` = file name) and reuse it on every later run.
- Set the alt text from the seed.

**ACF fields**

- Write values using the **same field definitions** as the `acf-json/` files (§111) — never a second copy of the field list.
- Store `{name}` = value and `_{name}` = field key, so ACF recognises the value.
- Repeaters: store the row count in `{name}` and each sub-field as `{name}_{i}_{sub}`.
- Image fields: convert the file name to an attachment ID through the image importer.

**SEO**

- Fill the theme's own SEO fields, and Yoast / Rank Math meta when either plugin is active.

**Robustness**

- Avoid timeouts by **small step batches (§99)**, not by one long request. `set_time_limit()` may be
  added per step as a safety margin, but it is not the fix (many hosts ignore it).
- `require_once wp-admin/includes/image.php`.
- `wp_slash()` data passed to `wp_insert_post` / `update_post_meta`.
- Check `is_wp_error()` on every insert; skip and report failures instead of stopping.
- Never delete content that is not in the seed.

---

# 102. IMPORTER QA

Before delivery, test on a **fresh** WordPress install and on staging:

- [ ] Fresh install → Import creates every page, post, CPT entry, image and menu; site matches the HTML.
- [ ] Second run (default mode) → nothing duplicated; report shows everything "kept".
- [ ] Overwrite run → items updated, menus rebuilt, no duplicates.
- [ ] Every ACF field (incl. repeaters and images) shows its value in the edit screen.
- [ ] Every page shows its sections in the original order, in Flexible Content and/or as ACF Blocks (§108).
- [ ] Front page, posts page and all menu locations are set.
- [ ] Import runs step by step via AJAX with a live progress log; no request times out, even on a slow host with all images.
- [ ] Front page is set right after the Home page is created (check it while the import is still on Pages).
- [ ] Closing the tab mid-import and clicking Import / Resume continues from the last step — no duplicates.
- [ ] Images appear once in the Media Library, with alt text.
- [ ] `wp {prefix} import` works from WP-CLI.
- [ ] No PHP warnings or notices during import.

Add the importer to the README **Install** steps:

```text
1. Copy theme to wp-content/themes/ and activate it.
2. Make sure ACF PRO is active (never install SCF, §110), then ACF → Field Groups → Sync (§111).
3. Tools → {Project} Setup → Import {Project} content.
4. Settings → Permalinks → Post name.
5. Remaining manual steps (site title, form shortcodes …).
```

---

# 103. CSS & GLOBAL STRUCTURE

*Noted during real builds. §20 still applies: preserve the original look and do not rewrite CSS for
its own sake. These rules decide **how** the CSS is organised while the output stays identical.*

**Section-level classes, wherever required**
- Every template section gets an appropriate section-level class (e.g. `about-section`,
  `partners-section`). Keep the original HTML class if it already has one.
- Shared components get a reusable class (e.g. `card`, `logo-grid`).
- Name classes by role. Never name them after a page or a colour.

**No unnecessary inline styles**
- Styling goes in the theme CSS (`assets/css/`), managed through reusable classes. No `style=""` in
  PHP templates.
- If the original HTML has inline styles, move them into classes. The visual result must not change.
- Only exception: a **dynamic value from the admin** (e.g. an ACF background image or colour) may be
  passed as a CSS variable, nothing else: `style="--section-bg: url(<?php echo esc_url( $bg ); ?>)"`.
  The CSS rule itself still lives in the stylesheet.

**CSS variables for common values**
- Font sizes, colours, spacing, radius, shadows and other repeated values live in one `:root` block
  at the top of the main stylesheet. Reuse the original HTML's variables if it has them.
- Section CSS uses `var(--…)`, not the same raw value repeated in many rules.

**Consistent spacing**
- Section top/bottom spacing is one variable (e.g. `--section-space`) applied the same way to every
  section.
- Container left/right spacing (gutter) is set **once** globally on the container class through
  CSS. Never re-declare it in each section.

**Clean, scalable, reusable**
- One comment banner per section or component.
- No duplicate rules, no dead CSS. Avoid `!important`.
- Responsive rules sit next to the section they belong to, using the original breakpoints.

---

# 104. SVG & IMAGE ACCESSIBILITY + NAMING

*Noted during real builds — image names and accessibility attributes were left incomplete before.*

### SVG icons

- **Meaningful SVG** (it carries information or is the only content of a link/button):
  - Inline SVG in a template: `role="img"` + `aria-label="<?php esc_attr_e( 'Phone', 'textdomain' ); ?>"`
    (or a `<title>` inside it).
  - Icon-only link/button: put the `aria-label` on the link/button and `aria-hidden="true"`
    `focusable="false"` on the SVG.
  - SVG `<img>`: proper `alt`.
- **Decorative SVG** (the text next to it already says the same thing): `aria-hidden="true"`, and
  no alt text.
- Never leave an icon-only control without an accessible name.

### Images — every image gets a proper name and alt text

- **Rename every image, avatar and SVG by what it actually shows.** Open or view the file first.
  - Use lowercase, hyphens, 2–5 meaningful words.
  - Never keep Figma or export names (`Ellipse 3`, `Group 1000004521`, `Frame 12`, `IMG_1234`,
    `image (1)`).
  - Rename **before** the file is referenced in the templates or the seed, and update every
    reference.

| File type | Pattern | Example | Alt text |
|---|---|---|---|
| Content image | `<what-it-shows>.webp` | `doctor-consulting-patient.webp` | `Doctor consulting a patient at a clinic desk` |
| Hero image | `<page>-hero-<subject>.webp` | `about-hero-team-workshop.webp` | describe the scene |
| Avatar — real person | `<first>-<last>-avatar.webp` | `priya-shah-avatar.webp` | `Priya Shah, Marketing Director at Acme` |
| Avatar — dummy | `<context>-avatar-<n>.webp` | `testimonial-avatar-01.webp` | `Testimonial author photo` (never invent a name) |
| Logo | `<brand>-logo.svg` | `acme-logo.svg` | `Acme` |
| SVG icon | `icon-<what-it-shows>.svg` | `icon-phone.svg`, `icon-arrow-right.svg` | its meaning, or empty if decorative |
| Decorative background | `bg-<what-it-is>.svg/webp` | `bg-wave-pattern.svg` | `alt=""` |

- **Alt text:** describe what is in the image and why it is there.
  - Keep it under ~125 characters. Don't start with "image of", and don't stuff keywords.
  - A purely decorative image gets `alt=""`. The `alt` attribute is never missing.
- **Where the alt comes from:**
  - Media Library images: use `wp_get_attachment_image()` / `the_post_thumbnail()` so the library's
    Alt Text is printed.
  - Set Alt Text and Title on every attachment (the importer sets it from the seed, §101).
  - Static theme images: write the alt in the template with `esc_attr()`.
- Do not leave image names or accessibility attributes incomplete. A build with default file names
  or blank alt text is not done.

---

# 105. MEDIA LIBRARY — UPLOAD EVERY IMAGE / ICON ONLY ONCE

*Noted during real builds — the same images and icons were uploaded to the Media Library twice.*

**Rule: one file = one Media Library item.** The same image used in five places is uploaded once
and reused by its attachment ID.

**Decide each file's home — theme or Media Library, never both:**
- **Fixed UI files** that admins never change stay in the theme's `assets/images/` and are **not**
  uploaded to the Media Library. These are UI icons, arrows, decorative shapes and the SVG sprite.
- **Content images** that admins can change go into the Media Library (through the importer). These
  are page images, post/CPT images, ACF image fields, partner logos and avatars.

**Before every upload (manual, script, WP-CLI or importer):**
1. Look the file up first: by the source meta key (`_{prefix}_source`, §101), by file name or by
   title.
2. Found → reuse that attachment ID. Not found → upload it once with its §104 name, alt and title.
3. Re-running the importer or a build step must not upload anything that already exists.

**Signs of a duplicate:**
- The same name with a WordPress suffix: `acme-logo-1.webp`, `acme-logo-2.webp`.
- Two items with the same title or the same thumbnail.

**Clean-up:**
1. Find every post, page, ACF field and option that uses the copy.
2. Re-point each one to the original attachment.
3. Only then delete the copy.

Never delete an attachment that is still in use.

---

# 106. ADDITIONAL PAGES — 404 AND SEARCH

*Noted during real builds — these two pages were missed before. They are part of every build.*

### 404 Page — `404.php`

- Build it **as per the design**: heading, message, button back to the homepage
  (`home_url( '/' )`), and the search form if the design has one.
- Use the normal `get_header()` / `get_footer()`, global classes, variables and buttons.
- Test it by opening a URL that does not exist. The response must be a real 404 status.

### Search Page — `search.php` + `searchform.php`

- Build `search.php` **as per the design**:
  - A heading with the search term (`get_search_query()`, escaped).
  - The results loop: title, excerpt/meta, and image if the design shows one.
  - Pagination (`the_posts_pagination()`).
  - A clear **"no results"** state, with the search form again.
- `searchform.php` holds the site's search form. It uses `role="search"`, a labelled input with
  `name="s"`, and `action="<?php echo esc_url( home_url( '/' ) ); ?>"`. The header/site search uses
  it via `get_search_form()`.
- Decide which post types search includes (Posts, Pages, Partners), using `pre_get_posts` on the
  main query only.
- Test it with a term that returns results, a term that returns nothing, and a term containing
  special characters.

If the design has no 404 or Search layout, build a simple one from the global design system, and
say so in the handover report.

---

# 107. SITEMAP PAGE

*Noted during real builds — the sitemap page was missed before. It is part of every build.*

### HTML Sitemap page (for visitors) — `page-templates/page-sitemap.php`

- A page template named "Sitemap", assigned to a Page with slug `sitemap`. The importer creates it
  (§98).
- The list must be **dynamic**. Never hardcode links. New pages and posts appear automatically.
  - Pages: `wp_list_pages( [ 'title_li' => '', 'exclude' => … ] )`.
  - Posts and the Partner CPT: `WP_Query` / `get_posts()`, grouped under headings.
  - Categories, if the design uses them.
- Leave out Thank You, 404, private/draft and utility pages.
- Style it with the global section classes and variables (§103).
- Link it in the footer (usually next to Privacy / Terms) through the WordPress footer menu.

### XML Sitemap (for search engines)

- If Yoast or Rank Math is active, use **its** XML sitemap and do not add another one.
- If neither is active, WordPress core's `/wp-sitemap.xml` is enough. Don't add a sitemap plugin, and
  don't build a custom XML sitemap.
- Check that Pages, Posts and the Partner CPT are included, and that Thank You / utility pages are
  excluded. Use the `wp_sitemaps_*` filters, or the SEO plugin's settings.
- Open the sitemap URL after the build and confirm it loads with the correct (Post name) URLs.

---

# 108. PAGE BUILDER — ACF FLEXIBLE CONTENT / GUTENBERG (ACF BLOCKS) / BOTH

*Added after a real build: pages were hardcoded in `front-page.php` / `page-{slug}.php`, so editors
could not add, remove or reorder sections, and the theme did not work in Gutenberg. Every page
section must now be reusable: **any section can go on any page, in any order.***

## Step 1 — Ask before building (MANDATORY)

This is the one question that must **always** be asked at the start of the project, even when
§65 says not to ask. Ask it before planning the architecture (§64 step 0). Never assume the answer.

```text
How should editors build the pages?
  1. ACF PRO — Flexible Content   (page sections as Flexible Content layouts, classic editor screen)
  2. Gutenberg — ACF Blocks       (every section is a Gutenberg block)
  3. Both                         (same sections available as Flexible Content layouts AND blocks)
```

Write the answer into the architecture report (§65) and build only what was chosen.
All three options need **ACF PRO** — free ACF has no Flexible Content and no ACF Blocks. Never
install SCF (§110).

## Step 2 — Rules for every option

- **One HTML section = one reusable section.** Go through the site inventory (§3) and list every
  page section (hero, logo grid, CTA, testimonials, FAQ, …). The same section on several pages is
  **one** section, not a copy per page.
- **One render file per section:** `template-parts/sections/{section-name}.php`. It receives its
  values through `$args` and outputs the original HTML with the original classes (§50, §69, §103).
  Flexible Content and ACF Blocks both call this same file — never two copies of the markup.
- **One field group per section,** saved as JSON in `acf-json/` and synced (§111, single source of truth).
  The Flexible Content layout and the ACF Block field group both reuse it through a Clone field.
- Name sections by role (`hero`, `logo_grid`, `cta_banner`), never by page (`home_hero`).
- Each section gets an optional **Section ID** field (anchor), so in-page links from the HTML
  (`#contact`) keep working.
- Every field is escaped on output and an empty field never prints empty markup (§23).
- **Not part of the page builder:** header and footer (§67, `header.php` / `footer.php`), blog post
  content (normal Gutenberg content, §14), and CPT single templates (§46) — these keep their own
  templates.
- `front-page.php` and `page.php` only loop the sections. They no longer hardcode page sections
  (this replaces the hardcoded approach in §17 and §18 for page content).

```text
theme-name/
├── template-parts/sections/   ← one PHP render file per section (shared)
├── blocks/{section-name}/     ← block.json per section (options 2 and 3 only)
├── theme.json                 ← options 2 and 3 only
└── inc/
    ├── custom-fields.php      ← ACF JSON paths (field groups live in acf-json/, §111)
    └── page-builder.php       ← section loop, block registration, editor settings
```

## Option 1 — ACF PRO Flexible Content

- One Flexible Content field `page_sections` on Pages (and the front page), one layout per section.
- `front-page.php` / `page.php`: `while ( have_rows( 'page_sections' ) ) : the_row();` →
  `get_template_part( 'template-parts/sections/' . get_row_layout(), null, $args );`
- Give each layout a clear label and a collapsed title (e.g. the heading text) so long pages stay
  easy to edit.
- Hide the block editor for Pages that use the builder (`use_block_editor_for_post` filter), so
  editors see only the section builder.

## Option 2 — Gutenberg with ACF Blocks

- Every section is an ACF Block: `blocks/{section-name}/block.json` with `"acf": { "mode": "preview",
  "renderTemplate": … }`, registered with `register_block_type()`. The render template calls the
  shared section file.
- Put the theme blocks in their own block category (`{Project} Sections`).
- Limit the inserter with `allowed_block_types_all` to the theme blocks plus the core blocks the
  design really needs (paragraph, heading, list, image, buttons).
- **`theme.json`:** colour palette, font sizes and spacing presets come from the same values as the
  `:root` variables (§103). Turn off custom colours / font sizes so editors stay inside the design.
- **Editor must look like the frontend:** `add_theme_support( 'editor-styles' )` +
  `add_editor_style()` with the theme CSS; the block preview loads the scripts the section needs.
- Core block CSS (`wp-block-library`) must not change the converted design. Check it, and dequeue
  what the frontend does not use.
- Make one **block pattern** per original page layout, so an editor can rebuild any HTML page with
  one click.

## Option 3 — Both

- Same section files and same field definitions, exposed **twice**: as Flexible Content layouts
  and as ACF Blocks.
- **Per page, only one system is active**:
  - Page template **"Flexible Sections"** (`page-templates/page-flexible.php`) → Flexible Content;
    the block editor is turned off for that template.
  - Default page template → Gutenberg with ACF Blocks (`the_content()`).
- Ask the user which system the importer uses for the seeded pages (default: Flexible Content).

## Importer (§96–§102)

- `pages.json`: each page gets `"builder": "flexible" | "blocks"` and a `sections` array in the page
  order: `[ { "layout": "hero", "fields": { … } }, … ]`. Values are parsed from the HTML (§98).
- **Flexible Content:** write the rows with `update_field( $field_key, $rows, $post_id )`, each row
  carrying `acf_fc_layout`. This stores the layout order and `_{name}` field keys the way ACF
  expects. Image fields → attachment IDs (§101, §105).
- **ACF Blocks:** build `post_content` with `serialize_block()` — one
  `<!-- wp:acf/{section-name} {"name":"acf/{section-name}","data":{…},"mode":"preview"} /-->` per
  section, where `data` holds `{field_name}` + `_{field_name}` (field key).
- The importer uses the same field keys as the `acf-json/` files (§111) — never a second list.

## QA

- [ ] The editor choice (Flexible / Gutenberg / Both) was asked and recorded before building.
- [ ] Every section can be added to any page, in any order, and still matches the HTML.
- [ ] The same section added twice on one page works (no duplicate IDs, JS still initialises).
- [ ] Removing or reordering sections does not break layout, spacing or animations.
- [ ] Each section has one render file, used by both Flexible Content and ACF Blocks.
- [ ] Gutenberg: editor preview matches the frontend; only allowed blocks appear; `theme.json`
      presets match the design.
- [ ] Importer recreates every page with its sections in the original order, using the chosen builder.

---

# 109. FORMS PLUGIN — GRAVITY FORMS OR CONTACT FORM 7

*Added after a real build: the forms plugin was guessed. The choice must come from the user.*

## Ask before building (MANDATORY)

Ask this right after the page-builder question (§108), at the very start of the project — as its own
question, one question per message (§65). Never
assume the answer, even if one plugin looks like the obvious choice.

```text
Which forms plugin should the site use?
  1. Gravity Forms
  2. Contact Form 7
```

- Write the answer into the architecture report (§65) and use **only** that plugin for every form on
  the site. Never mix two form plugins.
- If the existing WordPress site already uses a forms plugin (§84), say so in the question, so the
  user can decide whether to keep it.
- If the HTML form posts to a CRM / API / third-party service, keep that integration (§28) and
  connect it through the chosen plugin (Gravity Forms add-on / webhook, or a CF7 integration).

## Rules for both plugins

- Every form in the HTML (contact, newsletter, demo request, …) becomes one form in the chosen
  plugin, with the same fields, labels, placeholders, required fields, order and button text.
- **The form must look like the HTML form.** Use the original classes and styles from the theme CSS
  (§103). No default plugin look.
- Place the form through a field, not hardcoded in a template: a **Form** field in the section
  (§108) or in Theme Settings holds the form ID / shortcode.
- Success message, error messages and redirect (e.g. Thank You page) match the HTML site.
- Spam protection on every form (Gravity Forms honeypot / reCAPTCHA / Turnstile; CF7 reCAPTCHA /
  Turnstile / Akismet).
- Email notification goes to the admin email from Settings, never a hardcoded address.
- No API keys or credentials in the theme or seed files (§98).

## Gravity Forms

- Output with `gravity_form( $id, false, false, false, null, true )` or the `[gravityform]`
  shortcode (AJAX on).
- Turn off the default Gravity Forms theme CSS for these forms (form theme "None" / no-conflict
  styles) and style with the theme classes. Add the original classes through the field's
  **Custom CSS Class** setting.

## Contact Form 7

- Output with `[contact-form-7 id="…"]`.
- Write the form template in CF7 with the original HTML wrapper and classes, so the theme CSS
  applies directly.
- Load CF7 CSS/JS only on pages that have a form (`wpcf7_load_css` / `wpcf7_load_js` filters +
  enqueue where needed, §40).

## QA

- [ ] The forms plugin (Gravity Forms / Contact Form 7) was asked and recorded before building.
- [ ] Only the chosen plugin is used; every HTML form exists in it.
- [ ] Every form looks like the HTML form on mobile, tablet and desktop.
- [ ] Submissions arrive by email; success / error messages and redirects work.
- [ ] Spam protection is on; no console errors on pages with forms.

---

# 110. ACF PRO ONLY, NEVER INSTALL SCF + SITE LOGIN (login.md)

*Added after a real build: the site already runs **ACF PRO**. Installing Secure Custom Fields (SCF)
next to it causes plugin conflicts and duplicate field systems.*

## ACF PRO only (STRICT)

- **Never install, activate or require Secure Custom Fields (SCF)** — not on the user's site, not on a
  local server or staging, not in the README. Not even "as a fallback".
- The theme is built for **ACF PRO**, which is already installed on the site. Check that it is active
  before building; if it is not active, stop and tell the user — do not install anything in its place.
- Never install any other plugin on the site without asking the user first.

## Site login — `login.md`

- **`login.md`** in the project folder holds the details of the **user's own WordPress site** (site
  URL, admin URL, login). It does **not** hold local server details.
- Use it to log in to the user's site admin. With that login you may: upload / update the theme,
  **activate (change) the theme**, run the importer (§96–§102), and sync ACF field groups (§111).
- Before changing the theme on the site, keep a backup of the current theme (§75).
- Local server details are not in `login.md`. If a local server is needed and its details are not
  known, ask the user — never use the `login.md` details for anything other than the user's site.
- Never print, copy or commit the credentials from `login.md` — not in code, seed files, logs,
  reports or chat replies. Add `login.md` to `.gitignore`.
- If `login.md` is missing or the login fails, stop and ask the user. Never guess passwords or
  create new admin users.

---

# 111. ACF FIELDS — LOCAL JSON + SYNC (MANDATORY)

*Added after a real build: field groups existed only in PHP, so they could not be edited or synced
in the admin. Every ACF field group must also be saved as a JSON file and synced.*

## Local JSON

- The theme has an **`acf-json/`** folder. Every field group is saved there as one JSON file
  (`group_{key}.json`) — Partner fields, page sections (§108), Theme Settings, blocks, everything.
- Point ACF to that folder in `inc/custom-fields.php`:
  - `acf/settings/save_json` → `get_stylesheet_directory() . '/acf-json'`
  - `acf/settings/load_json` → add the same path.
- The `acf-json/` files are the **single source of truth** for fields. Do not also register the same
  groups in PHP with `acf_add_local_field_group()` — that creates a second copy that cannot be
  synced or edited in the admin.
- Field keys are fixed and unique (`field_{prefix}_{name}`, `group_{prefix}_{name}`) and never change
  after the first build — the importer and saved content depend on them.
- Section fields for §108: one field group per section in `acf-json/`. The Flexible Content layout
  and the ACF Block field group reuse it through an ACF PRO **Clone** field (seamless), so each
  section's fields exist once.

## Sync

- After the field groups are created, **sync them into the database**: WP Admin → ACF → Field
  Groups → **Sync available** → Sync all (log in with `login.md`, §110). Then every group is
  visible and editable in the admin.
- After any change to a field group in the admin, ACF writes the change back to `acf-json/`
  automatically. Keep those updated JSON files in the theme.
- After the theme is updated on another site, run Sync again there.
- The importer (§101) reads field keys from the same `acf-json/` files — never a second list.

## QA

- [ ] Every field group has a JSON file in `acf-json/`; no field group exists only in PHP or only in
      the database.
- [ ] ACF → Field Groups shows **no "Sync available"** items after the sync.
- [ ] Editing a field group in the admin updates its JSON file in the theme.
- [ ] Only ACF PRO is installed — SCF is not installed on the site (§110).

---

# 112. DEVELOPERAI META TAG IN `<head>` — EVERY PAGE (MANDATORY)

*Default on every build.* Every front-end page of the site — pages, posts, archives, CPT pages, 404,
Search, Sitemap — outputs this hidden tag in `<head>`:

```html
<meta name="DeveloperAI" content="Developer Skill">
```

- Copy it exactly: `name="DeveloperAI"`, `content="Developer Skill"`. It is a hidden meta tag; it is
  never shown on the page.
- Add it once, through the `wp_head` hook, in the theme's `inc/template-functions.php` (or the child theme's `functions.php` in Update / Extend mode, §73):

```php
function {prefix}_developer_meta() {
    echo '<meta name="DeveloperAI" content="Developer Skill">' . "\n";
}
add_action( 'wp_head', '{prefix}_developer_meta', 1 );
```

- Do not hard-code it in each template, and do not add it through an SEO plugin
  or a page-level field. The hook covers every template (page, single, archive, 404, search).
- It must appear only once per page (check the page source).

## QA

- [ ] View source on Home, an inner page, a post, 404 and Search: `<meta name="DeveloperAI" content="Developer Skill">` is in
      `<head>`, exactly once.

---

# 113. WOOCOMMERCE — SHOP PAGES, THEME-LEVEL CSS / JS + RESPONSIVE

*Use this section whenever the site sells products: the HTML has shop pages, or the user asks for
WooCommerce. All the other rules in this file still apply (§103 CSS, §112 DeveloperAI meta tag, §104 images, §105 Media Library once, §70 QA).*

**Site already built?** Add WooCommerce through Quick Update Mode (§114): follow the steps below and
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
├── woocommerce/               ← template overrides, ONLY when a hook can't do it
├── inc/woocommerce.php        ← theme support, hooks, image sizes, cart fragments
└── assets/
    ├── css/woocommerce.css    ← all shop styling + its responsive rules (step 7)
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

## 5. Templates (custom theme)

- Shop and category pages use `woocommerce/archive-product.php` only if the design needs a different
  wrapper; the product card is `woocommerce/content-product.php`. Otherwise use the hooks.
- Wrap WooCommerce output in the theme's own container/section markup with
  `woocommerce_before_main_content` / `woocommerce_after_main_content`, so header, footer, container
  and section spacing (§103) are the same as every other page.
- Sections around the shop (banner, CTA, USP strip) come from the page builder chosen in §108.
- WooCommerce blocks (Product Collection, Cart, Checkout) must look like the frontend in the editor
  too (`editor-styles`).

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
- **Never static:** no product name, price, sale price, image, rating, stock badge, link or
  "Add to cart" button typed into HTML, a page builder field, a text widget or an ACF text field.
  A static card goes stale the moment the price or stock changes and its button doesn't add to cart.
- **How to pull them (custom theme):** `wc_get_products()` / `WP_Query` (`post_type => product`),
  the `[products]` shortcode, or the WooCommerce **Product Collection** block. Render each card with
  the one shared product card (`woocommerce/content-product.php` or the same template part) — the
  same card as the shop, never a second hand-made card.
- **Admin chooses, code renders:** the section's selection is a setting, not copied content —
  an ACF field (Relationship / Post Object on `product`, or Taxonomy on `product_cat`), or a query
  rule: category, tag, featured, on sale, best sellers, newest, manual IDs. Plus how many to show
  and the order.
- Name, price (`get_price_html()`), image, link (`get_permalink()`), rating, sale / stock badges and
  the add-to-cart button (`woocommerce_template_loop_add_to_cart()`) all come from the product
  object, so simple and variable products, sale prices and out-of-stock all behave correctly.
- **Figma / HTML shows product cards?** Create those products in WooCommerce (importer, CRUD —
  step 6) and point the section at them. The design's cards are the *look* of the card, not its data.
- Empty state: if the query returns no products, hide the section (or show a short message) —
  never fall back to static placeholder products.

## 7. Theme-level CSS — design + responsive

Write all WooCommerce styling in `assets/css/woocommerce.css`, enqueued after the main stylesheet only on `is_woocommerce() || is_cart() || is_checkout() || is_account_page()`. Header mini-cart styles stay with the header CSS, because the header is on every page.

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

# 114. QUICK UPDATE MODE — SITE ALREADY BUILT, GO STRAIGHT TO THE POINT

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
3. **Do not ask again** what the earlier build already decided (page builder §108, forms plugin §109, ACF setup §110/§111). Read it from the
   theme / project docs. Ask only if this change needs a **new** decision (e.g. WooCommerce Block or
   Classic, if WooCommerce was never set up).
4. **Inspect only what the change touches:** the files / templates / widgets for that feature and
   the pages that use them.
5. **Back up only those files** before editing (git commit, or a dated copy) — the light version of
   §75. Log in with `login.md` if WP Admin is needed.
6. **Make the change** following that section's rules. Reuse the existing classes, variables,
   components and file structure — no parallel structure, no duplicate CSS/JS.
7. **Check only what it affects:** that section's own QA list + the pages that use the changed part,
   at desktop, tablet and mobile. Confirm nothing else broke there.
8. **Report briefly:** what changed, which files, what was checked, anything still open.

## Topic → section index

| Change asked for | Read only |
|---|---|
| Header / menu | §6, §7, §44, §67, §78, §80 |
| Footer | §8, §79 |
| CSS / design / spacing / colours | §20, §103 |
| JavaScript / sliders / animations | §21, §22, §51 |
| Responsive | §27, §103 |
| New page or section | §16, §17, §108 |
| Forms | §28, §109 |
| SEO / meta | §29, §112 |
| Favicon / site icon | §115 |
| Links / buttons / cards not linked | §116, §43 |
| Images / SVG / Media Library | §26, §104, §105 |
| 404 / Search / Sitemap | §106, §107 |
| ACF fields | §110, §111 |
| Setup / importer | §96–§102 |
| Partners / Blog | §11–§15, §45–§48 |
| WooCommerce (add or change) | §113 |

Always-on rules still apply in quick mode: no inline styles / widget CSS, CSS variables, images
once in the Media Library, the DeveloperAI meta tag, never expose login details.

## Example — add WooCommerce to an already-built site

1. Read only §113.
2. Ask only the 3 WooCommerce start questions in §113 — one at a time (§65).
3. Go through §113 step by step and **skip any step that already exists** (e.g. theme support
   already added, pages already set).
4. Reuse the site's existing variables, buttons, forms, header and breakpoints for the shop CSS/JS.
5. Run the §113 QA list and check the header (mini-cart) on a few non-shop pages.

## QA

- [ ] Only the asked change was made; nothing outside its scope changed.
- [ ] That section's own QA list passes.
- [ ] Pages using the changed part look and work the same as before, apart from the change.

---

# 115. FAVICON — MADE FROM THE LOGO ICON (MANDATORY)

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
  (the brand colour CSS variable) so it is visible on a light browser tab.
- Check it at **16 × 16 and 32 × 32**. If the mark is too detailed to read that small, use the
  simplest recognisable part of it.

## Add it to WordPress

- Use WordPress's own **Site Icon** (Appearance → Customize → Site Identity → Site Icon).
  WordPress creates the 32, 180 (Apple touch), 192 and 270 px sizes and prints all the
  `<link rel="icon">` / `apple-touch-icon` tags itself through `wp_site_icon()` in `wp_head`.
- Upload `site-icon.png` to the Media Library **once** (§105).
- Set it in the one-click setup / importer too, so a fresh install has the favicon
  (§96–§102):

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

# 116. LINKING — EVERY CLICKABLE ELEMENT IS LINKED (MANDATORY)

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

## How to set each link (theme code / page builder)

- Internal page / post → `get_permalink( $id )` (page found by slug, §43), never a
  typed URL or `.html` link. Home → `home_url( '/' )`.
- Product / product category → `get_permalink( $product_id )` / `get_term_link( $term )`.
- Archive / category / tag → `get_post_type_archive_link()`, `get_term_link()`.
- Menus → WordPress Menus (`wp_nav_menu()`, header / footer sections) — every menu item points to a real page.
- Editable buttons / CTAs → ACF **Link** field (returns url, title, target), so the admin can change
  it — never a plain text field for the URL. Print with `esc_url()`; add `target="_blank"
  rel="noopener"` only when the field's target is `_blank`.
- Whole card clickable → wrap the card (or use a stretched link) so the full card opens the post /
  product, not only the small "Read more" text.
- Phone → `tel:`, email → `mailto:`, address → Google Maps link — from the ACF options page, not
  typed again in each template.
- Social icons → the real profile URLs from the design / client, from the ACF options page, opened in
  a new tab with `rel="noopener"`.

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
