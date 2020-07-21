---
layout: default
title: Navigation Structure
nav_order: 5
---

# Navigation Structure
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Main navigation

The main navigation for your Just the Docs site is at the left side of the page on large screens, and at the top (behind a tap) on small screens. It can be structured to accommodate a multi-level menu of unlimited depth: pages can always have child pages.

By default, links to all pages appear in the main navigation at the top level, ordered alphabetically by page title. By adding fields to the YAML front matter of individual pages, the [order](#ordering-pages) of the links can be changed, links can be [excluded](#excluding-pages), and links can be displayed as [child pages](#pages-with-children).

For the construction of the navigation display to work (and to avoid potential confusion when browsing) page titles need to satisfy the following requirements:

* The titles of the top-level pages must be different.
* The titles of child pages with the same parent page must be different.
* The title of a child page must be different from the titles of all its ancestors (its parent page, any grandparent page, etc.).

Pages can have the same title whenever they have different parent pages. Moreover, parent pages with the same title can be distinguished by specifying _their_ parent pages, or their more distant ancestors. This allows different _sections_ of the navigation to use the same titles: the title of the top page of each section can be used as the distinguishing ancestor.

Additional navigation features include [auxiliary links](#auxiliary-links) at the top of the page, and in-page navigation with an automatically-generated [table of contents](#in-page-navigation-with-table-of-contents).

---

## Ordering pages

To specify a page order, use the `nav_order` field in the YAML front matter of your pages.

#### Example
{: .no_toc }

```yaml
---
layout: default
title: Customization
nav_order: 4
---
```

The specified `nav_order` fields of pages with the same parent page should be all numbers or all strings. Pages without a `nav_order` field are ordered alphabetically by `title`, and appear after any explicitly-ordered pages that have the same parent page.

By default, strings and titles are sorted lexicographically, and Capital letters come before lowercase letters. Adding `nav_sort: case_insensitive` in the configuration file ignores case differences when sorting strings and titles, but also sorts numbers lexicographically (so `10` comes before `2`!).

---

## Excluding pages

For specific pages that you do not wish to include in the main navigation (e.g., a 404 page or a landing page) set `nav_exclude: true` in the YAML front matter.

#### Example
{: .no_toc }

```yaml
---
layout: default
title: 404
nav_exclude: true
---
```

---

## Pages with children

Sometimes you will want to create a page with many children (a section). First, it is recommended that you store related pages together in a directory. For example, in these docs, we keep all of the written documentation pages in the `./docs` directory, and each of the sections in subdirectories like `./docs/ui-components` and `./docs/utilities`. This gives us an organization like this:

```
+-- ..
|-- (Jekyll files)
|
|-- docs
|   |-- ui-components
|   |   |-- index.md  (parent page)
|   |   |-- buttons.md
|   |   |-- code.md
|   |   |-- labels.md
|   |   |-- tables.md
|   |   +-- typography.md
|   |
|   |-- utilities
|   |   |-- index.md      (parent page)
|   |   |-- color.md
|   |   |-- layout.md
|   |   |-- responsive-modifiers.md
|   |   +-- typography.md
|   |
|   |-- (other md files, pages with no children)
|   +-- ..
|
|-- (Jekyll files)
+-- ..
```

#### Example
{: .no_toc }

```yaml
---
layout: default
title: UI Components
nav_order: 2
---
```

Here we're setting up the UI Components landing page that is available at URL `/docs/ui-components`, which is ordered second in the main navigation. (Note for users of previous versions of Just the Docs: parent pages no longer need the `has_children` field.)

### Child pages
{: .text-gamma }

On child pages, simply set the `parent:` YAML front matter to the parent page's  title, and set a navigation order (relative to pages having the same parent).

#### Example
{: .no_toc }

```yaml
---
layout: default
title: Buttons
parent: UI Components
nav_order: 2
---
```

The Buttons page appears as a child of UI Components and appears second in the UI Components section.

### Auto-generating Table of Contents

By default, all parent pages will automatically have a Table of Contents at the bottom, listing their child pages. To disable this automatic Table of Contents, set `has_toc: false` in the parent page's YAML front matter.

#### Example
{: .no_toc }

```yaml
---
layout: default
title: UI Components
nav_order: 2
has_children: true
has_toc: false
---
```

### Children with children
{: .text-gamma }

Child pages can themselves have children, recursively. 

#### Example
{: .no_toc }

```yaml
---
layout: default
title: Buttons
parent: UI Components
nav_order: 2
---
```

```yaml
---
layout: default
title: Buttons Child Page
parent: Buttons
nav_order: 1
---
```

This would create the following navigation structure:

```
+-- ..
|
|-- UI Components
|   |-- ..
|   |
|   |-- Buttons
|   |   |-- Button Child Page
|   |
|   |-- ..
|
+-- ..
```

If no two pages have the same title, the `parent` fields alone are sufficient to determine the navigation hierarchy. When grandchildren in different sections have parent pages with the same title, however, the intended parents need to be distinguished. This can sometimes be done using `grand_parent` fields (which are otherwise optional). The `grand_parent` field needs to be the same as the `parent` field of a potential parent page.

#### Example
{: .no_toc }

```yaml
---
layout: default
title: Buttons Child Page
parent: Buttons
grand_parent: UI Components
nav_order: 1
---
```

The optional `ancestor` field generalises the `grand_parent` field to refer to pages two or more levels up. See the navigation [test pages]({{ site.baseurl }}{% link docs/test/index.md %}) for examples of the use of `ancestor`.

---

## Auxiliary Links

To add auxiliary links to your site (in the upper right on all pages), add it to the `aux_links` [configuration option]({{ site.baseurl }}{% link docs/configuration.md %}#aux-links) in your site's `_config.yml` file.

#### Example
{: .no_toc }

```yaml
# Aux links for the upper right navigation
aux_links:
  "Just the Docs on GitHub":
    - "//github.com/pmarsceill/just-the-docs"
```

---

## In-page navigation with Table of Contents

To generate a Table of Contents on your docs pages, you can use the `{:toc}` method from Kramdown, immediately after an `<ol>` in Markdown. This will automatically generate an ordered list of anchor links to various sections of the page based on headings and heading levels. There may be occasions where you're using a heading and you don't want it to show up in the TOC, so to skip a particular heading use the `{: .no_toc }` CSS class.

#### Example
{: .no_toc }

```markdown
# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
```

This example skips the page name heading (`#`) from the TOC, as well as the heading for the Table of Contents itself (`##`) because it is redundant, followed by the table of contents itself. To get an unordered list, replace  `1. TOC` above by `- TOC`.

### Collapsible Table of Contents

The Table of Contents can be made collapsible using the `<details>` and `<summary>` elements , as in the following example. The attribute `open` (to expand the Table of Contents by default) and the styling with `{: .text-delta }` are optional.

```markdown
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>
```

The result is shown at [the top of this page](#navigation-structure). Note that Kramdown allows `{:toc}` to be used only once on each page.
