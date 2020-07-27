---
layout: default
title: Navigation Notes
nav_exclude: true
---
# Navigation Notes

## Background

The navigation hierarchy provided by Just the Docs is currently limited to three levels:

1. Top-level pages with no `parent` field.
2. Pages whose `parent` field is the title of a level-1 page.
3. Pages whose `parent` field is the title of a level-2 page, and whose `grand_parent` field is the title of a top-level page.

The current implementation requires parent pages to have `has_children: true`, and `grand_parent` fields to be consistent with the `parent` fields of their parent pages. Level-2 pages may have the same title when they have different `parent` fields. And level-3 pages may have the same title when they have different `parent` fields or different `grand_parent` fields.

In [PR #192](https://github.com/pmarsceill/just-the-docs/pull/192), [Eugene Kuzmenko](https://github.com/thealjey) proposed allowing the navigation to have unbounded depth, and provided a very elegant implementation (using recursive inclusion of Liquid files) with the following features:

* makes navigation recursive, which means that it can go down to an arbitrary depth
* makes the notion of grand parents/children obsolete. If a page is a child of a page, that itself is a child of another page, that automatically makes this page a grandchild.
* `has_children` and `grand_parent` options are removed as unnecessary
* breadcrumbs are no longer dependent on main navigation setting certain variables
* `nav_exclude` excludes the page from any depth in the menu hierarchy

The implementation of navigation in PR #192 assumes that all pages used as parents have different titles, so that the `parent` fields alone determine the hierarchy. If two parent pages happen to have the same title, the resulting navigation hierarchy includes their combined children twice. The only work-around is to change the page titles to make then all different.

It is possible to adapt the implementation from PR #192 using optional `grand_parent` fields. Existing 3-level sites would not require any changes. However, it is unclear how to best to generalise `grand_parent` to more then three levels, to support the kind of navigation hierarchy mentioned in the following comment on PR #192:

> [...] standardize pages and subpages for easy reading (e.g. all docs for microservices have the same format and same child page names).

## Current proposal

The current proposal is based on the following assumptions:

* Top-level pages cannot have the same title
* Children of the same parent cannot have the same title
* A child cannot have the same title as its parent or any of its ancestors

It implements the following features:

* If no two pages on your website have the same `title`, you only need to set the `parent` titles to fix the hierarchy. You can also have the same `title` on pages that have no children, provided that they have different `parent` titles.

* If two parents have the same `title`, but different grandparents, you can set their `grand_parent` titles to distinguish between their parents. The `grand_parent` title needs to be the same as the `parent` of the `parent`.

* For resolving parents in deeper navigation structures, you can set the `ancestor` field of a page to the title of any page above its `parent` page.  

* If you want the navigation structure in different parts of your website to look the same, you can add the title of the top page of each part as the `ancestor` of all its sub-pages. 

* Instead of using `ancestor`, you can choose an arbitrary number or string for each part, and add it as the `section` field on all the pages of that part.

## Implementation overview

[default layout]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_layouts/default.html

The [default layout] first calls [`init`]. The subsequent calls to [`links`], [`crumbs`], and [`toc`] are independent, so they can be in any order.

[`_includes/nav/`]: https://github.com/pdmosses/just-the-docs/tree/rec-nav/_includes/nav

All the following files are in [`_includes/nav/`]. All variables assigned by the code are prefixed by `nav_` (which is elided when referring to variables in the descriptions below).

[`init`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/init.html

[`init`]
- called from the [default layout]
- creates the `parenthood` grouped array based on the `parent` fields, and the `top_nodes` array
- calls [`page`] to find where the current page is located in the navigation hierarchy

[`page`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/page.html

[`page`]
- called from [`init`]
- traverses the nav hierarchy top-down until it reaches the current page, then:
- sets `page_ancestors` to the array of its ancestors, for use by [`crumbs`]
- sets `page_path` the string of indices leading to it, for use by [`links`]
- sets `page_children` the array of direct children, for use by [`toc`]
- uses [`children`] and [`sorted`] to determine the children of each node

[`links`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/links.html

[`links`]
- called from the [default layout]
- traverses the entire nav hierarchy top-down, outputting an HTML link for each node
- uses `page_path` to test whether each node is active
- uses [`children`] and [`sorted`] to determine the children of each node

[`crumbs`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/crumbs.html

[`crumbs`]
- called from the [default layout] with parameter `page_ancestors`
- outputs HTML for the given array of pages

[`toc`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/toc.html

[`toc`]
- called from the [default layout] with parameter `page_children`
- outputs HTML for the given array of pages

[`children`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/children.html

[`children`]
- called from [`page`] and [`links`] with an array of potential child pages
- sets `children` to those pages with matching `grand_parent`, `section`, and `ancestor` fields

[`sorted`]: https://github.com/pdmosses/just-the-docs/blob/rec-nav/_includes/nav/sorted.html

[`sorted`]
- called from [`page`] and [`links`] with an unsorted array of potential child pages
- sets `sorted` to the result of sorting the array using the `nav_order` fields

## Efficiency

TBA
